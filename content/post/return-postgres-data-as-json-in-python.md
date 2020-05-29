
+++
date = "2017-02-03 20:47:13+00:00"
draft = false
tags = ["python", "postgres"]
title = "Return Postgres data as JSON in Python"
url = "/post/156769849745/return-postgres-data-as-json-in-python"
+++
Postgres supports `` JSON `` and `` JSONB `` for a couple of years now. The support for JSON-functions landed in version 9.2. These functions let Postgres server to return JSON serialized data. This is a handy feature. Consider a case; Python client fetches 20 records from Postgres. The client converts the data returned by the server to tuple/dict/proxy. The application or web server converts tuple again back to JSON and sends to the client. The mentioned case is common in a web application. Not all API’s fit in the mentioned. But there is a use case.

### Postgres Example

Consider two tables, `` author `` and `` book `` with the following schema.

<div class="gist"><a href="https://gist.github.com/kracekumar/322a2fd5ea09ee952e8a7720fd386184" target="_blank">https://gist.github.com/kracekumar/322a2fd5ea09ee952e8a7720fd386184</a></div>

Postgres function `` row_to_json `` convert a particular row to JSON data. Here is a list of authors in the table.

<div class="gist"><a href="https://gist.github.com/kracekumar/3f4bcdd16d080b5a36436370823e0495" target="_blank">https://gist.github.com/kracekumar/3f4bcdd16d080b5a36436370823e0495</a></div>

This is simple, let me show a query with an inner join. The `` book table `` contains a foreign key to `` author table ``. While returning list of books, including author name in the result is useful.

<div class="gist"><a href="https://gist.github.com/kracekumar/eb9f1009743ccb47df2b3a5f078a4444" target="_blank">https://gist.github.com/kracekumar/eb9f1009743ccb47df2b3a5f078a4444</a></div>

As you can see the query construction is verbose. The query has an extra select statement compared to a normal query. The idea is simple. First, do an inner join, then select the desired columns, and finally convert to JSON using row\_to\_json. `` row_to_json `` is available since version `` 9.2 ``. The same functionality can be achieved using other function like `` json_build_object `` in 9.4. You can read more about it in the <a href="https://www.postgresql.org/docs/9.4/static/functions-json.html" target="_blank">docs</a>.

### Python Example

Postgres drivers `` pyscopg2 `` and `` pg8000 `` handles JSON response, but the result is parsed and returned as a tuple/dictionary. What that means, if you execute raw SQL the returned JSON data is converted to Python dictionary using `` json.loads ``. Here is the function that facilitates the conversion in <a href="https://github.com/psycopg/psycopg2/blob/51aa166d5219bf6bcda1f68f33399c930113a1f1/lib/_json.py#L109" target="_blank">pyscopg2</a> and <a href="https://github.com/mfenniak/pg8000/blob/4712bd870fec11b10b961a12b54ef7ccb0f70790/pg8000/core.py#L1433" target="_blank">pg8000</a>.

<div class="gist"><a href="https://gist.github.com/kracekumar/2d1d0b468cafa5197f5e21734047c46d" target="_blank">https://gist.github.com/kracekumar/2d1d0b468cafa5197f5e21734047c46d</a></div>

The psycopg2 converts returned JSON data to list of tuples with a dictionary.

One way to circumvent the problem is to cast the result as text. The Python drivers don’t parse the text. So the JSON format is preserved.

<div class="gist"><a href="https://gist.github.com/kracekumar/b8a832cd036b54075a2715acf2086d62" target="_blank">https://gist.github.com/kracekumar/b8a832cd036b54075a2715acf2086d62</a></div>

Carefully view the printed results. The printed result is a list of tuple with a string.

For SQLAlchemy folks here is how you do it

<div class="gist"><a href="https://gist.github.com/kracekumar/287178bcb26462a1b34ead4de10f0529" target="_blank">https://gist.github.com/kracekumar/287178bcb26462a1b34ead4de10f0529</a></div>

Another way to run SQL statement is to use <a href="http://docs.sqlalchemy.org/en/latest/core/sqlelement.html?highlight=text#sqlalchemy.sql.expression.text" target="_blank">`` text `` function</a>.

The other workaround is to unregister the JSON converter. These two lines should do

    import psycopg2.extensions as ext
    ext.string_types.pop(ext.JSON.values[0], None)

Here is a relevant issue in <a href="https://github.com/psycopg/psycopg2/issues/172" target="_blank">Pyscopg2</a>.

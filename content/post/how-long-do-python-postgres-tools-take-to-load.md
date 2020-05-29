+++
date = "2018-02-04 13:15:49+00:00"
draft = false
tags = ["python", "postgres", "performance"]
title = "How long do Python Postgres tools take to load data?"
url = "/post/170492996890/how-long-do-python-postgres-tools-take-to-load"
+++
Data is crucial for all applications. While fetching a significant amount of data from database multiple times, faster data load times improve performance.

The post considers tools like `` SQLAlchemy statement, SQLAlchemy ORM, Pscopg2, psql `` for measuring latency. And to measure the python tool timing, `` jupyter notebook’s timeit `` is used. Psql is for the lowest time taken reference.

### Table Structure

    annotation=> \d data;
                          Table "public.data"
    Column |   Type    |                     Modifiers
    --------+-----------+---------------------------------------------------
    id     | integer   | not null default nextval('data_id_seq'::regclass)
    value  | integer   |
    label  | integer   |
    x      | integer[] |
    y      | integer[] |
    Indexes:
        "data_pkey" PRIMARY KEY, btree (id)
        "ix_data_label" btree (label)

    annotation=> select count(*) from data;
       count
    ---------
    1050475
    (1 row)

### SQLAlchemy ORM Declaration

    class Data(Base):
        __tablename__ = 'data'
        id = Column(Integer, primary_key=True)
        value = Column(Integer)
        # 0 => Training, 1 => test
        label = Column(Integer, default=0, index=True)
        x = Column(postgresql.ARRAY(Integer))
        y = Column(postgresql.ARRAY(Integer))

### SQLAlchemy ORM

    def sa_orm(limit=20):
        sess = create_session()
        try:
            return sess.query(Data.value, Data.label).limit(limit).all()
        finally:
            sess.close()

### Time taken

    %timeit sa_orm(1)
    28.9 ms ± 4.5 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)

The time is taken in milliseconds to fetch `` 1, 20, 100, 1000, 10000 `` queries.

    {1: 28.9, 20: 26.6, 100: 26.2, 1000: 29.5, 10000: 70.2}

![](https://lh4.googleusercontent.com/RVTwghoKivcQDD6Fhx0HVj6Z65mcu2roe5zouMvyGI-OypghYcX23-v-fXXRVUngUtj3loYeaAxp8g=w1920-h990)

### SQLAlchemy Select statement

    def sa_select(limit=20):
        tbl = Data.__table__
        sess = create_session()
        try:
            stmt = select([tbl.c.value, tbl.c.label]).limit(limit)
            return sess.execute(stmt).fetchall()
        finally:
            sess.close()

### Time Taken

The time is taken in milliseconds to fetch `` 1, 20, 100, 1000, 10000 `` queries.

     {1: 24.7, 20: 24.5, 100: 24.9, 1000: 26.8, 10000: 39.6}

![](https://lh3.googleusercontent.com/lhtoHYN0bL9ZiG2lzOt_YJeIikKrhIhHRXCVjFc7_Jhtr6WgNOBSPPC03e1UdkE8bgl_sl7RSqnyNA=w1920-h990)

### Pscopg2

Pscopg2 is one of the Postgres drivers. You can use pscopg2 along with SQLAlchemy or as a stand-alone tool.

    def pscopg2_select(limit=20):
        conn = psycopg2.connect("dbname=db user=user password=password host=localhost")
        cur = conn.cursor()
        try:
            # Note: In prod, escape SQL queries.
            stmt = f"select value, label from data limit {limit}"
            cur.execute(stmt)
            return cur.fetchall()
        finally:
            cur.close()
            conn.close()

### Pscopg2 Time Taken

The time is taken in milliseconds to fetch `` 1, 20, 100, 1000, 10000 `` queries.

    {1: 17.0, 20: 16.9, 100: 17.3, 1000: 18.1, 10000: 30.1},

![](https://lh3.googleusercontent.com/xuobKLuMaq3w00mOiIb2cpWzsFI_cfDixDUeZp1fG9sfA-W1fKsjH-99m_qKJSm-YnD6gmsgrHV6Uw=w1920-h990)

### Psql

    annotation=> explain (analyze, timing off) select label, value from data limit 10000;
                                           QUERY PLAN
    ------------------------------------------------------------------------------------------------
    Limit  (cost=0.00..322.22 rows=10000 width=8) (actual rows=10000 loops=1)
     ->  Seq Scan on data  (cost=0.00..33860.40 rows=1050840 width=8) (actual rows=10000 loops=1)
    Total runtime: 7.654 ms
    (3 rows)
    Time: 7.654 ms

### Psql time taken

The time is taken in milliseconds to fetch `` 1, 20, 100, 1000, 10000 `` queries.

    {1: 0.9, 20: 0.463, 100: 1.029, 1000: 1.643, 10000: 7.654}

### All timings

    {'pscopg2': {1: 17.0, 20: 16.9, 100: 17.3, 1000: 18.1, 10000: 30.1},
    'sa_orm': {1: 28.9, 20: 26.6, 100: 26.2, 1000: 29.5, 10000: 70.2},
    'sa_select': {1: 24.7, 20: 24.5, 100: 24.9, 1000: 26.8, 10000: 39.6},
    'sql_select': {1: 0.9, 20: 0.463, 100: 1.029, 1000: 1.643, 10000: 7.654}}

### Chart of all the timings

![](https://lh6.googleusercontent.com/yfK3ne2netEyVdNs1px9hO9VRSPwI8mb8DoNOkqZa-iGZmW8QWg7PdGsCDRJEmdccd6ZK6jaa5r41g=w1920-h990)

Lower the bar, better for the performance.

As you can see, SQLAlchemy ORM is slowest, and pscyopg2 is fastest. SQLAlchemy select statement is close to the pscopg2 performance, provides a sweet spot for not writing SQL Queries and handling data at a high level.

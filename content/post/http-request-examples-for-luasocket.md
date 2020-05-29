
+++
date = "2013-07-19 08:01:00+00:00"
draft = false
tags = ["lua", "http", "luasocket"]
title = "http request examples for luasocket"
url = "/post/55856724724/http-request-examples-for-luasocket"
+++
I was looking for `` http `` library in lua and landed in <a href="http://w3.impa.br/~diego/software/luasocket/http.html" target="_blank">luasocket.http page</a>. It isn’t well documented, sent few `` GET, POST, PUT `` requests and figured few bits. This blog post aims in bridging the gap(code examples).

In this example, I will use <a href="https://httpbin.org" target="_blank">httpbin</a> as target site. The complete code is available as <a href="https://gist.github.com/kracekumar/6037243" target="_blank">gist</a>.

The following code should be executed like a standalone lua file(`` lua lua_httpbin.lua ``) and while executing the code in interpreter please make local variables `` http, ltn12, base_url `` as global variables.

<pre><code>local http = require("socket.http")
local ltn12 = require("ltn12")

local base_url = "https://httpbin.org/"

--Helper for priniting nested table
function deep_print(tbl)
    for i, v in pairs(tbl) do
        if type(v) == "table" then 
            deep_print(v)
        else 
            print(i, v) 
        end
    end
end

function http_request( args )
--http.request(url [, body])
--http.request{
--  url = string,
--  [sink = LTN12 sink,]
--  [method = string,]
--  [headers = header-table,]
--  [source = LTN12 source],
--  [step = LTN12 pump step,]
--  [proxy = string,]
--  [redirect = boolean,]
--  [create = function]
--}
--
--
    local resp, r = {}, {}
    if args.endpoint then
        local params = ""
        if args.method == nil or args.method == "GET" then
            -- prepare query parameters like <a href="http://xyz.com" target="_blank">http://xyz.com</a>?q=23&amp;a=2
            if args.params then
                for i, v in pairs(args.params) do
                    params = params .. i .. "=" .. v .. "&amp;"
                end
            end
        end
        params = string.sub(params, 1, -2)
        local url = ""
        if params then url = base_url .. args.endpoint .. "?" .. params else url = base_url .. args.endpoint end
        client, code, headers, status = http.request{url=url, sink=ltn12.sink.table(resp),
                                                method=args.method or "GET", headers=args.headers, source=args.source,
                                                step=args.step,     proxy=args.proxy, redirect=args.redirect, create=args.create }
        r['code'], r['headers'], r['status'], r['response'] = code, headers, status, resp
    else
        error("endpoint is missing")
    end
    return r
end
</code></pre>

`` http_request `` takes table as an argument which is almost similar to `` http.request `` function with an extra parameter `` params `` which will be helpful for passing parameters to `` GET `` request.

`` http_request{endpoint=endpoint, params={age=23, name="kracekumar"}} `` rather than `` http_request{endpoint=endpoint .. "?age=23&amp;name=kracekumar"} ``.

Remaining piece of code

    function main()
        -- Normal GET request
        endpoint = "/user-agent"
        print(endpoint)
        deep_print(http_request{endpoint=endpoint})
        -- GET request with parameters
        endpoint = "/get"
        print(endpoint)
        deep_print(http_request{endpoint=endpoint, params={age=23, name    ="kracekumar"}})
        -- POST request with form
        endpoint = "/post"
        print(endpoint)
        local req_body = "a=2"
        local headers = {
        ["Content-Type"] = "application/x-www-form-urlencoded";
        ["Content-Length"] = #req_body;
        }
        deep_print(http_request{endpoint=endpoint, method="POST", source=ltn12.source.string(req_body), headers=headers})
        -- PATCH Method
        endpoint = "/patch"
        print(endpoint)
        deep_print(http_request{endpoint=endpoint, method="PATCH"})
        -- PUT Method
        endpoint = "/put"
        print(endpoint)
        deep_print(http_request{endpoint=endpoint, method="PUT", source    =ltn12.source.string("a=23")})
        -- Delete method
        endpoint = "/delete"
        print(endpoint)
        deep_print(http_request{endpoint=endpoint, method="DELETE",     source=ltn12.source.string("a=23")})
    
    end
    
    main()

_output_

    # First lua is directory name 
    ➜  lua  lua lua_httpbin.lua
    /user-agent
    status    HTTP/1.1 200 OK
    code    200
    connection  Close
    content-type    application/json
    date    Fri, 19 Jul 2013 07:32:14 GMT
    content-length  37
    access-control-allow-origin *
    server  gunicorn/0.17.4
    1   {
      "user-agent": "LuaSocket 2.0.2"
    }
    /get
    status  HTTP/1.1 200 OK
    code    200
    connection  Close
    content-type    application/json
    date    Fri, 19 Jul 2013 07:32:15 GMT
    content-length  258
    access-control-allow-origin *
    server  gunicorn/0.17.4
    1   {
      "headers": {
        "Host": "httpbin.org",
        "Connection": "close",
        "User-Agent": "LuaSocket 2.0.2"
      },
      "url": "http://httpbin.org/get?age=23&amp;name=kracekumar",
      "args": {
        "age": "23",
        "name": "kracekumar"
      },
      "origin": "106.51.166.47"
    }
    /post
    status    HTTP/1.1 200 OK
    code    200
    connection  Close
    content-type    application/json
    date    Fri, 19 Jul 2013 07:41:54 GMT
    content-length  350
    access-control-allow-origin *
    server  gunicorn/0.17.4
    1   {
        "json": null,
        "origin": "106.51.166.47",
        "form": {
            "a": "2"
        },
        "url": "http://httpbin.org/post",
        "args": {},
        "headers": {
        "Connection": "close",
        "Host": "httpbin.org",
        "User-Agent": "LuaSocket 2.0.2",
        "Content-Type": "application/x-www-form-urlencoded",
        "Content-Length": "3"
        },
    "files": {},
    "data": ""
    }
    /patch
    status  HTTP/1.1 200 OK
    code    200
    connection  Close
    content-type    application/json
    date    Fri, 19 Jul 2013 07:32:17 GMT
    content-length  251
    access-control-allow-origin *
    server  gunicorn/0.17.4
    1   {
      "files": {},
      "headers": {
        "Host": "httpbin.org",
        "Connection": "close",
        "User-Agent": "LuaSocket 2.0.2"
      },
      "args": {},
      "json": null,
      "form": {},
      "origin": "106.51.166.47",
      "url": "http://httpbin.org/patch",
      "data": ""
    }
    /put
    status  HTTP/1.1 200 OK
    code    200
    connection  Close
    content-type    application/json
    date    Fri, 19 Jul 2013 07:32:18 GMT
    content-length  276
    access-control-allow-origin *
    server  gunicorn/0.17.4
    1   {
      "json": null,
      "origin": "106.51.166.47",
      "form": {},
      "url": "http://httpbin.org/put",
      "args": {},
      "headers": {
        "Connection": "close",
        "Host": "httpbin.org",
        "User-Agent": "LuaSocket 2.0.2",
        "Content-Length": "0"
      },
      "files": {},
      "data": ""
    }
    /delete
    status  HTTP/1.1 200 OK
    code    200
    connection  Close
    content-type    application/json
    date    Fri, 19 Jul 2013 07:32:19 GMT
    content-length  223
    access-control-allow-origin *
    server  gunicorn/0.17.4
    1   {
      "args": {},
      "json": null,
      "headers": {
        "Connection": "close",
        "User-Agent": "LuaSocket 2.0.2",
        "Host": "httpbin.org"
      },
      "origin": "106.51.166.47",
      "data": "",
      "url": "http://httpbin.org/delete"
    }

_My 2p_

1.   When sending `` GET `` request don’t send paramters in request body.
2.   When sending `` POST `` request set custom header for form.

My experience of `` luasocket.http `` produces solid reason to create a new library with helper functions like <a href="http://python-requests.org" target="_blank">requests</a>.

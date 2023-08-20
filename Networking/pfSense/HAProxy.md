---
title: HAProxy
description: HAProxy
published: true
date: 2022-07-25T13:08:39.586Z
tags: pfsense, networking, Reverse Proxy, Loadbalacing
editor: markdown
dateCreated: 2022-05-16T14:33:14.193Z
---
# Accelerate Your APIs by Using the HAProxy Cache

[Nick Ramirez](https://www.haproxy.com/blog/author/nramirez/ "Posts by Nick Ramirez") | Oct 26, 2020 | [MICROSERVICES](https://www.haproxy.com/blog/category/microservices/), [PERFORMANCE](https://www.haproxy.com/blog/category/performance/) | [3 comments](https://www.haproxy.com/blog/accelerate-your-apis-by-using-the-haproxy-cache/#respond)

![Accelerate Your APIs by Using the HAProxy Cache](https://cdn.haproxy.com/wp-content/uploads/2020/10/API-Caching-1-1024x512.png)

_HAProxy’s cache helps boost API performance by serving saved messages to your users._

The age of rendering most of a web page’s contents on the server and then delivering it as a colossal HTML file is fading into the past. Modern web frameworks like Angular, React, and Vue push towards creating components instead—individual elements on the page that fetch their data in the background and poll for asynchronous updates—which can be reused across your site. Meanwhile, the major browsers have added support for [Web components](https://developer.mozilla.org/en-US/docs/Web/Web_Components), which may eventually cement components as an official web standard. The latest version of the HTTP protocol also makes components more attractive: HTTP/2, makes asynchronous communication with backend services more efficient by allowing [better use of connections](https://developers.google.com/web/fundamentals/performance/http2#request_and_response_multiplexing) and utilizing multiplexing.

Components call RESTful APIs to get data from the backend servers. Be cautious, though. It’s not always the best idea to have client-side code connect directly to these servers. Doing so tightly couples frontend code to specific endpoints, which makes it harder to shuffle servers, do maintenance on them, or deploy updates. You can gain many benefits from placing HAProxy in between to act as an [API gateway](https://www.haproxy.com/blog/using-haproxy-as-an-api-gateway-part-1/). An API gateway is a proxy that relays messages back and forth. It also adds functions like authentication, TLS encryption, rate limiting, and observability.

Something else that HAProxy adds is the ability to cache API responses, which can boost how quickly clients receive data. In this blog post, you will learn how to set up HAProxy’s cache feature, which will improve how fast you can deliver messages and lessen the load placed upon your backend servers.

## Why You Should Cache API Responses

There are two readily available caches: a client-side (browser) cache and a server-side (HAProxy) cache. A browser’s cache will boost performance for a single user. HAProxy’s cache, which is known as a _proxy cache_, will speed it up for all users because once a resource is cached in the proxy, it’s available for anyone making the same request until it expires. It’s easy to enable, but you should know how to use it effectively.

HAProxy’s cache runs in memory, which makes it fast. Other proxy caches need to read and write state on the filesystem, which incurs some I/O latency. Also, because it runs within HAProxy, you don’t need to contact an upstream cache server, which means you have one less transfer across the network. In some cases, however, you’ll want the extra features of a shared cache server like Varnish. HAProxy’s caching feature is modest in comparison, but it might be exactly what you need for caching API responses.

Why should you cache API responses?

For one thing, it will speed up the time components take to receive their data, which has a huge effect on how responsive your website seems overall. One of the biggest obstacles to adopting a component-based design is the fear that your webpage will render its initial HTML page quickly, but linger in an unusable state while the individual components wait to load. A lot of that time waiting is spent processing the request on the web server, pulling the requested data out of the database, and forming the JSON-encoded response. Caching allows you to perform those steps only once and then serve the saved message to other clients.

Another reason to love proxy caching is because it reduces load on your servers. They don’t need to process nearly as many requests, many of which are likely the same request they saw earlier. It’s perfectly fine to serve a slightly stale response for content that doesn’t change extremely often, such as daily news feeds, product descriptions, reviews, and comment boards. Even caching this content for five or ten seconds could have a worthwhile impact, depending on the number of users viewing that same data. By the way, caching for a very short period of time is known as _microcaching_.

API functions that return data, rather than modify it, are best suited for caching. This typically includes any function called with GET. Just be sure that your API responses do not include any information that is specific to a user, such as API keys, user profile data, and the like.

## How to Cache with HAProxy

In your HAProxy configuration file, add a `cache` section. It goes at the same level as a `global` or `defaults` section. You can have more than one cache section to create multiple caches for different purposes, and each can set its own `max-age` and other attributes.

global

# global settings

defaults

# default settings

```
cache mycache

total-max-size 4095 # MB

max-object-size 10000 # bytes

max-age 30 # seconds
```
[view raw](https://gist.github.com/haproxytechblog/c36b02678c5449b9f0876af3d15c4774/raw/93b6537c17e23f44ec4fdb6606baefb765eaa302/blog20201021-01.cfg)[](https://gist.github.com/haproxytechblog/c36b02678c5449b9f0876af3d15c4774#file-blog20201021-01-cfg)[](https://github.com/)

The `total-max-size` directive sets the total amount of memory that this cache can consume; It has a maximum value of 4095 megabytes. The `max-object-size` directive sets the largest size of a single item you can store in the cache, and it can only be half of the `total-max-size` value. In this example, I’ve set it to 10,000 bytes, which is 10 kilobytes. If a response is larger, it simply won’t be cached. The last directive, `max-age`, sets the time-to-live (TTL) in seconds for an item in the cache. After the TTL expires, the item will be removed from memory.

Next, add an `http-request cache-use` and an `http-response cache-store` directive to your `backend` section. The former uses a cached resource if it’s found and the latter adds it to the cache. Both take the name of a `cache` section.

frontend fe_api

bind :80

default_backend be_api

backend be_api

# Get from cache / put in cache

http-request cache-use mycache

http-response cache-store mycache

# server list

server s1 172.25.0.10:8080 check

[view raw](https://gist.github.com/haproxytechblog/c36b02678c5449b9f0876af3d15c4774/raw/93b6537c17e23f44ec4fdb6606baefb765eaa302/blog20201021-02.cfg)[](https://gist.github.com/haproxytechblog/c36b02678c5449b9f0876af3d15c4774#file-blog20201021-02-cfg)[](https://github.com/)

You can also restrict which responses should be cached by appending an _if_ statement to the end of the `http-request cache-use` directive. For instance, if you wanted to cache only when the requested URL path begins with **/api/news_feed/**, you would use the following:

http-request cache-use api_cache if { path_beg /api/news_feed/ }

http-response cache-store api_cache

[view raw](https://gist.github.com/haproxytechblog/c36b02678c5449b9f0876af3d15c4774/raw/93b6537c17e23f44ec4fdb6606baefb765eaa302/blog20201021-03.cfg)[](https://gist.github.com/haproxytechblog/c36b02678c5449b9f0876af3d15c4774#file-blog20201021-03-cfg)[](https://github.com/)

Notice that you add a condition to the _http-request_ line, but you do not need one on the _http-response_ line. HAProxy is designed to skip caching if there’s no chance the item will ever be used. Alternatively, the backend server can return a [Cache-Control](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control) header with a _no-store_ attribute to disable caching of a particular response.

> Cache-Control: no-store

The Cache-Control header also supports the _s-maxage_ attribute, which lets you override the TTL that was set in HAProxy’s cache section. Consider the following Cache-Control header, which allows the response to be cached, but sets its TTL to 10 seconds:

> Cache-Control: public,s-maxage=10

To see the TTL that was set on an item in the cache, call the [HAProxy Runtime API](https://www.haproxy.com/blog/dynamic-configuration-haproxy-runtime-api/) `show cache` command, which shows the TTL as the _expire_ field:

$ echo "show cache" | socat tcp-connect:127.0.0.1:9999 -

0x7fcad7c9603a: api_cache (shctx:0x7fcad7c96000, available blocks:4193280)

0x7fcad7c960c0 hash:3075548050 size:363 (1 blocks), refcount:0, expire:10

[view raw](https://gist.github.com/haproxytechblog/c36b02678c5449b9f0876af3d15c4774/raw/93b6537c17e23f44ec4fdb6606baefb765eaa302/blog20201021-04.sh)[](https://gist.github.com/haproxytechblog/c36b02678c5449b9f0876af3d15c4774#file-blog20201021-04-sh)[](https://github.com/)

You can also get metrics about your cache, which can be displayed in Grafana. If you’ve enabled [Prometheus metrics](https://www.haproxy.com/blog/haproxy-exposes-a-prometheus-metrics-endpoint/) in HAProxy, scrape the following metrics from HAProxy’s Prometheus endpoint (where the _proxy_ label would be the name of your frontend or backend):

-   haproxy_frontend_http_cache_lookups_total{proxy=”fe_api”}
-   haproxy_frontend_http_cache_hits_total{proxy=”fe_api”}
-   haproxy_backend_http_cache_lookups_total{proxy=”be_api”}
-   haproxy_backend_http_cache_hits_total{proxy=”be_api”}

These metrics show you how many cache lookups were performed and how many resulted in a cache hit. You can use that to adjust your TTL values.

One last trick: You can return a response header that shows whether the requested resource was found in the cache. Currently, this method is a temporary solution until a future version of HAProxy adds a better way. Add these two lines to your `frontend`. They check if the `srv_id` fetch method returns the name of a server that was used to handle the request. If no value is returned, it means that HAProxy used the cache.

http-response set-header X-Cache-Status HIT if !{ srv_id -m found }

http-response set-header X-Cache-Status MISS if { srv_id -m found }

[view raw](https://gist.github.com/haproxytechblog/c36b02678c5449b9f0876af3d15c4774/raw/93b6537c17e23f44ec4fdb6606baefb765eaa302/blog20201021-05.cfg)[](https://gist.github.com/haproxytechblog/c36b02678c5449b9f0876af3d15c4774#file-blog20201021-05-cfg)[](https://github.com/)

HAProxy will set the _X-Cache-Status_ header to _HIT_ if the item was found in the cache, or to _MISS_ otherwise.

## *PFSENSE*
To verify if cache is working try running on a ssh/console this command:

```
/usr/local/pkg/haproxy/haproxy_socket.sh show cache
```

Don't edit haproxy.inc.

Add the cache section to the 'Global Advanced pass thru' textbox.

```
cache MyCache
  total-max-size 10
  max-age 60
```

and add some cache items to the backend 'Backend pass thru':

```
http-request cache-use MyCache if TRUE
http-response cache-store MyCache if TRUE
```

or we can set it conditionally by

```
http-request cache-use MyCache if { path_beg /api/ }
http-response cache-store MyCache
```

For a basic test that is.. For actual usage you should think about what acl's to apply before trying to use the cache for everything..

## Additional Tuning
we can tune things like window size. SSL ciphers cache size etc. here is an example of a working HAProxy Global Advanced Pass thru config on pfsense
```
tune.ssl.cachesize 100000
tune.ssl.lifetime 600
tune.ssl.maxrecord 1460
tune.h2.initial-window-size 1048576
tune.ssl.default-dh-param 2048
ssl-default-bind-ciphers ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256
ssl-default-bind-options no-sslv3 no-tlsv10 no-tlsv11 no-tls-tickets
ssl-default-server-ciphers ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256
ssl-default-server-options no-sslv3 no-tlsv10 no-tlsv11 no-tls-tickets
cache MyCache
    total-max-size 4095   # MB
    max-object-size 10000 # bytes
    max-age 60            # seconds
```
## Conclusion

HAProxy’s cache helps boost the speed of your API services, resulting in a more responsive website. Define how long responses should be cached using the `max-age` directive, which you can override with a Cache-Control header. If there are certain responses that should not be cached at all, you can use an _if_ statement to filter them out or you can set your Cache-Control header to _no-store_. The HAProxy Runtime API will show you how long items will live in the cache and HAProxy’s Prometheus metrics endpoint exposes counters for lookups and cache hits. Now go and enjoy the benefits of proxy caching!
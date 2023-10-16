---
title:  "MISSing Cache"
date: 2023-05-22 09:00:00 +0100
img_path: /assets/img/bypass-web-server-cache/
categories: [misconfigurations]
tags: [cache,techniques,server,bypass]
image:
  path: cache.png
---

# Bypassing a Web Server's Cache: Common Techniques


During my development of monitors for e-commerce and retail websites, it was always essential to make the software as fast as possible. A very common way to obtain server's response before anyone else is by bypassing cache and getting a sweet MISS from a server. Below are some common techniques I used to bypass web server's cache and obtain origin server's content directly.


## 1. Query String Manipulation

Web servers often use the *Uniform Resource Locator (URL)* and its query string parameters to identify unique resources. By manipulating these parameters, an attacker can trick the server into treating each request as a new resource, effectively bypassing the cache. Here are a few examples:

```
https://www.example.com/page?param=value1
https://www.example.com/page?param=value2
```

In this scenario, the server might cache the response for the first URL, but if an attacker modifies the parameter's value, it becomes a unique request, bypassing the cache.

## 2. URL Capitalization

URLs are case-insensitive, but web servers may treat URLs with different capitalization as distinct resources. An attacker can take advantage of this by accessing the same resource with different capitalization, effectively bypassing the cache. For example:

```
HIT : https://www.example.com/page 
MISS: https://www.example.com/pAge
```


In this case, even though the resource is the same, the server treats each request as unique due to the differing cookie values.


## 3. HTTP Method Manipulation

HTTP methods, such as GET, POST, PATCH, PUT, DELETE, and others, define the purpose of a request and how the server should handle it. Trying different HTTP methods manipulation can be an effective way to bypass a web server's cache, a common way I had success with is by utilizing the POST request method.

Web servers typically treat POST requests differently from GET requests. While GET requests are often cached to improve performance, POST requests are usually not cached. This is because POST requests typically involve sending data to the server, which may result in state changes or modifications on the server side.

Other methods such as PATCH or PUT could also lead for a server's cache to be bypassed.

## 4. *Extra*: Appending 0's to Product Identifiers

Many e-commerce websites display their products using numerical product identifiers such as:
`https://example.com/pid/19035.html`

On some of these websites I have found that appending 0's before the *PID (Product Identifier)* yields in the server's cache treating the URL differently and thus bypassing it.

```
https://example.com/pid/000019035.html
https://example.com/pid/019035.html
https://example.com/pid/00000019035.html
```
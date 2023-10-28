---
title:  "Monitoring JavaScript files for bug hunting"
date: 2023-10-28 10:42:00 +0200
img_path: /assets/img/monitoring-js-files/
categories: [research]
tags: [automation,monitoring]
image:
  path: notification.png
---

## Understanding JS Client-Side

When performing recon on a target, it's common to start by exploring hidden assets, subdirectories and paths. Sometimes, we miss interesting information from something which is loaded by default: **JavaScript files**.

Nowadays, we have interesting tools such as [JS Miner](https://portswigger.net/bappstore/0ab7a94d8e11449daaf0fb387431225b), a Burp Suite extension which scans for secrets, URLs and other revealing information from the JS files loaded by a website. While this is effective when scanning directly the target, it's even more interesting to see the historic changes on those JavaScript files, to potentially find more relevant information. 

I got the idea of writing this post from a [publicly disclosed H1 report](https://hackerone.com/reports/2218334), where a researcher, [bebiks](https://hackerone.com/bebiks) found a hidden GraphQL endpoint under HackerOne's bug bounty program, by scanning it's JS files and finding a change in the client-side code.

## Extracting strings from JS files

In order to extract strings from JS files the researcher mentioned, he used [Tree-Sitter](https://tree-sitter.github.io/tree-sitter/), a parser generator tool. Interestingly, the tool has a [CLI](https://github.com/tree-sitter/tree-sitter/blob/master/cli/README.md) through which we can perform queries and parse Javascript files.

The CLI has different commands to use but we are interested in the `query` command to extract information directly from the JavaScript file.

```
tree-sitter-query 
Search files using a syntax tree query

USAGE:
    tree-sitter query [FLAGS] [OPTIONS] <query-path> [paths]...
```

This command works by providing a query, and a path or multiple paths to JavaScript files which will be analyzed. 

A simple query which returns all strings from a JS file is the following:

```
(string) @string
```

You can also extract template strings by adding at the end:
```
(template_string) @template_string
``` 

However, I personally don't use this method to monitor files because it can lead to false positives, as template strings can sometimes include variable names which are changed very frequently in JS files.

So we save the query we want on a file such as `strings.scm` and then we run:
```js
tree-sitter query strings.scm <jsfile>.js
```

When running this query under HackerOne's main JavaScript file for example, we get many results.
![Tree Sitter Query Results](query.png)

From here on, it is interesting to extract the strings, which can be simply done with sed:
```
tree-sitter query strings.scm <jsfile>.js | sed 's/.*text: //' > <jsfile>.js.strings
``` 

Now that we have stored all strings on a file, we can create a cron job which fetches the JavaScript file routinarily, downloads it, extracts the strings and is compared to the previous version stored. If it finds any new strings, it can send a notification so we can check what has changed. 

I wrote my own POC script in Python to do this for HackerOne's main JavaScript file. After running for a few hours I was already getting the first new discoveries!

![New Strings discovered](notification.png)

## Conclusions

Personally, I think this is a very effective way of potentially finding interest information, such as new features implemented by the target, or changes into existing ones, which may happen to be vulnerable. For anyone who likes automation and monitoring a target without missing things, monitoring JS files can be a gold mine to be explored.
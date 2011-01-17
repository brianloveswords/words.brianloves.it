title: Notes for couch
date: 1900/01/01
author: Brian J Brennan
tags: couchdb learning
draft: true

## pitfalls

### nginx
When using `curl` trying to `PUT` for new databases doesn't work without `d ''`.
Nginx spits back `HTTP Error 411 Length required`: nginx doesn't allow PUT without a Content-Length.

A simple workaround is to add `d ''` to the end, which will make curl
automatically set a Content-Length header of 0. This is annoying.

Some people believe this is nginx being too strict [[ link to those people here ]].

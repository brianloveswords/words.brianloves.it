title: Notes for couch
date: 1900/01/01
author: Brian J Brennan
tags: technical couchdb learning
draft: true

## nginx

When using `curl` trying to `PUT` for new databases doesn't work without `d ''`.
Nginx spits back `HTTP Error 411 Length required`: nginx doesn't allow PUT without a Content-Length.

A simple workaround is to add `d ''` to the end, which will make curl
automatically set a Content-Length header of 0. This is annoying.

Some people believe this is nginx being too strict [[ link to those people here ]].


## other

I built couch (v1.0.1) from source. When I ran the test suite, things did not
go so well. Pretty much every test failed and the load on my tiny Linode 512
hovered around 3.0 for several hours.

<img src='http://cl.ly/2W1d2b30043r1L3q0z0c' title='Not good' alt='Screenshot of the emails linode was sending me'>

This is a problem.


title: What and Why
date: 1900/01/01
author: Brian J Brennan
tags: badges 
draft: true 

## what am I doing
I'm building a badge system for awarding badges that mean something

## why am I doing it
Because people deserve recognition for what they do.

## design & technical details
**Note**: this system is still in its infancy and parts, or all, of the design
may change. I'm working on a pilot right now with P2PU.org to see how this
system works and will write about the results of the

The system is designed in three parts. The **issuer** is a site that already
exists. This site may even already be giving badges. To contribute badges to
this system, the issuer will send a message to the **hub** with some
information about the badge and some information about the user who earned the
badge.

The **hub** is a service that sits out on the way accepting requests to store
badges, or fufilling requests to display badges on third-party sites,
or **consumers**. Issuers store badges in the hub on behalf of users,
and **consume** can request to display a user's badges on their site.

A **consumer** site can be 


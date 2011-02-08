title: Notes for couch
date: 1900/01/01
author: Brian J Brennan
tags: technical couchdb learning

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

<img src="http://cl.ly/2W1d2b30043r1L3q0z0c/Screen_shot_2011-01-17_at_5.54.05_PM.png" title='Not good' alt='Screenshot of the emails linode was sending me'>

There is more than likely something horribly wrong in the way I set up couch or
there could be some strangeness with the nginx proxying. But even when I
created a database at [CouchOne](http://couchone.com) the test suite didn't
come up clean (though it fared way better).

Despite the test suite problems, I soldiered on with my instance. Things came
to a complete stop afterI tried to put in a design document and get back the
view: my instance would never respond no matter how long I waited.

At this point I gave up and replicated my db instance to CouchOne, which
worked beautifully.

* following along with couchguide
* finally got to the part where I'm using couchapp
* cloned the sofa app, tried to push it to CouchOne. This is what happened
    $ couchapp push http://badgehub.couchone.com/sofa. 
    2011-01-17 23:28:24 [CRITICAL] unknown error [[Errno 35] Resource temporarily unavailable]

    Traceback (most recent call last):
      File "/usr/local/Cellar/python/2.7.1/lib/python2.7/site-packages/couchapp/dispatch.py", line 48, in dispatch
        return _dispatch(args)
      File "/usr/local/Cellar/python/2.7.1/lib/python2.7/site-packages/couchapp/dispatch.py", line 92, in _dispatch
        return fun(conf, conf.app_dir, *args, **opts)
      File "/usr/local/Cellar/python/2.7.1/lib/python2.7/site-packages/couchapp/commands.py", line 77, in push
        doc.push(dbs, noatomic, browse, force)
      File "/usr/local/Cellar/python/2.7.1/lib/python2.7/site-packages/couchapp/localdoc.py", line 119, in push
        db.save_doc(doc, force_update=True)
      File "/usr/local/Cellar/python/2.7.1/lib/python2.7/site-packages/couchapp/client.py", line 281, in save_doc
        resp = self.res.put(docid, payload=json.dumps(doc), **params)
      File "/usr/local/Cellar/python/2.7.1/lib/python2.7/site-packages/restkit/resource.py", line 167, in put
        headers=headers, params_dict=params_dict, **params)
      File "/usr/local/Cellar/python/2.7.1/lib/python2.7/site-packages/couchapp/client.py", line 130, in request
        raise RequestFailed("unknown error [%s]" % str(e))
    RequestFailed: unknown error [[Errno 35] Resource temporarily unavailable]

* fine, I'll try to get couch working on my server
* Uninstalled my version of couch, sudo apt-get install couchdb.
* `couchapp push . sofa` actually works...
* ...but when I try to go to the url, I get `{"error":"render_error","reason":"function raised error: ReferenceError: require is not defined"}`
* found out that Ubuntu 10.04 comes with CouchDB 0.10.x, and that version doesn't support require, which is used in Sofa.
* misleading because couchguide says:
> If you get an error, make sure your target CouchDB instance is running by making a simple HTTP request to it....The response should look like: `{"couchdb":"Welcome","version":"0.10.1"}`
* so now trying to install 1.0.1 via [build scripts provided by CouchOne](https://github.com/couchone/build-couchdb)
* still can't push using `couchapp push . http://remoteserver/sofa`, but at least I can ssh into the remote server and do `couchapp push . sofa` there.


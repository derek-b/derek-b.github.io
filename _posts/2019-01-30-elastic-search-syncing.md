---
layout: post
title:  "Keeping Elasticsearch in Sync with a Database"
categories: search data
---
Elasticsearch is a great way to store and search document based data. However it isn't 
always going to satisfy all of your storage needs. So, you will want to find ways to keep
Elasticsearch up to date with the latest data from your database.

## No fast writes

The easiest way to keep your Elasticsearch documents up to date would be to make a 
PUT call with your new document every time you commit a transaction to your database.
This can have disastrous effects on performance because Elasticsearch is built to read 
and search data.  A lot of effort has gone in to making
those run quickly and reliably, at the expense of writing data. Due to 
the way the data behind an index is stored writing data will take resources away from 
your reads and searches. So, you'll need to find another strategy.

## Bulk API saves

The bulk API is the best way to insert or update data. Not only will you save network
traffic from multiple POST or PUT calls, you will also minimize the impact on your
Elasticsearch installation by allowing it to write data less often. 

```
POST my-index/doc/_bulk
{"index": {"_id" : "1"}}
{"field1":"Value One","field2":"Value Two"}
{"index": {"_id" : "1"}}
{"field1":"Another Field Element","field2":"More text here"}
```

The bulk API expects one action line followed by a JSON document. In this example we 
specify the index name and type (document name) in the URI.

## Keying your data

You will save yourself some headaches if you keep the database key in Elasticsearch, and 
not the Elasticsearch key in your database. It is tempting to let Elasticsearch create
a key for you and then save that key back in your database. This can make the connection
between your two data stores very clear. I prefer not to do this because it will increase
the load on your database and will unnecessarily complicate your application. You will
have created a situation which requires two way syncing between your data sources.  Ideally most
of your application logic and database will not be aware of the Elasticsearch installation.
You should consider specifying a key in Elasticsearch and having it be created from a
unique element(s) of your data such as the primary key.


## Queueing data

A common strategy is to have your app push your updates to a message queue. Then 
you can periodically read data from your queue and make a bulk insert. This strategy
works well when you have the ability to add code in the places where you save data to
your database. You can even do so asynchronously which will have very little impact on
your application's performance. For more information on promises in PHP check out the
[React PHP library](https://reactphp.org/promise)

## Copying sections of data

The strategy of queueing your data can work well but does require you to make a lot of 
additions to your application's code. What if you have multiple applications updating
your data? Or maybe you have legacy code that would be more difficult to update.
In these cases you should consider reading your data out of your database in chunks.
If your database has an update timestamp you could use that to get all data that was 
updated since your last bulk index. Depending upon the size of your data and how up to date
you need your search data to be, you could select based upon specific data elements
and run through your entire data store at a certain interval. I would only do this
if you are guaranteed a small dataset and downtime each night. Even then this solution
could grow troublesome as your data grows.

## Wrapping Up
Well, there you have it. Elasticsearch can be a wonderful tool if used for its strengths.
So, don't be afraid to get started and keep your data stores and indexes in sync.
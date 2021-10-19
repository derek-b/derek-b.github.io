---
layout: post
title:  "Primary Keys - Surrogate v. Natural"
categories: database
---

This Saturday I'll be speaking about databases at [Chippewa Valley Code Camp](http://chippewavalleycodecamp.com/).
 So I've been thinking a lot about various database topics including how to choose a 
 primary key for your database table. I am a big proponent of using surrogate keys, 
 however, on a few occasions I have been convinced otherwise.

To me, a surrogate key means the primary key of a database that is not related at all to 
the actual data being saved. This usually ends up being an auto-incremented integer column. 
I have been using surrogate keys for so long I forget that many developers use natural keys 
by making a key out of data from the table.

To make a good primary key you need a unique value that cannot be null. It also needs be 
unchanging for the row it refers to and a data type that is easily indexed. While there 
are certainly other options I always choose an integer.

So, why do we care whether or not this number has meaning? Once you assign it meaning it 
becomes subject to your business rules or even worse the business rules of another group. 
There was a time when social security numbers were a common choice of primary key within 
the United States. The logic was that since the SSN was controlled by the US government 
everybody would have a unique one that never changed so it would meet all the rules of 
a good primary key. Not so fast. This design doesn't take into account non-citizens or 
victims of identity theft that get assigned new SSNs.  Now for compliance reasons most 
companies should be encrypting the SSN column so it no longer is useful as a primary key. 
I have made similar arguments against pretty much any data column that could be used as 
a key.

I often caution developers about assigning meaning to a surrogate primary key. I have 
heard arguments that if the meaning is only behind the scenes it doesn't break these 
rules. However, this can still lead to problems. One example I remember seeing is a users 
table where an ID less than 100 was an administrator, 100 to 999 was an non administrator 
employee and 1000 and above was a customer. Let's think about what that means. First, you 
are limiting yourself to a particular number of employees. Next, your user creation script 
will be unnecessarily complicated. Also, what happens when a standard employee gets a 
promotion and now gets administrator rights? You have to re-key that user which is not 
an easy task. This can get dangerous too because it allows for accidentally assigning 
admin rights to a user.

I myself have accidentally turned a surrogate key into a natural key. About fifteen years 
ago I created a table for user's of a system. A few years later the business wanted a bar 
code on reports that a user submitted, so an easy choice was to add the user ID there in 
bar code form. At this point business users started referring to this ID and at that point 
I lost control over it. Now, much to my surprise this same application is still running 
and the business has requested changes to the ID. They want the ID to start at certain 
values at various times so we can quickly tell when the user created their account. There 
has also been talk of other numbering schemes that are not necessarily sequential so that 
this ID can mean even more. In this case the fix isn't too bad. I have just created a 
column in our users table to hold this new meaningful number and have left the primary 
key alone.

So, is there ever a time when a natural key is a good choice? Sometimes I use them in 
lookup tables. A common example for me is a U.S. state table. We want more information 
saved about what happens to mail in a certain state. However, for practical reasons we 
want to save the two digit state code in our mailing addresses table.

So, by now even if I haven't convinced you to always use surrogate keys hopefully you'll 
at least be able to make your decision with a little more knowledge and understanding.
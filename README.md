# Data Exploration

## Overview

This section explores a Twitter dataset by querying MongoDB.  The queries were performed in the Cloudera environment.

This project is based on assignments from Big Data Integration and Processing by University of California, San Diego on Coursera.

MongoDB was started by executing the following commands:

```linux
./mongodb/bin/mongod --dbpath db
./mongodb/bin/mongo
```

where dbpath db in the first line specifies that the directory db should be used for the MongoDB directory for datafiles.
The second line is executed from a different terminal window.  It runs mongodb shell so that we can query the server.

## Exploration of Twitter Data

The command `show dbs` shows the databases:

```
journaldev  0.000GB
local       0.000GB
sample      0.004GB
test        0.000GB
```

We will switch to the `sample` database that contains Twitter JSON data by executing `use sample`
Then we will show collections by executing `show collections`:

```
collection
users
```

The Twitter data are stored in the `users` collection.

The following command outputs the number of documents in the `users` collection:

```java
db.users.count()
```

The output shows that there are 11,188 records in the collection.

Next we will output one of the documents: 

```java
db.users.findOne()
```

The following output allows us to examine the contents of the document:

```
{
	"_id" : ObjectId("578ffa8e7eb9513f4f55a935"),
	"user_name" : "koteras",
	"retweet_count" : 0,
	"tweet_followers_count" : 461,
	"source" : "<a href=\"http://twitter.com/download/iphone\" rel=\"nofollow\">Twitter for iPhone</a>",
	"coordinates" : null,
	"tweet_mentioned_count" : 1,
	"tweet_ID" : "755891629932675072",
	"tweet_text" : "RT @ochocinco: I beat them all for 10 straight hours #FIFA16KING  https://t.co/BFnV6jfkBL",
	"user" : {
		"CreatedAt" : ISODate("2011-12-27T09:04:01Z"),
		"FavouritesCount" : 5223,
		"FollowersCount" : 461,
		"FriendsCount" : 619,
		"UserId" : 447818090,
		"Location" : "501"
	}
}
```

The document contains several fields, including nested fields under "user".  

We can use `distinct` command to output distinct values for a specific field:

```java
db.users.distinct("user_name")
```

The above command returns the following output:

```
[
  "koteras",
  "AllieLovesR5_1D",
  "Tonkatol",
  ...
```

The next line of code searches for a filed with a specific value, i.e., "AllieLovesR5_1D" in `user_name`:

```java
db.users.find({user_name: "AllieLovesR5_1D"}).pretty()
```

The results are shown below:

```
{
	"_id" : ObjectId("578ffa8f7eb9513f4f55a937"),
	"user_name" : "AllieLovesR5_1D",
	"retweet_count" : 0,
	"tweet_followers_count" : 4601,
	"source" : "<a href=\"http://twitter.com/download/iphone\" rel=\"nofollow\">Twitter for iPhone</a>",
	"coordinates" : null,
	"tweet_mentioned_count" : 3,
	"tweet_ID" : "755891632759681024",
	"tweet_text" : "RT @NiallOfficial: @Louis_Tomlinson @socceraid when I retired from playing because of my knee . I went and did my uefa A badges in Dublin",
	"user" : {
		"CreatedAt" : ISODate("2012-10-19T00:47:23Z"),
		"FavouritesCount" : 15758,
		"FollowersCount" : 4601,
		"FriendsCount" : 5059,
		"UserId" : 890030330,
		"Location" : null
	}
}
```

The code below selects only one field from the tweet above, the tweet_ID:

```java
db.users.find({user_name: "AllieLovesR5_1D"}, {tweet_ID: 1})
```

which results in the following output:

```
{ "_id" : ObjectId("578ffa8f7eb9513f4f55a937"), "tweet_ID" : "755891632759681024" }
```

The next line of code removes the primary key, `_id`, from the results:

```java
db.users.find({user_name: "AllieLovesR5_1D"}, {tweet_ID: 1, _id: 0})
```

resulting in the following output:

```
{ "tweet_ID" : "755891632759681024" }
```

Next, we perform a regular expression search for the word "football", and count the results:

```java
db.users.find({tweet_text: /football/}).count()
```

This search results in 2,868 documents.

The next line outputs the count of all tweets with `tweet_mentioned_count` greater than 10:

```java
db.users.find({tweet_mentioned_count: {$gt: 2}}).count()
```

This search results in 271 documents.

Next, we would like to count the number of tweets with tweet_mentioned_count is greater than tweet_followers_count:

```java
db.users.find({$where : "this.tweet_mentioned_count > this.tweet_followers_count"}).count()
```

This search results in 18 documents.

Finally, we would like to further restrict the search by counting documents with `tweet_text` ending with the word "football" and `tweet_metnioned_count` greater than 2:

```java
db.users.find({$and : [ {tweet_text : /football$/}, {tweet_mentioned_count: {$gt: 2}}]}).count()
```

This search results in 3 documents.

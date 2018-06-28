# Data Exploration

## Overview

This section explores a Twitter dataset by querying MongoDB.  The queries were performed in the Cloudera environment.

This project is based on assignments from Big Data Integration and Processing by University of California, San Diego on Coursera.

MongoDB was started by executing the following commands:

```sql
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

```
db.users.count()
```

The output shows that there are 11,188 records in the collection.

Next we will output one of the documents: 

```
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

```
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





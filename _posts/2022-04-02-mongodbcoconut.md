---
title:  "Exploring COCONUT Locally - Part 1"
---
### COCONUT, MongoDB, PyMongo, Jupyter Notebooks

In my [previous post](https://adelenel.ai/coconutrdkit/), I showed how to work with just the SMILES in COCONUT, downloaded as a `.smi` file from the official COCONUT [webpage](https://coconut.naturalproducts.net/download) (January 2022 version has 407,270 molecules).

But what if you want to explore the **whole COCONUT dataset** and not just the SMILES?

On one hand, you could just stick to using the handy [Advanced Search](https://coconut.naturalproducts.net/search/advanced) tool on the COCONUT web interface - lots of search options (structural properties, molecular descriptors, data sources), allows AND/OR query combinations, and you can even specify value ranges, with search results downloadable as SDF.

However, there are some advantages to being able to query COCONUT locally:

1. Search results not limited to 10,000 hits (current maximum on COCONUT web).

2. Ability to customise exotic queries not currently available as options on COCONUT web.

3. Ability to build an analysis pipeline if you wanted to do further cheminformatic transformations or queries on your search results (*e.g.*, substructure searches).

4. No more querying a web service :)

So - many reasons for having a local version of COCONUT to play with!

Here's how to get started:

### Downloading & Restoring COCONUT: MongoDB + PyMongo

The entire COCONUT is only downloadable as a MongoDB dump. This format includes all the rich metadata and annotations that we see when browsing COCONUT on its wonderful web application (created by M. Sorokina and maintained by colleagues at the [Steinbeck Group](https://cheminf.uni-jena.de/) at FSU Jena).

Below, I will show how to start working with a local copy of COCONUT using MongoDB in Python. The plan is to then explore COCONUT using RDKit and Pandas in a Jupyter Notebook (blogpost Part 2!).

#### Step 1 - Install the necessary

Install MongoDB. On macOS, I ran:

```
$ brew tap mongodb/brew
$ brew install mongodb-community@5.0
```



#### Step 2 - Start MongoDB
```
$ brew services start mongodb-community@5.0
```
Normally at this point we should be able to run ```mongo``` commands, but for macOS Catalina or later, it seems there are [issues](https://www.mongodb.com/community/forums/t/error-couldnt-connect-to-server-127-0-0-1-27017/705/10) with creating `/data/db` upon installation of Mongo.

I used the workaround suggested. In a separate terminal window:

```
$ cd /Users
$ cd <home dir>
$ mkdir data
$ cd data
$ mkdir db
$ mongod --dbpath ~/data/db
```
Press enter and keep it running in the background.




#### Step 3 - Restore COCONUT as a MongoDB

Go back to the original terminal and `cd` into the COCONUT dir (after unzipping). You should be in the dir containing all the `.bson` and `.json` files.

Then, restore COCONUT as a MongoDB database:

```
$ mongorestore --db=COCONUT_2021_11 --noIndexRestore .
.
.
.
...	finished restoring COCONUT_2021_11.sourceNaturalProduct (1536262 documents, 0 failures)
...	2043368 document(s) restored successfully. 0 document(s) failed to restore.

```

I opted to restore without indexes - a faster and safer option - according to the `README`. We can add them later if we want.




#### Step 4 - COCONUT in a Jupyter Notebook

Fire up your Notebook (make sure you activated your rdkit environment which has `pymongo` installed.)

Then, a few simple commands and you're good to go :)

``` python
from pymongo import MongoClient
client = MongoClient('localhost',27017) #the default
db = client.COCONUT_2021_11
db.list_collection_names()

Out:
['sourceNaturalProduct',
 'pubFingerprintsCounts',
 'uniqueNaturalProduct',
 'fragment',
 'nPDatabase',
 'quarantined']
```

In my next post I will explore COCONUT more. Stay tuned and feel free to leave a comment or suggestion for what would be interesting to see about COCONUT :)

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fadelenel.ai%2Fmongodbcoconut&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)

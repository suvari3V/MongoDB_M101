# Summary

`M101P: MongoDB for Developers` is part of MongoDB University series of online courses with duration of 7 weeks. Official link of the website is https://university.mongodb.com/. 
After completing this course each student should have a good understanding as to how applications are built on top of MongoDB using Python and also be prepared to take the developer 
associate exam and become a MongoDB Certified Professional.


- Week 1: Introduction and Overview
- Week 2: Creating, Reading and Updating Data (CRUD)
- Week 3: Schema Design
- Week 4: Performance
- Week 5: Aggregation Framework
- Week 6: Application Engineering
- Week 7: Case Studies

** For more information: https://university.mongodb.com/courses/M101/about?trk=profile_certification_title

## WEEK 1

> Homework 1.1: 

```
Install MongoDB on your computer and run it on the standard port.

Download the HW1-1 from the Download Handout link and uncompress it.

Use mongorestore to restore the dump into your running mongod. Do this by opening a terminal window (mac) or cmd window (windows) and navigating to the directory so that you are in the 
parent directory of the dump directory (if you used the default extraction method, it should be hw1/). Now type:

mongorestore dump
Note you will need to have your path setup correctly to find mongorestore.

Next, go into the Mongo shell, perform a findOne on the collection called hw1 in the database m101. That will return one document. Please provide the value corresponding to the "answer" 
key from the document returned.

Hint: if you get back a document that looks like { "_id": 1234, "answer": 2468 }, please only put in 2468 (with no spaces) for your answer.
```

Answer: 42

> Homework 1.2:

```
Get PyMongo installed on your computer. To prove its installed, run the program:

python hw1-2.py
Note that you will need to get MongoDB installed and the homework dataset imported from the previous homework before attempting this problem.

This program will print a numeric answer. Please put just the 4-digit number into the box below (no spaces).
```

Answer: 1815

> Homework 1.3:

```
We are now going to test that you have bottle installed correctly and can run a bottle-based project. Download the handout and run it as follows:

python hw1-3.py
It requires that:

bottle be installed correctly
your mongodb to be running
you have run mongorestore properly
From a different terminal window type the following from the command line: curl http://localhost:8080/hw1/50

Alternatively, you can put the url above into your web browser.

Type the two-digit answer into the box below (no spaces).
```

Answer: 53

## WEEK 2

> Homework 2.1:

```
In this problem, you will be using a collection of student scores that is similar to what we used in the lessons. Please download grades.json from the Download Handout link and import 
it into your local mongo database as follows:

mongoimport -d students -c grades < grades.json
The dataset contains 4 scores for 200 students.

First, let’s confirm your data is intact; the number of documents should be 800.

use students
db.grades.count()
You should get 800.

This next query, which uses the aggregation framework that we have not taught yet, will tell you the student_id with the highest average score:

db.grades.aggregate({'$group':{'_id':'$student_id', 'average':{$avg:'$score'}}}, {'$sort':{'average':-1}}, {'$limit':1})
The answer should be student_id 164 with an average of approximately 89.3.

Now it’s your turn to analyze the data set. Find all exam scores greater than or equal to 65, and sort those scores from lowest to highest.

What is the student_id of the lowest exam score above 65?
```

Answer: 22

> Homework 2.2:

```
Write a program in the language of your choice that will remove the grade of type "homework" with the lowest score for each student from the dataset in the handout. Since each document 
is one grade, it should remove one document per student. This will use the same data set as the last problem, but if you don't have it, you can download and re-import.

The dataset contains 4 scores each for 200 students.

First, let’s confirm your data is intact; the number of documents should be 800.

use students
db.grades.count()
Hint/spoiler: If you select homework grade-documents, sort by student and then by score, you can iterate through and find the lowest score for each student by noticing a change in 
student id. As you notice that change of student_id, remove the document.

To confirm you are on the right track, here are some queries to run after you process the data and put it into the grades collection:

Let us count the number of grades we have:

db.grades.count() 
The result should be 600. Now let us find the student who holds the 101st best grade across all grades:

db.grades.find().sort( { 'score' : -1 } ).skip( 100 ).limit( 1 )
The correct result will be:

{ "_id" : ObjectId("50906d7fa3c412bb040eb709"), "student_id" : 100, "type" : "homework", "score" : 88.50425479139126 }
Now let us sort the students by student_id and score, and then see what the top five docs are:

db.grades.find( { }, { 'student_id' : 1, 'type' : 1, 'score' : 1, '_id' : 0 } ).sort( { 'student_id' : 1, 'score' : 1, } ).limit( 5 )
The result set should be:

{ "student_id" : 0, "type" : "quiz", "score" : 31.95004496742112 }
{ "student_id" : 0, "type" : "exam", "score" : 54.6535436362647 }
{ "student_id" : 0, "type" : "homework", "score" : 63.98402553675503 }
{ "student_id" : 1, "type" : "homework", "score" : 44.31667452616328 }
{ "student_id" : 1, "type" : "exam", "score" : 74.20010837299897 }
To verify that you have completed this task correctly, provide the identity of the student with the highest average in the class with following query that uses the aggregation framework. 
The answer will appear in the _id field of the resulting document.

db.grades.aggregate( { '$group' : { '_id' : '$student_id', 'average' : { $avg : '$score' } } }, { '$sort' : { 'average' : -1 } }, { '$limit' : 1 } )
Enter the student ID below. Please enter just the number, with no spaces, commas or other characters.
```

Answer: 54

> Homework 2.3:

```
Blog User Sign-up and Login
Download the handout and unpack it.

You should see three files at the highest level: blog.py, userDAO.py and sessionDAO.py. There is also a views directory which contains the templates for the project.

The project roughly follows the model/view/controller paradigm. userDAO and sessionDAO.py comprise the model. blog.py is the controller. The templates comprise the view.

If everything is working properly, you should be able to start the blog by typing:

python blog.py
Note that this project requires the following python modules be installed on your computer: cgi, hmac, datetime, json, sys, string, hashlib, urllib, urllib2, random, re, pymongo, and 
bottle. 
If you have python installed at all, you probably already have most of these installed except pymongo and bottle.

If you have python-setuptools installed, the command "pip" makes this simple. Any other missing packages will show up when validate.py is run, and can be installed in a similar fashion. 
As of this recording, we are still using the beta version of PyMongo so we will install directly from GitHub. 
Note that this directions are identical to what we taught you within the lessons and you should already have PyMongo 3.x and bottle installed. If not, use the following commands.

$ pip install https://github.com/mongodb/mongo-python-driver/archive/3.0b1.tar.gz
$ pip install bottle
If you go to http://localhost:8082 you should see a message, "this is a placeholder for the blog"

Here are some URLs that must work when you are done.

http://localhost:8082/signup
http://localhost:8082/login
http://localhost:8082/logout
When you login or sign-up, the blog will redirect to http://localhost:8082/welcome and that must work properly, welcoming the user by username

We have removed two pymongo statements from userDAO.py and marked the area where you need to work with XXX. You should not need to touch any other code. 
The pymongo statements that you are going to add will add a new user to the database upon sign-up, and validate a login by retrieving the right user document.

The blog stores its data in the blog database in two collections, users and sessions. Here are two example docs for a username 'erlichson' with password 'fubar'. 
You can insert these if you like, but you don't need to.

> db.users.find()
{ "_id" : "erlichson", "password" : "d3caddd3699ef6f990d4d53337ed645a3804fac56207d1b0fa44544db1d6c5de,YCRvW" }
> db.sessions.find()
{ "_id" : "wwBfyRDgywSqeFKeQMPqVHPizaWqdlQJ", "username" : "erlichson" }

Once you have the the project working, the following steps should work:

go to http://localhost:8082/signup
create a user
It should redirect you to the welcome page and say: welcome username, where username is the user you signed up with. Now
Go to http://localhost:8082/logout
Now login http://localhost:8082/login.

Ok, now it's time to validate you got it all working.

There was one additional program that should have been downloaded in the project called validate.py.

python validate.py
For those who are using MongoHQ, MongoLab or want to run the webserver on a different host or port, there are some options to the validation script that can be exposed by running

python validate.py -help
If you got it right, it will provide a validation code for you to enter into the box below. Enter just the code, no spaces. Note that your blog must be running when you run the validator.
```

Answer: jkfds5834j98fnm39njf0920f02

## WEEK 3

> Homework 3.1: 

```
Download the students.json file from the Download Handout link and import it into your local Mongo instance with this command:

mongoimport -d school -c students < students.json
This dataset holds the same type of data as last week's grade collection, but it's modeled differently. You might want to start by inspecting it in the Mongo shell.

Write a program in the language of your choice that will remove the lowest homework score for each student. 
Since there is a single document for each student containing an array of scores, you will need to update the scores array and remove the homework.

Remember, just remove a homework score. Don't remove a quiz or an exam!

Hint/spoiler: With the new schema, this problem is a lot harder and that is sort of the point. 
One way is to find the lowest homework in code and then update the scores array with the low homework pruned.

To confirm you are on the right track, here are some queries to run after you process the data with the correct answer shown:

Let us count the number of students we have:

use school
db.students.count() 
The answer will be 200.

Let's see what Tamika Schildgen's record looks like:

db.students.find( { _id : 137 } ).pretty( )
This should be the output:

{
	"_id" : 137,
	"name" : "Tamika Schildgen",
	"scores" : [
		{
			"type" : "exam",
			"score" : 4.433956226109692
		},
		{
			"type" : "quiz",
			"score" : 65.50313785402548
		},
		{
			"type" : "homework",
			"score" : 89.5950384993947
		}
	]
}
To verify that you have completed this task correctly, provide the identity (in the form of their _id) of the student with the highest average in the class with following query that 
uses the aggregation framework. The answer will appear in the _id field of the resulting document.

db.students.aggregate( { '$unwind' : '$scores' } , { '$group' : { '_id' : '$_id' , 'average' : { $avg : '$scores.score' } } } , { '$sort' : { 'average' : -1 } } , { '$limit' : 1 } )
```

Answer: 13

> Homework 3.2:

```
Making your blog accept posts
In this homework you will be enhancing the blog project to insert entries into the posts collection. After this, the blog will work. 
It will allow you to add blog posts with a title, body and tags and have it be added to the posts collection properly.

We have provided the code that creates users and allows you to login (the assignment from last week). 
To get started, please download hw3-2and3-3.zip from the Download Handout link and unpack. You will be using these files for this homework, and for HW 3.3.

The areas where you need to add code are marked with XXX. You need only touch the BlogPostDAO.py file. 
There are three locations for you to add code for this problem. Scan that file for XXX to see where to work.

As a reminder, to run your blog you type
python blog.py
To play with the blog you can navigate to the following URLs
http://localhost:8082/
http://localhost:8082/signup
http://localhost:8082/login
http://localhost:8082/newpost
You will be proving that it works by running our validation script as follows: 
python validate.py
You need to run this in a separate terminal window while your blog is running and while the database is running. 
It makes connections to both to determine if your program works properly. Validate connects to localhost:8082 and expects that mongod is running on localhost on port 27017.

As before, validate will take some optional arguments if you want to run mongod on a different host or a use an external webserver.

This project requires Python 2.7. The code is not 3.0 compliant.

Ok, once you get the blog posts working, validate.py will print out a validation code for HW 3.2. 

Please enter it below, exactly as shown with no spaces.
```

Answer: 89jklfsjrlk209jfks2j2ek

> Homework 3.3:

```
Making your blog accept comments
In this homework you will add code to your blog so that it accepts comments. You will be using the same code as you downloaded for HW 3.2.

Once again, the area where you need to work is marked with an XXX in the blogPostDAO.py file. 
There is just one location that you need to modify. You don't need to figure out how to retrieve comments for this homework because the code you did in 3.2 already pulls the 
entire blog post (unless you specifically projected to eliminate the comments) and we gave you the code that pulls them out of the JSON document.

This assignment has fairly little code, but it's a little more subtle than the previous assignment because you are going to be manipulating an array within the Mongo document. 
For the sake of clarity, here is a document out of the posts collection from a working project.

{
	"_id" : ObjectId("509df76fbcf1bf5b27b4a23e"),
	"author" : "erlichson",
	"body" : "This is a blog entry",
	"comments" : [
		{
			"body" : "This is my comment",
			"author" : "Andrew Erlichson"
		},
		{
			"body" : "Give me liberty or give me death.",
			"author" : "Patrick Henry"
		}
	],
	"date" : ISODate("2012-11-10T06:42:55.733Z"),
	"permalink" : "This_is_a_blog_post_title",
	"tags" : [
		"cycling",
		"running",
		"swimming"
	],
	"title" : "This is a blog post title"
}
Note that you add comments in this blog from the blog post detail page, which appears at

http://localhost:8082/post/post_slug
where post_slug is the permalink. For the sake of eliminating doubt, the permalink for the example blog post above is http://localhost:8082/post/This_is_a_blog_post_title
You will run validation.py to check your work, much like the last problem. Validation.py will run through and check the requirements of HW 3.2 and then will check to make sure 
it can add blog comments, as required by this problem, HW 3.3. It checks the web output as well as the database documents.

python validate.py
Once you have the validation code, please copy and paste in the box below, no spaces.
```

Answer: jk1310vn2lkv0j2kf0jkfs

## WEEK 4

> Homework 4.1:

```
Suppose you have a collection with the following indexes:

> db.products.getIndexes()
[
	{
		"v" : 1,
		"key" : {
			"_id" : 1
		},
		"ns" : "store.products",
		"name" : "_id_"
	},
	{
		"v" : 1,
		"key" : {
			"sku" : 1
		},
                "unique" : true,
		"ns" : "store.products",
		"name" : "sku_1"
	},
	{
		"v" : 1,
		"key" : {
			"price" : -1
		},
		"ns" : "store.products",
		"name" : "price_-1"
	},
	{
		"v" : 1,
		"key" : {
			"description" : 1
		},
		"ns" : "store.products",
		"name" : "description_1"
	},
	{
		"v" : 1,
		"key" : {
			"category" : 1,
			"brand" : 1
		},
		"ns" : "store.products",
		"name" : "category_1_brand_1"
	},
	{
		"v" : 1,
		"key" : {
			"reviews.author" : 1
		},
		"ns" : "store.products",
		"name" : "reviews.author_1"
	}
Which of the following queries can utilize at least one index to find all matching documents or to sort? Check all that apply.
```

Answer: 
"db.products.find( { 'brand' : "GE" } ).sort( { price : 1 } )"
 and 
"db.products.find( { $and : [ { price : { $gt : 30 } },{ price : { $lt : 50 } } ] } ).sort( { brand : 1 } )"

> Homework 4.2:

```
Suppose you have a collection called tweets whose documents contain information about the created_at time of the tweet and the user's followers_count at the time they issued the tweet. 
What can you infer from the following explain output?

> db.tweets.explain("executionStats").find({"user.followers_count":{$gt:1000}}).limit(10).skip(5000).sort( { created_at : 1 } )
{
    "queryPlanner" : {
        "plannerVersion" : 1,
        "namespace" : "twitter.tweets",
        "indexFilterSet" : false,
        "parsedQuery" : {
            "user.followers_count" : {
                "$gt" : 1000
            }
        },
        "winningPlan" : {
            "stage" : "LIMIT",
            "limitAmount" : 0,
            "inputStage" : {
                "stage" : "SKIP",
                "skipAmount" : 0,
                "inputStage" : {
                    "stage" : "FETCH",
                    "filter" : {
                        "user.followers_count" : {
                            "$gt" : 1000
                        }
                    },
                    "inputStage" : {
                        "stage" : "IXSCAN",
                        "keyPattern" : {
                            "created_at" : -1
                        },
                        "indexName" : "created_at_-1",
                        "isMultiKey" : false,
                        "direction" : "backward",
                        "indexBounds" : {
                            "created_at" : [
                                "[MinKey, MaxKey]"
                            ]
                        }
                    }
                }
            }
        },
        "rejectedPlans" : [ ]
    },
    "executionStats" : {
        "executionSuccess" : true,
        "nReturned" : 10,
        "executionTimeMillis" : 563,
        "totalKeysExamined" : 251120,
        "totalDocsExamined" : 251120,
        "executionStages" : {
            "stage" : "LIMIT",
            "nReturned" : 10,
            "executionTimeMillisEstimate" : 500,
            "works" : 251121,
            "advanced" : 10,
            "needTime" : 251110,
            "needFetch" : 0,
            "saveState" : 1961,
            "restoreState" : 1961,
            "isEOF" : 1,
            "invalidates" : 0,
            "limitAmount" : 0,
            "inputStage" : {
                "stage" : "SKIP",
                "nReturned" : 10,
                "executionTimeMillisEstimate" : 500,
                "works" : 251120,
                "advanced" : 10,
                "needTime" : 251110,
                "needFetch" : 0,
                "saveState" : 1961,
                "restoreState" : 1961,
                "isEOF" : 0,
                "invalidates" : 0,
                "skipAmount" : 0,
                "inputStage" : {
                    "stage" : "FETCH",
                    "filter" : {
                        "user.followers_count" : {
                            "$gt" : 1000
                        }
                    },
                    "nReturned" : 5010,
                    "executionTimeMillisEstimate" : 490,
                    "works" : 251120,
                    "advanced" : 5010,
                    "needTime" : 246110,
                    "needFetch" : 0,
                    "saveState" : 1961,
                    "restoreState" : 1961,
                    "isEOF" : 0,
                    "invalidates" : 0,
                    "docsExamined" : 251120,
                    "alreadyHasObj" : 0,
                    "inputStage" : {
                        "stage" : "IXSCAN",
                        "nReturned" : 251120,
                        "executionTimeMillisEstimate" : 100,
                        "works" : 251120,
                        "advanced" : 251120,
                        "needTime" : 0,
                        "needFetch" : 0,
                        "saveState" : 1961,
                        "restoreState" : 1961,
                        "isEOF" : 0,
                        "invalidates" : 0,
                        "keyPattern" : {
                            "created_at" : -1
                        },
                        "indexName" : "created_at_-1",
                        "isMultiKey" : false,
                        "direction" : "backward",
                        "indexBounds" : {
                            "created_at" : [
                                "[MinKey, MaxKey]"
                            ]
                        },
                        "keysExamined" : 251120,
                        "dupsTested" : 0,
                        "dupsDropped" : 0,
                        "seenInvalidated" : 0,
                        "matchTested" : 0
                    }
                }
            }
        }
    },
    "serverInfo" : {
        "host" : "generic-name.local",
        "port" : 27017,
        "version" : "3.0.1",
        "gitVersion" : "534b5a3f9d10f00cd27737fbcd951032248b5952"
    },
    "ok" : 1
}
```

Answer: 
"The query examines 251120 documents."
 and 
"The query uses an index to determine the order in which to return result documents.

> Homework 4.3:

```
NOTE there is a bug (TOOLS-939) affecting some versions of mongoimport and mongorestore that causes mongoimport -d blog -c posts < posts.json to fail. 
As a workaround, you can use mongoimport -d blog -c posts < posts.json --batchSize 1.

Making the Blog fast
Please download hw4-3.zip from the Download Handout link to get started. This assignment requires Mongo 3.0 or above.

In this homework assignment you will be adding some indexes to the post collection to make the blog fast.

We have provided the full code for the blog application and you don't need to make any changes, or even run the blog. But you can, for fun.

We are also providing a patriotic (if you are an American) data set for the blog. There are 1000 entries with lots of comments and tags. You must load this dataset to complete the problem.

From the mongo shell:

use blog
db.posts.drop()
From the mac or PC terminal window

mongoimport -d blog -c posts < posts.json
The blog has been enhanced so that it can also display the top 10 most recent posts by tag. There are hyperlinks from the post tags to the page that displays the 10 most recent blog 
entries for that tag. (run the blog and it will be obvious)

Your assignment is to make the following blog pages fast:

The blog home page
The page that displays blog posts by tag (http://localhost:8082/tag/whatever)
The page that displays a blog entry by permalink (http://localhost:8082/post/permalink)
By fast, we mean that indexes should be in place to satisfy these queries such that we only need to scan the number of documents we are going to return.
To figure out what queries you need to optimize, you can read the blog.py code and see what it does to display those pages. Isolate those queries and use explain to explore.

Once you have added the indexes to make those pages fast run the following.

python validate.py
(note that for folks who are using MongoLabs or MongoHQ there are some command line options to validate.py to make it possible to use those services) Now enter the validation code below.
```

Answer: 893jfns29f728fn29f20f2

> Homework 4.4:

```
In this problem you will analyze a profile log taken from a different mongoDB instance and you will import it into a collection named sysprofile. 
To start, please download sysprofile.json from Download Handout link and import it with the following command:

mongoimport -d m101 -c profile < sysprofile.json
Now query the profile data, looking for all queries to the students collection in the database school2, sorted in order of decreasing latency. 
What is the latency of the longest running operation to the collection, in milliseconds?
```

Answer: 15820

# WEEK 5

> Homework 5.1:

```
Finding the most frequent author of comments on your blog
In this assignment you will use the aggregation framework to find the most frequent author of comments on your blog. We will be using a data set similar to ones we've used before. 

Start by downloading the handout zip file for this problem. Then import into your blog database as follows:
mongoimport -d blog -c posts --drop posts.json
Now use the aggregation framework to calculate the author with the greatest number of comments. 

To help you verify your work before submitting, the author with the fewest comments is Mariela Sherer and she commented 387 times. 

Please choose your answer below for the most prolific comment author:
```

Answer: Elizabet Kleine

> Homework 5.2:

```
Crunching the Zipcode dataset
Please calculate the average population of cities in California (abbreviation CA) and New York (NY) (taken together) with populations over 25,000. 

For this problem, assume that a city name that appears in more than one state represents two separate cities. 

Please round the answer to a whole number. 
Hint: The answer for CT and NJ (using this data set) is 38177. 

Please note:
Different states might have the same city name.
A city might have multiple zip codes.


For this problem, we have used a subset of the data you previously used in zips.json, not the full set. For this set, there are only 200 documents (and 200 zip codes), 
and all of them are in New York, Connecticut, New Jersey, and California. 

You can download the handout and perform your analysis on your machine with
mongoimport -d test -c zips --drop small_zips.json

Once you've generated your aggregation query and found your answer, select it from the choices below. 

Please use the Aggregation pipeline to solve this problem.
```

Answer: 44805

> Homework 5.3:

```
Who's the easiest grader on campus?
Download the handout and mongoimport. 

The documents look like this:
{
	"_id" : ObjectId("50b59cd75bed76f46522c392"),
	"student_id" : 10,
	"class_id" : 5,
	"scores" : [
		{
			"type" : "exam",
			"score" : 69.17634380939022
		},
		{
			"type" : "quiz",
			"score" : 61.20182926719762
		},
		{
			"type" : "homework",
			"score" : 73.3293624199466
		},
		{
			"type" : "homework",
			"score" : 15.206314042622903
		},
		{
			"type" : "homework",
			"score" : 36.75297723087603
		},
		{
			"type" : "homework",
			"score" : 64.42913107330241
		}
	]
}
There are documents for each student (student_id) across a variety of classes (class_id). Note that not all students in the same class have the same exact number of assessments. 
Some students have three homework assignments, etc. 

Your task is to calculate the class with the best average student performance. This involves calculating an average for each student in each class of all non-quiz assessments and 
then averaging those numbers to get a class average. To be clear, each student's average includes only exams and homework grades. Don't include their quiz scores in the calculation. 

What is the class_id which has the highest average student performance? 

Hint/Strategy: You need to group twice to solve this problem. You must figure out the GPA that each student has achieved in a class and then average those numbers to get a class average. 
After that, you just need to sort. The class with the lowest average is the class with class_id=2. Those students achieved a class average of 37.6 

You can download the handout and perform your analysis on your machine with
mongoimport -d test -c grades --drop grades.json

Below, choose the class_id with the highest average student average.
```

Answer: 1

> Homework 5.4:

```
In this problem you will calculate the number of people who live in a zip code in the US where the city starts with a digit. We will take that to mean they don't really live in a city. 
Once again, you will be using the zip code collection, which you will find in the 'handouts' link in this page. Import it into your mongod using the following command from the command 
line:

> mongoimport -d test -c zips --drop zips.json
If you imported it correctly, you can go to the test database in the mongo shell and conform that

> db.zips.count()
yields 29,467 documents.

The project operator can extract the first digit from any field. For example, to extract the first digit from the city field, you could write this query:

db.zips.aggregate([
    {$project: 
     {
	first_char: {$substr : ["$city",0,1]},
     }	 
   }
])
Using the aggregation framework, calculate the sum total of people who are living in a zip code where the city starts with a digit. Choose the answer below.

You will need to probably change your projection to send more info through than just that first character. Also, you will need a filtering step to get rid of all documents where the 
city does not start with a digital (0-9).

Note: When you mongoimport the data, you will probably see a few duplicate key errors; this is to be expected, and will not prevent the mongoimport from working. There is also an 
issue with some versions of MongoDB 3.0 where it claims that 0 documents were mongoimported, when in fact there were 29,467 documents imported. You can verify this for yourself by 
going into the shell and counting the documents in the "test.zips" collection.
```

Answer: 298015
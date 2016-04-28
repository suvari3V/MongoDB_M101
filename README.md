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

# WEEK 6

> Homework 6.1:

```
Which of the following statements are true about replication in MongoDB? Check all that apply.
```

Answer:
"The minimum sensible number of voting nodes to a replica set is three."
 and
"The oplog utilizes a capped collection."

> Homework 6.2:

```
Let's suppose you have a five member replica set and want to assure that writes are
committed to the journal and are acknowledged by at least 3 nodes before you proceed
forward. What would be the appropriate settings for w and j?
```

Answer: w="majority", j=1

> Homework 6.3:

```
Which of the following statements are true about choosing and using a shard key?
```

Answer:
"MongoDB can not enforce unique indexes on a sharded collection other than the shard key itself, or indexes prefixed by the shard key."
 and
"There must be a index on the collection that starts with the shard key."
 and
"Any update that does not contain the shard key will be sent to all shards."

> Homework 6.4:

```
You have a sharded system with three shards and have sharded the collections "students" in the "school" database across those shards. The output of sh.status() when connected to mongos looks like this:

mongos> sh.status()
--- Sharding Status ---
  sharding version: {
	"_id" : 1,
	"minCompatibleVersion" : 5,
	"currentVersion" : 6,
	"clusterId" : ObjectId("5531512ac723271f602db407")
}
  shards:
	{  "_id" : "s0",  "host" : "s0/localhost:37017,localhost:37018,localhost:37019" }
	{  "_id" : "s1",  "host" : "s1/localhost:47017,localhost:47018,localhost:47019" }
	{  "_id" : "s2",  "host" : "s2/localhost:57017,localhost:57018,localhost:57019" }
  balancer:
	Currently enabled:  yes
	Currently running:  yes
		Balancer lock taken at Fri Apr 17 2015 14:32:02 GMT-0400 (EDT) by education-iMac-2.local:27017:1429295401:16807:Balancer:1622650073
	Collections with active migrations:
		school.students started at Fri Apr 17 2015 14:32:03 GMT-0400 (EDT)
	Failed balancer rounds in last 5 attempts:  0
	Migration Results for the last 24 hours:
		2 : Success
		1 : Failed with error 'migration already in progress', from s0 to s1
  databases:
	{  "_id" : "admin",  "partitioned" : false,  "primary" : "config" }
	{  "_id" : "school",  "partitioned" : true,  "primary" : "s0" }
		school.students
			shard key: { "student_id" : 1 }
			chunks:
				s0	1
				s1	3
				s2	1
			{ "student_id" : { "$minKey" : 1 } } -->> { "student_id" : 0 } on : s2 Timestamp(3, 0)
			{ "student_id" : 0 } -->> { "student_id" : 2 } on : s0 Timestamp(3, 1)
			{ "student_id" : 2 } -->> { "student_id" : 3497 } on : s1 Timestamp(3, 2)
			{ "student_id" : 3497 } -->> { "student_id" : 7778 } on : s1 Timestamp(3, 3)
			{ "student_id" : 7778 } -->> { "student_id" : { "$maxKey" : 1 } } on : s1 Timestamp(3, 4)

If you ran the query
use school
db.students.find({'student_id':2000})
Which shards would be involved in answering the query?
```

Answer: s1

> Homework 6.5:

```
In this homework you will build a small replica set on your own computer. We will check that it works with validate.py, which you should download from the Download Handout link.

Create three directories for the three mongod processes. On unix, this could be done as follows:
mkdir -p /data/rs1 /data/rs2 /data/rs3
Now start three mongo instances as follows. Note that are three commands. The browser is probably wrapping them visually.
mongod --replSet m101 --logpath "1.log" --dbpath /data/rs1 --port 27017 --smallfiles --oplogSize 64 --fork

mongod --replSet m101 --logpath "2.log" --dbpath /data/rs2 --port 27018 --smallfiles --oplogSize 64 --fork

mongod --replSet m101 --logpath "3.log" --dbpath /data/rs3 --port 27019 --smallfiles --oplogSize 64 --fork
Windows users: Omit -p from mkdir. Also omit --fork and use start mongod with Windows compatible paths (i.e. backslashes "\") for the --dbpath argument (e.g; C:\data\rs1).

Now connect to a mongo shell and make sure it comes up
mongo --port 27017
Now you will create the replica set. Type the following commands into the mongo shell:
config = { _id: "m101", members:[
          { _id : 0, host : "localhost:27017"},
          { _id : 1, host : "localhost:27018"},
          { _id : 2, host : "localhost:27019"} ]
};
rs.initiate(config);
At this point, the replica set should be coming up. You can type
rs.status()
to see the state of replication.

Now run validate.py to confirm that it works.
python validate.py
Validate connects to your local replica set and checks that it has three nodes. It has been tested under Pymongo 2.3 and 2.4. Type the validation code below.
```

Answer: kjvjkl3290mf0m20f2kjjv

# Final Exam

> Question 1:

```
Please download the Enron email dataset enron.zip, unzip it and then restore it using mongorestore. It should restore to a collection called "messages" in a database called "enron". Note that this is an abbreviated version of the full corpus. There should be 120,477 documents after restore.

Inspect a few of the documents to get a basic understanding of the structure. Enron was an American corporation that engaged in a widespread accounting fraud and subsequently failed.

In this dataset, each document is an email message. Like all Email messages, there is one sender but there can be multiple recipients.

Construct a query to calculate the number of messages sent by Andrew Fastow, CFO, to Jeff Skilling, the president. Andrew Fastow's email addess was andrew.fastow@enron.com. Jeff Skilling's email was jeff.skilling@enron.com.

For reference, the number of email messages from Andrew Fastow to John Lavorato (john.lavorato@enron.com) was 1.
```

Answer: 3

> Question 2:

```
Please use the Enron dataset you imported for the previous problem. For this question you will use the aggregation framework to figure out pairs of people that tend to communicate a lot. To do this, you will need to unwind the To list for each message.

This problem is a little tricky because a recipient may appear more than once in the To list for a message. You will need to fix that in a stage of the aggregation before doing your grouping and counting of (sender, recipient) pairs.

Which pair of people have the greatest number of messages in the dataset?
```

Answer: susan.mara@enron.com to jeff.dasovich@enron.com

> Question 3:

```
In this problem you will update a document in the Enron dataset (which can be found here) to illustrate your mastery of updating documents from the shell.

Please add the email address "mrpotatohead@mongodb.com" to the list of addresses in the "headers.To" array for the document with "headers.Message-ID" of "<8147308.1075851042335.JavaMail.evans@thyme>"

After you have completed that task, please download final3-validate-mongo-shell.js from the Download Handout link and run

mongo final3-validate-mongo-shell.js
to get the validation code and put it in the box below without any extra spaces. The validation script assumes that it is connecting to a simple mongo instance on the standard port on localhost.
```

Answer: vOnRg05kwcqyEFSve96R

> Question 4:

```
Enhancing the Blog to support viewers liking certain comments
NOTE there is a bug (TOOLS-939) affecting some versions of mongoimport and mongorestore that causes mongoimport -d blog -c posts < posts.json to fail. As a workaround, you can use mongoimport -d blog -c posts < posts.json --batchSize 1.

In this problem, you will be enhancing the blog project to support users liking certain comments and the like counts showing up the in the permalink page.

Start by downloading the code in final4.zip and posts.json from the Download Handout link and loading up the blog dataset posts.json. The user interface has already been implemented for you. It's not fancy. The /post URL shows the like counts next to each comment and displays a Like button that you can click on. That Like button POSTS to the /like URL on the blog, makes the necessary changes to the database state (you are implementing this), and then redirects the browser back to the permalink page.

This full round trip and redisplay of the entire web page is not how you would implement liking in a modern web app, but it makes it easier for us to reason about, so we will go with it.

Your job is to search the code for the string "XXX work here" and make the necessary changes. You can choose whatever schema you want, but you should note that the entry_template makes some assumptions about the how the like value will be encoded and if you go with a different convention than it assumes, you will need to make some adjustments.

It is possible to solve this problem by putting NOTHING in one of the XXX spots and adding only a SINGLE LINE to the other spot to properly increment the like count. If you decide to use a different schema than the entry_template is expecting, then you will likely to work in both spots. The validation script does not look at the database. It looks at the blog.

The validation script, validate.py, will fetch your blog, go to the first post's permalink page and attempt to increment the vote count. You run it as follows:
python validate.py
Remember that the blog needs to be running as well as Mongo. The validation script takes some options if you want to run outside of localhost.

After you have gotten it working, enter the validation string below.
```

Answer: 3f837hhg673ghd93hgf8

> Question 5:

```
Suppose your have a collection stuff which has the _id index,

  {
    "v" : 1,
    "key" : {
      "_id" : 1
    },
    "ns" : "test.stuff",
    "name" : "_id_"
  }
and one or more of the following indexes as well:

  {
    "v" : 1,
    "key" : {
      "a" : 1,
      "b" : 1
    },
    "ns" : "test.stuff",
    "name" : "a_1_b_1"
  }
  {
    "v" : 1,
    "key" : {
      "a" : 1,
      "c" : 1
    },
    "ns" : "test.stuff",
    "name" : "a_1_c_1"
  }
  {
    "v" : 1,
    "key" : {
      "c" : 1
    },
    "ns" : "test.stuff",
    "name" : "c_1"
  }
  {
    "v" : 1,
    "key" : {
      "a" : 1,
      "b" : 1,
      "c" : -1
    },
    "ns" : "test.stuff",
    "name" : "a_1_b_1_c_-1"
  }
Now suppose you want to run the following query against the collection.

db.stuff.find({'a':{'$lt':10000}, 'b':{'$gt': 5000}}, {'a':1, 'c':1}).sort({'c':-1})
Which of the indexes could be used by MongoDB to assist in answering the query? Check all that apply.
```

Answer: "a_1_b_1_c_-1" and "a_1_c_1" and "a_1_b_1"

> Question 6:

```
Suppose you have a collection of students of the following form:
{
	"_id" : ObjectId("50c598f582094fb5f92efb96"),
	"first_name" : "John",
	"last_name" : "Doe",
	"date_of_admission" : ISODate("2010-02-21T05:00:00Z"),
	"residence_hall" : "Fairweather",
	"has_car" : true,
	"student_id" : "2348023902",
	"current_classes" : [
		"His343",
		"Math234",
		"Phy123",
		"Art232"
	]
}

Now suppose that basic inserts into the collection, which only include the last name, first name and student_id, are too slow (we can't do enough of them per second from our program). What could potentially improve the speed of inserts. Check all that apply.
```

Answer: "Remove all indexes from the collection, leaving only the index on _id in place" and "Set w=0, j=0 on writes"

> Question 7:

```
You have been tasked to cleanup a photo-sharing database. The database consists of two collections, albums, and images. Every image is supposed to be in an album, but there are orphan images that appear in no album. Here are some example documents (not from the collections you will be downloading).

> db.albums.findOne()
{
	"_id" : 67
	"images" : [
		4745,
		7651,
		15247,
		17517,
		17853,
		20529,
		22640,
		27299,
		27997,
		32930,
		35591,
		48969,
		52901,
		57320,
		96342,
		99705
	]
}

> db.images.findOne()
{ "_id" : 99705, "height" : 480, "width" : 640, "tags" : [ "dogs", "kittens", "work" ] }

From the above, you can conclude that the image with _id = 99705 is in album 67. It is not an orphan.

Your task is to write a program to remove every image from the images collection that appears in no album. Or put another way, if an image does not appear in at least one album, it's an orphan and should be removed from the images collection.

Download final7.zip from Download Handout link and use mongoimport to import the collections in albums.json and images.json.

When you are done removing the orphan images from the collection, there should be 89,737 documents in the images collection. To prove you did it correctly, what are the total number of images with the tag 'kittens" after the removal of orphans? As as a sanity check, there are 49,932 images that are tagged 'kittens' before you remove the images.
Hint: you might consider creating an index or two or your program will take a long time to run.
```

Answer: 44,822

> Question 8:

```
Suppose you have a three node replica set. Node 1 is the primary. Node 2 is a secondary, and node 3 is a secondary running with a delay of two hours. All writes to the database are issued with w=majority and j=1 (by which we mean that the getLastError call has those values set).

A write operation (which could be insert or update) is initiated from your application using the pymongo driver at time t=0. At time t=5 seconds, the primary (Node 1), goes down for an hour and Node 2 is elected primary. Note that your write operation has not yet returned at the time of the failure. Note also that although you have not received a response from the write, it has been processed and written by Node 1 before the failure. Node 3, since it has a slave delay option set, is lagging.

Will there be a rollback of data on Node 1 when it comes back up? Choose the best answer.
```

Answer: No, never

> Question 9:

```
Imagine an electronic medical record database designed to hold the medical records of every individual in the United States. Because each person has more than 16MB of medical history and records, it's not feasible to have a single document for every patient. Instead, there is a patient collection that contains basic information on each person and maps the person to a patient_id, and a record collection that contains one document for each test or procedure. One patient may have dozens or even hundreds of documents in the record collection.

We need to decide on a shard key to shard the record collection. What's the best shard key for the record collection, provided that we are willing to run inefficient scatter-gather operations to do infrequent research and run studies on various diseases and cohorts? That is, think mostly about the operational aspects of such a system. And by operational, we mean, think about what the most common operations that this systems needs to perform day in and day out.
```

Asnwer: patient_id

> Question 10:

```
Understanding the output of explain

We perform the following query on the enron dataset:

var exp = db.messages.explain('executionStats')

exp.find( { 'headers.Date' : { '$gt' : new Date(2001,3,1) } }, { 'headers.From' : 1, '_id' : 0 } ).sort( { 'headers.From' : 1 } )
and get the following explain output.
{
  "queryPlanner" : {
    "plannerVersion" : 1,
    "namespace" : "enron.messages",
    "indexFilterSet" : false,
    "parsedQuery" : {
      "headers.Date" : {
        "$gt" : ISODate("2001-04-01T05:00:00Z")
      }
    },
    "winningPlan" : {
      "stage" : "PROJECTION",
      "transformBy" : {
        "headers.From" : 1,
        "_id" : 0
      },
      "inputStage" : {
        "stage" : "FETCH",
        "filter" : {
          "headers.Date" : {
            "$gt" : ISODate("2001-04-01T05:00:00Z")
          }
        },
        "inputStage" : {
          "stage" : "IXSCAN",
          "keyPattern" : {
            "headers.From" : 1
          },
          "indexName" : "headers.From_1",
          "isMultiKey" : false,
          "direction" : "forward",
          "indexBounds" : {
            "headers.From" : [
              "[MinKey, MaxKey]"
            ]
          }
        }
      }
    },
    "rejectedPlans" : [ ]
  },
  "executionStats" : {
    "executionSuccess" : true,
    "nReturned" : 83057,
    "executionTimeMillis" : 726,
    "totalKeysExamined" : 120477,
    "totalDocsExamined" : 120477,
    "executionStages" : {
      "stage" : "PROJECTION",
      "nReturned" : 83057,
      "executionTimeMillisEstimate" : 690,
      "works" : 120478,
      "advanced" : 83057,
      "needTime" : 37420,
      "needFetch" : 0,
      "saveState" : 941,
      "restoreState" : 941,
      "isEOF" : 1,
      "invalidates" : 0,
      "transformBy" : {
        "headers.From" : 1,
        "_id" : 0
      },
      "inputStage" : {
        "stage" : "FETCH",
        "filter" : {
          "headers.Date" : {
            "$gt" : ISODate("2001-04-01T05:00:00Z")
          }
        },
        "nReturned" : 83057,
        "executionTimeMillisEstimate" : 350,
        "works" : 120478,
        "advanced" : 83057,
        "needTime" : 37420,
        "needFetch" : 0,
        "saveState" : 941,
        "restoreState" : 941,
        "isEOF" : 1,
        "invalidates" : 0,
        "docsExamined" : 120477,
        "alreadyHasObj" : 0,
        "inputStage" : {
          "stage" : "IXSCAN",
          "nReturned" : 120477,
          "executionTimeMillisEstimate" : 60,
          "works" : 120477,
          "advanced" : 120477,
          "needTime" : 0,
          "needFetch" : 0,
          "saveState" : 941,
          "restoreState" : 941,
          "isEOF" : 1,
          "invalidates" : 0,
          "keyPattern" : {
            "headers.From" : 1
          },
          "indexName" : "headers.From_1",
          "isMultiKey" : false,
          "direction" : "forward",
          "indexBounds" : {
            "headers.From" : [
              "[MinKey, MaxKey]"
            ]
          },
          "keysExamined" : 120477,
          "dupsTested" : 0,
          "dupsDropped" : 0,
          "seenInvalidated" : 0,
          "matchTested" : 0
        }
      }
    }
  },
  "serverInfo" : {
    "host" : "dpercy-mac-air.local",
    "port" : 27017,
    "version" : "3.0.1",
    "gitVersion" : "534b5a3f9d10f00cd27737fbcd951032248b5952"
  },
  "ok" : 1
}
Check below all the statements that are true about the way MongoDB handled this query.
```

Answer:
"The query scanned every document in the collection."
 and
"The query avoided sorting the documents because it was able to use an index's ordering."
 and
"The query used an index to figure out which documents match the find criteria."

**That's all folks!**
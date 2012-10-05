---
layout: post
title: "SQL: Schemin' with Schemas"
date: 2012-10-04 23:24
comments: true
categories: 
---
Today was spent working on databases. I had a lot of fun writing SQL queries. We were tasked to create the schema for a simple quiz app. Here's the schema:

<pre>

CREATE table user (
	id INTEGER PRIMARY KEY,
	name TEXT
);

CREATE table quiz (
	id INTEGER PRIMARY KEY,
	name
);

CREATE table question (
	id INTEGER PRIMARY KEY,
	content TEXT,
	quiz_id INTEGER
);

CREATE table choice (
	id INTEGER PRIMARY KEY,
	content TEXT,
	correct BOOLEAN,
	question_id INTEGER
);

CREATE table answer (
	id INTEGER PRIMARY KEY,
	user_id INTEGER,
	question_id INTEGER,
	choice_id INTEGER
);

</pre>

With this schema, we had to write some queries and insert sample data. I pair programmed with Matt, and the last challenge was a bit tricky for us, but it was a fun challenge to complete. It feels really good to be learning so much so quickly.

Create a stats.sql that will show the percentage (if you want to try to use MATH functions within SQL) or the total right answers to each question. This will need to use joins too and might get complicated.

<pre>

SELECT question.content, (COUNT (*) * 100.0) / (SELECT COUNT (*) FROM answer WHERE question_id = question.id)
FROM question
INNER JOIN choice
ON question.id = choice.question_id
INNER JOIN answer
ON question.id = answer.question_id
WHERE choice.correct = 1 AND
question.id = 1 AND
answer.choice_id = choice.id;

</pre>

We then iterate through all the questions replacing the question.id value in the where clause. That outputs the following data with the percentage of responses that were correct:

<pre>

How do you initialize a Git repository?|100.0
What git command stages changes?|50.0
How do you make a commit once changes are staged?|100.0
What command do you use to see whether there are any untracked files in your repository?|100.0
How do you create a new branch and switch to it with one line?|50.0
How do you create a table in SQL?|100.0
How do you remove a table in SQL?|50.0
How do you write a query that returns all rows from a table?|50.0
What is the difference between an inner join and an outer join?|50.0
How do you add a row to a table?|50.0

</pre>
---
layout: post
title: "How to Make Conditional Requests to Github's API using Octokit"
date: 2012-11-30 01:25
comments: true
categories: 
---

For the last month, my team and I have been working on Gitbo, an app for finding open source software issues on Github and providing incentives for improvement by having users issue monetary bounties for solutions.

One of the challenges that we had to overcome was managing our calls to the Github API. Github's starting limit on API requests for developers is 5000 per hour. Gitbo needs to pull all sorts of data from Github's API about developer's repositories, their related issues and comments, as well as user details.

Furthermore, we wanted to keep repository and issue information as fresh as we could in Gitbo. This required frequent calls to the Github API for every record in our database containing information from Github. It wasn't long before we started hitting the limit, which became a huge roadblock in development as well as a large red flag of future scalability problems for our app in production.

The Github API documentation outlines how to make [conditional requests]("http://developer.github.com/v3/#conditional-requests") by adding an `If-Modified-Since` header to the HTTP request to the API which will return a `304 Not Modified` HTTP response if the resource hasn't been modified since that time. This was a great solution for our problem because any `304` responses from Github do not count against our rate limit. If the API resource has been modified since the time you requested, the API returns the normal JSON response.


The question was how to add this into our app. We had already implemented a fantastic open source API wrapper called [`octokit`]("https://github.com/pengwynn/octokit"), maintained by [`pengwynn`]("https://twitter.com/pengwynn") who has been a super helpful resource for our group. My fellow team members [Adam]("http://students.flatironschool.com/adamjonas") and [Kevin]("http://students.flatironschool.com/kevinmcnamee") found a [pull request]("https://github.com/pengwynn/octokit/pull/142") to Octokit from [`bratsche`]("https://github.com/bratsche") which adds support for making conditional requests. However, Wynn decided not to merge into Octokit, preferring a middleware solution, so they merged the new commits from [`bratche/octokit`]("https://github.com/bratsche/octokit") into Adam's [fork]("https://github.com/ajonas04/octokit") of `octokit`.

You can view the additional source code that adds the support in the [merge commit]("https://github.com/ajonas04/octokit/commit/e978e441bdd810ae29754aa44250baec25bfcab0").

If you'd like to make conditional requests in your application using `octokit` simply add `octokit` into your `Gemfile` with Adam's repo as the source:

{% codeblock lang:ruby %}
gem 'octokit', :github => 'ajonas04/octokit'
{% endcodeblock %}

Make sure to `bundle install` or `bundle update` depending on whether you already have `pengwynn`'s original `octokit` loaded.

Now with conditional requests, you can add a `:since` option into all requests to Github that will make the request conditional, returning the full JSON response of the resource if its `Last-Modified` header in the conditional request's response is later than the datetime query string you sent.

In order for it to work, you'll need to send the query string to Github in [RFC 1123 Time Format]("http://www.csgnetwork.com/timerfc1123calc.html"). Luckily, Ruby's Time library has a method `httpdate` which will convert a Ruby Datetime or Time object into a string with this format.

Let's see it in action, I'll make a conditional request with the current time as the `:since` parameter. Since the resource will probably not have been modified in the time between the time object's creation and when Github receives the request, we'll see the benefits for our rate limit:


{% codeblock lang:ruby %}
client = Octokit::Client.new(:login => username, :password => password)
client.rate_limit_remaining
#=> 5000
current_time = Time.now.httpdate
client.repositories("joshrowley", :since => current_time)
#=> nil # this nil tells us we received a 304 response
client.rate_limit_remaining
#=> 5000
{% endcodeblock %}

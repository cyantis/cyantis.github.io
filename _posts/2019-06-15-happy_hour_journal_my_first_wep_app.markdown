---
layout: post
title:      "Happy Hour Journal: My First Wep App"
date:       2019-06-15 18:50:39 +0000
permalink:  happy_hour_journal_my_first_wep_app
---


[Happy Hour Journal](https://github.com/cyantis/happy_hour_journal) (HHJ) arose from a discussion with a fellow cohort-mate who was building a tracker app for easy searching of nearby happy hours. Having found a spot for early-evening provisions, I figured the diner might like to be able to keep track of the who/what/when/where of their happy hour experience. HHJ requires creating an account and, from that point forward, logging in via a username and password. Follow the [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) convention, HHJ allows users to log, browse, change, and delete journal entries. 

Built with the [Sinatra](http://sinatrarb.com/) DSL, HHJ is a Mode-View-Controller web app that makes use of a number of Ruby gems and tools, notably ActiveRecord, which facilitates easy interactions between Ruby and SQL. A lot of trial and error, testing and refactoring went into getting HHJ up and running, though I was glad to’ve had to work through [this previous bear of a lab](https://learn.co/tracks/online-software-engineering-structured/sinatra/sinatra-project-mode/fwitter), as it forced me to understand the inner-workings of a basic web app at every step in the process, especially class associations and restricting CRUD actions user-specific data.

While the Fwitter lab, mentioned above, prepped me fairly well for this project, a number of unique challenges arose during the building of HHJ that lead to some real insight on my part. Here are a few noteworthy ones:

* As best as possible, I wanted to control the quality of the data persisted to the database, notably safeguarding the same restaurant from being input into ActiveRecord multiple times--duplicate restaurant objects would wreak havoc on the data returned to the user. Thanks to a tip from my TA, I looked into building a dropdown menu that displays previous restaurant entries for ease of choice on the user’s part and keeps a restaurant from being persisted to the database more than once. I ultimately settled on a [HTML datalist](https://www.w3schools.com/tags/tag_datalist.asp), as that allowed for selecting a previous restaurant or entering a new one into the same form field. 
* In order to ease the editing of a happy hour post, I used [HTML form placeholders](https://www.w3schools.com/tags/att_input_placeholder.asp) to display the previous entry data. I also used conditional logic to update the database with only the changes (and without wiping out any previous data in the process).
* HTML forms are a pain to style. I had issues styling multiple form fields with the same declaration, using nested classes as style identifiers, and I still can’t style the submit buttons as I’d like (I just want my buttons centered. Why doesn’t `margin: 0 auto;` work?)
* Speaking of styling, Sinatra + Google Chrome seemed to cache everything, so I found myself frequently clearing the cache in order to see style changes. Like every few minutes.
* PSA: Anytime you install a new dependency, don't forget to kill your server session and launch again. I spent longer than I’d like to admit wondering why Rack Flash 3 completely tanked my app before I realized I hadn’t rebooted my server after installing it.
* [Rack Flash 3](https://rubygems.org/gems/rack-flash3) was a ton of fun to play with--I’m glad I took the time to get it up and running. Adding Flash messages to the app not only makes it infinitely more user-friendly, it also really solidified my knowledge of how routes work. [This blog post](https://ididitmyway.herokuapp.com/past/2011/3/15/rack_flash_/) was super helpful in getting me up and running with Rack Flash, especially the tip on dropping an iterator into the`layout.erb` file.

For V2 of HHJ, the foundation is already in place to expand CRUD functionality to locations and the user profiles, and some other related features. Stay tuned for possible updates on that front...



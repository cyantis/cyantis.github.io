---
layout: post
title:      "Professional Development Tracker: A Rails+JS prototype app"
date:       2019-10-19 17:33:38 -0400
permalink:  professional_development_tracker_a_rails_js_prototype_app
---


_Professional Development Tracker_ (PDT) is a prototype commissioned by [Skookum](https://skookum.com/), a digital strategy, UX design, and software development company with offices here in Denver. Skookum provides an annual allowance to employees to be used for professional development, e.g attending a conference, taking an online course, purchasing and reading a book, etc. PDT aims to allow Skookum more accurate tracking of their employees' professional development.  

I met with a manager at Skookum to discuss the requirements for the app. This was my first opportunity translating the needs of end-users to deliverable code, and it was an enlightening process. I have stakeholders, now! That really ups the ante, as it were, in my accountability as a software engineer. Can I build a product that is useful and usable for an entire organization?  

As the current iteration of PDT is a mere prototype, my ultimate success has yet to be verified, though I’ve been assured by Skookum that I’m well on the right path. Below are some of the more interesting and insightful aspects of building this app, thus far:  

### Whiteboarding
Following the advice to [never just start coding](https://anyonecanlearntocode.com/think-like-a-software-engineer/videos/33), I spent a full day whiteboarding before ever typing `rails new`. I’m glad I did! With a solid understanding of PDT’s needs, MVC relationships, and greater functionality, I was able to build the Rails portion of the app in a weekend. And, thus far, I’ve only had to make one major model revision and implement a couple of minor migrations. Whiteboarding FTW.

### Weekly check-ins
Throughout this prototyping process, I’ve been meeting weekly with a manager at Skookum to demo my progress, discuss any changes, and refine the scope and needs. These meetings have been great for accountability as well as a periodic confidence booster, in getting to hear that I’m on the right track. Checking-in regularly, we also caught a bug early on in a model relation and, thus, I was able to make the change with relative ease.

### Manager / Employee model relations
The core requirement from Skookum is that Managers be able to easily track their Employee’s learning. Initially, I created separate models for Managers and Employees, wher an Employee `belongs_to` a Manager. This worked well in regards to mode relations, but, in talking through the functionality with Skookum, it quickly became apparent that this could lead to potential issues when implementing authentication (not to mention the fact that Managers are technically Employees, so this model setup seemed inaccurate).  

The solution ended up being rather simple and straight-forward: I made every app user an Employee, thus doing away with the Manager model entirely. Employees all have a `manager_id` field, which is the (Employee) id of their manager. If an Employee is a Manager, the `manager_id` value is set to `nil`. I also wrote some Employee model methods: one that checks whether or not an Employee is a Manager, as well as one that returns a Manager as an object, both based on the `manager_id` value.  

In the event that this app scales to cover a larger, more multi-tiered organization, this approach will likely need to be reworked. But for the prototype requirements, it offered a clear and concise route to functionality.

### API-only Rails
As in any project, there’s always an unforeseen hiccup or two. For PDT, I learned the hard way about the limitations of building an API-only Rails app. Knowing from the start that the ultimate configuration of the app will likely be a Rails backend and JS frontend, my first version of PDT was built using the Rails API flag. I’d read that this approach is great for “headless” configurations, as it doesn’t load a lot of the middleware that comes standard in a full Rails app. I figured this would affect how my views could be rendered, though I underestimated the difficulty in working around this limitation for the needs of my prototype.  

Perhaps the most interesting restraint I encountered was that API-only Rails does away with the Asset Pipeline, which caused me a bit of a headache as I began to implement the JS requirements. I looked into ways around this but ultimately ported PDT over to a standard Rails configuration. At least I’ll be going into my next project with eyes wide open when deciding whether or not to raise the API flag!

### Additional serializers for customizability
Being a librarian in my previous career, one of the most fun aspects of this project was getting to organize and manipulate data relations so as to create clean and useful API feeds. The [Active Model Serializers](https://rubygems.org/gems/active_model_serializers/versions/0.10.7) gem is a fabulous tool for the easy creation of custom serializers. This was particularly crucial in my `has_many :through` relationships, as adding additional serializers allowed me control over exactly what is rendered when, where, and how.

### Custom validations with JS
Using AJAX to display newly created content is great for user experience (and fun to build!). But, without a page refresh, I noticed a number of my validation-based flash messages wouldn't work. The validations still functioned as needed in Rails--notably preventing the persistence of bad data to the database--but that doesn't keep JS from making it look like form data was submitted (not to mention displaying a bunch `undefined` responses to the user).  

All it took, though, was a simple conditional on the value of a newly created object’s id to either notify the user that they missed a required field or display the correctly persisted data back to them.

### Next Steps
As mentioned earlier, the production version of PDT is shaping-up to be a “headless” CMS, with Rails in back and, likely, React in front. Thus, in the current version, there is minimal--and purposefully un-styled--Rails views. I’m looking forward to tackling React.js in the coming weeks, refactoring the Rails portion of PDT to be API-only, and inching this app toward a production-worthy state. Stay tuned...


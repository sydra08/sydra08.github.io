---
layout: post
title:      "Rails Portfolio Project: BeerMe"
date:       2018-01-17 03:27:11 +0000
permalink:  rails_portfolio_project_beerme
---

When I started my first project - the CLI Gem - one piece of advice that I was given was to choose something I like so that when I get stuck, it’ll be easier to get unstuck. I’ve carried that piece of advice with me through three projects now and it really does make a difference. Case in point: my Rails app has taken me almost two months to complete, but through all the ups and downs, having an app about something I find interesting has definitely kept me going. 

BeerMe is an app that allows you to track beers that you have tried or want to try. Similar to my Sinatra Project, the BeerMe database is community-powered. So in addition to adding your own beers, you can explore the database and see what other people have added.

To create a better database-browsing user experience, there are filters on a number of pages. On each of the pages, the filter is a form connected to a drop-down menu that contains all of the Breweries or Categories (filter dependent) in the database. When a user applies the filters, a request is sent to the appropriate controller. The controller then talks to either the `Beer` or the `UserBeer` model (depending on which page the request comes from) and performs the corresponding class level ActiveRecord scope method in order to generate the proper beer list for the user. 

As mentioned earlier, users can track beers from their own list. There is a form that allows users to add a beer to his/her list. Within the form a user can either add an existing beer to his/her list or they can simultaneously add a new beer to his/her list and to the database. In addition, a user can assign the beer to a new or existing category and/or brewery. In order to prevent duplicate records in the database, I set up a uniqueness validation on the beer name and created custom attribute writers for the brewery and category. This is reflected in the form via a pseudo autocomplete field for the brewery name, beer name, and category fields that I created via a combination of the `<input>` tag `list` option and `<datalist>`. 

Some ideas that I have for future iterations of this app include: 
* Allowing individual users to rate beers they’ve tried and which would then be aggregated into an average score for the beer that anyone can see. 
* Having a custom validation that would allow users to add a beer with the same name as an already existing beer, as long as it is unique to the brewery that it is associated with. 
* Create an Admin role that would be able to edit the Brewery, Category, and Beer details for all of the database entries.


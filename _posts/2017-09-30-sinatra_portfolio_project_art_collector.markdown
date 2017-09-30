---
layout: post
title:  "Sinatra Portfolio Project: Art Collector"
date:   2017-09-30 12:00:17 -0400
---


For my Sinatra Project I built a web app, Art Collector, that allows users to build and manage their own art collections. The idea grew out of a desire to build an app that allows you to keep notes about different artworks, artists, and museum/gallery shows that you’re interested in. However, as I began to sketch out the different models and relationships, I quickly realized that I needed to scale back my project into something more feasible. Currently a user can:

* create multiple art collections 
* add artworks to his/her collections 
* remove artworks from his/her collections 
* rename his/her collections 
* delete his/her collections 
* add an artwork and artist to the database that is then available for others to add to their collections

The idea is that the database is community-powered, so I only seeded the database with a few artworks to start. There are four main models in the app: `User`, `Collection`, `Artwork`, and `Artist`. In order to allow artworks to be in many different collections and for users to have many collections, I set up two join tables: `User_Collections` and `Collection_Artworks`. 

With respect to functionality, I struggled to implement a feature that allowed the user to add an artwork to a collection. At first I wanted to include it all in the `Collections Controller` since adding an artwork was an act of editing the collection. However, I remembered the Single Responsibility Principle, where each controller should only encapsulate logic that relates to a singular entity in the application domain. As I broke down the different pieces of what was to happen when an artwork is added to a collection, it became clearer that it should be handled by the `Artworks Controller`. 

The next issue I had to solve was how to get around the statelessness of the HTTP requests. Since every request happens in isolation, I needed to figure out how to transfer the information about which collection is being edited from the `Collections Controller` to the `Artworks Controller`. My solution involved adding the `collection id` to the `session` hash. This allowed me to capture the relevant information from the `/collections/:id` route and pass it to the `/artworks/new` route. 

Some ideas I have for future iterations of this app include :
* Allow users to add artworks to their collection by adding artists. When they select an artist to add, the default action would be to select all artworks by that artist and then the user can deselect as they please. 
* Allow users to submit edits/updates for different artists and artworks. These submissions would be reviewed by an editor or admin user (new user role) who has the ability to edit the artist and artwork database entries

* Allow users to upload images for artworks and artists


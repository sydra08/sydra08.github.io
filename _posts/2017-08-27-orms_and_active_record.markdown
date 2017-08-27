---
layout: post
title:  "ORMs and Active Record"
date:   2017-08-27 23:27:36 +0000
---


One of the many things that has been blowing my mind lately is Active Record, which is Ruby gem that provides you with a ginormous database toolkit for Ruby. When it boils down to it, Active Record is a dynamic ORM. In order to understand how amazing Active Record is, it is important to understand what an ORM is and how it works. 

### ORMs

An ORM (or Object Relational Mapping) is a concept that is not specific to Ruby. It is a convention to follow in order to organize a program to connect with a database. The key is that when you “map” your program to a database, the classes are database tables and the instances of the classes are the table rows. 

For example, say you have a program that allows users to read and write blog posts. Each `Post` instance has a `title` and `content`. 
```
class Post
	attr_accessor :title, :content
	
	def initialize(title:, content: nil)
		@title = title
		@content = content
	end
end
```
Following the mapping convention of class => database table, we would translate the `attr_accessor`s to column headers and each row in the table would be an instance of the Post class. It would look something like this:

| id | title | content |
| -- | ------------- | ---------------- |
| 1 | My First Post | I’m a post!|
| 2 | My Second Post | I’m another post! |

As you can imagine, users of our blog program want to be able to access posts from previous sessions. By connecting a program to a database, you can persist data. *It is important to note that it is not the actual Ruby objects that are getting stored in the database, but the object’s attributes.* These attributes are then retrieved from the database and used to reify a Ruby object.

The process of creating Ruby objects, saving them to the database, retrieving them and reifying a Ruby object can become quite repetitive. Do it enough times and a number of conventions start to appear. The code you write in order to perform basic CRUD (Create, Read, Update, Delete) functions within your program can essentially be copied into each class definition and be up and running after a few minor adjustments. This repetitiveness is a huge code smell. 

### Enter Dynamic ORMs

To avoid this code smell, you could write a dynamic ORM (~an abstract ORM). The methods within a dynamic ORM are abstract, there are no explicit references to specific table or object properties. For comparison’s sake here is a comparison of a regular ORM to the code from a dynamic ORM:

Basic ORM
```
class Post
  attr_accessor :id, :title, :content
	
	def save
	  sql = "INSERT INTO posts (title, content) VALUES (?,?)"


	  DB[:conn].execute(sql, self.title, self.content)
	  @id = DB[:conn].execute(“SELECT last_insert_rowid() FROM posts)[0][0]
	end 
end
```

Dynamic ORM
```
class InteractiveRecord
  def save
    sql = "INSERT INTO #{table_name_for_insert} (#{col_names_for_insert}) VALUES (#{values_for_insert})"
		
    DB[:conn].execute(sql)
    @id = DB[:conn].execute("SELECT last_insert_rowid() FROM #{table_name_for_insert}")[0][0]
  end
end
```
The `#save` method in the `InteractiveRecord` class  is more abstract than the `#save` method in the `Post` class. There are no references to the literal table name, column names, and insert values that are needed for the SQL query. Instead they are all abstracted away into other methods. Having such abstract and flexible code allows you to create one superclass (ie. `InteractiveRecord`) that other classes in your program can inherit from. Doesn't sound too bad, right? Well...the bad news is that writing your own dynamic ORMs can become cumbersome to maintain. The good news is that Active Record can help you out. 

### Active Record
As the Rails Guide describes it:

>Active Record is the M in MVC - the model - which is the layer of the system responsible for representing business data and logic. Active Record facilitates the creation and use of business objects whose data requires persistent storage to a database. It is an implementation of the Active >Record pattern which itself is a description of an Object Relational Mapping system -- [Rails Guides](http://guides.rubyonrails.org/active_record_basics.html)

So basically Active Record is a dynamic ORM on steroids. Not only does it provide you with basic CRUD functionality, but it also provides you with an easy way to implement and maintain different relationships between models (ie. `has_many`, `belongs_to`, `has_many :through`). I get the feeling that I’ve only scratched the surface with Active Record and am excited to see how everything fits together in a complex web app.  

Onwards to Sinatra! 

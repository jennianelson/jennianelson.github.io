---
layout: post
title:      "Flatiron Project #2: Sinatra App"
date:       2017-10-24 18:09:45 -0400
permalink:  flatiron_project_2_sinatra_app
---


I'm about to submit my second portfolio project for Flatiron School!  While my first project felt like treading through quicksand, I felt much more confident this time around.  Hooray for progress and growth!

The purpose of this project is to build a basic CRUD (Create, Read, Update, Delete) web application using the Sinatra framework in Ruby.  The application should also validate user input and protect passwords.  I'm a former English teacher and love to read, so I built an app called "Character Keeper" that allows a user to keep track of his or her favorite book characters.  As an additional feature (and because I wanted to practice join tables), character traits are a big component of the app, allowing users to sort characters by their traits and to see all of the favorite characters and accompanying traits that users have added.  I've learned quite a bit in this section of the program, from SQL queries to ActiveRecord, so as a way to review and hopefully help other newbies, I'm going to explain the important components of my application and how they work together to provide both a usable and enjoyable user experience.

I'm not going to go into detail about the file structure of this type of app, except to say that I learned a lot throughout the labs in this section about "separation of concerns"--i.e. separating tasks into folders and files to keep things organized and easy to understand.  I also have a much better understanding of what directories are needed and what each file does for the application.

After getting my environment setup and installing necessary gems, I sketched out the **objects** I would want to create and what **associations**, or relationships, they would need to have.  These would become my **models**:
* A User **has many** characters
* A Character **belongs to** a User and has many traits
* A Trait **has many** characters

Because of the many-to-many relationship between a Character and a Trait, I would need to create a **join table**, and thus another model called CharacterTrait:
* A CharacterTrait **belongs to** a Character and **belongs to** a Trait.

So my Character and Trait models would need to have the following associations:
* Character **has many** character_traits, and **has many** traits **through** character_traits
* A Trait **has many** character_traits, and **has many** characters **through** character_traits

Next, I created **database** **migrations** for four **tables** that would correspond to my models:
* users: has columns for username, email, password_digest (password protection using the Bcrypt Gem)
* characters: has columns for name, book, description, user_id (because a character belongs to one user)
* traits: has a column for name
* character_traits: has columns for character_id, trait_id

The models also needed a way to ensure that objects were not being created without the necessary attributes.   I added the following **validations**:
* User: validates_presence_of :username, :email
* Character: validates_presence_of :name, :book, :description

With the help of the **bcrypt** gem, I was also able to ensure that a user's password would be secure:
* User: **has_secure_password**

And for some added readability in my URLs, I created methods in the User and Character classes to "**slugify**" a username and a character name.  Instead of the resource having to use ID numbers, they included the user's username or a character's name in a URL approved format.  For example, my list of characters would be www.character-keeper.com/users/jennianelson, and Hermione's showpage would be www.character-keeper.com/characters/hermione-granger.

Because each model corresponds to a table and inherits from **ActiveRecord::Base** it provides the connection to the database, allowing the app to create, read, update, and delete information.

Next came creating an **Application Controller** to handle the HTTP requests.  It included the configure settings, such as enabling sessions and setting the public folder, views, and session secret, as well as a get request to the app's home directory.  From the homepage, the user would be able to either create an account (Sign Up) or Log In.

Now came the "real" work of writing the logic in the controllers that would get the correct information from the user, create objects, associate them correctly, and persist them to the database.  After creating a basic view for the homepage, I added a **UsersController**, telling my config.ru to use this controller in addition to running the Application Controller.  All other controllers would inherit from the main controller.

I would bore you to tears if I described every request and view in my controllers, so I'll summarize.  I ended up adding two more controllers for Characters and Traits.  The user only has access to the Login and Signup pages if they are not logged in, and the app takes a user straight to their page of characters if they are already logged in.  On the "character/new" page, a user is able to create a new character with a book/series, and a brief description.  They are also able to choose from a list of traits and/or add a new trait to the the app.  They are then taken to that character's "show" page.  This page includes links to both edit and delete the character, and these links are also included under each character when the user views all of their saved characters.  In addition to being able to view their characters alphabetically by name, they can also view them alphabetically by trait.  I found it interesting to see what type of characters my testers and I prefered!  Because this app's users are few, I also included the ability to view all of the characters that users have created and organize them by trait.  The CharacterController ensures that a user is only able to edit and delete characters they have created, however.

Because I do not know how to write tests yet, I tested each feature as I went using the **shotgun** gem, and jumping into **rake console** or **tux** frequently to make sure my code was doing what I expected it to.  I had so much fun creating this and found myself wanting to work on it constantly!  Well, until it was time to make it look pretty...

Front-end styling is a requirement of this project (beyond basic HTML), but I was inspired (and challenged) after seeing projects from fellow students.  I told myself that I would give myself one day to learn, relearn, and do all the bootstrapping/CSSing I could.  While there is MUCH more I would do before ever making this a "real" app, I got a good refresher in CSS and am proud of the way it turned out.

Feel free to check out my project's GitHub repo [here](https://github.com/jennianelson/sinatra-character-keeper), and "steal" whatever you like!  I learned pretty early in life that you often have to imitate before you can create, and I'm thankful for all of the great examples my friends on Learn have shared with me.  Thanks for reading, and happy coding!





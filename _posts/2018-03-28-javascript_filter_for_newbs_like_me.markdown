---
layout: post
title:      "JavaScript .filter() for Newbs Like Me"
date:       2018-03-28 17:25:17 -0400
permalink:  javascript_filter_for_newbs_like_me
---

I just started learning JavaScript after the wonderful world of Ruby, so my brain is attempting to adjust to the curly braces, excessive parentheses, semi-colon disputes, explicit returns, and how important the year 2015 was.  Things were going along okay until I hit the **filter()** method.  I even felt like I was comprehending what was going on "under the hood"...right up until I had to actually write my own functions.  It's like that moment in French class when you're feeling all confident because you finally figured out how to conjugate verbs, but then your teacher is like, "Okay, now use them correctly when speaking."  To help me (and hopefully you) make sense of how this method works, I'm going to make myself explain it.  Disclaimer: I've just been introduced to this method, so there are most certainly some nuances I am (unintentionally) leaving out, and potentionally some "not best practices" involved here, but my code works, so that's something.  Here goes...

**Filter ()** allows us to select certain elements from an array and return them in a **new array**.  The original array could contain strings, numbers, or objects.  Here's example of an array of objects:

<a href="https://imgur.com/HEwYRMX"><img src="https://i.imgur.com/HEwYRMX.png" title="source: imgur.com" /></a>

In order to decide if the element meets our specifications, the JS engine needs to look at each one individually.

**Filter()** takes one argument: the **function** we would like to use on each element in the array.  

This passed-in function is called a **callback**, and it also takes an argument:  the variable you would like to use to refer to each item in the array.  So if you have an array of foods, you might call this callback variable *food* or *foodObj*. 

If the **callback** function returns true for the element, that element will be inserted into your new array.  The callback function is called for each element, and so you end up with an array of selected elements.

This makes pretty good sense and isn't so different from Ruby's #select method.  What I struggled with, however, was turning this logic into appropriately syntaxed JavaScript that would actually return what I wanted.  Key word here: **return**

In Ruby, a method returns the last executed line of code.  Not so in JavaScript.  You have to expicitly tell it what to return.  You can also do a lot of work with conditionals without using any "ifs" at all--something else that really tripped me up.

As an example, we'll user our yummyFoods array, and write a function that will allow us to specify what feature we have a craving for and give back to us the foods that have that feature.

<a href="https://imgur.com/DMXv2jx"><img src="https://i.imgur.com/DMXv2jx.png" title="source: imgur.com" /></a>

The first time I wrote one of these filter methods, I left out the return before the filter.  Then I wanted to write a conditional statement in the callback function, but the "if...then" logic is already inherently part how a callback function works.  If the callback returns true, the element you are currently working with will get pushed to the new array that filter() is building. It helps me to remember that I need to **return** the element and **return** the result of the callback (true or false).

I look forward to reading this blog post a few months from now, rolling my eyes at my baby JavaScript self, and celebrating how far I've come.  For now, I'm moving on to **map()**!

## Update after completing the "Filter and Map Lab":

### SPOILER ALERT: I recommend doing this lab first before you read on!

This lab works with an array of drivers for "Scuber", the hilarious company made up for these labs that involves carpooling people around on scooters

One of the functions you are tasked to write is exactMatch().  It should take in the array of drivers and a "matcher"--a piece of data in the form of an object.

<a href="https://imgur.com/nI2xFLo"><img src="https://i.imgur.com/nI2xFLo.png" title="source: imgur.com" /></a>

Oh, did I struggle with this.  I tried all sorts of methods like include and split, but the format of the matcher made it difficult.

I ended up doing what I knew would pass the test but would only work with this particular array of data.  

<a href="https://imgur.com/VssU1F3"><img src="https://i.imgur.com/VssU1F3.png" title="source: imgur.com" /></a>

We've learned as programmers that our code should  be flexible and reusable, so I knew there had to be a better way.  After passing all the tests, I took a look at the solution.

<a href="https://imgur.com/jbHsWrT"><img src="https://i.imgur.com/jbHsWrT.png" title="source: imgur.com" /></a>

At first I was like, "Hey! I tried a for...in loop!"  But other than that, I wasn't close to getting this right.  So I plugged in a *debugger* into the callback function to figure out how this works.

line 25 *let matches* declares a variable (so far undefined) that we can use both inside the for...in loop and outside of it.  Remember that in the scope chain, variables have access to their parent scopes but not their children scopes.  We use let so that we can set the value of this variable in our for..in loop.

The for...in loop allows us to access the key of the matcher.  It sets the value of matches to be true or false: true if the key of the driver is equivalent to the key of the matcher, and false otherwise.

Then by returning matches on line 30, we are returning either true or false--the whole point of a callback function.  If matches is true, driver will get pushed to the filter array!







---
layout: post
title:      "A Different Kind of Post Route"
date:       2017-10-22 15:04:57 -0400
permalink:  a_different_kind_of_post_route
---


<a href="https://imgur.com/0BzIVel"><img src="https://i.imgur.com/0BzIVelm.png?1" title="source: imgur.com" /></a>

I've lived in Birmingham, Alabama all my life, and I grew up knowing about post routes.  They were what my dad taught me to run when we played football in the back yard.  I'd run straight out from the line of scrimmage, then cut diagonally toward the middle of the field to catch the ball.

Years later, I found my self chuckling when I came across this familar term when learning about http requests, imagining how different the meanings of the two would be.  Surprisingly, I found my football knowledge a pretty good metaphor for understanding the roles of Models, Controllers, and Views (or MVC) in a basic Sinatra web application.  It may be a stretch in a few places, but here goes:

<a href="https://imgur.com/76cIIhV"><img src="https://i.imgur.com/76cIIhVm.png" title="source: imgur.com" /></a>

The Models contain the plays--the logic behind the game strategy.  In order to design great plays, the coaches have to consult statistics, film, etc.--basically *databases* of information.  In an MVC application, the Models are the classes that interact with the app's database and provide the logic to the Controllers. The Controllers are the team leaders, in charge of taking in information (like the play) and distributing it to the players.  A web app can have several controllers, but they all inherit from the Application Controller.  The app "runs" this App Controller and "uses" the rest in the config.ru file.  You could call this main one the coach or the quarterback, depending on who's really calling the shots (wink, wink). 

<a href="https://imgur.com/ZwwPAu5"><img src="https://i.imgur.com/ZwwPAu5m.jpg" title="source: imgur.com" /></a>

In the case of a web app, the controllers "get" this information from what is appropriately named a "get" route.  In these get routes, information is shown to the User through a View.  The Views are the playmakers on the field--what the User (anyone watching the game) get to see and interact with.

Then we get to a post route.  This is a magical moment because those watching don't necessarily see all the complex strategy involved.  They just see the ball leave their quarterback's hand, spiral down the field, and, if all goes as planned, a tremendous catch and some major positive yardage.  In a web app, a post route takes the information received in a "get" request, and uses it, along with information from the Models, and does something with it.  Then it makes another get request, presenting the user with a View that says, "Tada! Look what I can do!"  I don't think a wide receiver has ever said words as lame as those, but they mean something similar to a [T.O. touchdown celebration](https://youtu.be/Fevwa9p3s6Y).  Enjoy.





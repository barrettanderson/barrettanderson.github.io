---
layout: post
title:      "Sinatra Project - Operation Recipe Creation"
date:       2020-07-18 03:04:33 +0000
permalink:  sinatra_project_-_operation_recipe_creation
---


My favorite baseball player growing up, was, of course, the greatest baseball to ever play.  Ken Griffey Jr.  The Kid.  Watching him walk up to the plate, bat in hand, and flawlessly, effortless, with so much precision and skill and the swing of the bat crush a ball into the upper deck.

Fast forward to my little league game, stepping up, and it feeling so uncomfortable and foreign.  I didn't realize it then, but that "effortless" only came because of years, and tens of thousands, if not millions of swings of the bat to get to that point.

Fast forward to 5 days ago.  Me walking up to the plate of this project, seeing my instructor go through setting up file structure, the way that model, view, and controller interact. Create and import seed data, set up the HTML to make the website, look like, to the user for all the know, how it looks.

And just like when I stepped up to the bat and felt overwhelmed with incopetence, I took a swing.  And missed.  And missed, and missed.  Seeing is so much different than having to do.  Although I might have "learned" how MVC is created, set up, and the layout, it wasn't until I had to do it myself that I realized how little I knew, and it certainly wasn't cemented (and even now is just a little firmer and drier).

One of my first big issues was in the initial stages of the MVC, where, in ping-pong like fashion, we bounce around to the views and controllers in order to start the flow of our program/website.  At this point, and I should have realized it earlier, I had so many things in my head going on.  I knew I needed to work on connecting both models through the `has_many` and `belongs_to` relationship of my two models.

I realized later that this relationship could of course be developed after getting the MVC created.  But during that time, I was trying to do far too many things.  This led to quite a few small errors which took a TON of time.  I also realized that I wasn't testing much along the way, and forgot about my dear friend `binding.pry`.  This led to a few blocks that I didn't necessarily know if I fixed, because there were quite a few mistakes, and was unsure what they were.

One of those errors was found in my post method in my `class RecipesController`.  This line `@recipe = current_user.recipes.build(params[:recipe])` which is there to save an instance variable that I will alter us in rendering `erb :'recipe/new'` takes in the params of a :recipe.  However, one PESKY little s at the end of `:recipes` cause hours of delay as I looked over similar code and examples from lessons and lectures, unsure why my `@recipe` was constantly returning nil.  I was convinced it was something about my get methods, or an error in my views.  Eventually, I set up a `binding.pry` and took it line by line, making sure that it would only build this recipe for the current user using the params.  My params was still returning a value, and there was a key in it of recipe, so I couldn't figure it out.  Finally, I found the S, and after a little `ctrl+r` on my website page, I FINALLY saw something.

From there it was going through some of the swings that I had felt a little more confident in, about setting up some generic html in my views to get showing, using erb and `<% %>` for logic, and `<%= %>` to use the ruby logic and display whatever might need to be displayed.

One of the last wins of the project, as my swing started to feel my own came from some code that I had already written in my controller, but was running into an issue where other users could still click the `delete recipe` button on a recipe, but it wouldn't actually delete, it would just send them back to the recipe page, as I had protected in that instance already with 
`if current_user == @recipe.user
@recipe.destroy
end`

The last issue I ran into, was the .env session_secret.  I followed all the guides, just like in the lessons and lectures, only to realize that I had not created a .env file in my project.  After a quick creation of setting my variable `SINATRA_SECRET` to an additional layer of encryption with the hexadecimal, I finally stopped receiving the error that I had been in my terminal about unprotected password. This line of code in my terminal whenever someone logged in let me know something was wrong, but couldn't figure it out without a little help.  

```SECURITY WARNING: No secret option provided to Rack::Session::Cookie.7
				This poses a security threat. It is strongly recommended that you
				provide a secret to prevent exploits that may be possible from crafted
				cookies. This will not be supported in future versions of Rack, and
				future versions will even invalidate your existing user cookies.```
				
After creating the .env and setting the variable SINATRA SECRET all was done (at least the bones of the project and website).  I had thought that SINATRA_SECRET was some magic that was going on behind the scenes, but learned that it needs to be set to the second layer of encryption.

And with that, a week of frustration, triumph, joy, and a sense of accomplishment, I walk away from this week after some more swings at the plate, a little more confidence in my swing, some LENGTHY reminders about going slow, testing my work, doing one thing at a time, and mapping out my plan will help me down the road with my future at bats.
			 


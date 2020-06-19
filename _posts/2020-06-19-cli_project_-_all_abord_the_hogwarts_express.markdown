---
layout: post
title:      "CLI Project - All Abord the Hogwarts Express"
date:       2020-06-19 17:47:16 +0000
permalink:  cli_project_-_all_abord_the_hogwarts_express
---



Hello to both of the people who read this blog.  Hope you enjoy!

API, CLI, my oh my!  I went into this week quite unsure of what I wanted to have a project about, let alone the technical aspects to pull the data from the api, and then translate it to a user experience through a CLI.  

I was then surprised, how looking back to the start of this, that only a few weeks in that this is something that I was able to accomplish.  At times, there have been points where the last few weeks have been greuling, but then getting to sit down and start coding through this, that throught the blood, sweat, and tears (paper cuts are dangerous, beware) felt worth it as I began to create my first thing.  Program I guess might be a better word.

One of the first issues that I ran into, was that the API that I chose was one that required a key.  So, for the 2398403298th time in my life, I created an account, user name, password, email address, so that I would have access to this generated to key be able to pull from their API.  Once I had done this, it was smooht sailing and I had 0 issues at all.  If only ;).  

One of the things I am aware that I need to work on is being able to understand and read through syntax directions (like on Stack Overflow).  After some wonderful guidance and coaching from our cohort lead, I was able to access all that I wanted to.  It was almost as though I had some magical key, through whose lock I was able to pass, accessing everything Harry Potter, my very own Room of Requirements.

Here is what that looked like 
` response = RestClient.get(HP_URL + "characters?key=blahblahblahgetyourownkey")`.  
I was missing the keyword key, which of course was on the site, but I didn't realize that was what it was meaning.

From there, I started going through the data that I had access to, by setting variable data `data = JSON.parse(response)`, to start combing through all the data I had and decide what I wanted the user to have access to.

As probably noted in the length of these posts, the more information the better, and I ended up choosing nearly all ove them, accept whichever index of the array "(underscore (I can't do the actual underscore because it either makes text bold or italics, sorry!))v" means.  For every character I looked at, it seemed to give a nil value, so if you can figure it out, I will give your house 50 points.

The next issue I ran into was, that even after watching the API videos and doing the lessons, I never understood the purpose when we iterate through the data of the API, that we also set up our own .new object.  
```
 Character.new(
                name: name,
                role: role,
                house: house,
                school: school,
                boggart: boggart,
                ministryOfMagic: ministryOfMagic,
                orderOfThePhoenix: orderOfThePhoenix,
                dumbledoresArmy: dumbledoresArmy,
                deathEater: deathEater, 
                bloodStatus: bloodStatus,
                species: species,
                patronus: patronus,
                wand: wand,
                animagus: animagus,
                _id: _id
            )
```

That includes the information that I wanted to include for my characters, as well as the fact that it created a new Character.  So, I tried to delete it, and lo and behold, I no longer had access to the array that my code had been storing them into.  It was an aha moment rivaling the reveal of Quirrel at the end of book one (#sorryaboutthespoiler!).

The last issue I ran into is how to really get to one level deep within the CLI for the user.  It was pretty straightforward for them to open the first menu, but then pulling information from the `user_input = gets.strip` proved to take some extra time, help, and attention.

Eventually, I realized that very similar to the first menu of the CLI, I could do something similar of calling on a method from within that method in order to then call another `user_input = gets.strip` from within there to have them choose the character.

In there, I tried to use the method each, but I ran into the issue of it iterating not only the first time based off the user input, but 194 times after that. I needed to select (notice what I did there) a method that woudl only look for the first time that the user_input == a character.name.

Again, realization dawned on me that .select method would be better. Then I ran into my final issue.  I wanted the user_input to either be the name or the number of index associated to the characters.  I couldn't call `character.name == user_input` because there was no instance variable for the number associated to the index.  Eventually through help of a fellow student, we realized that it was understanding the user_input as a string, and not necessarily as an integer.  The final line of code became `found_character = Character.all.select.with_index(1) {|character, index| character.name == user_input || index == user_input.to_i}` to either select the character based off the name, or the integer provided, and then we stored that in a local variable.

And voila, the Room of Requirements has been opened.

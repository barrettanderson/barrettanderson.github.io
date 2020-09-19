---
layout: post
title:      "Javascript Portfolio Project - Cocktails on Cocktails"
date:       2020-09-19 17:15:30 +0000
permalink:  javascript_portfolio_project_-_cocktails_on_cocktails
---


Oh man what a ride the last week has been.  Compared to past projects, this project for me felt like one of the biggest growth times and learning from a project.

Starting off, I spent quite a bit of time trying to come up with the idea for the project, then outlining what my models, relationships, and overall structure would look like.  This was certainly an improvement on past projects, where I had a general idea of where I wanted to go, and problem solving along the way.  The issue was that I ran into many issues that, with more pre-planning, could have been avoided.

So what is this project? 4 months ago, the next sentence would have been no more intelligible than me trying to understand a dissertation in Javanese (Indonesian dialect of those on the island of Java, but it sounds like javascript so I giggled).  I created a backend, running rails in api mode, and a frontend comprised of js, css, and html, fetching databases from the backend, creating objects in the front end through js, and persisting them to the backend.  Oof that was a mouthful.  So let's break it down.

First off, to create use rails API mode, one adds  `--api` onto the end of the creating of the directory, i.e. `rails new insert-project-name-here --api`.  This gives a couple different functions.  First off, because API mode knows that we are going to be giving the views functionality (what the user actually sees when interacting with the app), there is no application.erb file, instead, this is all done on the frontend with js, css, and html.  Another big difference is that the controllers are not inheriting from` ActionController::BASE` through `ApplicationController`, but instead from `ActionController:API`.  API, or "Application Programming Interface," is a lightweight version of BASE, that does not include all of the functionality of BASE, and rather extends the necessary requirements for API, such as the example above.  More information can be found in the [API - Ruby On Rails guide](https://api.rubyonrails.org/classes/ActionController/API.html#:~:text=Class%20ActionController%3A%3AAPI%20%3C%20Metal&text=API%20Controller%20is%20a%20lightweight,need%20for%20API%20only%20applications.).

That brings us to our dear friend, css.  In 7th grade, my parents were called by my art teacher gave me my first grade below an A (it was a D), for demonstrating "a lack of trying or putting in effort".  To be fair, I'm sure my snarky, angsty 7th grade self made it unenjoyable for certain teachers, but I can truthfully say that I tried in art, but always felt like I wasn't good at it, which I'm sure limited my ability.  After realizing, to her horror, that no, I was just really that bad at art, she gave me a chance to work and improve.  Fast forward a couple decades, Materialize is a think, which, for those of us that are let's say, *lacking,* in the design world, makes it so so easy to create within minutes a layout that avoids me from garnering another D.  Link for my fellow artistically challenged readers [Materialize](https://materializecss.com/).

Now.  This has all been the easy part.  I would say in this project, there were 2 difficult pieces to get my mind wrapped around.

First, is how the front end and backend communicate with one another.  The backend is where the initial data is stored, and where the front end, upon creation, will persist that data.  So the backend needs controller actions to do any type of CRUD (Create, Read, Update, Destroy) that my front end is wanting to access or maniuplate.  One of the things that ActionController::API gives is all `The default API Controller stack includes all renderers, which means you can use render :json and brothers freely in your controllers` as per the Ruby On Rails guide above.  This is key, because my javascript when accessing this data, really likes the format that JSON, or JavaScript Object Notation gives.  It is easy for the app to parse and generate, and this is key to make sure that data going in and out of the backend will look the same to ensure that the app can understand it and manipulate it correctly.  The front end talks to the backend with fetch requests, to do all the above CRUD options.  But one still has to create the objects on the front end, so they can be rendered without having to refresh the page.  So for example, my create action for cocktails (oh yeah, my project is creating cocktails with ingredients), looks like this.

```
def create
        @cocktail = Cocktail.new(cocktail_params)
        if @cocktail.save
            render json: @cocktail, :except => [:created_at, :updated_at], :include => [:ingredients]
        else
            render json: @cocktail.errors.full_messages, status: :unprocessable_entity
        end
    end
```
If my cocktail saves correctly based off the strong params that I am passing through, instead of referring to a view like in rails non-API mode, it renders it as json which looks like this -
```
[
   {
      id: 4,
      name: "Manhattan",
      description: "The Manhattan was the most famous cocktail in the world shortly after it was invented in New York       City’s Manhattan Club, some time around 1880 (as the story goes). Over the years, the whiskey classic has dipped in and out of fashion before finding its footing as one of the cornerstones of the craft cocktail renaissance.",
   ingredients: [
      {
         id: 3,
         name: "Bitters - Angostura Aromatic",
      },
      {
         id: 22,
         name: "Orange Juice"
      },
      {
         id: 31,
         name: "Vermouth - Dry"
      },
      {
         id: 33,
         name: "Whiskey"
      }
   ]
  }
]
```
So the data is rendered as a nested array, and an object with key and values, in ruby we call this hashes, but in javascript it is an object (although functioning very similarly to hashes).  Notice that within cocktails, there is a relationships to ingredients. where one can call Cocktail.ingredients and find a list of ingredients for that cocktail.  This is done with a join table I named mixer, where a cocktail `has_many :ingredients, through: :mixers`, this is reciprocated from ingredients, and mixer `belongs_to` both.  With this association, this gives me many methods, the most important of which (at least to solving the association issue, is found in ingredient_ids.  So how does this work?

Within the javascript to create the front end object, we use the constructor method
```
constructor(id, name, description, ingredients) {
        this.id = id
        this.name = name
        this.description = description
        this.ingredients = ingredients
    }
```
This ensures that when we create an object, it is getting passed into this special type of function of javascript.  Because we want to let javascript do what it is best at, making things happen without reloading, we want to be creating this object and not only saviging it on the backend, but also creating it onto the frontend without reloading.

Lastly, came the challenge of how to manipulate the DOM to show what was going, give interactivity for the user to create cocktails with associated ingredients, etc.  One issue I realized I had, was that I had given ids to different parts of the html to both cocktail and ingredient.  I realized, to my horror, that my old fashioned id was the same as id for apple juice, and how should html know any different.  So I ended up creating custom id's for both cocktail and ingredients, with our magical friend, `this`.  This refers to whatever is currently executing the javascript code.  So when creating cocktail object with name and description, h4.innerText = this.name

`  h4.id = ``c-name-${this.id}`
        
        p.innerText = this.description
        p.id = `c-description-${this.id}`

But for my ingredients, they needed their own, enter         
```
        ul.innerText = "Ingredients"
        ul.id = `ing-l-${this.id}`
```
This made sure that when we create an object, within the html is going to be, under ul, the unique id of the ingredient, which is nested under the unique id of the cocktail created, but no longer conflicting.  And this brought me to my last, and toughest problem.

When someone is going to edit a cocktail, I want it to autopopulate the checkboxes that I had done when creating the cocktail, of which ingredients were already associated.  I had originally set up an arrow function, and tried to call this, but unfortunately, you can't call this from within an arrow function.  Thank goodness for google, and wonderful cohort and pod leads to help solve this.  It is a two part solution, first, we needed to grab all the ingredients of a cocktail when we hit the edit button.

```
  data.ingredients.forEach( ing => {
                const li = document.createElement('li')
                li.innerText = ing.name
                li.className = 'ingredient-text'
                ul.appendChild(li)
            })
						
						editedCocktail.ingredients = data.ingredients
```
The data.ingredients is the second .then part of the fetch request that is being made.  We set that to the editedCocktail that is being passed into the form to edit them.  From there, the second part is much simpler where 
```
cocktail.ingredients.forEach( ing => {
        document.getElementById(`ing-checkbox-${ing.id}`).checked = true

    })
``` we iterate over each of the ingredients in this cocktail that is being passed through, and then changing the value of checkboxes.check = true, which is to say checked.

And so, at 6:30PM last night, I had finally gotten over the last hurdle of functionality for my project.  And looking back at a few months ago, where I had never written a line of code, and now continuing to sometimes plod, sometimes burst through, these challenges and steps to learning is exhilerating.  Until next time!

Barrett


---
layout: post
title:      "Rails Portfolio Project - Food Banks"
date:       2020-08-22 19:27:45 +0000
permalink:  rails_portfolio_project_-_food_banks
---


My initial intention was to create an application where a user would create an account, say the number of members in their family, which would then dictate the requirements for what order they could place to a specific food bank.  I also wanted to have the functionality for the food bank to receive these orders, and be able to tell from their own database of food if they had sufficient inventory, and if not, let the customer know that that order could not be fulfilled.

Needless to say, after mapping out the models, and looking at what would need to be done in terms of validation and communication across models, I realized that this task would not be able to be done in the time allowed, and instead would have to be relegated to the future.  In a world of COVID, how great woudl it be for customers to see what their choices are, make the order, and then do contactless pick up and help keep them and food bank employees safer!  Alas.

What I did end up with, after hours of wrestling with relationships, is a `user model that has_many orders`, and `has_many food_banks through those orders`.  Likewise, a `food_bank has_many users through the orders`.  The `orders belong_to` both the `users and foodbank`.

Again, I had some intial dreams `f.collection_select` coming from my `form_for([current_user, order]),` one of the necessary components for allowing nested routing, in this case orders nested into the current user, would be communicating with the food_bank inventory database.  However, practically I did not know how to make this happen yet, and for the time being is an `f.text_field` where the user will enter in their order.  I was able to figure out how to have the custom `f.collection_select(:food_bank_id, FoodBank.all, :id, :location)` which, looking back 3 months ago, would have made about as much sense as peanut butter and pickles, dont @ me.  But what this awesome line of code allows me to do is use the food_bank_id found in my order object, iterate through FoodBank.all, find the id, and show the location in order to associate orders with the food banks.

Upon creation of an order, they are automatically associated with a user, but figuring out how to give a form way for the user to be able to associate their order with a specific food bank took many skills and learning from the last few months in order to do this.

One other issue that I ran into was how to use the scope methods.  My first couple ideas where to have scope methods that would then be able to give a list of all of thge orders of a food bank or a user.  However, this could easily be done with other ruby methods.  Instead, I realized that a food bank would like to be able to run a list of the orders in the last 24 hours that would then allow them to see what needs to be fulfilled.  `scope :todays_orders, -> { joins(:orders).where("orders.created_at >?", 1.days.ago) }` would allow me to call this scope method on my FoodBank model and return all orders made in the last one day.  It creates this method `todays_orders` then uses SQL syntax to `join orders on orders.food_bank_id` equal to `food_banks.id` where it then requires quotes to pass in the string `orders.created_at` are greater than (in this case greater than means a time that is greater, aka newer) than 1 day.

Whew.  The other difficulty that I ran into was Omniauth with facebook.  I had set the .env file correctly to store the key and password to give access to facebook for the app, but I initially did not have my routes and actions correct to work alongside the omniauth-facebook gem to allow users to sign in with facebook.  I had forgotten to create an action within sessions that I could then route to in the scenario that a user is signing in with facebook instead of the sessions sign up and login option also present int he project.  Given that my action was named `facebook`, i had to ensure that in my routes.rb file was this route ` get '/auth/facebook/callback' => 'sessions#facebook'`.  Lastly, through binding.pry in my sessions create controller, I was able to run auth which then told me the layout of the has needed for my class method `def self.find_or_create_from_omniauth(user_info)
        User.first_or_create(uid: user_info["uid"]) do |user|
            user.email = user_info["info"]["email"]
            user.password = SecureRandom.hex
        end
    end`
		
	I felt like I learned and struggled with this project more than the others.  Some of the abstraction caused me to miss and get confused about what was going on.  Again, I felt like I had this grand idea of what I wanted to create, but getting down to what each step does, what needs to happen next, is so so so important in creating an app and project.  This time I think I did a better job than last time where I sat for 2 days before really get things started.  The steps necessary for how rails is structured, creates routes, controllers, models, and views to walk a user through a task or objective is a thrilling thing.
	
	Until next time
	
	Barrett

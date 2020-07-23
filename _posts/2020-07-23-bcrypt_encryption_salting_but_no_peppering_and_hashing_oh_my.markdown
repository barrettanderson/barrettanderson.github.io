---
layout: post
title:      "bcrypt, encryption, salting (but no peppering) and hashing oh my!"
date:       2020-07-23 16:35:51 +0000
permalink:  bcrypt_encryption_salting_but_no_peppering_and_hashing_oh_my
---


In a day where encryption, securing user data, and breaches are common discussions and at the forefront of our brains, I am seeking to better understand how things things work and interact.

When a user 'logs in', what happens is that a user's ID is stored in the sessions hash.  This is found in the code in my project ->
```
post '/signup' do
        @user = User.new(params[:user])
        if @user.save
            session[:user_id] = @user.id
            redirect '/recipes'
        else
            flash.now[:error] = @user.errors.full_messages
            erb :'users/new'
        end
    end
```

It creates a new user with the params [:user] found in my schema for users.  If done correctly, it will save the instance variable user, set the @user.id to my session hash :user_id, then send the person on their merry way to recipes to become the next Paul Holleywood, Pru (who really knows her last name, sorry I'm not from England and don't know this).

If, however, the user instance is not saved because of ```validates :username, presence: true, uniqueness: true```, then it will flash the error message from ``` flash.now[:error] = @user.errors.full_messages``` then render the ```erb :'users/new'``` view.

But now, onto the important stuff.  Password protection.  Enter bcrypt gem.  Our valiant, hacker defending gem to help secure our user passwords.  This is going to help write some methods to authenticate users, as well salting and hashing the password.  Although my project was about recipes, this salt is a little different than your run of the mill NaCl. More on that later.

Alongside bcrypt, is going to be ```has_secure_password``` that is the other part of the dynamic duo in helping protect and store user's log in information and validate while using the website.  ```has_secure_password``` is inherited from Active:Record, which the user class inherits from.  This does 3 things.  It validates that password is present on creation, no less than 72 bytes, and confirmation of password.  It will then assign the password to ```password_digest``` column found in the users table.  has_secure_password also gives a few other methods, importantly one that is used in my project ```.authenticate```.  

```
if @user && @user.authenticate(params[:user][:password])
            session[:user_id] = @user.id
            redirect '/recipes'
```

I have set my instance variable @user to ```User.find_by_username(params[:user][:username])```.  if @user is true and the authenticate method is true, where the params of user, password, then it stores in the session hash key :user_id the value @user.id. If the password returns true, then the user is successfully able to login, otherwise it returns false.

 "bcrypt" will be looking for this ```has_secure_password``` in order to function.  In order to secure the password, it will "salt" the password.  This sets the password to cryptographically strong random value and adds it to the input of the hash, regardless of whether the input was unique or not.  So if two users, Tom and Jerry, both have the password ```defeatNeme$i$```, when bcrypt salts them, they will not be the same once salted.  Once this salt is added to the password, it then is hashed to a hexadecimal value as an extra layer of security.

has_secure_password and bcrypt validate that :password exists, and also that in the user table, the presence of ```:password_digest```.

Whew.  That is a lot.  So has_secure_password from ActiveRecord::Base and bcrypt together help with the storing of passwords as the cookies and server (session) communicate to each other, helping provide protection and security to the user's password through a two step process, salting and hashing, where the user's password goes through multiple phases to be less readable and less patterned if anyone wants to try to get into their account information. And let's be real, why would anyone want to hack into this poor recipe database and mess up with some unforgettable recipes, like hot tea, water, and hot pie.




# Section 1 Prerequisites


## At a Glance
Before we dig into the nuts and bolts of Laravel, let's first zoom out and discuss what exactly happens when a request comes in.


## Install PHP, MySQL and Composer
Before we get started, you must first ensure that up-to-date versions of both PHP and MySQL are installed and available on your machine. In this episode, we'll review how to go about this. Once complete, we can then install Composer.


## The Laravel Installer
Now that we have Composer setup, we can pull in the Laravel installer and make it accessible globally on our machine. This allows you to run a single command to build a fresh Laravel installation: laravel new app.


## Laravel Valet Setup
If you're a Mac user, rather than running php artisan serve, you might instead choose to install Laravel Valet. Valet is a blazing fast development environment for Laravel that's a cinch to setup.



# Section 2 Routing


## Basic Routing and Views
When I learn a new framework, the first thing I do is figure out how the framework's default splash page is loaded. Let's work through it together. Our first stop is routes/web.php.


## Pass Request Data to Views
The request() helper function can be used to fetch data from any GET or POST request. In this episode, we'll learn how to fetch data from the query-string, pass it to a view, and then encode it to protected against potential XSS attacks.


## Route Wildcards
Often, you'll need to construct a route that accepts a wildcard value. For example, when viewing a specific post, part of the URI will need to be unique. In these cases, we can reach for a route wildcard.


## Routing to Controllers
It's neat that we can provide a closure to handle any route's logic, however, you might find that for more sizable projects, you'll almost always reach for a dedicated controller instead. Let's learn how in this lesson.



# Section 3 Database Access


## Setup a Database Connection
So far, we've been using a simple array as our data store. This isn't very realistic, so let's learn how to set up a database connection. In this episode, we'll discuss environment variables, configuration files, and the query builder.


## Hello Eloquent
In the previous episode, we used the query builder to fetch the relevant post from the database. However, there's a second option we should consider: Eloquent. Not only does an Eloquent class provide the same clean API for querying your database, but it's also the perfect place to store any appropriate business logic.


## Migrations 101
In a previous episode, we manually created a database table; however, this doesn't reflect the typical workflow you'll follow in your day-to-day coding. Instead, you'll more typically reach for migration classes. In this episode, we'll discuss what they are and why they're useful.


## Generate Multiple Files in a Single Command
It can quickly become tedious to generate all the various files you need. "Let's make a model, and now a migration, and now a controller." Instead, we can generate everything we need in a single command. I'll show you how in this episode.


## Business Logic
When possible, the code you write should reflect the manner in which you speak about the product in real life. For example, if you run a school and need a way for students to complete assignments, let's work those terms into the code. Perhaps you should have an Assignment model that includes a complete() method.



# Section 4 Views


## Layout Pages
If you review the welcome view that ships with Laravel, it contains the full HTML structure all the way up to the doctype. This is fine for a demo page, but in real life, you'll instead reach for layout files.


## Integrate a Site Template
Using the techniques you've learned in the last several episodes, let's integrate a free site template into our Laravel project, called SimpleWork.


## Set an Active Menu Link
In this episode, you'll learn how to detect and highlight the current page in your navigation bar. We can use the Request facade for this.


## Asset Compilation with Laravel Mix and webpack
Laravel provides a useful tool called Mix - a wrapper around webpack - to assist with asset bundling and compilation. In this episode, I'll show you the basic workflow you'll follow when working on your frontend.


## Render Dynamic Data
Let's next learn how to render dynamic data. The "about" page of the site template we're using contains a list of articles. Let's create a model for these, store some records in the database, and then render them dynamically on the page.


## Render Dynamic Data: Part 2
Let's finish up this exercise by creating a dedicated page for viewing a full article.


## Homework Solutions
Let's review the solution to the homework from the end of the previous episode. To display a list of articles, you'll need to create a matching route, a corresponding controller action, and the view to iterate over the articles and render them on the page.



# Section 5 Forms


## The Seven Restful Controller Actions
There are seven restful controller actions that you should become familiar with. In this episode, we'll review their names and when you would reach for them.


## Restful Routing
Now that you're familiar with resourceful controllers, let's switch back to the routing layer and review a RESTful approach for constructing URIs and communicating intent.


## Form Handling
Now that you understand resourceful controllers and HTTP verbs, let's build a form to persist a new article.


## Forms That Submit PUT Requests
Browsers, at the time of this writing, only recognize GET and POST request types. No problem, though; we can get around this limitation by passing a hidden input along with our request that signals to Laravel which HTTP verb we actually want. Let's review the basic workflow in this episode.


## Form Validation Essentials
Before we move on to cleaning up the controller, let's first take a moment to review form validation. At the moment, our controller doesn't care what the user types into each input. We assign each provided value to a property and attempt to throw it in the database. You should never do this. Remember: when dealing with user-provided data, assume that they're being malicious.



# Section 6 Controller Techniques


## Leverage Route Model Binding
So far. we've been manually fetching a record from the database using a wildcard from the URI. However, Laravel can perform this query for us automatically, thanks to route model binding.


## Reduce Duplication
Your next technique is to reduce duplication. If you review our currentArticlesController, we reference request keys in multiple places. Now as it turns out, there's a useful way to reduce this repetition considerably.


## Consider Named Routes
Named routes allow you to translate a URI into a variable. This way, if a route changes at some point down the road, all of your links will automatically update, due to the fact that they're referencing the named version of the route rather than the hardcoded path.



# Section 7 Eloquent


## Basic Eloquent Relationships
Let's now switch back to Eloquent and begin discussing relationships. For example, if I have a $user instance, how might I fetch all projects that were created by that user? Or if I instead have a $project instance, how would I fetch the user who manages that project?
For a deeper dive, please review the Eloquent Relationships Laracasts series.


## Understanding Foreign Keys and Database Factories
Let's put our learning from the previous episode to the test. If an article is associated with a user, then we need to add the necessary foreign key and relationship methods. As part of this, though, we'll also quickly review database factories and how useful they can be during the development and testing phase.


## Many to Many Relationships With Linking Tables
Next up, we have the slightly more confusing "many to many" relationship type. To illustrate this, we'll use the common example of articles and tags. As we'll quickly realize, a third table is necessary in order to associate one article with many tags, and one tag with many articles.


## Display All Tags Under Each Article
Now that we've learned how to construct many-to-many relationships, we can finally display all tags for each article on the page. Additionally, we can now filter all articles by tag.


## Attach and Validate Many-to-Many Inserts
We now understand how to fetch and display records from a linking table. Let's next learn how to perform inserts. We can leverage the attach() and detach() methods to insert one or many records at once. However, we should also perform the necessary validation to ensure that a malicious user doesn't sneak an invalid id.



# Section 8 Authentication


## Build a Registration System in Mere Minutes
Thanks to the first-party package, Laravel UI, you can easily scaffold a full registration system that includes sign ups, session handling, password resets, email confirmations, and more. And the best part is you can knock out this tedious and common requirement...in minutes.


## The Password Reset Flow
In this episode, we'll discuss the basic password reset flow. If a user forgets their password, a series of actions need to take place: they request a reset; we prepare a unique token and associate it with their account; we fire off an email to the user that contains a link back to our site; once clicked, we validate the token in the link against what is stored in the database; we allow the user to set a new password. Luckily, Laravel can handle this entire workflow for us automatically.



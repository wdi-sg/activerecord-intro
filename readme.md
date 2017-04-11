# Active Record

## Screen Casts for code
- [Part 1](https://youtu.be/wbAPxnuIXck)
- [Part 2](https://youtu.be/J9w1CDXrOXc)
- [Part 3](https://youtu.be/NQNBnmpG5tE)
- [Part 4](https://youtu.be/EkD5LEyALvM)
- [Part 5](https://youtu.be/VOH26fAzc4o)

## Learning Objectives

- Define the term `ORM` and why we use it over a database language.
- Explain what Active Record is and what problems it solves.
- Explain convention over configuration and how it relates to Active Record
- Define a class that inherits from AR
- Utilize AR to perform CRUD actions on a database
- Differentiate between class and instance methods in Active Record objects/classes
- Utilize `has_many`, `belongs_to` to establish associations/relationships with AR
- Seed a database using AR

## Framing (5 / 5)

So far, we've learned principles of object-oriented programming and how to get data to persist in a database using SQL.

Object-oriented principles allow us to represent and describe real-world things in our programs as data.

We now have a way to persist data in a database, although it's reasonable to feel that databases are a cryptic storage space on our local machines. We have to make super long SQL statements to do CRUD.

 Being limited to writing SQL queries through the command-line isn't ideal for the Ruby programs we are aiming to write. We need some way or tool to interact with the database in the context of our Ruby applications.

It'd be ***really*** nice if a bunch of genius programmers had already worked out some kind of way to interface between the database and our servers/applications in order to streamline the process of reading and writing data. Enter ORMs and [The Active record pattern](https://en.wikipedia.org/wiki/Active_record_pattern).



### Information Dive (5 / 10)

For the next 5 minutes, pair up and research what ORM's are.

[ORM - wikipedia](https://en.wikipedia.org/wiki/Object-relational_mapping)
> Don't spend too much time with this wikipedia article, just glance it over.

[AR Read 1.1 - 1.3](http://guides.rubyonrails.org/active_record_basics.html)

Try to answer these questions:

1. What is the Active record pattern in a nutshell?
2. At a high level, what are ORM's and how might they be useful?
3. What is the importance of interfacing the server with the database?

## ORM's & Active Record (10 / 25)

- *Official* wikipedia definition. A programming technique for converting data between incompatible type systems in object-oriented programming languages.

We need a way to encapsulate our databases into objects so that we can talk to our server. ORM's serve that purpose. Remember those tables we created in SQL? Well, it's an object represented on our server now. That's what ORM's do.

More concretely ORM's:
- *'Map'* (translate) objects to rows in our DB (and vice versa)
- **Conventions**:
  - 1 table per Model/Class/Entity
  - table name is model name pluralized
  - each column is an attribute for that model
- Table associations are handled using foreign keys

It just so happens you will be learning one of the best ORM's on the market. It has some of the best documentation and best syntax (because Ruby is awesome) the industry has to offer. This ORM is Active Record.

> Active Record is the M in MVC - the model - which is the layer of the system responsible for representing business data and logic. Active Record facilitates the creation and use of business objects whose data requires persistent storage to a database. It is an implementation of the Active Record pattern which itself is a description of an Object Relational Mapping system. (from AR docs)

## Active Record

Active Record is a library that allows us to translate database records into objects that we can use in our Ruby applications.

In order to use Active Record in our Ruby code to manipulate data in a database, we need to be able to talk about the **models** of our data.

But before we even do that, we have to decide on what our data is! How much of the real world are we going to attempt to represent in our programs? What data do our programs need to fulfill their purpose?  To be able to think about how to model our data, we need a process where we can precisely identify what we need represented in our programs as data.

Programmers are constantly modeling domains, real world, fictional, abstract, mathematical or otherwise.

<details>

<summary>What is Domain modeling and why do we do it?</summary>

> Domain modeling is the act of describing entities and their relationships in an application's data. This method is useful for deciding data what needs to be persisted.

</details><br>


When we data model, we tend to be talking about the **Nouns** in our
application. These are the names of the *tables* in our database and the names
of our Ruby *classes*.

Likewise when we write queries, we use **Verbs** to describe the specific data
we want.

Essentially, in order to store and retrieve information, a lot of what we do
today in Ruby will look like some form of the equation



> **Noun** + **Verb** = **Data**
>
> Class.method # Class (Noun) method (Verb)
>
> Taco.all # returned all Taco data!
>
> 🌮 🌮 🌮 🌮 🌮 🌮 🌮 🌮 🌮 🌮 🌮 🌮


With the help of Active Record, we can begin to write programs that follow this
simple pattern to manipulate data.


### Convention Over configuration (10 / 35)

Before we get started with code, let's highlight a reoccurring theme with Active Record and Rails in general. You'll often hear us say, "Convention Over Configuration." Before we discuss the concept as a class, take 30 seconds to think about what that phrase means--why *might* we prefer convention over configuration?

<details>
<summary><strong>Question:</strong>  Without getting into the specifics of AR, what do you think we mean by convention over configuration?</summary>
<br>

> Active Record and Rails, and other frameworks have a whole bunch of conventions that they follow so that you do not have to mess with different configuration details later. These conventions exist because developers arrive at a consensus on best practices. These road-tested conventions allow us to spend less time trying to configure when there already is an accepted way to do things. Thanks to the programmers who have come before us, we inherit a well-designed, default configuration that spares us from many headaches that we'd encounter if we were building things from scratch (yikes!).
>
> Some of the common ones we will encounter are naming conventions such as: plural vs single, Capitalized/ALL_CAPS_SNAKE_CASE/lowercase, camelCase/kabob-case/snake_case. Obeying the naming conventions in Active Record, particularly regarding what is singular vs. what is plural, saves you a good deal of headaches.

</details>

##### If you don't follow the conventions, you're going to have a bad time.

Obeying the naming conventions in Active Record saves you a good deal of headaches.

##### Alright! Let's get started with some code!

### You Do: Setup SQL - Tunr (10 / 45)

> [Tunr Deployed link](https://wdi-dc-tunr.herokuapp.com/artists)

Make sure to review our domain model for tunr.

![Tunr ERD](tunr-erd.png)

We want to be able to do CRUD for these models with Active Record. We'll be going into greater detail about how we are going to use Active Record as an interface between our server and our database, but to start, the first thing that we want to do is create/setup a database.

```bash
$ git clone git@github.com:ga-wdi-exercises/tunr-active-record.git
$ cd tunr-active-record
$ bundle install
```

<details>
<summary>Did you get an error from `bundle install`?</summary>
If you get an error message like this one:

```
An error occurred while installing json (1.8.3), and Bundler cannot continue.
Make sure that `gem install json -v '1.8.3'` succeeds before bundling.
```

Then run this command: `$ bundle update`
</details>

Continue with the following `bash` commands:

```bash
$ createdb tunr_db
$ psql -d tunr_db < db/schema.sql
$ psql -d tunr_db < db/seeds.sql
$ atom .
```


Check you did everything correctly.

- Run your program(`$ ruby app.rb`)
- When you see the `pry` REPL, run this ruby code: `Artist.first`

Your output should be something like this (it won't be the same letters and numbers next to `#<Artist:`)
```bash
pry(main)> pry(main)> Artist.first
=> #<Artist:0x007ff851821bc0
 id: 1,
 name: "Weird Al Yankovich",
 photo_url: "http://i.huffpost.com/gen/1952378/images/o-WEIRD-AL-facebook.jpg",
 nationality: "American">
```

**STOP**

### Code Review / Walkthrough - Tunr (I Do 10 / 45)

Let's do a quick walkthrough of our code base so far...

> The `app.rb` file is our main application file. This is where a lot of the main program logic will live.
>
> The `Gemfile` contains all the dependencies for our program.
>
> The `models/artist.rb` file will contain the class definition for the Artist class that will represent the artists table in SQL
>
> **Note**: by convention we always name our model file names singular

#### The `Gemfile` - an aside

A `Gemfile` is a file we create which is used for describing gem dependencies for Ruby programs. Allows for easier collaboration of apps. TLDR: Take someone's code, use it in your program.

We stand on the shoulders of giants. Oh there's code for that? yoink.

In the `Gemfile`:

```ruby
source 'https://rubygems.org'

gem "pg"  # this gem allows Ruby to talk to postgres
gem "activerecord"  # this gem provides a connection between your Ruby classes to relational database tables
gem "pry"  # this gem allows access to REPL
```

> Without these gems, we would be unable to talk to our SQL database.

#### Defining our Models

In the `models/artist.rb` file, we define our `artist` model:

```ruby
class Artist < ActiveRecord::Base
  # AR classes are singular and capitalized by convention
end
```

> In this Ruby file, we create a class of Artist that inherits from `ActiveRecord::Base`. Essentially, when we inherit from `ActiveRecord::Base`, it gives this class a whole bunch of functionality that maps the Ruby `Artist` class to the `artists` table in postgres.

#### Connecting to Postgres

Now, in order to connect our application to our database we have `db/connection.rb` file:

```ruby
ActiveRecord::Base.establish_connection(
  :adapter => "postgresql",
  :database => "tunr_db"
)
```

#### Wiring the application together

Finally, we have our `app.rb` file(which in this case is just dropping us into pry):

```ruby
require "bundler/setup" # require all the gems we'll be using for this app from the Gemfile. Obviates the need for `bundle exec`
require "pg" # postgres db library
require "active_record" # the ORM
require "pry" # for debugging

require_relative "db/connection" # require the db connection file that connects us to PSQL, prior to loading models
require_relative "models/artist" # require the Artist class definition that we defined in the models/artist.rb file


# This will put us into a state of the pry REPL, in which we've established a connection
# with the tunr_db database and have defined the Artist Class as an
# ActiveRecord::Base class.
binding.pry

puts "end of application"
```

> note the difference between `require` and `require_relative`. With `require` we are getting gems and `require_relative` we are getting files relative to the location of the file we wrote `require_relative` in

**STOP**

## Break (10 / 60)

### Methods - Tunr (I Do 15 / 75)

Great! We've got everything done that we need to get setup with single model CRUD in our application. Let's run it in the terminal:

```bash
$ ruby app.rb
```

When we run this app, we can see that it drops us into pry. Let's write some code in pry to update our database... **IN REALTIME!!!**

We'll create an instance of the `Artist` object on the Ruby side:

> **Note** the syntax for creating a new instance.

```ruby
kanye = Artist.new(name: "Kanye West", nationality: "American")
```

To save our instance to the database we use `.save`:

```ruby
kanye.save
```

If we want to initialize an instance of an object AND save it to the database we use `.create`:

```ruby
elvis = Artist.create(name: "Elvis Presley", nationality: "American")
```

<!-- SQL for create  -->
<details>
<summary>This would roughly translate to `SQL` code</summary>

<code>
INSERT INTO artists (name, nationality) VALUES ('Elvis Presley', 'American');
</code>

</details>

<br>

**(ST-WG)** Why is there a distinction between when it's saved in one command versus two?

One really handy feature we get from an Active Record inherited class is that all of the attribute columns of our model have `attr_accessor`'s as well. So we can do things like:

```ruby
kanye.name
# gets the name of kanye
kanye.name = "Yeezus"
# sets name to "Yeezus"
kanye.save
# saves the model to the database
```

> **Note:** Should be noted that when we set the attribute using a setter, it doesn't automatically save to the database, it's not until we call `.save` on the object that it saves to the database

To get all of the songs we use `.all`:

```ruby
Artist.all
```

We can also find a song by its ID using `.find`:

```ruby
Artist.find(1)
```

<!-- SQL for find  -->
<details>
<summary>This would roughly translate to `SQL` code</summary>
<code>
SELECT * FROM artists WHERE id = 1 LIMIT 1;
</code>
</details>

<br>
Additionally we can also find a song by an attribute using `.find_by`:

```ruby
Artist.find_by(name: "Elvis Presley")
```

> Note that find_by only returns the first object that meets the requirements of the arguments

If you want all songs that meet a certain query then we use `.where`:

```ruby
Artist.where(nationality: "American")
```

We can also update attributes and save at the same time using the `.update` method:

```ruby
elvis = Artist.find_by(name: "Elvis Presley")
elvis.update(nationality: "Canadian")
```

> If we want to update the attribute and not save we would have to use something from this [post](http://www.davidverhasselt.com/set-attributes-in-activerecord/)

Finally we can also just destroy/delete rows in our database with the `.destroy` method:

```ruby
kanye = Artist.find_by(name: "Yeezus")
kanye.destroy
# goodbye kanye you're gone forever
```

> This is exciting stuff by the way, imagine, while we do these things, that our artists model is instead a post on Facebook, or a comment on Facebook. So the next time you comment on someone's Facebook page you have an idea now of whats happening on the database layer. Maybe not the whole picture, but you have an idea. We're going to build on that idea in the coming week and half, and thats really exciting.

### You Do: Methods - Tunr ( 15 / 90)

We will use `binding.pry` in your ruby app to test out ActiveRecord class and instance methods.

> Don't forget to require 'pry' when you want to use binding.pry in your program.

[Part 1.1 - Use Your Artist Model](https://github.com/ga-wdi-exercises/tunr-active-record#part-11---use-your-artist-model)

## Associations

### Reframing (10 / 100)

<details>
<summary><strong>Q</strong>. We have a lot of choice when it comes to databases, why are we using SQL?</summary>

<br>


> We use SQL because it is a <strong>relational</strong> database. But what does that really mean? Basically we want the ability to associate models in our domain. That can come in a variety of ways in a relational database, but at the heart of it is essentially this: One model has many other instances of another model. And that other model belongs to the original.


</details>

<br>
Let's take a look at an example in the wild.

With the application Facebook, there are many users. Each of these users have several models associated with them. Let's look at one more closely. We'll be looking at posts(status updates)

The first model we'll be looking at is Users. The second is Posts.

A User has many Posts
And every Post belongs to a certain user.

> Note the plurality of the nouns used in these two sentences

When we start organizing our objects in this manner and program these associations, it becomes much easier to query our database for what we need. If we were on Adrian's  Facebook page, we'd see all his posts. These are coming from some database. For that database, it wouldn't make sense for it to query EVERY post in Facebook and then check if going through every Facebook post ever and seeing if each post's user was the user whose page you have open.

The fact that a bunch of posts were associated with Adrian's account, means that the entire database doesn't have to be queried. The database only has to pull information **related** to one account. **Relating** the 'categories' of things that go in the database is just a way of **structuring** or **organizing** the data in such a way that it is very useful to a program.

Let's see what some of this stuff looks like in code. We're going to be adding an artist model to our program.

### Associations in Schema - Tunr (I Do - 5 / 105)

**NOTE:** In this section, we are reviewing our schema and how it reflects associations for our domain. We are NOT updating the schema file.

Looking back at our schema, in `schema.sql`:

```sql
DROP TABLE IF EXISTS songs;
DROP TABLE IF EXISTS artists;

CREATE TABLE artists(
  id SERIAL PRIMARY KEY,
  name TEXT,
  photo_url TEXT,
  nationality TEXT
);

CREATE TABLE songs(
  id SERIAL PRIMARY KEY,
  title TEXT,
  album TEXT,
  preview_url TEXT,
  artist_id INT
);
```

Make note of the foreign key in `songs`

### Updating Class Definitions - Tunr (I Do - 5 / 110)

Next we need to update the models to reflect the relationships in our application.

```ruby
class Artist < ActiveRecord::Base
  has_many :songs
end
```

Now In our `models/song.rb` we have to reflect the association:

```ruby
class Song < ActiveRecord::Base
  belongs_to :artist
end
```
> note the plurality of `songs` and singularity of `artist`.  

We also need to include the `models/song.rb` file into our `app.rb` so in `app.rb` we need to add

```ruby
require_relative "models/song"
```

### You Do: Updating Class Definitions - Tunr (5 / 115)

[Part 1.2 - Create Your Song Model / Setup Associations](https://github.com/ga-wdi-exercises/tunr-active-record#part-12---create-your-song-model--setup-associations)

[solution code](https://github.com/ga-wdi-exercises/tunr-active-record/archive/v1.2.zip)

### Association Helper Methods - Tunr (I Do - 10 / 125)

So we added some code, but we can't yet see the functionality it gives us.

Basically when we added those two lines of code `has_many :songs` `belongs_to :artist` we created some helper methods that allow us to query the database more effectively.

Lets create some objects in our `app.rb` so we can see what were talking about:

```ruby
Song.destroy_all
Artist.destroy_all

ratatat = Artist.create(name: "Ratatat", nationality: "American")
beatles = Artist.create(name: "The Beatles", nationality: "British")

# NOTE: you can pass an array to create for bulk creation
Song.create([
    {title: "Wildcat", album: "Classics", artist: ratatat },
    {title: "Loud Pipes", album: "Classics", artist: ratatat },
    {title: "Neckbrace", album: "LP4", artist: ratatat },
    {title: "Twist and Shout", album: "Please Please Me", artist: beatles},
    {title: "Hello, Goodbye", album: "Magical Mystery Tour", artist: beatles},
    {title: "Revolution", album: "The Beatles", artist: beatles},
  ])
```

> ST-WG Why do we need to use `.destroy_all`?

Now that we have this association, we can now easily query the database for the relevant records.

If we want to get all of The Beatles' songs or set The Beatles' songs, we can now write this code:

```ruby
beatles = Artist.find_by(name: "The Beatles")
beatles.songs
# will return all songs that belong to The Beatles, this is .songs being used as a getter method

beatles.songs = [Song.first, Song.last]
# this is .songs being used as a setter method
```
> note that when songs is being used as a setter method above, it actually changes the artist_id column for those songs to match The Beatles' primary ID. Any song that previously was assigned to The Beatles and not reassigned in the setter will now have an artist_id of nil

Alternatively if wewanted to get a song's artist wecould write this code:

```ruby
loud_pipes = Song.find_by(title: "Loud Pipes")
loud_pipes.artist
# will return loud_pipes' artist, this is .artist being used as a getter method

beatles = Artist.last
loud_pipes.artist = beatles
loud_pipes.save
# this .artist being used as a setter method, and now loud_pipes's artist is Adrian
```

We can also create new songs under a certain artist by doing the following:

```ruby
beatles.songs.create(title: "Hey Jude", album: "Beatles Chillout (Vol. 1)")
# this will create a new song that belongs to the Artist object named beatles.
```
> **Note** that we did not pass in an artist id above. Active Record is smart and does that for us.

### You Do: Association Helper Methods - Tunr (10 / 135)

[Part 1.3 - Use Your Model Assocations](https://github.com/ga-wdi-exercises/tunr-active-record#part-13---use-your-model-associations)

### Seeding a Database - Tunr (I Do - 10 / 145)
Seeding a database is not all that different from the things we've been doing today. What's the purpose of seed data? **(ST-WG)**

We want some sort of data in our database so that we can test our applications. Let's create a seed file in the terminal: `$ touch db/seeds.rb`

In our `db/seeds.rb` file, let's put the following code:

```ruby
require "bundler/setup" # require all the gems we'll be using for this app from the Gemfile. Obviates the need for `bundle exec`

require "pg"
require "active_record"
require "pry"

require_relative "../models/song"
require_relative "../models/artist"

require_relative "../db/connection.rb"


Artist.destroy_all
Song.destroy_all
# destroys existing data in database

chili_peppers = Artist.create(name: "Red Hot Chili Peppers", nationality: "American")
led = Artist.create(name: "Led Zeppelin", nationality: "British")

chili_peppers.songs.create([
    {title: "Can't Stop", album: "By The Way" },
    {title: "Scar Tissue", album: "Californication" },
    {title: "Californication", album: "Californication" },
    {title: "Dani California", album: "Stadium Arcadium" },
    {title: "Dark Necessities", album: "The Getaway"}
  ])

led.songs.create([
    {title: "Whola Lotta Love", album: "Led Zeppelin II" },
    {title: "Stairway to Heaven", album: "Led Zeppelin IV" },
    {title: "Kashmir", album: "Physical Graffiti" },
    {title: "Black Dog", album: "Led Zeppelin IV" },
    {title: "All My Love", album: "In Through the Out Door"}
    ])
```

Once we get rid of this duplicate CRUD code in `app.rb` we can just run this seed file once and know our data is good.

Now when we run our application with `ruby app.rb`, we enter into Pry with all our data loaded.

## Closing (5 / 150)

Who misses writing SQL queries by hand? Exactly. Active Record is extremely powerful and helpful, and allows us to easily interface with the business models for our applications.

Review Learning Objectives


### Resources
- [Active Record Basics](http://guides.rubyonrails.org/active_record_basics.html)
- [Active Record Query Interface](http://guides.rubyonrails.org/active_record_querying.html)

### Appendix

#### Instance vs Class Methods
| method              | instance | class    |
|---------------------|:--------:|:--------:|
| .new                |          |     x    |
| .save               |     x    |          |
| .create             |          |     x    |
| attribute accessors |     x    |          |
| .all                |          |     x    |
| .find               |          |     x    |
| .find_by            |          |     x    |
| .where              |          |     x    |
| .update             |     x    |          |
| .destroy            |     x    |          |
| .destroy_all        |          |     x    |

#### Conventions in AR
|description      | Rule    |
|-----------------|---------|
|table names      |snake case and plural|
|model file names |snake case and singular|
|model definition |Camel case and singular|
|argument for has_many| snake case and plural|
|argument for belongs_to| snake case and singular|
|more to follow....|||

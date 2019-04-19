---
title: "Install"
date: 2019-04-18T23:38:39-04:00
draft: true
---

<h2> by Matthew Barlowe</h2>
<br />
<p>Ok this is the first in a series of SQL tutorials I will be releasing. This tutorial will
cover the basics of getting Postgres setup on your computer and running. We won't actually
be working with in data in this article, but instead the focus is on the nuts and bolts
of what Postgres is and and its structure. But before I get into all that let's start
at the beginning. As always these tutorials will be focused on working in a Mac OS X
environment. The Postgres commands and the SQL syntax should work no matter what, but
the details of installing will definitely be different on a Windows system.</p>
<h2>What is SQL?</h2>
<p>SQL, or sequel as its commonly pronounced, is bascially an acronym that stands for
Structured Querying Language. SQL has been around forever since 1974 and isn't anywhere
near cutting edge in technology. Some may see this as a drawback, but it also has its
advantages as well. One being that it's a well documented system and language, and two
is that most people are at least somewhat familiar with its terminology.</p>
<p>But what does SQL do? Well SQL is the language used to query and manipulate data
in a relational database system. And a relational database system is a system that uses
the relational model; a model consisting of unique keys to create
relations between different tables on the database. Its not really important you
understand this all right now but just keep it in mind for future tutorials.</p>
<h2>What is Postgres?</h2>
<p>Postgres is just a flavor of SQL. I chose this one for several reasons. One being
it is noted for its speed, and secondly because it is open source and therefore free.
There are plenty other choices out there such as MySQL or SQLite3 that are perfectly
suitable, but these tutorials will focus on Postgres as it is the one I am most comfortable
using. There are differences between these options, but the basic commands we
wil go over in this series should be universal between all of these systems, however I can't
speak for all of them as there may be slight differences in syntax.</p>
<p>Still the majority of the lessons learned using Postgres and these tutorials will
be portable. Especially the ideas behind <code>JOIN</code> and how keys create
relations between tables.</p>
<h2>Installing Postgres</h2>
<p>To get Postgres onto our system we are going to use Homebrew to install the files. If
you don't have Homebrew please refer to this <a href='homebrew.html'>tutorial</a> to get
it up and running. So like our other brew installations, Postgres is installed with a
command you may find familiar if you've been following these tutorials.</p>
<pre><code class='Command Line'>
$ brew install postgresql
</code></pre>
<p>And you'll see a whole bunch of text on the screen showing Homebrew installing the
Postgres files. It's often a good idea to skim this output even if you don't understand
it as sometimes the output will have very important info. When installing Postgres this
is one of those times. Because at the bottom it will give you two commands: <code>pg_ctl
-D /usr/local/var/postgres start</code> and <code>brew services start postgresql</code>
<p>There are two major difference between these commands the first one will just start up
the postgres server manually. If you ever shut down the computer then you will have to
start the server again. The second one will keep Postgres running in the background at all
times. This is the option I use, but depending on your computer it may not be best for you.
If you just want to manually start and stop the server you'd use the first command above
to start it and this one to stop it: <code>pg_ctl -D /usr/local/var/postgres stop</code>.
So choose whichever option you'd like and start up your new Postgres server!
<h2>Logging in to Postgres</h2>
<p>Ok now that your server is up and running you'll need to login in to the Postgres server
before you can do anything:</p>
<pre><code class='Command Line'>
$ psql postgres
</code></pre>
<p>Ok the psql command is what logs you into the Postgres database server and postgres is
a default database that is created whenever you create/install the Postgres server itself.
This database is used by third party programs and it will be the database you connect to
run database maintenance commands.</p>
<p>So after you type the above command you should see that your prompt changes to something
like this:</p>
    psql (10.3, server 10.1)
    Type "help" for help.

    postgres=#
<p>A couple notes about background things before we go further. When you brew postgres
brew does a couple things behind the scenes. It tells you in the text it prints out, but
I'm going to highlight them here to make it easier for you. First Homebrew creates a superuser
for the Postgres server with the same name as the OS X User you are currently logged in as.</p>
<p>Secondly it sets trust authentication for local connections meaning you don't need to provide a
password to log into the data base. Obviously, this is a huge security flaw and if you were
going to provide outside access you would change that, but for our purposes in these tutorials
we won't get into how to do that.</p>
<p>Ok back to the prompt above. As you can see it tells us what version Postgres we are running,
and that we can type help for help. But if you're like me, I find when I'm learning something new
those help menus offer no help at all until I get a little knowledge about the system. The next
line is where we'll actually put our commands for the Postgres server. That line tells us we
are connected to the <code>postgres</code> database and the '#' informs you that you are logged
in as a superuser.</p>
<p>As with my rant in other tutorials about using <code>sudo</code> unless you know
what you are doing don't work as a superuser if you can avoid it. So we are going to create
another user and give them database creation privileges:</p>

    postgres=# CREATE ROLE username WITH LOGIN PASSWORD 'quoted password';

<p>You would change <code>username</code> and <code>'quoted password'</code> with whatever values
you wanted. Next will give our new user the ability to create a database:</p>

    postgres=# ALTER ROLE username CREATEDB;

<h2>Creating Your First Database</h2>
<p>Ok now that we've created our new user lets logout of the superuser with <code>\q</code> and
log in with your new user with this command <code>psql -U username postgres</code>. The <code>-U</code>
flag just tells postgres that you want to login with that particular user and <code>postgres</code>
is again just the name of the database. Normally you would be asked for the password you created
for the role, but remember that local connections are trusted so you won't need to enter it.
<p>Ok now you should see a command line that looks like this:</p>

    psql (10.3, server 10.1)
    Type "help" for help.

    postgres=>

<p>As you can see our '#' has changed to a '>' indciating that we are not a superuser anymore. So now you're
logged in to your new user lets create a database that we are going to load our data in to so we can
play around with it:</p>

    postgres=> CREATE DATABASE baseballstats;

<p>At this moment I'll point out, if you haven't noticed, that all SQL commands and queries will end with
a semicolon. This let's the system know that it has reached the end of the query and there are no more
words to parse. If you hit enter without a semicolon a new terminal will pop up with the '=' changed
to a '-'. This means that Postgres is expecting more commands and it will wait until you pass it a semicolon.
This is helpful if you are working with complex queries and you can break them up to make them easier to read,
but can be annoying if you are wondering why your commands aren't working.</p>
<p>So to check and make sure our database is created type <code>\l</code> into the terminal.
This command will give you a list of all the databases on the Postgres server. Commands
beginning with a '\', also known as psql commands, don't need a semicolon and will throw and error
if you use one. It is a little confusing but one way to keep it apart is that commands that directly
deal with the data itself need semicolons and commands that tell you info about the server don't. You
should see some output similar to this:</p>

                                           List of databases
         Name      |    Owner    | Encoding |   Collate   |    Ctype    |      Access privileges
    ---------------+-------------+----------+-------------+-------------+-----------------------------
     MattBarlowe   | MattBarlowe | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
     baseballstats | MattBarlowe | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
     postgres      | MattBarlowe | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
     template0     | MattBarlowe | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/MattBarlowe             +
                   |             |          |             |             | MattBarlowe=CTc/MattBarlowe
     template1     | MattBarlowe | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/MattBarlowe             +
                   |             |          |             |             | MattBarlowe=CTc/MattBarlowe

<p>You should see the <code>baseballstats</code> in the table and that the Owner of the database is
the username you created. As `Owner` of the database that means you have superuser powers over it and
that nobody can even read it until you give them acess to the database.
<h2>Importing Data to the Database</h2>
<p>So we have created our new <code>baseballstats</code> database so lets log into it. Start by typing <code>\q</code>
to log out of the <code>postgres</code> database and now we'll use this command to log into our new database
<code>psql -U username baseballstats</code>, and you'll see your command prompt is now this:</p>

    baseballstats=>

<p>Ok so we have our database running but we don't have any data in yet so that's our next step. Data in a SQL
database is stored in different tables. And it is the relationships between these tables that forms the basis
of the relational database system I touched on earlier. Before we can import data we have to create a table.
A table can kind of be thought of as similar to dataframes if you are familiar with Pandas or R, or a giant Excel
spreadsheet if you are not. Again this is a simplified comparison but for the purposes of this tutorial
they can essentially be thought of as the same.</p>
<p>And like a dataframe, or spreadsheet, a table has rows and columns. Unlike a dataframe/spreadsheet, you can't
just import data in and the SQL server will figure it out. You have to create what's called a Table Schema. I just
think of Schema as short for Schematic, which means that its a framework for the data we're about to import. In SQL
you have to declare the columns in the table and the type of data that goes into it. There is text, or char, for
strings and such like names, or category labels. The other two will be using for this tutorial is int for
integers and real for real numbers.</p>
<p>So the data we will be preparing the table for is the data I just recently uploaded to this
<a href='https://drive.google.com/open?id=1m1ouTIYh4x6yUMIPW3ioVtkKBU6Ky4DN'>link</a>. In this zip file
there are different files representing different backups of my databases. I'll be using the baseballstats file. You can
add more files to the database using these methods to get more seasons if you want more data in your database. You
won't have to repeat creating the table schema though, you just will need to import new data from csv files.
<p>Now to create our new table we'll use the <code>CREATE TABLE</code>command and pass it our defined columns inside
parenthesis as seen below. This is how you would create any table you wanted you would just make sure to setup
the same number of columns and the right type of data in each column.</p>

    CREATE TABLE baseballstats(
    );

<p>So just cut and paste that at your postgres command line and hit enter and it should say <code>CREATE TABLE</code>
and just to double check we'll type <code>\dt</code> the command to list all the tables on the database and this
should be the output:</p>

           List of relations
    Schema |    Name     | Type  | Owner
    --------+-------------+-------+-------
    public | baseballstats | table | matt1

<p>So now we have a baseballstats table in our baseballstats database, but we are still missing data. To add the data
we will be using the <code>\copy</code> command. You can also use the <code>COPY</code> command but Postgres
won't allow that unless you are logged in as the super user; however the <code>\copy</code> is available to all
users. This is our next command:</p>
<pre><code class='Command Line'>
baseballstats=> \copy baseballstats FROM 'file_path' DELIMITER '|' CSV HEADER;
</code></pre>
<p>So lets break down this command a bit. We are <code>\copy</code>ing to our table we created in the last section,
<code>baseballstats</code>. The <code>DELIMITER</code> tells Postgres what delimiter is seperating the values. Often
it is commas however if you are using the data from my data dump it will always be pipe, '|', characters to avoid
issues when commas themselves are actually in the data. And the last part, <code>CSV HEADER</code> lets Postgres
know there is a header line at the top of the file and to skip it. So after you run that command this should be
your output:</p>
<pre><code class='Command Line'>
COPY 176191
</code></pre>
<p>And know lets run one more check to make sure our data is in there and run this command:</p>
<pre><code class='Command Line'>
SELECT * FROM baseballstats LIMIT 1;
</code></pre>
<p>Which should produce output similar to this:</p>
<pre><code class='Command Line'>
erstats=> select * from baseballstats limit 1;
player      | team | toi  | cf | ca | c_plus_minus | ff | fa |   xgf   |   xga   | gf | ga | g | a1 | a2 |   ixg    |  cf60   |  ca60   | cf_percent |  ff60   |  fa60   | ff_percent | gf60 |  ga60   | gf_percent |  xgf60  |  xga60  | xgf_percent | g60 | a160 | a260 | p60 |  ixg60   |  game_id   | game_date  |  season  | session |                   db_key
-----------------+------+------+----+----+--------------+----+----+---------+---------+----+----+---+----+----+----------+---------+---------+------------+---------+---------+------------+------+---------+------------+---------+---------+-------------+-----+------+------+-----+----------+------------+------------+----------+---------+---------------------------------------------
ANDREJ.MESZAROS | BUF  | 19.4 | 21 | 31 |          -10 | 18 | 25 | 1.00917 | 1.33976 |  0 |  1 | 0 |  0 |  0 | 0.122063 | 64.9485 | 95.8763 |    40.3846 | 55.6701 | 77.3196 |    41.8605 |    0 | 3.09278 |          0 | 3.12115 | 4.14358 |     42.9631 |   0 |    0 |    0 |   0 | 0.377513 | 2014020099 | 2014-10-23 | 20142015 | R       | ANDREJ.MESZAROS2014-10-23201402009920142015
(1 row)
</code></pre>
<p> Thats a lot text and its hard to make sense of, but if you are seeing that means we have great success
and your first database containing all the player stats from the 2018 season broken down by games is
up and running. If you've made it this far then your are doing a fantastic job. I know it was a lot,
it was a lot for me to write it. So go take a break and celebrate and we'll come back with our next
tutorial covering basic SQL commands to shape all that data into something you can use.</p>
<h2>Sources:</h2>
<p><a href='https://www.codementor.io/engineerapart/getting-started-with-postgresql-on-mac-osx-are8jcopb'>Getting Started with PostgreSQL on Mac OSX</a></p>
<p><a href='http://blog.bradlucas.com/posts/2017-10-06-install-postgresql-on-a-mac-using-brew/'>Install PostgreSQL on Mac using Brew</a></p>
<p><a href='https://stackoverflow.com/questions/7975556/how-to-start-postgresql-server-on-mac-os-x'>How to Start PostgreSQL Server on Mac OSX</a></p>
<p><a href='https://gist.github.com/nepsilon/f2937fe10fe8b0efc0cc'>Importing and Exporting CSV Files with PostgreSQL</a></p>
<p><a href='https://en.wikipedia.org/wiki/Relational_model'>Relational Model</a></p>
<p><a href='https://gist.github.com/Kartones/dd3ff5ec5ea238d4c546'>psql Cheatsheet</a> I would definitely bookmark this folks!</p>
<p><a href='https://stackoverflow.com/questions/2370525/default-database-named-postgres-on-postgresql-server'>Default Postgres Database</a></p>
<p><a href='https://gist.github.com/lxneng/741932'>Postgres Homebrew Output</a></p>
<p><a href='https://www.youtube.com/watch?v=2OA2lLRe70Q'>Create tables and import CSV</a></p>

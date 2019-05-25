---
title: "Postgres Remote Login"
date: 2019-05-23T22:25:11-04:00
draft: false
---

<h2>Remote Postgres Login</h2>

<br />
<p>Ok if you've been following along with the other tutorials you have seen me
use the <code>psql</code> command to login into your database that you've setup
on your computer. But that's not all <code>psql</code> is use for, and in fact it
has many diverse properties that you can use to pass SQL queries to the database
without ever logging into the server itself. But I won't get into that with this
tutorial, instead I'm going to focus on using psql to login into a remote Postgres
server</p>
<p>A couple notes here first about OS differences. If you are on a Mac which I
assume most people will be if you're following these tutorials, the psql will work
anywhere at the command line. Windows though is a bit different. If you have your
<code>cmd</code> window up and ready to log into your Postgres server, you will need
to change to the <code>bin</code> directory located in the directory of your Postgres
to run the <code>psql</code> command.</p>
<h2>Login Syntax</h2>

    psql --host=<hostname> --port=5432 --username=<username> --dbname=<dbname>

<p>Ok this is a sample command you would type in the command line if you were going
to login in to a remote Postgres server. Let's break it down piece by piece.
<code>--host=&lt;hostname&gt;</code>: This is the address of the postgres server. It
could be an IP address, or a url, or it's hosted on amazon it will have something
that ends like this <code>rds.amazonaws.com</code>. This what tells <code>psql</code>
where to go and connect so you can login to the Postgres server.</p>
<p><code>--port=5432</code>: This is the port that <code>psql</code> will attempt to
connect. This is the default port for Postgres and will almost always be the same no
matter what Postgres server you are connecting to and you shouldn't have to change
this unless otherwise specified.</p>
<p><code>--username=&lt;username&gt;</code>: This one is pretty self explanatory. You will
place your user name where &lt;username&gt; goes and it will be the username given
to you by your database administrator</p>
<p><code>--dbname=&lt;dbname&gt;</code>: This will be the name of the database you
want to connect to and you will need to know this before you connect because you can't
connect to a Postgres server without specifying a database. This should also be
given to you by your database administrator as well.</p>
<p>Ok that's basically it for using <code>psql</code> to log into a Postgres server.
But what if you don't want to type all that in everytime you want to log into
a Postgres server? Well glad you asked because I'll briefly touch on how you can
do that.</p>
<h2>Aliases</h2>
<p>Ok I talked about aliases in some other SQL tutorials and this is similar but
not quite the same. The alias I'm going to talk about right now is an alias for
a command for the Bash shell in Mac OS X. This alias is a shortcut that you create
so you don't have to type in long complicated commands. To create a shortcut
we will be editing the <code>.bash_profile</code> file with a text editor.</p>
<p>The '.' before a file in a Unix/Linux file structure means that it is a hidden
file and won't normally show up when you do a standard <code>ls</code> command. The
<code>.bash_profile</code> file controls how the Bash shell operates for you. I discussed
editing it briefly in the <a href='gittutorial.html'>Git Tutorial</a> so if you
forgot how to edit it you can refresh your memory there. But to create our alias
you'll need to open the file and put this line inside it:</p>
<pre><code class='Command Line'>
alias dblogin='psql --host=&lt;host_name&gt; --port=5432 --username=&lt;username&gt; --dbname=&lt;dbname&gt;'
</code></pre>
<p>So after you add that all on one line to your <code>.bash_profile</code> file and
<code>source ~/.bash_profile</code> to effect the changes to your shell all you would need
to do is type <code>dblogin</code> and the Bash shell would automatically log you
into your Postgres server with the selected database. And that's all it takes to create
the alias. Aliases can be used for any series of Bash commands not just the psql so
if you run into another command that requires a lot of inputs, or even a chain of different
commands now you know how to set up an alias to make it easier to type into the
command line</p>

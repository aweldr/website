---
title: "Creating a New Postgres User"
date: 2019-05-24T21:33:42-04:00
draft: false
---

<p>Ok so if you've come to this point you now have your database up and
running and you want to allow others to access it. Some people say
it's not the brightest idea, but hey, this data ain't going to share
itself. Obviously letting them use your account is out of the question
because as a superuser that will just allow them to wreak havoc any time
they want on your meticulously gathered data. That's were new users come
in to save the day</p>
<p>Postgres users work pretty much the same way as Mac OSX users. You have
a username and a password, and each user is given rules and limitations
on what they can do. I'll get more into what privileges are available
for a Postgres user in the next tutorial, but for now just keep in mind
that they are the rules that let the database administrator know what
each individual user can do on the Postgres server</p>
<h2>Difference Between a User and a Role</h2>
<p>In older versions of Postgres a User and a Role used to be more
seperately defined. But from version 8.0 onward a User has been defined
as a subset of a Role, with the main difference being that when you create
a User Postgres assumes that it has login privileges to the Postgres
sever. If you create a role it does not have login privileges unless you
specifically define those privileges.</p>
<p>Gennerally what I like to do is create users and then create roles that
I assign to each individual user to manage their privileges of what databases
they can connect to and what actions they can take on each database.</p>
<p>Yes it is a bit confusing I understand, but I'll go over how I generally
do things that helps keep it seperate for me. So ok let's look at it in practice
and see if that makes things a little clearer.</p>
<h2>Creating Your First User</h2>
<p>Ok so you need to add one user to your Postgres server then this would be the
syntax:</p>
<pre><code class='Command Line'>
CREATE USER matt WITH PASSWORD 'password';
</code></pre>
<p>Ok so with that you created one user with the user name <code>matt</code>
and the password <code>password</code>. Obviously, that's an awful password
and don't ever do that unless you want to have a bad time. The good news is
you created your first user, the bad news is they can't do anything yet other
than login to the Postgres server.</p>
<p>If you ever lose track of the Users, or Roles, you've created on your
Postgres server all you need to do is type <code>\du</code> and Postgres
will show you a table of all the roles available on the server.</p>
<p>But what good is a user if they can't do anything? No good if you ask me
that's why I'll show you how to grant them <code>SELECT</code> privileges
to a table so they can at least access your data. Here is the syntax to do
that:</p>
<pre><code class='Command Line'>
GRANT SELECT ON ALL TABLES IN SCHEMA public TO matt;
</code></pre>
<p>So now the user <code>matt</code> has the ability to <code>SELECT</code>
query all the tables that have the schema public. If you are just starting
out with postgres most of your tables will use the public schema as that is
the default choice. This also raises another issue as well with your new
user. Since they have access to the public schema by default on Postgres
that means they have <code>USAGE</code> or read permissions and <code>CREATE</code>
which means they can create new tables on your database.</p>
<p>Obviously as a superuser you could just delete what they create, but
it's better to just not allow it in the first place. This means you
need to revoke that privilege which is done with this syntax:</p>
<pre><code class='Command Line'>
REVOKE CREATE ON SCHEMA public FROM PUBLIC;
</code></pre>
<p>This removes the ablity for your new user to create a new table on the
database using the public schema. This is generally a good idea as a user
that creates a table becomes an onwer of that table; meaning they can insert
whatever data they want to the data and grant other users privileges to it
as well. You always want to make sure that your users can only do what you
want them to do. If you're fine with them creating new tables then ignore this,
but most likely you won't be.</p>
<h2>Sources</h2>
<p><a href='https://www.postgresql.org/docs/current/static/sql-createuser.html'>Postgres Create Users Docs</a></p>
<p><a href='https://www.postgresql.org/docs/current/static/sql-createrole.html'>Postgres Create Role Docs</a></p>
<p><a href='https://serverfault.com/questions/60508/grant-select-to-all-tables-in-postgresql'>Grant SELECT on all Tables</a></p>
<p><a href='https://dba.stackexchange.com/questions/35316/why-is-a-new-user-allowed-to-create-a-table'>Why is new user allowed to create a table</a></p>

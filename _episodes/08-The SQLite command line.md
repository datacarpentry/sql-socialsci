---
title: "The SQLite command line"
teaching: 0
exercises: 0
questions:
- "How can I save my code in a file and run it again?"
objectives:
- "Use the SQLite shell to re-run a file of SQL code"

- "Save the output from the SQLite shell to a file"

keypoints:
- "SQLite databases can be created, managed and queried from the SQLite shell utility"
- "You can run the shell interactively from the commandline, typing queries or dot cammands at the prompt"
- "You can call the SQLite3 program and specify a database and a set of commands to run. This aids automation"
---

## Running SQL code using the SQLite shell

Before you can run the SQLite3 shell program you must have installed it. Instructions for doing this are included in the xxxx| here.

I will assume that you have added the location of the program to your local PATH environment variable as this will make it easier to refer to the database file and other files we may want to use.

1. Open a command prompt and 'cd' to the folder location of the SN7577.sqlite database file.
2. run the command 'sqlite3' This should open the SQLite shell and present a screen similar to that below.

![SQLite shell](../fig/SQL_08_SQLite_shell.png)

3. By default a "transient in-memory database" is opened. You can change the database by use of the .open command

.open SN7577.sqlite

It is imprtant to remember the .sqlite suffix, otherwise a new database simply called SN7577 would be created

4. Once the database is opened you can run queries by typing directly in the shell. Unlike the plugin, you must terminate your select command with a ";". Although easy to forget, it generally works to your advantage as it allows you to split a long query command across lines as you did in the plugin

![SQLite shell query example](../fig/SQL_08_SQLite_shell_query_example.png)

The output from the query is displayed on the screen. If we just wanted to look at a small selection of data this may be OK. It is however more likely that not only are the results from the query somewhat larger, but also we would prefer to save the output to a file for later use. We might also want to change the field seperator from the default "|" to a comma so that we get a standard csv file.

These problems can be resolved with further "dot" commands.

There are in fact a large number of "dot" commands and they are all explained in the official SQLite documentation [here](https://sqlite.org/cli.html). 

The commands we need are 
> .mode csv

to change the field seperatator to ",". There are many other modes available see the [documentation](https://sqlite.org/cli.html). 

> .output my.filename

to direct the output to a file of my choice. The file will be created if needed or overwrite an already existing file, so exercise care

![SQLite shell dot commands](../fig/SQL_08_SQLite_shell_dot_commands.png)

Yes you can have a file called "my.filename" if you want. The contents of which contains the expected output from the query

![SQLite my.filename](../fig/SQL_08_my_filename.png)

Notice the use of quotes in the rows where the value data item is multiple words. 

## Saving the output from the SQLite shell

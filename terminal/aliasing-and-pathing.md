# Aliasing and Pathing

## Creating Aliases

An alias is a terminal shortcut. These are handy for frequently performed, lengthy commands. 

To create a permanent alias you need to modify your *~/.bashrc* file.

Sample aliasing:

`alias activate='source env/bin/activate'`

## Creating Paths

A `PATH` variable is how the shell knows where to find commands entered on the command line. `PATH` variables are often set by the `/etc/profile` startup file with code like: 

```
PATH=$PATH:$HOME/bin
export PATH
```

The first line tells the shell where to look for the file, and the second line tells the shell to make the contents of `PATH` available to other shell processes.

Typically, you will make changes in your `.bash_profile` (or some equivalent) when you want to add directories to your `PATH`.

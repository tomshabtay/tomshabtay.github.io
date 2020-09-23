---
layout: post
title:  "Better ways to cd"
date:   2020-07-23 12:35:56 +0300
categories: short 
---
Update
One of the first function every bash user is likely to tweak is how to get around directories.  
Sure can `cd` around, use `ls` to figure out the next directory and tab compilation to avoid too much typing, 
but is there a better way?  
At first I was trying to work with [Ranger](https://github.com/ranger/ranger).
a console file manager full of features and vim keybinding, its a great tool but I was looking for something simpler.  
Someday I came across z:
```
DESCRIPTION
       Tracks your most used directories, based on 'frecency'.

       After  a  short  learning  phase, z will take you to the most 'frecent'
       directory that matches ALL of the regexes given on the command line, in
       order.

       For example, z foo bar would match /foo/bar but not /bar/foo.
```
I really liked the idea that everyplace I cd into will be sorted by visits.  
Lets say im working on a `my-project/src/services` then I can enter something like:
`z mpjsrser`
and im right in.
But there was a issue for me, sometimes I get:
`'xy' did not match any results`
It happens sometimes when I don't remember the path or never visited the directory.
I prefer to confirm the result on the screen before entering the directory.

skimming through the manual I came across the `-l` flag.  
It lets you list the saved paths ordered by score.  
PICTURE . 
By combaining this command and `fzf` we can show the list and filter it on real time.  
Final command:  
`cd (z -l | awk -F ' ' '{print $2}' | fzf)`

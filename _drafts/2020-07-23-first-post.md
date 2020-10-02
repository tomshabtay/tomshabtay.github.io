---
layout: post
title:  "Better ways to navigate your file system"
date:   2020-07-23 12:35:56 +0300
categories: short 
---
This is *basic* but...
How do you move to a different directories in your terminal?
At the beginning I used `cd` and `ls` (to figure out the next directory) but it was too much time consuming.
I added aliases to the most impotent directories, but as time passed those directories changed and I was too lazy to keep track.

### Lets try z 
One day I came across z, a plugin that tracks your most used directories.
z will run in the background and remember the paths you will visit from now on. After recording some paths, we can start using it to navigate around.

If we want to visit `foo/bar/project` using z, we can type `z fbp` to the command line. The argument (`fbz` in our example) is a string built from some of the path characters (**f**oo/**b**ar/**p**roject).

But what will happen if I also have those directories?
```
foo/bar/project-1
foo/bar/project-2
```
Now when typing `fbz` z will have multiple directories to choose from, it will take the most frequently visited one. This can be done because z is keeping track also how many times each directory is visited

### z + fzf

But there was a small issue for me, sometimes I didn't remembered the path or even the name of the directory that I wanted to visit.

After seeing too many errors like: `'xy' did not match any results` I got an idea.
Maybe I can display a list of z known paths and then search and choose the correct path that I wants to visit.

#### fzf for the help
fzf is a command-line fuzzy finder. one way to use it will be to pipe a list to the command like `ls | fzf`. It will then lunch an interactive program letting you filter and choose item from the list, the selected item will be printed.

![Alt Text](/assets/fzf-example-vim.gif)

I
I prefer to confirm the result on the screen before entering the directory.

While z is a great tool on its own  
skimming through the manual I came across the `-l` flag.  
It lets you list the saved paths ordered by score.  
PICTURE . 
By combaining this command and `fzf` we can show the list and filter it on real time.  
Final command:  
`cd (z -l | awk -F ' ' '{print $2}' | fzf)`
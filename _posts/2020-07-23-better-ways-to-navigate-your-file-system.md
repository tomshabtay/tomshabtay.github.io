---
layout: post
title:  "Better ways to navigate your file system"
date:   2020-09-23 12:35:56 +0300
categories: terminal bash ranger fzf z 
---

How do you navigate to a different directories in your terminal?

For me it was `cd` and `ls` (to figure out the next directory) but it took too long to get where you wanted.
To make it easier I added aliases to the most impotent directories but as time passed those directories changed and keep tracking was hard.

I started looking for some tools to make this task easier.

## Ranger
[Ranger](https://github.com/ranger/ranger) is a console file manager with VI key bindings. You can use `/` to search for a file or directory `h` and `n` to move in or out a directory.

<br>
![Ranger](/assets/ranger.png){:class="img-responsive"}

It was super convenient for a while but has its downsides:
- Ranger boot time was a bit slow on my old machine (Although still under 1 second)
-  A bunch of keypresses is needed to find your desired path.

## Lets try - z
One day I came across [z - jump around](https://github.com/rupa/z), a plugin that tracks your most used directories.

z will run in the background and will remember the paths you will visit from now on. after recording some paths, we can start using it to navigate around.

### Usage
If we want to visit `foo/bar/project` using z, we can type `z fbp` to the command line. The argument (`fbz`) is a string built from some of the path characters (**f**oo/**b**ar/**p**roject) z will pick the most relevant path and will take you there.

But what will happen if use `fbz` and also have those directories?
```js
foo/bar/project-1
foo/bar/project-2
```
Now when typing `fbz` z will have multiple directories to choose from.

z will take the most frequently visited one. this is done by keeping track on the visit count for each path.

## z + fzf

But there was a small issue for me, sometimes I didn't remembered the path or even the name of the directory that I wanted to visit.

this could be solved by displaying a list of z known paths (ordered by visit frequently) and then search and choose the correct path that I wants to visit.

### fzf
fzf is a command-line fuzzy finder. It accepts list of string and will lunch an interactive program letting you filter and choose item from the list, the selected item will be returned.

One way to use it will be to pipe a `ls` output to the `fzf` and then return the result to `vim`.

```bash
$ vim (ls | fzf)
```

![Alt Text](/assets/fzf-example-vim.gif)

### Putting the two tougher
Luckily, z have `-l` flag to list the paths, we will use the list with fzf.

When we pass an argument (`demo`) with the `-l` flag we get paths relevant for it with their scores. by omitting the argument to get all results.
<br>
```bash
$ z demo -l | count
3

$ z -l | count # full list
124

$ z demo -l
common:    /Users/ts/code/demos
1.25       /Users/ts/code/demos
0.25       /Users/ts/code/demos/repos/my-node-service
0.25       /Users/ts/code/demos/repos
```

To filter the scores column we can use `awk`

**Final command:**

```bash
cd (z -l | awk -F ' ' '{print $2}' | fzf)
```

Being able to confirm my choice with fzf before getting in the directory instantly is nice, and z is really helpful by keeping a db of your visited directories.

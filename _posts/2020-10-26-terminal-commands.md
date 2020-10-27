---
layout: post
title: Terminal commands that make you look like a wizard.
date: 2020-10-26 05:00:00
tags: docker terminal commands
author: Izzy
published: true
---

There is no sweeter feeling than pulling off some sexy terminal commands when your coworkers are watching.
As we enter into the second year of covid and work from home becomes more common place, virtual pair programming sessions will become the new normal.
I hope you execute these commands with finesse and show your coworkers how cool you really are.

#### Copy Text
Have you ever needed to paste code into slack, but you had to open the file like a caveman?
__pbcopy__ is what you need.
```
cat corgis.txt | pbcopy
```
If the command prints to screen, then you can pipe it into __pbcopy__.
This command will copy whatever you pipe into it, into your system's copy clipboard.
My only warning is that, without seeing the file, this command is notorious for accidentally leaking secrets on to slack.
```
printenv | pbcopy
```
I've had some pretty close calls with this command.

#### Search for Text
Have you ever been searching for logs in `/etc` and you're manually scanning through the entire directory?
__grep__ is what you need to start using.
```
ls -la | grep filename
```
You don't need the `-la` flag for this, but as an additional tip, if someone sees you type `-la`, then they know not to mess with you cause you deal with _file permissions_.
__grep__ will print out all lines where your param exists.
Here are some other commands to combo with `grep`.
```
printenv | grep DB_URL
docker ps | grep awesome-api_web
cat .env | grep ACCESS_KEY
```

#### Search and Kill Process
Have you ever been developing an API and notice that your dev server cannot be exited with `ctrl+c`?
This generally happens to me when i'm in the middle of a debugging session and the dev server tries to restart due to a file save.
Instead of just closing the terminal window, you can use this command.

```
// list the process id (pid) of the dev server
lsof -i :8000

// terminatest the process given a pid
sudo kill -9 <pid>
```
There are actually other ways of doing this, but I like this method because it uses the `kill` command.
`lsof` will list the processes running on the port passed in via the `-i` flag.
Since you're doing api development, you're most likely exposing a port.
`kill` does what you would guess it does.
It terminates a process given a Process ID.

#### Git Fixup
If there was ever a sign of a junior dev, it's a string of commit messages like
- update component
- update component again
- forgot to update component
- final update component
- final final update component

You do all of this and now you have to `fixup` and `squash` these commits into one commit so that your commit history is nice and clean.
If you don't do this already by the way, then you definitely need to start.
You can use the `--fixup` command when you commit to make this process a lot easier.

```
git commit --fixup <git hash to squash into>
// do some dev'ing

git commit --fixup <git hash to squash into>
// do some dev'ing

git commit --fixup <git hash to squash into>
// do some dev'ing

git rebase -i --autosquash master
```
The hash that you point the fixup commit to will be the commit that it is squashed into.
You can then run the last command above and it'll squash all of the fixup commits for you.
It'll look something like this.
```
pick f690ebe yo
fixup c02cc1d fixup! yo
fixup 0058d77 fixup! yo

# Rebase 327a18a..0058d77 onto 327a18a (3 commands)
```
Without `--fixup`, you'd be doing all of this manually.

#### Move Cursor and Command Search
This one is technically not a terminal command, but you'll definitely be using it there.
When you're on the terminal, you can't be using the mouse to position the cursor.
Here are some shortcuts that can help you to quickly move your cursor around.

```
// move to beginning of line
ctrl + a

// move to end of line
ctrl + e

// historically search for command
ctrl + r
```

If you're on a mac, then you can actually do the first two shortcuts in any input and it beats out using arrow keys any day.
That includes inputs on chrome, terminal, editors, messengers, or whatever you want.

The last command `ctrl + r` will look back in your command history and suggest one that matches.
This way, instead of pressing the _up arrow_ several times, you can just run this shortcut and start typing in the command that you want to use.

#### Conclusion
I hope you found these commands to be super useful.
Not gonna lie, when I see others using commands like these to help make their work more efficient, I can't help but think that they are so cool.
Thanks for reading!


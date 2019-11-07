# Intro to Command Line

The command line is an extremely powerful tool. It often gets overlooked by
data scientists in favor of "fancier" coding languages. You really shouldn't
do that though. Here are three (of many) reasons why:

  * It's very likely you won't have a GUI if you're working on remote servers
  like Amazon's AWS or Google Cloud. You'll have to do command line.
  * There are command line tools that can make your life a lot easier like
  scp, grep, curl, etc
  * Jupyter Notebooks aren't an endpoint for development - so running python
  scripts from the command line is going to be mandatory at some point in your
  career.

With that in mind, we're going to explore the basics of surviving in the
command line today. Note that this exercise assumes you are working from top
to bottom and some code may require previous steps. Also anything that looks
like this:

```bash
laa-dee-da
```

is meant for you to run those commands in your terminal.

### Moving around in the command line

Start by opening the `terminal` program on your computer. When this opens, you
will start out in a place known as the `HOME` directory. This is a different
place for every user. Let's figure out where we are for each of you. 

```bash
pwd
```

This stands for "print working directory". The **working directory** is basically
where the command line thinks it is. In terms of a file browser, it's
essentially saying "what directory am I in right now?". So if we were to add a
new file, it would show up in our **working directory**.

That's handy, but we don't want to be stuck in our home directory. So let's
make our own directory to start practicing with.

```bash
mkdir test_directory
```

This is going to "make directory" and call it `test_directory`. We've told the
command line we want a new folder called `test_directory` and it's made it for
us. Let's make sure it's there.

```bash
ls
```

`ls` is the command to list all of the files in the current working directory.
You should see a bunch of files and folders come up, and amongst them should
be `test_directory` that we just created.

That's neat. However, we want to be able to do more than create folders, we
want to use them. So let's make that folder our new working directory by
moving the terminal into it. 

```bash
cd test_directory
```

This "changes directory" and makes the named directory our new working
directory. 

> Question: How could we make sure that we moved?

>> Answer: Check the current directory.
```bash
pwd
```

Let's get a few special things out of the way as well for command line. When
we talk about folders, we don't always want to move "into" the next folder
down. Sometimes we want to move back up a folder. To do that we can do:

```bash 
cd ..
```

the `..` means "go back one".

Or, we might want to move back to our HOME folder from wherever we are. In
command line we can use `~` to mean "go home"

```bash
cd ~
```

This will take us back to our home folder. Let's chain a few commands together
to get back to our `test_directory`

```bash
cd ~/test_directory
```

### Making and Reading Files

So we're able to move around between directories now. Great. Let's get some
file action going. Let's start by learning about input/output in a terminal.

In the terminal, if there's output from a program, it prints in the terminal
itself. This is called "standard out" or `Stdout` for short. Let's use a
command to write our own text to standard out.

```bash
echo "HELLO WORLD"
```

That command tells the terminal to write out anything we put after the `echo`
to standard out. If we want it to not go to standard out, we have to tell it
where to go instead. 

```bash
echo "HELLO WORLD" > test_file.txt
```

Now nothing shows up on the screen. List the files and see if you can tell
why.

> Hint: we learned how to list all the files up above.

The greater than says, "put that output into the following file instead."

What if we want to read that file back out to the terminal?

```bash
cat test_file.txt
```
`cat` takes whatever is in the file and reads it out to stdout one line at a
time.

Let's get a bit fancier and load in a dataset. Let's pull it from the internet
using `curl`.

```bash
curl https://archive.ics.uci.edu/ml/machine-learning-databases/auto-mpg/auto-mpg.data
```

_(note, you might need to use `wget` if your system says `curl` isn't
installed. Just swap in `wget` any place you see `curl`)_

This prints the data directly to stdout and we can see the whole dataset.
Nice. 

> Let's save the output of the `curl` command to a file called `data.txt`. How
would we do that?
```bash
curl https://archive.ics.uci.edu/ml/machine-learning-databases/auto-mpg/auto-mpg.data > data.txt
```

Now that we have the data file, let's learn how to manipulate files.

### Copying, Moving, and Removing Files

Let's start out by seeing what's in our working directory.

```bash
ls
```

Now let's make a copy of the data file

```bash
cp data.txt data2.txt
ls
```

Now we have two copies of the data file! Nice. How do we know which came
first? Let's get a bit fancier with our `ls` command and include a special
flag.

```bash
ls -l
```

This tells the command line to show us the files and "list" all the
information about them. These break down into:

```
--- Do not run this, just for explanation ---

Permissions   Owner            Group  File   Last         File name
                                      size   Modified

-rw-r--r--  1 zachariahmiller  staff  834582 Sep 27 10:38 Untitled.ipynb
-rw-r--r--  1 zachariahmiller  staff      10 Sep 26 09:28 my_file.txt
```

Mostly, we'll care about things like file size (in bytes) and modified date.
We can get even fancier by adding a second flag

```bash
ls -lh
```

> What changed?

If instead copying we just want to change the name of a file, we can do:


```bash
mv data2.txt NEW_FILE_NAME.txt
ls
```

There's no need to keep two of the same file, so let's go ahead and delete the
one we just renamed.

```bash
rm NEW_FILE_NAME.txt
ls
```

Now let's try to make and copy a directory to see how things change when we
aren't working with solo files anymore.

```bash
mkdir test_directory
cp test_directory test_directory2
ls
```

That doesn't work because the command line says, "whoa, are you sure you want
to copy EVERYTHING inside this directory?" To tell it to do that, we just
have to give it a flag that means, "go inside this directory and copy
everything that's there, then copy the directory itself." That's the `-r` or
"recursive" flag.

```bash
cp -r test_directory test_directory2
ls
```

If we want to delete a whole directory, we have to do exactly the same thing.
`rm` doesn't want to let you accidentally delete all the things inside of a
directory, so it will only delete whole directories if you give it a proper
flag.

```basg
rm -r test_directory
rm -r test_directory2
ls
```

**Warning: There is no recycle bin in command line. When you do `rm` it's gone
for good. There's no "oops, I'll just go save that." So be sure you're doing what you mean to do!**

### Some advanced command line techniques

We've now covered the basics. Moving around in directories, copying, renaming,
creating, and deleting files. Let's start to dive into why the command line
can be a powerful data science tool. 

#### grep

Let's get started by learning how to `pipe`. We can take the output of a
command and hand that output directly into the next command. So for instance,
we can take the output of `cat` and then run some sort of modifying commands
on that. Let's take a look at `grep` to test this out.

`grep` is a command that searches for RegEx matches. So for example if I asked
it to look into a set of text and find all the lines that have the word
"honda" it would then print just those lines. We can combine that with our
dataset by doing:

```bash
cat data.txt | grep "honda"
```

This does cat on the file, hands the output to grep, and says, "show me all
the lines that have the characters "honda" in them. `grep` can be a great tool
for searching through files and finding how our data looks.

`grep` can also work by just looking into the files themselves like so:

```bash
grep "honda" data.txt
```

Or I can even check ALL of the files in the folder by introducing the
`wild card`. A wild card is the command line way of saying, I'm not quite sure
exactly which files I mean. Imagine we have these files:

* test1.txt
* test2.txt
* tes.txt
* pyfile.py

A wild card like `test*.txt` would match: 

* test1.txt
* test2.txt

but not the others. A wild card like `tes*` would match:

* test1.txt
* test2.txt
* tes.txt

And a wildcard like `*.py` would match:

* pyfile.py

I can also use this logic with something like `grep`

```bash
grep "honda" *
```
which will search every file in this directory for the string "honda".

#### wc (word count)

We can also use pipes to count how many lines are in a file. Let's test this
with our dataset. 

```bash
cat data.txt | wc
```
This returns the number of: lines, words, and characters in the file. So we
can immediately see that our dataset has 398 rows.

#### which

Finally, the last very useful piece of command line is `which`. Try

```bash
which python
```

This will tell you where the file that runs "python" lives. So if you had
installed both python 3 and python 2, you could type `which python` and see
whether the command `python` was associated with python 3 or python 2. This
works on any executable. Try also:

```bash
which grep
```
and it will show you where `grep` is installed on your system.


# Important Take-aways

Using the command line is an acquired skill set. It's almost certainly not
going to feel natural after one day of exposure. That said, force yourself to
use it - it's a skill worth having as almost any data science job will involve
a heavy amount of command line at some point, whether it be setting up
databases with psql, manipulating files, moving around and exploring a system,
figuring out how to make your own python modules, writing production code that
executes from the command line or something else.

For today, the main take-aways are:

* pwd
* cd
* cp
* mv
* cat
* mkdir
* stdout vs pipes

If you got all of that, then you can start to pickup more skills as you use
things more.

# MAN pages

If you ever want to learn more about a command, just type:

```
man command_name
```

for example:

```
man grep
```

This will take you to a page with all of the details you never wanted to know
about grep. You can scroll up and down with arrow keys, or quit by pressing
`q`. There are man pages for pretty much every command. 

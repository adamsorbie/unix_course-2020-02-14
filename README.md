#### Adapted from Introduction to the Unix shell for biologists by Konrad U. Förstner: https://github.com/konrad/2017-03-29-Software_Carpentry_Munich_Teaching_Material/edit/master/Unix_Shell/Unix_Shell_Handout.md

(This was the first course I attended which really helped me learn how to use the Unix shell)

#### Adam Sorbie


# Table of Contents 


- [Installation instructions](#installation-instructions)
- [Background](#background)
- [Course conventions](#course-conventions)
- [The basic anatomy of a command line call](#the-basic-anatomy-of-a-command-line-call)
- [How to get help](#how-to-get-help)
- [Tab Completion](#tab-completion)
- [Files, folders, locations](#files--folders--locations)
- [Manipulating files and folder](#manipulating-files-and-folder)
- [File contents - Viewing and Editing files](#file-contents---viewing-and-editing-files)
- [File content - Sorting, counting and Filtering files](#file-content---sorting--counting-and-filtering-files)
- [Connecting tools](#connecting-tools)
- [Repeating command using the `for` loop](#repeating-command-using-the--for--loop)
- [Shell scripting](#shell-scripting)
- [Useful Example of Shell Scripting](#Useful-Example-of-Shell-Scripting)
- [Bonus](#Bonus)
- [Useful Links](#Useful-Links)


# Unix_shell_course-2020-02-14
Short Unix shell course Feb 2020 - Haller Lab


## Installation instructions

To take part in this course you need to have a Unix/Linux bash shell installed on your computer. Luckily Windows 10 has made this so much easier than it used to be and you can run an almost complete Linux subsystem on your windows PC.


Open a powershell (search for powershell in the search bar) **as an administrator** (right click on the program) and copy and paste the following command:

Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux

You will then be prompted to restart your computer. After booting up again, open the Microsoft store app and search for your preferred linux distribution. We will be using Ubuntu for this course, so I would suggest using that but if you want to use another one that's also ok.

Once installed, open the terminal and provide a username and password. To download the contents of this course type or copy paste the following into your terminal:

```
git clone https://github.com/adamsorbie/unix_shell_course-2020-02-14.git
```

if that doesn't work then you may need to install git first, you can do this by typing:

```
sudo apt update
```

followed by:

```
sudo apt install git`
```
then repeat the above command.

# Background

In this short course you will learn the very basics of how to use the Unix
shell. Most tools used in Bioinformatics are written for command line use and
understanding how to use the shell will allow you to use these tools in your
own research. Additionally, knowing a little bit of bash scripting is very
helpful for automating annoying, repetitive tasks and can help make your
analyses more reproducible.

# Course conventions

* Anytime you see a path with yourusername in it e.g. `/home/yourusername` please replace "yourusername" with
  the username you chose for yourself during the installation.
* If you have any questions or something isn't working, you can interrupt me at any time to ask. 



# The basic anatomy of a command line call

Running a tool in the command line follows a simple
pattern. At first you have to write the name of the command. 
Some command line tools require additional parameters. 
While the parameters are the requirement of the program
the actual values we give are called arguments. There are two
different ways how to pass those arguments to a program - via keyword
parameters (also called named keywords, flags or options) or via
positional parameters.  The common pattern looks like this (`<>`
indicates required items, `[]` indicates optional items):

```
<program name> [keyword parameters] [positional parameters]
```

An example is calling the program `ls` which lists the content of a
directory. You can call it without any argument

```
$ ls
```

or with one or more keyword arguments

```
$ ls -l
$ ls -lh
```

or with one or more positional arguments

```
$ ls test_folder
```

or with both keyword and positional arguments

```
$ ls -l test_folder
```

The output of a command is written usually to the so called *standard
output* of the shell which is shown on the screen when you call the command.

# How to get help

Perhaps the most important command to know, although not particularly helpful if you don't
understand anything yet, is `man` which stands for *manual*. Most commands have a manual
and `man` allows you to read those. Reading the manual should tell you what the command does and how to use it.
For example, to get the man of `cd` type

```
$ man ls
```

To close the manual use `q`. Note some tools may offer help in a different way, common ones are
`-h`, `-help` or `--help`. For example `cd`:

```
$ cd --help
```

# Tab Completion

One of the most useful things about bash is tab completion. Pressing Tab after typing the first
few letters of a command/file/folder will autocomplete it. Be aware that if there are multiple commands
or files starting with the same few letters then the bash shell will not know which one to choose. You can double press
to reveal all of the options or just continue typing until the name is unique. Programmers are lazy, be lazy too.

* Tab - extend commands and file/folder names

Type the following and press tab to see how it works
```
$ ls unix_
```

# Files, folders, locations

Topics:

* `ls `
* `pwd`
* `cd`
* `mkdir`
* Relative vs. absolute path
* `~/`

In this part you will learn how to navigate through the Unix file system.

Firstly, it always useful to know where we are. When you open a new terminal you
should be in your home directory. To test
this, call the program `pwd` which stands for **p**rint **w**orking **d**irectory.

```
$ pwd
/home/adamsorbie
```

On WSL, the output of `pwd` should be `/home/username`, yours
may differ slightly however. The next command we need is `ls`.
This command simply lists the contents of a folder.
If you call it without any arguments it will output the content
of the current folder. We can use the `ls` command to get a rough overview
of what a common Unix file system tree looks like and learn how to address
files and folders. The root folder of a system starts with `/`. Type

```
$ ls /
```

to see the content of the root folder. You should see something like

```
bin   c    etc  home  lib    media  opt   root  sbin  srv  tmp  var
boot  dev  h    init  lib64  mnt    proc  run   snap  sys  usr
```

Most of these folders are not particularly important for you right now.
Those are more important if you are the administrator of the system.
Normal users do not have the permission to make changes here.
Currently your home directory is where you will work today and
probably where you will end up doing most of your work in future. In here we
will learn how work with paths. A file or folder can be addressed
by its *absolute* or its *relative path*. As you have
downloaded the github repo containing todays course materials, when you type `ls`
you should see a folder with the name of todays course.
Assuming you are in the home folder (`/home/yourusername/`) the relative path to
the folder is `unix_course-2020-02-14`. You can see what's inside the folder by
calling `ls` like this:

```
$ ls unix_shell_course-2020-02-14
```

This is the so called *relative path* as it is relative to where you are right now
`/home/yourusername/` i.e. your current working directory. The *absolute path* or full path
starts with a `/` and is `/home/yourusername/unix_course-2020-02-14`. Call `ls` like this:

```
$ ls /home/yourusername/unix_shell_course-2020-02-14
```
You can think of it a little like being give a street address without the city or town. Gregor-Mendel Str. 2
may make sense to you if you are in Freising (*relative* to where you are), however if you are located elsewhere you would also need to know
in which place this street is located.

There are some conventions regarding *relative* and *absolute paths*. One
is that a dot (`.`) represents the current folder you are in. The command

```
$ ls ./
```

should return the same as

```
$ ls
```

Two dots (`..`) represents the folder above your current working directory. If you call

```
$ ls ../
```

you should see the content of `/home`. If you call

```
$ ls ../../
```

you should see the content of the parent folder of the parent folder which on
a normal linux system is the root folder (`/`) assuming you are in `/home/yourusername/`.
However, on WSL it may be slightly different. Another
convention is that `~/` represents the home directory of the user. The
command

```
$ ls ~/
```
should list the content of your home directory independent of where you are in the file system.

Now we know where we are and what's there already we can start to move around the file system.
To do this we use the command `cd` (change directory). If
you are in your home directory `/home/youruserbname/` you can go into the
folder `unix_shell_course-2020-02-14` by typing

```
$ cd unix_shell_course-2020-02-14
```

After that call `pwd` to make sure you're in the correct folder.

```
$ pwd
/home/yourusername/unix_shell_course-2020-02-14
```

To go back into your home directory you have a couple of different options.

You could use the *absolute path*

```
$ cd /home/yourusername/
```

or the above mentioned convention for the home directory `~/`:

```
$ cd ~/
```

or the *relative path*, in this case the parent directory of
`/home/yourusername/unix_shell_course-2020-02-14`:

```
$ cd ../
```

As the home directory is such an important place `cd` actually uses this as
a default argument, thus if you just call `cd` without anything you will
automatically go to the home directory. Test this behavior by calling

```
$ cd
```

You can try now to move around the file system yourself and list the
files and folders located there.

Now we will create our own folder using the command `mkdir` (*make
directory*). Go into the home directory and type:

```
$ mkdir my_first_folder
```

In Unix and often many programming languages "No
news is good news." The command successfully created the folder
`my_first_folder`. You can check by calling `ls`, but `mkdir` did
not tell you this. If you do not get a message this _usually_ means
everything went fine. If you call the same `mkdir` command again you
should get an error message:

```
$ mkdir my_first_folder
mkdir: cannot create directory ‘my_first_folder’: File exists
```

So if a command does not complain you can usually assume there was no
error.

# Manipulating files and folder

Topics:

* `touch`
* `cp`
* `mv`
* `rm`

Now we will learn how to manipulate files and folders. We can create some
empty files `touch`. The main purpose of the `touch` command is actually to
change the time stamps of files but you can also use it to create empty files. Let's
use touch to create a file called `test_file_1.txt`:

```
$ touch test_file_1.txt
```

Use `ls` to check it worked.

The `cp` command (*copy*) can be used to copy files or folders (**only with a specific flag**).
For this it requires at least two arguments: the source and the target file. In
the following example we generate a copy of the file `test_file_1.txt`
called `a_copy_of_test_file.txt`.

```
$ cp test_file_1.txt a_copy_of_test_file.txt
```

Use `ls` to confirm that this worked. We can also copy the file to the folder we
created earlier `my_first_folder` :

```
$ cp test_file_1.txt my_first_folder
```

Now there should be also a file `test_file_1.txt` in the folder
`my_first_folder`. If you want to copy a folder and its content you
have to use the flag `-r`. The r stands for recursive, which you can think of
as repeatedly.

```
$ cp -r my_first_folder a_copy_of_my_first_folder
```

You can use `mv` command(*move*) to rename or move files
or folders. To rename the file `a_copy_of_test_file.txt` to
`test_file_with_new_name.txt` call

```
$ mv a_copy_of_test_file.txt test_file_with_new_name.txt
```

With `mv` you can also move a file into a folder. For this the second
argument must be a folder. For example, to move the file now named
`test_file_with_new_name.txt` into the folder `my_first_folder` use

```
$ mv test_file_with_new_name.txt my_first_folder
```

You are not limited to one file if you want to move them into a
folder. Let's create and move two files `file1` and `file2` into the
folder `my_first_folder`.

```
$ touch file1 file2
$ mv file1 file2 my_first_folder
```

Now we can introduce another useful feature most shells offer
called *globbing*. Imagine you want to apply the same
command to several files. Instead of explicitly writing all the file
names (which would take a lot of time) you can instead use a *globbing pattern*
to refer to all of them. There are different "wildcards" that can be used for these patterns.
The most important one is the asterisk (`*`). It can replace none, one or more
characters. Let explain this with a quick example:

```
$ touch file1.txt file2.txt file3
$ ls *txt
$ mv *txt my_first_folder
```

The `ls` shows the two files matching the given pattern
(i.e. `file1.txt` and `file2.txt`) while dismissing the one not
matching (i.e. `file3`). In this case the `*` basically means match
anything before txt.

Similarly for `mv` - it will only move the two
files ending with `txt`.

We now have several empty test files that we don't need anymore. The last command we
will learn in this section is `rm` (*remove*) which allows you to delete files
and folders. **Danger Ahead** there is no trash bin if you remove items using `rm`.
They will be gone for good and without further notice. It's good practice to use `rm -i`,
this flag askes before deleting a file, which can be a lifesaver. Note, once you are more
advanced and comfortable using a unix system you can modify the default behaviour of rm so
that it will always ask you before deleting.

To delete a file in `my_first_folder` call:

```
$ rm my_first_folder/file1.txt
```

To remove a folder use the parameter `-r` (*recursive*):

```
$ rm -r my_first_folder
```

Alternatively you can use the command `rmdir`:

```
$ rmdir my_first_folder
```

However, keep in mind `rmdir` will only work on empty directories.

# File contents - Viewing and Editing files

Topics:

* `less` / `more`
* `cat`
* `echo`
* `head`
* `tail`
* `cut`

We haven't really looked at how to view and edit files yet, so now we will move on
and look at some of the commands we can use for this. Please go into the folder `unix_course-2020-02-14`
if you aren't already and unzip files.zip using the following command:

```
unzip files.zip 
```

You should now see some files there (*and by now you should know how to check).
To read the content of files with the ability to scroll around we need
a so called pager program. We will use the tool `less` which should be available on
all of your systems. Let's start with the file:

`origin_of_species.txt`

```
$ less origin_of_species.txt
```

The file contains Charles Darwin's *Origin of species* in plain
text. You can scroll up and down line-wise using the arrow keys or page-wise
using the page-up/page-down keys. To quit use `q`. With
pager programs you can read file content interactively, but sometimes
you just need the content of a file. The command `cat` (*concatenate*) does just
that for one or more files. Let us use it to see what is in the example file
`lorem_ipsum.txt`. Assuming you are still in the folder `unix_course-2020-02-14`
you can call

```
$ cat lorem_ipsum.txt
```

The content of the file is shown to you. You can apply the command to
two files and the content is concatenated and returned:

```
$ cat lorem_ipsum.txt lorem_ipsum_2.txt
```

This is a good time to introduce *standard input* and *standard
output* and what you can do with them. Stdin is the standard input stream
and accepts text as input. Stdout is the standard output stream, and as you might expect,
outputs text. You can redirect the *standard output* into a file by using the `>` operator.
Let try this to generate a new file that contains
the combined content of both files:

```
$ cat two_lines.txt three_lines.txt > five_lines.txt
```

Use cat again to have a look at the content of this file

```
$ cat five_lines.txt
```

*standard output* can also be redirected to other tools as
*standard input*. We will go into more detail a little later. With `cat` we used
existing file content. To create something entirely new we can use the `echo` command
which writes an input string to standard output.

```
$ echo "Linux is cool"
```

To redirect the output into a target file use `>`.

```
$ echo "Linux is cool" > super_cool.txt
```

Please note that this can be dangerous. You can easily overwrite the content of an
existing file. For example if you call now

```
$ echo "Linux is boring" > super_cool.txt
```

only the last string will be written to the file and the
previous one will be overwritten (Worst of all the text in your file will be a lie ;) ).
To append output of a command to a file without overwriting use `>>`.

```
$ echo "Linux is cool" > super_cool.txt
$ echo "I love Linux" >> super_cool.txt
```

Now your file should contain two lines

Often you just want part a file, for example just the
first few or last few lines. For this the commands `head` and `tail` can
be used. By default 10 lines are shown. You can use the parameter `-n
<NUMBER>` (e.g. `-n 20` or just `-<NUMBER>` (e.g. `-20`) to specify the
number of lines you want to be displayed. Test `head` and `tail` with the file
`origin_of_species.txt`:

```
$ head origin_of_species.txt
$ tail origin_of_species.txt
```

This is super useful if you want to look at large files (e.g. FASTQ), because you can't
normally open them in a text editor.


You can also extract text horizontally using the
command `cut`. This is especially useful if you have a file with different columns.
To see how this works lets extract the first 10 characters of each line
in the file `origin_of_species.txt`:

```
$ cut -c 1-10 origin_of_species.txt
```

Lets now look at a more common usage (at least for me). Have a look at the content of the
file `mapping_file.tab`. You see that it contains different columns that are
tabular-separated. You can extract selected columns with `cut`:

```
$ cut -f 1,4 mapping_file.tab
```

# File content - Sorting, counting and Filtering files

Topics:

* `wc`
* `sort`
* `uniq`
* `grep`

There are also several tools that allow you to manipulate the content of a
text file or find out information about it. For example if you would like to 
find out the number of characters, words or lines in a file, you can use the
`wc` command. Try to count the number of lines in the file:
`origin_of_species.txt`:

```
$ wc -l origin_of_species.txt
```

You can use `sort` to sort a file alpha-numerically. Try the following commands 
on the file `unsorted_numbers.txt`  

```
$ sort unsorted_numbers.txt
$ sort -n unsorted_numbers.txt
$ sort -rn unsorted_numbers.txt
```

and see if you can understand the output.

`uniq` takes a sorted list of lines returns the uniques. Let's imagine we got a file 
containing a list of bacterial taxa from our sequencing data. Due to the nature of 16S
sequencing we will often have mutiple instances with the same taxonomic assignment, and in 
this case we want to look at the uniques. Have a quick look at `taxa.txt`. Then use `uniq` to generate 
a non-redundant list of taxa:

```
$ uniq taxa.txt
```

If you call `uniq` with `-c` you can count the number of occurrence of each genus
```
$ uniq -c taxa.txt
```

With the tool `grep` you can extract lines that match a given
pattern. For instance, let's say we are interested in the order 
Bacteroidales. Note that grep by default is case
sensitive, to remove this behaviour you can use the `-i` flag. 

```
$ grep Bacteroidales taxa.txt
```

If you are only interested in the number of lines that match the pattern
use `-c`:

```
$ grep -c Bacteroidales taxa.txt
```

# Connecting tools

The real power of Unix is built on its capability to readily connect tools. 
For this so-called *pipes* are used. To use the *standard output* of one tool 
as *standard input* of another command `|` is used. For example, based on what
we just did above, let's say we want to find the unique taxonomic assignments
within the order Bacteroidales and write this output to a file

```
$ grep Bacteroidales taxa.txt | uniq > Bacteroidales.txt
```

# Repeating command using the `for` loop

Now we want to generate a copy of all the text files in your working directory. Running this

```
cp *txt copy_of_*txt
```

would not work.

With `for` loops you can solve this problem. Let's start with a simple
one.

```
for FILE in three_lines.txt two_lines.txt
> do
> head -n 1 $FILE
> done
```
You can not only just use one command inside of a loop but multiple, 
for example now we want to look at Clostridiales and Lactobacillaes as well as
Bacteroidales, to do this we can a for loop:

```
for taxa in Bacteroidales Clostridiales and Lactobacillales
> do 
> grep $taxa taxa.txt | uniq >> taxa_list.txt 
> done
```
# Shell scripting

Open a new file in a text editor of you choice, call it
`count_lines.sh` and add the following text:

```
echo "Number of lines that contain species":
wc -l origin_of_species.txt
```

Save the file, make sure the file `origin_of_species.txt` is in the
same folder and run the script:

```
$ bash count_lines.sh
```

You should get something like

```
Number of lines that contains species:
15322 origin_of_species.txt
```

This a very first shell script. Now we can make it a little more
flexible. Instead of hard coding (setting this variable within the script itself) 
the input file for `wc -l` we want to be able to give this as argument. 
To do this we change the shell script to:

```
echo "Number of lines in the given file":
wc -l $1
```

`$1` is a variable that represents the first argument given. 
Now you can call the script like this:

```
$ bash count_lines.sh origin_of_species.txt
```

You should get the same results as before. If you want to your script to take
a second argument you can use `$2`. To use all arguments
given to the shell script use the variable "$@". Change the shell
script to:

```
echo "Number of lines in the given file(s)":
wc -l $@
```

and run it with several input files:

```
bash count_lines.sh origin_of_species.txt taxa.txt
```

You should get something like:

```
Number of lines in the given file(s):
 21648 origin_of_species.txt
      313 taxa.txt
 21961 total
```
# Useful Example of Shell Scripting

Now we will look at a use for shell scripts that can be really useful for you 
(**if you are studying the microbiome**). After doing your differential abundance 
analysis you often want to extract the sequences of the OTUs which were identified 
as differentially abundant and perhaps check their taxonomy using something like
BLAST or EZtaxon. This is extremely annoying to do manually and takes a lot of 
time, but luckily with a bit of BASH magic you can automate this process easily. 

Consider that the headers in a fasta file have a consistent format, and also
that grep can take a txt file as input (use man to check this), can you think of 
a way we could do this? 



<details><summary>Solution</summary>
<p>

An easy way to do this (**not necessarily the best but it works**)

Firstly you need a text file with your OTUs, you should be able to generate
your own or you can use the one provided

```
echo "Extracting OTUs"
grep -f $1 -w -A8 $2 >> $3

# -f means use a file as input, -w means match the whole word, and A means 
# print X number of lines of context after, in this case 8 because there our
# sequences are 8 lines
``` 

</p>
</details>

You may notice this prints a couple of extra OTU numbers and this is because the 
sequences in the FASTA files are not always exactly 8. A better way to do this
but much more complicated, would be to make your FASTA file single line before 
extracting the sequences. 

``` 
echo "enter sequence filename: "
read filename
echo "enter file containing patterns to match: "
read patterns
echo "enter output filename: "
read output
perl -pe '/^>/ ? print "\n" : chomp' $filename | grep -f $patterns -w -A1 | grep -v -- "^--$" > $output

``` 

# Bonus 

I thought it might also be useful to quickly show an example of a BASH script 
used for analysis. This is one I wrote to run Salmon on some RNA seq data: 

```
#!/bin/bash

for fn in trimmed_reads/F123{54..72};
do
	samp=`basename ${fn}`
	echo "Processing sample ${samp}"
	echo ${fn}
        salmon quant -i mouse_index.idx -l A \
	-1 ${fn}_R1_001.qc.fq.gz \
	-2 ${fn}_R2_001.qc.fq.gz \
        -p 8 --validateMappings -o quants/${samp}_quant
done 

```

Can you understand what it's doing? 

# Useful Links

here are some useful links if you want to learn more about working with 
the unix shell 
http://www.linuxcommand.org/lc3_learning_the_shell.php
https://www.datacamp.com/courses/introduction-to-shell-for-data-science
https://www.bash.academy/
https://www.learnshell.org/




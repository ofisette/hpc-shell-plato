---
title: Writing and reading files
teaching: 30
exercises: 15
questions:
- "How do I create/edit text files?"
- "How do I move/copy/delete files?"
objectives:
- "Learn to use the `nano` text editor."
- "Understand how to move, create, and delete files."
keypoints:
- "Use `nano` to create or edit text files from a terminal."
- "`cat file1 [file2 ...]` prints the contents of one or more files to terminal."
- "`mv old dir` moves a file or directory to another directory `dir`."
- "`mv old new` renames a file or directory."
- "`cp old new` copies a file."
- "`cp old dir` copies a file to another directory `dir`."
- "`rm path` deletes (removes) a file."
- "File extensions are entirely arbitrary on UNIX systems."
---

Now that we know how to move around and look at things, let's learn how to read, write, and handle
files! We'll start by moving back to our home directory and creating a scratch directory:

```
$ cd ~
$ mkdir hpc-test
$ cd hpc-test
```
{: .language-bash}


## Creating and Editing Text Files

When working on an HPC system, we will frequently need to create or edit text files.
Text is one of the simplest computer file formats, defined as a simple sequence of text lines.

What if we want to make a file? There are a few ways of doing this, the easiest of which is simply
using a text editor. For this lesson, we are going to us `nano`, since it's more intuitive than many
other terminal text editors.

To create or edit a file, type `nano <filename>`, on the terminal, where `<filename>` is the name of the
file.  If the file does not already exist, it will be created.
Let's make a new file now, type whatever you want in it, and save it.

```
$ nano draft.txt
```
{: .language-bash}

![Nano in action]({{ site.url }}{{ site.baseurl }}/fig/nano-screenshot.png)

Nano defines a number of *shortcut keys* (prefixed by the <kbd>Control</kbd> or <kbd>Ctrl</kbd> key)
to perform actions such as saving the file or exiting the editor.
Here are the shortcut keys for a few common actions:

* <kbd>Ctrl</kbd>+<kbd>O</kbd> --- save the file (into a current name or a new name).

* <kbd>Ctrl</kbd>+<kbd>X</kbd> --- exit the editor.
  If you have not saved your file upon exiting, `nano` will ask you if you want to save.

* <kbd>Ctrl</kbd>+<kbd>K</kbd> --- cut ("kill") a text line.
  This command deletes a line and saves it on a clipboard.
  If repeated multiple times without any interruption (key typing or cursor movement),
  it will cut a chunk of text lines.

* <kbd>Ctrl</kbd>+<kbd>U</kbd> --- paste the cut text line (or lines).
  This command can be repeated to paste the same text elsewhere.


> ## Using `vim` as a text editor
>
> From time to time, you may encounter the `vim` text editor. Although `vim` isn't the easiest or
> most user-friendly of text editors, you'll be able to find it on any system and it has many more
> features than `nano`.
>
> `vim` has several modes, a "command" mode (for doing big operations, like saving and quitting) and
> an "insert" mode. You can switch to insert mode with the `i` key, and command mode with `Esc`.
>
> In insert mode, you can type more or less normally. In command mode there are a few commands you
> should be aware of:
>
> * `:q!` --- quit, without saving
> * `:wq` --- save and quit
> * `dd` --- cut/delete a line
> * `y` --- paste a line
{: .callout}

Do a quick check to confirm our file was created.

```
$ ls
```
{: .language-bash}

```
draft.txt
```
{: .output}


## Reading Files

Let's read the file we just created now. There are a few different ways of doing this, one of which is
reading the entire file with `cat`.

```
$ cat draft.txt
```
{: .language-bash}

```
It's not "publish or perish" any more,
it's "share and thrive".
```
{: .output}

By default, `cat` prints out the content of the given file.
Although `cat` may not seem like an intuitive command with which to read files, it stands for
"concatenate". Giving it multiple file names will print out the contents of the input files in the order
specified in the `cat`'s invocation.
For example,

```
$ cat draft.txt draft.txt
```
{: .language-bash}

```
It's not "publish or perish" any more,
it's "share and thrive".
It's not "publish or perish" any more,
it's "share and thrive".
```
{: .output}

> ## Reading Multiple Text Files
>
> Create two more files using `nano`, giving them different names such as `chap1.txt` and
> `chap2.txt`. Then use a single `cat` command to read and print the contents of `draft.txt`,
> `chap1.txt`, and `chap2.txt`.
{: .challenge}


## Creating Directory

We've successfully created a file. What about a directory? We've actually done this before, using
`mkdir`.

```
$ mkdir paper
$ ls
```
{: .language-bash}
```
draft.txt  paper
```
{: .output}


## Moving, Renaming, Copying Files

**Moving**---We will move `draft.txt` to the `files` directory with `mv` ("move") command.
The same syntax works for both files and directories: `mv <file/directory> <new-location>`

```
$ mv draft.txt paper
$ cd paper
$ ls
```
{: .language-bash}
```
draft.txt
```
{: .output}

**Renaming**---How do we go about changing the name of a file?
It turns out that `mv` is also used to rename files and directories. Although this may not seem
intuitive at first, think of it as *moving* a file to be stored under a different name. The syntax is
quite similar to moving files: `mv oldName newName`.

```
$ mv draft.txt final.txt
$ ls
```
{: .language-bash}
```
final.txt
```
{: .output}

> ## File extensions are arbitrary
>
> In the last example, we changed both a file's name and extension at the same time. On UNIX
> systems, file extensions (like `.txt`) are arbitrary. A file is a `.txt` file only because we say
> it is. Changing the name or extension of the file will *never* change a file's contents, so you
> are free to rename things as you wish. With that in mind, however, file extensions are a useful
> tool for keeping track of what type of data it contains. A `.txt` file typically contains text,
> for instance.
{: .callout}

**Copying**---What if we want to copy a file, instead of simply renaming or moving it?
Use `cp` command (an abbreviated name for "copy"). This command has two different uses that work in the same way as `mv`:

- Copy to the same directory: `cp file newFilename` (the copy gets a new name)
- Copy to another directory: `cp file directory` (the copy retains the original name)

Let's try this out.

```
$ cp final.txt backup.txt
$ ls
```
{: .language-bash}

```
backup.txt  final.txt
```
{: .output}
```
$ cp backup.txt ..
$ cd ..
$ ls
```
{: .language-bash}
```
backup.txt  paper
```
{: .output}

## Removing files

We've begun to clutter up our workspace with all of the directories and files we've been making.
Let's learn how to get rid of them. One important note before we start... **when you delete a file
on UNIX systems, they are gone *forever***. There is no "recycle bin" or "trash". Once a file is
deleted, it is gone, never to return. So be *very* careful when deleting files.

Files are deleted with `rm file [moreFiles]`. To delete the `newname.testfile` in our current
directory:

```
$ ls
```
{: .language-bash}

```
paper backup.txt
```
{: .output}
```
$ rm backup.txt
$ ls
```
{: .language-bash}
```
paper
```
{: .output}

That was simple enough. Empty directories can be deleted with `rmdir`. Non-empty
directories can be removed (along with their content) using `rm -r` (where `-r`
stands for 'recursive').

```
rmdir paper
```
{: .language-bash}
```
rmdir: failed to remove `paper/': Directory not empty
```
{: .output}
```
$ rm -r paper
$ ls
```
{: .language-bash}
```
```
{: .output}

What happened? `rmdir` is unable to remove directories that have files in them.
To delete a directory and everything inside it, we use the `-r` option. This
deletes the directory and all of its content, **without prompting for
confirmation**. Be very careful!

> ## What happens when you use `rm -r` accidentally?
>
> Files deleted using `rm` cannot be recovered. Always be careful when using
> `-r`, and consider using the `-i` option to get a confirmation prompt.
> Alternatively, use `rm` to remove files, and then `rmdir` to remove the
> directory once it is empty.
{: .callout}

## Looking at files

Sometimes it's not practical to read an entire file with `cat`: the file might be way too large,
take a long time to open, or maybe we want to only look at a certain part of the file. As an
example, we are going to look at a large and complex file type used in bioinformatics- a .gtf file.
The GTF2 format is commonly used to describe the location of genetic features in a genome.

Let's grab and unpack a set of demo files for use later. To do this, we'll use
[`wget`](https://www.gnu.org/software/wget/) (`wget link` downloads a file from a link).

```
$ wget {{site.url}}{{site.baseurl}}/files/bash-lesson.tar.gz
```
{: .language-bash}

> ## Another tool: `curl`
>
> [cURL]( https://curl.haxx.se/) is the command-line interface to `libcurl`, a
> powerful library for programming interactions with remote resources over a
> wide variety of network protocols. You can use cURL as an alternative to wget:
>
> ```
> $ curl -O {{site.url}}{{site.baseurl}}/files/bash-lesson.tar.gz
> ```
> {: .language-bash}
{: .callout}

You'll commonly encounter `.tar.gz` archives while working in UNIX. To extract the files from a
`.tar.gz` file, we run the command `tar -xvf filename.tar.gz`:

```
$ tar -xvf bash-lesson.tar.gz
```
{: .language-bash}

```
dmel-all-r6.19.gtf
dmel_unique_protein_isoforms_fb_2016_01.tsv
gene_association.fb
SRR307023_1.fastq
SRR307023_2.fastq
SRR307024_1.fastq
SRR307024_2.fastq
SRR307025_1.fastq
SRR307025_2.fastq
SRR307026_1.fastq
SRR307026_2.fastq
SRR307027_1.fastq
SRR307027_2.fastq
SRR307028_1.fastq
SRR307028_2.fastq
SRR307029_1.fastq
SRR307029_2.fastq
SRR307030_1.fastq
SRR307030_2.fastq
```
{: .output}

> ## Unzipping files
>
> We just unzipped a .tar.gz file for this example. What if we run into other file formats that we
> need to unzip? Just use the handy reference below:
>
> * `gunzip` extracts the contents of .gz files
> * `unzip` extracts the contents of .zip files
> * `tar -xvf` extracts the contents of .tar.gz and .tar.bz2 files
{: .callout}

That is a lot of files! One of these files, `dmel-all-r6.19.gtf` is extremely large, and contains
every annotated feature in the *Drosophila melanogaster* genome. It's a huge file- what happens if
we run `cat` on it? (Press `Ctrl + C` to stop it).

So, `cat` is a really bad option when reading big files... it scrolls through the entire file far
too quickly! What are the alternatives? Try all of these out and see which ones you like best!

- `head file`: Print the top 10 lines in a file to the console. You can control the number of lines
  you see with the `-n numberOfLines` flag.
- `tail file`: Same as `head`, but prints the last 10 lines in a file to the console.
- `less file`: Opens a file and display as much as possible on-screen. You can scroll with `Enter`
  or the arrow keys on your keyboard. Press `q` to close the viewer.

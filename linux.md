---
layout: asides
toc: true
tasks: false
---

# Linux Cheatsheet

By Jamie Flores

## Description

Linux is the OS you all are using on your VM.  Unix (runs on Macs) is another OS that has many of the same commands.
Here are some of the most useful ones!  Hopefully you never open the file explorer on your VM again after reading this!

## Directory Aliases

A directory is what you think of as a folder.

- `.` = your current directory
- `..` = your parent directory
- `~` = your home directory

So, if you are in `~/user/cs104/hw-username/`:

- `.` = `~/user/cs104/hw-username/`
- `..` = `~/user/cs104/`
- and as always `~` = `~`

## Commands

Type these commands directly into your terminal.

### Navigating Directories

- `cd <directory_name>` moves you into that directory.
  So, `cd ..` moves you back into the parent directory of your current directory.
- `ls` lists all the files and directories.
  If you specify a directory in your current directory, `ls <directory_name>` will list the contents of that directory.
  So, if you are in the `hw-username` directory, but you want to see what files you have in your `hw1` directory, simply type `ls hw1`.

### Creating a Directory

- `mkdir <directory_name>` will make a new, empty directory in your current directory with the name `<directory_name>`.

### Moving Files Around

- `cp <src_file_name> <dest_directory_name>` or `cp <src_directory_name> <dest_directory_name>` will COPY `src_file_name` or the contents of `<src_directory_name>` to `<dest_directory_name>` (leaving the original unchanged).
- `mv <src_file_name> <dest_directory_name>` or `mv <src_directory_name> <dest_directory_name>` will MOVE the contents of the source to the destination (the original is no longer at the source).
  This can also be used to rename files.

### Opening a Text Editor

- `subl` opens a blank sublime document.
- `subl <file_name>` opens only that file.
- `subl <directory_name>` opens all the files in the directory and is SUPER CONVENIENT!

NOTE: if you are using VS Code, just replace `subl` with `code`.
For Emacs, replace with `emacs`.

---
title: Linux
nav_order: 4
nav_exclude: false
has_children: true
has_toc: false
permalink: /linux/
---

![linux_logo](https://upload.wikimedia.org/wikipedia/commons/d/dd/Linux_logo.jpg){: width="160" }

# Linux Introduction

---

## 🎯 Learning objectives

By the end of the Linux module, you should be able to:

- use the terminal and understand your current working directory
- navigate the filesystem using absolute and relative paths
- interpret special path symbols such as `~`, `.`, and `..`
- create, inspect, edit, copy, move, rename, and remove files and directories
- find help for commands using man and --help
- search text using `grep`
- combine commands using pipes (`|`)
- extract selected columns from tabular files
- write and execute simple Bash scripts

---

![gui_vs_cli](https://www.fossmint.com/wp-content/uploads/2018/06/Linux-Cli-vs-Gui.png)

## 💻 Why Command Line?

Most people interact with computers through a **graphical user interface (GUI)** by clicking icons, opening menus, and moving files with a mouse.

GUIs are convenient for everyday tasks, but they are inefficient for large, repetitive, or complex analyses. For example, manually combining hundreds of sequencing files would be slow and prone to errors. 

---

## ⌨️ Command-line interface

A **command-line interface (CLI)** allows you to control the computer by entering text commands.

Using the command line, you can:

- process many files at once
- automate repetitive tasks
- combine several tools into a pipeline
- record commands in scripts
- reproduce an analysis
- work on remote servers and computing clusters

These features make the command line particularly important in bioinformatics, where analyses often involve large datasets and multiple processing steps. 

---

## 🐚 The shell

The program that interprets command-line instructions is called a **shell**. One of the most commonly used Unix shells is **Bash**.

The shell is both:

- an interface for running commands
- a scripting language for combining and automating commands

{: .note }
Learning the command line is similar to learning a new language: commands are the vocabulary, while syntax determines how they can be combined.

---

## 🧬 Why it matters for bioinformatics

Many bioinformatics tools are designed primarily for the command line. Shell commands can be combined into reproducible workflows that process large numbers of sequencing files quickly and consistently.

{: .discussion }
What bioinformatics task would be difficult or time-consuming to perform manually through a graphical interface?

## The shell prompt

![prompt](https://linuxconfig.org/images/linux-bash-prompt.jpg)

When you open a terminal, the shell displays a **prompt**. The prompt indicates that the terminal is ready to receive a command.

A typical prompt may look like this:

```bash
user@workstation:~/direcotory$ █
```

The prompt commonly contains:
- `user`            → Current username
- `@`               → Separates the username and computer name
- `workstation`     → Name of the computer or server
- `:`               → Separates the computer name and current directory
- `~/direcotory`    → Current working directory
- `$`               → Indicates that the shell is ready for a command

The exact appearance of the prompt may differ between computers. For those who are curious, you can [customize your bash prompt](https://www.howtogeek.com/307701/how-to-customize-and-colorize-your-bash-prompt/) by changing the color and information contained in it.


---

## Structure of a shell command

Most shell commands follow this general structure:

```bash
command [options] [arguments]
```

For example:

```
ls -l ~
```

This command contains:

| Component | Example | Purpose                                  |
| --------- | ------- | ---------------------------------------- |
| Command   | `ls`    | Specifies the action to perform          |
| Option    | `-l`    | Modifies how the command behaves         |
| Argument  | `~`     | Specifies what the command should act on |

In this example, `ls` lists files, `-l` requests a detailed listing, and `~` specifies the home directory.

{: .warning }
> **Spaces** are important. The shell uses spaces to **separate** commands, options, and arguments!
>
> For example:
> `ls -l` is valid, while `ls-l` causes the shell to search for a command named `ls-l`.

---

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

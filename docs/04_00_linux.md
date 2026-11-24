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
![gui_vs_cli](https://www.fossmint.com/wp-content/uploads/2018/06/Linux-Cli-vs-Gui.png)

## 💻 Why Command Line?

Most people interact with computers through a graphical user interface (GUI) by clicking icons, opening menus, and moving files with a mouse. GUIs are convenient for everyday tasks like copying single files and folders, but they are inefficient for large, repetitive, or complex analyses as the ones we do in bioinformatics. For example, manually merging hundreds of sequencing files would be slow and prone to errors. 

A command-line interface (CLI) allows you to control the computer by entering text commands.

Using the command line, you can:

- process many files at once
- automate repetitive tasks
- combine several tools into a pipeline
- record commands in scripts
- reproduce a task
- work on remote servers

---

## 🧬 Why it matters for bioinformatics

Many bioinformatics tools are designed primarily for the command line. Shell commands can be combined into reproducible workflows that process large numbers of sequencing files quickly and consistently.

{: .discussion }
What bioinformatics task would be difficult or time-consuming to perform manually through a graphical interface?

---

## 🖥️ What am I working in?

When you work on the command line, several layers work together:

- **GUI** (graphical user interface): you click icons, menus, and windows.
- **CLI** (command-line interface): you type text commands.
- **Terminal**: the window where you type commands and see the output.
- **Shell**: the program behind the terminal. It reads your command, runs it, and shows you the result.
- **Bash**: the most common shell on Linux, and the one we use in this workshop.

What happens when you press Enter:

```
You type a command
  → in the terminal (the window)
  → the shell, e.g. Bash, reads and runs it
  → Linux (the operating system) does the work
  → the result appears in the terminal
```

The shell is both:

- an interface for running commands
- a scripting language for combining and automating commands

{: .note }
Learning the command line is similar to learning a new language: commands are the vocabulary, while syntax determines is the grammar, i.e. how they can be combined.

---

## 🔎 How do I read a line in the terminal?

### The shell prompt

![prompt](https://linuxconfig.org/images/linux-bash-prompt.jpg)

When you open a terminal, the shell displays a prompt. The prompt indicates that the terminal is ready to receive a command.

A typical prompt may look like this:

```bash
user@workstation:~/directory$ █
```

The prompt commonly contains:

- `user` → Current username
- `@` → Separates the username and computer name
- `workstation` → Name of the computer or server (the host)
- `:` → Separates the computer name and current directory
- `~/directory` → Current working directory (`~` is short for your home directory)
- `$` → Indicates that the shell is ready for a command

The exact appearance of the prompt may differ between computers. For those who are curious, you can [customize your bash prompt](https://www.howtogeek.com/307701/how-to-customize-and-colorize-your-bash-prompt/) by changing the color and information contained in it.

### Structure of a shell command

Most shell commands follow this general structure:

```bash
command [options] [arguments]
```

For example:

```bash
ls -l ~
```

This command contains:

| Component | Example | Purpose                                  |
| --------- | ------- | ---------------------------------------- |
| Command   | `ls`    | Specifies the action to perform          |
| Option    | `-l`    | Modifies how the command behaves         |
| Argument  | `~`     | Specifies what the command should act on |

In this example, `ls` lists files, `-l` requests a detailed listing, and `~` specifies the home directory.

Options start with a dash. Short options use one dash and a single letter (`-l`), long options use two dashes and a word (`--help`). Options and arguments are optional: `ls` on its own works, too.

{: .warning }
> Spaces are important. The shell uses spaces to separate commands, options, and arguments.
>
> For example:
> `ls -l` is valid, while `ls-l` causes the shell to search for a command named `ls-l`.

### Putting it together: one line, part by part

A line in the terminal often contains both the prompt and a command:

```bash
ziska@laptop:~/data$ ls -l /home
```

| Part | In the example | Meaning |
| --- | --- | --- |
| User | `ziska` | Who is logged in |
| Host | `laptop` | The name of the computer |
| Current directory | `~/data` | The folder you are in right now |
| `$` | `$` | End of the prompt: the shell is ready |
| Command | `ls` | The action: list files and folders |
| Option | `-l` | Changes how `ls` behaves: show details |
| Argument | `/home` | What `ls` acts on: the folder `/home` |

{: .note }
Everything up to the `$` is written by the shell. Everything after it is what you type.

---

## 📌 Terminology at a glance

| Term | Meaning |
| --- | --- |
| GUI | Graphical user interface: you click icons, menus, and windows |
| CLI | Command-line interface: you type text commands |
| Terminal | The window where you type commands and see the output |
| Shell | The program behind the terminal: reads your command, runs it, shows the result |
| Bash | The most common shell on Linux, used in this workshop |
| Prompt | The text before the cursor; shows the shell is ready |
| User | Who is logged in |
| Host | The name of the computer |
| Current directory | The folder you are in right now (`~` = your home directory) |
| `$` | End of the prompt; you type after it |
| Command | The action to perform, e.g. `ls` |
| Option | Changes how the command behaves; starts with `-` or `--`, e.g. `-l` |
| Argument | What the command acts on, e.g. `/home` |

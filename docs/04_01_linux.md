---
title: Linux - First Commands and Getting Help
parent: Linux
nav_order: 1
nav_exclude: false
permalink: /linux_first_commands/
---

# Linux Basics: Prompt, First Commands, and Getting Help

---

## 🎯 Learning objectives

By the end of this lesson, you should be able to:

- Identify the parts of the shell prompt: `user`, `host`, current directory, and `$`
- Identify the command, option(s) and argument(s) in a given shell command
- Run `pwd` and `ls` to check your location in the file system
- Look up command options with `--help` or `man` to find what an unfamiliar option does

---

## Before you start

Open a terminal in VSCodium via Terminal → New Terminal. You start in your home directory. Stay there for this whole lesson. Moving around the file system comes in the [next lesson](04_02_linux.md).

---

## 1. Read your prompt

Look at the last line in your terminal. This is your prompt. It may look similar to this:

```bash
user@workstation:~$
```

{: .task}
> - Write your own prompt on paper and label the `user`, the `host`, the current directory, and the `$`.
> - Compare your prompt with your neighbor's. Which parts are different, and why?

Now ask the shell where you are:

```bash
pwd
```

{: .discussion}
> - Compare the output of `pwd` with the current directory shown in your prompt. What does `~` stand for?

---

## 2. Your first commands

List the contents of the current directory:

```bash
ls
```

Try a few common variants:

```bash
ls .
ls ..
ls -l
ls -lh
ls -lt
```

{: .tip}
> Several short options can be combined behind one dash:
>
> - `ls -lh` is the same as `ls -l -h`
> - The order does not matter: `ls -lt` and `ls -tl` give the same result

{: .task}
> For each command above, name the command, the option(s) and the argument(s). Which commands have no option or no argument?

{: .discussion}
> - What is the difference between `ls`, `ls -l`, and `ls -lh`?
> - What does `ls ..` show?
> - Which option sorts by modification time, newest first?

---

## 3. Getting help

Most commands provide built-in documentation. Open the manual page of `ls`:

```bash
man ls
```

Scroll with the arrow keys or `Space`, and press `q` to quit.

{: .note}
> On some WSL installations, `man` is not available and only shows a message about a "minimized" system. In this case, use `--help` instead.

Or use the short help page:

```bash
ls --help
cp --help
mv --help
rm --help
mkdir --help
```

{: .discussion}
> - Which `ls` option shows human-readable file sizes?
> - Which `cp` option lets you copy directories recursively?

{: .task}
> Stretch task: try `man cd`. Why does it fail? Find out how to get help for `cd` anyway (hint: `help cd`).

---

## 4. Useful terminal habits

Try these shortcuts:

- Press the `↑` Up Arrow to reuse previous commands.
- Type `cle` and press Tab to autocomplete `clear`.
- Type `ls ~/2026` and press Tab to autocomplete the path.

Linux is case-sensitive:

- `clear` works
- `CLEAR` does not

---

## 📌 Summary

| Command | Purpose |
| --- | --- |
| `pwd` | Show current directory |
| `ls` | List files and folders |
| `ls -l` | List with details |
| `ls -lh` | List with details and human-readable sizes |
| `ls -lt` | List with details, newest first |
| `man <command>` | Open the manual (quit with `q`) |
| `<command> --help` | Show short help |
| `help <command>` | Show help for commands built into the shell, such as `cd` |
| `clear` | Clear the terminal screen |

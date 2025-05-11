---
layout: post
title:  "How to setup GNU Stow"
date:   2025-05-11 16:03:20 +0200
categories: jekyll update
---

<img src="/assets/images/dotfile.png" alt="stow" height=300px>

If there's one UNIX tool that makes managing dotfiles across multiple machines effortless, it's **GNU Stow**.

When used correctly, it enables version control for your configuration files and makes tracking changes seamless.
Setting up your dotfiles on a new machine also becomes a simple, three-step process.

In this tutorial, I'll help you setup stow for your UNIX systems (Linux, Mac, BSD) so you can finally make dotfiles management easier.

# Woah woah, what's a GNU Stow?

As said by the stow documentation:

GNU Stow is a symlink farm manager which takes distinct packages of software and/or data located in separate directories on the filesystem,
and makes them appear to be installed in the same place.

Not the best way to describe it, so here is a simple version of what stow does:

Stow is a commandline tool that automates the creation of symlinks on your machine. Therefore it can create links from one part of the system to the other.

For example:
    .config/i3 could be a symbolic link that actually points to ~/dotfiles/i3/.config/i3

### Symlink?
as a quick explainer: a symlink is a file that acts as a pointer to another file.
So when you open the i3 file in .config/i3 it will open the file that is located in ~/dotfiles/i3/.config/i3

# Lets download Stow

Before you start implementing what i'll be showing of, it's a good idea to make sure that **Git** is installed on your system

you can download both git and stow through your package manager:

```bash
# For mac
brew install git stow

# For Debian systems
sudo apt install git stow

# For OpenSUSE systems
sudo zypper install git stow

# For Arch systems
sudo pacman -S git stow
```

If everything went alright, you should now have everything you need to continue, great!

# Setting up a dotfiles directory

In my experience, stow works best when you symlink from a directory somewhere in your home directory.
so open the commandline on your system and make sure that you are in your home directory:

```bash
cd $HOME

# Just typing cd will do the same thing
cd
```
Then create a directory, you can choose what name to give, but for this tutorial I will go with **dotfiles**:

```bash
mkdir dotfiles

# cd into your new directory
cd dotfiles
```
# How to add config files in stow

Stow has a certain way off adding dotfiles that might seem a bit weird, but makes it very easy to work with.
Inside of dotfiles you create a new folder with the name of the package, and inside of there you will recreate the path of the actual dotfiles.
Here is a small example using zsh and neovim:

```txt
dotfiles/
├── zsh/
│   ├── .zshrc
│   └── .zsh_aliases
├── nvim/
│   └── .config/nvim/init.lua
```

So lets implement it for zsh!
go into your dotfiles folder and create a new directory named 'zsh':

```bash
mkdir zsh
cd zsh
```

Then we need to move our current .zshrc file into this new directory.

```bash
# Move your zshrc into your current directory
mv ~/.zshrc ~/dotfiles/zsh/
```
In order to test if this has worked, open a new terminal, and check if you see a message saying that no zshrc has been found.
**Do not create a new one!** We are going to fix this problem right now.

Alright go back into your dotfiles directory and type out the following command:

```bash
stow zsh
```
No errors? Good, that means the symlinking was successful.
If you look in your file explorer, you should see a .zshrc there with a special sign depending on the file explorer that you use.

Can't see the file? Make sure that hidden files and folders are visible in your file explorer.

Let's try and open a new terminal, if everything went alright your zsh config should be there when you first start your shell.
Not entirely sure? Add a debug message into your config to make sure. If you see that message, you know it worked.

```bash
echo "It works!" >> ~/dotfiles/zsh/.zshrc
```

# Adding version control

This step is by far the most simple.
Go into your dotfiles folder and initialize a new Git repository

```bash
git init .
```
And that's it, if you want to link it up to a github repository, you can use the following command.
Make sure that you create your own **PRIVATE** repository and use your own link for this:

```bash
git remote add origin https://github.com/your-username/your-repo-name.git
```

In order to safe your changes just use the following commands:

```bash
# Stage all files in your dotfiles directory
git add .

# Create a commit with message
git commit -m "Added: zsh config"

# And... Push!
# change main to the name of your branch (fe: master, head)
git push -u origin main
```

# conclusion

GNU Stow might take a bit of time before you really understand it, but that's oke!
It's only when you understand the basics that you can see what cool things you can do with it.
For example: I used git branches to create different themes for my stowed dotfiles, even if it isn't the cleanest way to do things, it works surprisingly well.

Thank you for following along, and enjoy your newfound vision of the art of dotfiles management!

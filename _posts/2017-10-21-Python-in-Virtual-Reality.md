---
layout: post
title: "Virtual Reality for Python"
date: 2017-10-21
tag: Python
blog: true
author: chidonna
projects: false
summary: "Virtual Environments for Python"
headerImage: false
draft: false
---
![Virtual Headset]({{ site.url }}/assets/images/virtual.jpg)
Growing up (i.e learning programming, of course), I started with Python, and ended up breaking something. Why that was always happening, I never knew.

Well at least now I am older than I was then and I can see clearly, that life is not like that. Life is difficult, hard and mistakes are bound to be made and those can come back to hunt you, except the mistake is done in a "virtual world" of course.

Working in a virtual environment makes mistakes acceptable and easy to handle. This post is for those coming into programming through Python or learning Python from another language. If you already work in a virtual environment, then this blog post is not for you. However, if you work in virtual environment and you are not using **[virtualenvwrapper][virtualenvwrapper]** then this post is definitely for you.

## What is a Virtual Environment
A virtual environment is what the name suggests, an environment that is not "really real". Well it is not completely like that. It really is a layer beyond the reach of your computer operating system. Packages, frameworks or libraries installed in a virtual environment can not be accessed outside the virtual environment in which it was created.

## Why Virtual
Reality is harsh, No one likes reality.

Well in this case the reality of your computer is harsh and could only get worse, if you do not learn to manage your resources well. As every government should manage the resources of their nation, to make reality not seem worse than it is, so should you manage those of your computer.

One reason you will need a virtual environment is, you do not want your computer littered with packages you do not really make use of (you only needed it for this one program you wrote and you do not want to remove it, because you might need it again). Also you might need version 1 for program A you wrote, and version 2 of that package for program 2 you are writing, how do you handle different versions of the same package, Virtual Environment !!!

Another Reason is, installing Python packages in the {superuser of your operating system} can cause a lot of problems and this is coming from someone with battle scars to show for it. Problems such as, breaking [pip](https://pypi.python.org/pypi/pip/) (pip is a package management system used to install and manage software packages written in Python), which is not a funny problem to have. And this is a bonus, if you want to install a package you feel you should not be installed in a virtual environment, use this:

```shell
pip install --user {package}
```

This way it is not installed in the {superuser of your operating system}, and cannot affect pip.

I hope I have been able to convince you and not to confuse you as to why you should a virtual environment in Python. So now we go into setting up a virtual environment for python

## Setting Up a Virtual Environment
Setting up a virtual environment for python is very easy
First, make sure you have Python installed (of course):

```shell
$ python -V
```
And pip installed:

```shell
$ pip --version
```
If you do not visit [here](https://pypi.python.org/pypi/pip/), to get pip installed.

### Virtualenv and Virtualenvwrapper
For our virtual environment, we will make use of the "[virtualenv](https://virtualenv.pypa.io/en/stable/)", which is a tool to create isolated Python environments. virtualenv creates a folder for any virtual environment you create in a directory, which would contain all the packages you install while in that virtual environment. From the virtualenv home documentation page, they said this:
> It creates an environment that has its own installation directories, that doesn't share libraries with other virtualenv environments (and optionally doesn't access the globally installed libraries either).

While using "virtualenv" alone would be good, creating and deleting and activating a virtual environment can get annoying sometimes. So we would be using it along with [virtualenvwrapper][virtualenvwrapper].

virtualenvwrapper provides a set of extensions to virtualenv. It creates means for handling creating, deleting, activating and managing your development work flow effectively.

```shell
$ pip install virtualenvwrapper
```

This command installs both the virtualenv (since it's a dependency of virtualenvwrapper) and virtualenvwrapper.
After installation is complete, edit your ".bashrc" or ".bashrc_profile" and add the following:

```sh
export WORKON_HOME=$HOME/{a directory of your choice}
source ~/.local/bin/virtualenvwrapper.sh
```

Then we source the "bashrc" and create the `WORKON_HOME` directory we indicated in the "bashrc"

```shell
$ source ~/.bashrc
$ mkdir -p $WORKON_HOME
```

And now you can easily create your virtual environments, or delete or activate them.

```shell
$ mkvirtualenv web-bot
$ pip install Scrappy beautifulsoup4
$ workon web-bot
```

The `mkvirtualenv` command creates a virtual environment called "web-bot", then the `pip install Scrappy beautifulsoup4` installs both the "Beautiful Soup" and "Scrappy" packages that you would most likely need if you wanted to create a web bot or web scrapper or your own search engine in python.

To deactivate a virtual environment:

```shell
$ deactivate
```

This deactivates the current virtual environment you are working in.
Check out the [documentation][virtualenvwrapper] for the virtualenvwrapper, if you want to know the various other commands that exist for managing you virtual environment effectively.

# Conclusion
With this blog post, I hope you saw reasons as to why you should immediately start working in a virtual environment if you using Python, or start making use of "virtualenvwrapper", if you are not.

If you have anything to say to me suggestions, questions, concerns or you just disagree with me, feel free to [email](mailto:pogbonna34@gmail.com) me or send me a tweet. I would make sure to reply ASAP.
If you feeling particularly charitable, you can follow me on twitter or github just to see what am up to :wink:.


[virtualenvwrapper]: http://virtualenvwrapper.readthedocs.io/en/latest/

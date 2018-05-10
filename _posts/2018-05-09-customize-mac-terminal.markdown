---
layout: post
title:  "Customize Mac Terminal"
date:   2018-05-09 16:20:32 -0500
categories: Mac
---

### Getting bored with your Mac terminal??
When you first get your Macbook, you are so execited and thinking you'll become a geek. When you open you terminal, you are assuming you'll get a fansy UI you saw on other people's screen. However, what you actually get is this:
![image]({{"/assets/2018-5-10/terminal.jpg" | relative_url}})

Here's a simple way to do to make it looks better:
1. Open you white bored Terminal and type <span style="color:Brown">nano .bash_profile</span>.
2. Paste below scripts in the file:
{% highlight bash %}
export PS1="\[\033[36m\]\u\[\033[m\]@\[\033[32m\]\h:\[\033[33;1m\]\w\[\033[m\]\$ "
export CLICOLOR=1
export LSCOLORS=ExFxBxDxCxegedabagacad
alias ls='ls -GFh'
{% endhighlight %}
3. Press <span style="color:Brown"> Control + O </span> to save your change. And press <span style="color:Brown"> Control + X </span> to exit.
4. In the terminal, type <span style="color:Brown"> cat .bash_profile</span> to verify your change.
5. Close the current terminal window and open a new one.

It will become like this:
![image]({{"/assets/2018-5-10/new_terminal.jpg" | relative_url}})
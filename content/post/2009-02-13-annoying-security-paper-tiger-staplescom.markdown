---
author: JD Long
date: 2009-02-13 20:31:57+00:00
draft: false
title: Annoying Security Paper Tiger - Staples.com
type: post
url: /2009/02/annoying-security-paper-tiger-staplescom/
tags:
- rant
---

Today I have a couple I want to print for reading later and I want to print them duplex so the stack of crap would be smaller. I need to go to Staples (the office supply store) later so I thought I would just use their online printing tool to print the document at their store and then pick it up when I go there later today. As with most sites they want a bunch of personal info from me so I diligently fill out 15 text boxes. As with most sites they want me to make a username and password. Fine. I use [MiniSafe desktop and MiniSafe Blackberry ](http://www.simprit.com/minisafe_bb/)app to keep my passwords straight. I also use a simple algorithm for password creation to keep things from getting too complex. I always use a number followed by a phrase followed by the name of the site. So for Staples I wanted to use the pass '45JimmyCrackCorn_staples'. I was shocked when the site returned an error saying the "password is not strong"

[caption id="attachment_139" align="alignnone" width="493" caption="Staples Wants More Entropy"]![staples_sux1](https://www.cerebralmastication.com/wp-content/uploads/2009/02/staples_sux1.jpg)
[/caption]

This blows my mind. My password is 24 characters long, contains numbers, mixed case,  and punctuation yet they think it is not strong? Damn. So I try again with a new routine adding more numbers more punctuation. Same error. Finally I used MiniSafe to produce a 30 character random password which I appended with "_staples". Here's the password:


<blockquote>r!-MzZDsXhczm&m#$@L%25HXw66cnb_staples</blockquote>


Looks friggin' strong to me! But not to staples.com. I got the same error. I am left to conclude that Staples.com requires a password that does not contain the string "staples". There is no warning of this. But since I entered a password with more entropy than Lyle Lovet's hair I can only assume that the presence of the string "staples" causes the site to reject my password.

So after trial and error led me to this conclusion I chose a password without the forbidden string in it. Which password did I choose? The following:


<blockquote>password</blockquote>


Yep, and staples.com took it without so much as a warning. Nice ey? And their backup question about the name of my first dog? Yeah, his name was Rex. So my password is 'password' and my first dog is Rex. But my password with over 138 bits of entropy was rejected. I'm going to Office Max.

<!-- more -->Technical Detail:

If you want to know how many bits of strength or entropy a password has, the [Wikipedia article on password strength](http://en.wikipedia.org/wiki/Password_strength) is a great place to start. My 30 character password was created with a generator that used all alphabet characters upper and lower case (26*2), all numbers (10) and 10 punctuation marks for a total of 72 possibilities for each of the 30 characters. To calculate the entropy I used the formula from Wikipedia and got:

30 * Log2(72) = 185.1

+++
date = "2016-04-05T17:49:51+03:00"
title = "Yet another money manager"

+++

I got a little bored and decided to do some web development. I did several ASP.NET Web Forms projects back in university days and this is as far as my web development experience goes. That's why I needed to go through a bunch of MVA courses and to read Jon Galloway's "Professional ASP.NET MVC 5" book to update my skills. Just to test myself I wanted to develop a small project that would be at least of some use for me and give me enough drive to finish it.

<!--more-->

I'm trying to control how much money I spend and I need a solution to help me with this. There are plenty of expense managers already, but all of them never quite meet everyone's exact dreams and wants as everyone handles it a little differently. They are also either not free, or with ads, or provide no cloud synchronization. 

## Project description

The goal of this project is to help you track your expenses and incomes. I want to keep it as simple as possible. I'd like to have a handy way to note my expenses anywhere I go with my phone and to have it all aggregated in a convenient form on my PC later.
The core of this solution is ASP.NET MVC application and a Web API for synchronization with an Android client. Android app is still in development.

Web application screenshot is here just to get your attention (Disclaimer: I'm not a designer).

<div class="standardBorder verticalMargins" markdown="1">
	<img src="/images/moneyManScreen.PNG">
</div>

## How can it interest you?

There are many similar projects. But none of them combine both of the following features:

1. Open source. You are free to fork it and modify it to meet your needs. You have a full control over your data. You can host your own copy anywhere you want.
2. Both Android and web clients with synchronization.

Some good alternatives:

1. [Money Manager EX](http://www.moneymanagerex.org) - a free, open-source, cross-platform personal finance software.  Unfortunately it doesn't have a web client and DropBox is required for synchronization.
2. [AndroMoney](https://web.andromoney.com) - easy to use expense manager for Android, iOS and web.

## Money Man links:

[Sources on GitHub](https://github.com/tolikcode/MoneyMan)

[Running application in Azure](http://moneyman.azurewebsites.net/)
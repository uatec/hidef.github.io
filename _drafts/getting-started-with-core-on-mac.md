---
title: Getting Started With .NET Core on Mac
<!--date: 2017-01-03-->
tags: [".NET Core", "Mac", "Linux"]
layout: post
---

I've been writing C# for years and that has meant that I have always been constrained to working with Windows.

Not any more! With the advent of .NET Core and Visual Studio Code I am finally free to move completely to a Unix environment and take advantage of all the tools and technologies that are available in Mac OS or Linux environments.

Here are the steps I took to get my new .NET Core projects up and running.

## Install .NET Core

Getting .NET Core environment installed is actually very simple.

The [.NET Core Download Page](https://www.microsoft.com/net/core) is quite complete.

## The IDE

Visual Studio Code is a god send. Unlike it's namesake Visual Studio, Code is fully cross platform (being implemented on top of Github's Electron platform). It's also very light weight.

[Download](https://code.visualstudio.com/download) Visual Studio Code and install it. This will take minutes as the download package is only 51meg for mac.

There are three points about Visual Studio Code that make it a winner for me.

### It's light weight

Being built on Electron, which is in turn built on Chrome, it quick to start and very responsive. It feels like you're working in a text editor, but you have the power of an IDE behind you.

### Breadth of tooling

It has a very rich collection of plugins. There must be one for every language under the sun and having many plugins installed does not seem to slow Code down.

### Depth of tooling

Language support is not just limited to a bit of syntax highlighting. Many language plugins also provide Intellisense (intelligent code completion), Code Lens (sneak peaks at object definitions, reference counts or test runners, inline with your code) and proper integrated debugging.

Environments that support integrated debugging include C# and Node.JS, but also Unity3D, Java, Python, Bash, Ruby, PHP... it seems like everything. I have personally never come across an IDE with such a variety of tools. The .Net support is obviously excellent, given Code's background, but this phenomenal amount of tooling means that Code is an excellent defacto choice for many different environments and technologies.

## Creating The Project

TODO 

Finally we need to retrieve all the referenced dependencies, again through the CLI. This will restore all dependencies on all projects in child directories.

    dotnet restore

## Running and Testing Locally



    cd tests
    dotnet test

    cd src
    dotnet run

## Publishing

appveyor
drone io
myget

Heroku Docker

Link to 12 Factor

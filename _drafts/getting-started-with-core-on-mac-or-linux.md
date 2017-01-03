---
title: Getting Started With .NET Core on Mac or Linux
<!--date: 2017-01-03-->
tags: [".NET Core", "Mac", "Linux", "Visual Studio Code"]
layout: post
---

I've been writing C# for years and that has meant that I have always been constrained to working with Windows.

Not any more! With the advent of .NET Core and Visual Studio Code I am finally free to move completely to a Unix environment and take advantage of all the tools and technologies that are available in Mac OS or Linux environments.

Here are the steps I took to get my new .NET Core projects up and running.

<!--more-->

(I usually work on a Mac, but for the purposes of this article, everything I write here has been tested on a clean Debian installation.)

## Install .NET Core

Getting .NET Core environment installed is actually quite simple. Unfortunately each platform has a slightly different approach so you're best heading over to the  [.NET Core Download Page](https://www.microsoft.com/net/core) and following the instructions for your environment. At the time of writing, the latest version (and the version I will be referencing later on is 1.1.0).

## The IDE

Visual Studio Code is a god send. Unlike it's namesake Visual Studio, Code is fully cross platform (being implemented on top of Github's Electron platform). It's also very light weight.

[Download](https://code.visualstudio.com/download) Visual Studio Code and install it. This will take minutes as the download package is only 51meg for mac.

There are three points about Visual Studio Code that make it a winner for me:

- It's light weight
- It's got support for many different languages/environments
- It's a fully fledged IDE with debugger and intellisense support.

Once installed, it's helpful to install the `code` command so you can launch Code and open files from the command line, e.g. `code myfile.cs`.

    Cmd/Ctrl + P > Install 'code' command

### Extensions

There are many extensions but we only need one right now. Hit `Cmd/Ctrl + Shift + X` to open the Extensions manager and install the `C#` extension. Once you've found the extension you want, it's a click to install it, and then it'll ask you to restart the window. You'll be ready in seconds.

## Creating A New Project

### Yeoman

I use Yeoman to do alot of the boiler plate when creating a new project. the `aspnet-generator` is published by the [Omnisharp](http://www.omnisharp.net/) team which, although not endorsed by Microsoft, are closely connected to the Visual Studio Code team, and the C# extension (a core extension for Code) is driven by Omnisharp. 

#### Install Node

##### Mac with Home Brew

    brew install node

##### Ubuntu/Debian

    curl -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -
    sudo apt-get install -y nodejs

#### Install yeoman

    npm i -g yo

#### Install the omnisharp dotnet generator

    npm i -g generator-aspnet

#### Create the Library

    yo aspnet
    > Class Library
    Name: CoreBootstrapLibrary

#### Create the test Library

    yo aspnet
    > Unit test project (xUnit.net)
    Name: CoreBootstrapLibrary.Tests

#### Update Yeoman's .NET version

Yeoman is quite helpful, but it *does* make a few assumptions that don't quite fit.

Firstly, the Yeoman generated code references NetCore 1.0.1, but the currently latest version is 1.1.0, so we need to upgrade the test project. In `/CoreBootstrapLibrary.Tests/project.json` we must change `Microsoft.NetCore.App` version from `1.0.1` to `1.1.0` so file contains this section:

    "frameworks": {
        "netcoreapp1.0": {
            "dependencies": {
                "Microsoft.NETCore.App": {
                    "type": "platform",
                    "version": "1.1.0"
                }
            }
        }
    },

#### Tidy up Yeoman's .gitignore files

Secondly let's remove the .gitignore files from the project folders and put them at the root of the project.

    mv CoreBootstrapLibrary/.gitignore ./.gitignore
    rm CoreBootstrapLibrary.Tests/.gitignore

#### Initiate our directory as a git repo

    git init
    git add .
    git commit -m "Initial Commit"

#### Restore initial dependencies

Finally we need to retrieve all the referenced dependencies, again through the CLI. This will restore all dependencies on all projects in child directories. The first time you run this, it has to download the whole of .NET which, althought not as big as in the old days, still takes a few minutes.

    dotnet restore

## Add Some Code and Tests

I'm not going to walk you through writing code or tests, but obviously we need to make the test project reference the code project.

In `CoreBootstrapLibrary.Tests/project.json`:

    {
        ...
        "dependencies": {
            ...
            "CoreBootstrapLibrary": {
                "target": "project"
            }
        },
        ...

We can run the tests using the `test` command:

    cd tests
    dotnet test

## Publishing Nuget Packages with AppVeyor

There are many cloud CI services out there, and finding one which is right for you is like buying a new pair of shoes. However I don't think I need to explain the meaning of Agile, so I'm going to recommend that you dive in with [AppVeyor](https://www.appveyor.com/) and then you can change your mind later, if you see fit.

AppVeyor has a nice interface for adding new projects, is configurable using an `appveyor.yml` file in the root of your project, supports .NET Core natively, and it's all round pretty good. It even has a free tier for public projects and is quite reasonably priced for private ones too, and mighty fine documentation to boot.

Follow the [Documentation](https://www.appveyor.com/docs/) for how to set up a new project in AppVeyor, and you'll have it hooked up to your git repo in no time. Then we just need to get it configured.

By default AppVeyor will discover traditional .Net solutions, build them and run tests. They're not that slick for .NET Core yet though.

Dropping this script in the root of your repository will tell AppVeyor how to build, test and publish our project as a NuGet package.

`appveyor.yml`

    version: 0.0.{build}
    build_script:
    - cmd: |
        dotnet restore
        dotnet build CoreBootstrapLibrary
        dotnet pack CoreBootstrapLibrary
    test_script:
    - cmd: >
        dotnet test CoreBootstrapLibrary.Tests
    artifacts:
    - path: '**/*.nupkg'

The last bit of AppVeyor goodness is that any `*.nupkg` files published as artifacts (in this case, our library and it's symbols) are published to a private NuGet feed for your account. Access control is a bit of a blunt instrument, but for internal teams, or your own private projects, it's a great starting point.

## Consuming Private Nuget Packages

One of the differences of working in a Unix environment, rather than Windows, is that we no longer configure Visual Studio with our NuGet sources. Instead we configure them in our environment. Our per-user NuGet.config file is located at `    ~/.nuget/NuGet/NuGet.config`, so to add our private NuGet source with our newly built packages, we just have to add a PackageSource and a set of credentials.

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
        <packageSources>
            ...
            <add key="Appveyor" value="https://ci.appveyor.com/nuget/<my account>" />
        </packageSources>
        <packageSourceCredentials>
            <Appveyor>
                <add key="Username" value="<appveyor email>" />
                <add key="ClearTextPassword" value="<appveyor password>" />
            </Appveyor>
        </packageSourceCredentials>
    </configuration>  

Unfortunately I wasn't able to find any documentation on how to encrypt the password stored in this file, other than that it is possible to do so.

Now we can create an application to consume this package.

## Creating a Service

One of the beauties of a Unix environment is that you don't need to create complicated Windows Services with an installer and a framework like TopShelf (much respect to TopShelf, but it's filling a gap that should never have existed).

A service can be a simple console application. You can run it on the console you self to test it, or you can run it using `upstart` in Ubuntu or inside a docker container in exactly the same way.

So in a new directory for a new project:

    yo aspnet
    > Console application
    CoreBootstrapApp

The new console application can by run using the `run` command:

    cd CoreBootstrapApp
    dotnet run

Next we add the dependency for the NuGet package we created in `CoreBootstrapApp/project.json`:

    {
        ...
        "dependencies": {
            ...
            "CoreBootstrapLibrary": "*",
        },
        
And the rest is up to you. You can now write library code, publish it to a private NuGet feed using AppVeyor and consume it from your own application.

Another time I will write about hosting your new .NET Core application in Docker courtesy of Heroku, and implementing a 12 Factor app using .NET Core.

You can find the code for these example projects here:
- [CoreBootstrapLibrary](https://github.com/hidef/CoreBootstrapLibrary)
- [CoreBootstrapApp](https://github.com/hidef/CoreBootstrapApp)

And CI for the Library over at [AppVeyor](https://ci.appveyor.com/project/uatec/corebootstraplibrary)

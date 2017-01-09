---
title: .NET Core with Docker and Heroku
---
## Why Docker?

separation of concerns

isolation

replication of prod in dev/test

alternatives: e.g. rkt

## Why Heroku?

free tier

## Running .NET Core in Docker

bind individual port



Upload 

    $PORT : 5000

    FROM microsoft/dotnet:latest
    RUN ["mkdir", "/app"]
    COPY TwelveDotNet/project.json /app
    WORKDIR /app
    RUN ["dotnet", "restore"]
    COPY TwelveDotNet /app
    RUN ["dotnet", "build"]
    ENV PORT 5000
    EXPOSE 5000/tcp
    CMD ["dotnet", "run"]



## Using Heroku With Docker
heroku plugins:install heroku-container-registry
heroku container:login
heroku create
heroku container:push web

## Limitations

no internal network linking
no volume mounted storage
only use userspace clients to s3 or blob storage

not run as root user, docker usually is. 

test it with your own user

    RUN adduser -D myuser
    USER myuser

HTTP only
Only a single port
Public Only

## Where do we go from here?

Amazon EC2 Container Service
Azure Container Smush

kubernetes
with deis

Mesos

cloud foundry
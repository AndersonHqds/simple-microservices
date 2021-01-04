## Overview

This is a simple project to study microservices, is important to say that this project should not be used in production, We have more ways and tools to manage a real project, so please, use this project only for study.

- **[Requirements](#-getting-started)**
- **[Getting Started](#-how-it-works?)**

### Requirements

- [Kubernetes](https://www.docker.com/products/kubernetes)
- [Docker](https://docs.docker.com/get-docker/)
- [Scaffold](https://skaffold.dev/)

### Getting started

This project will use kubernetes and docker to start a **ReactJS** application and multiple services using **NodeJS**. The first thing you should do is run the following command.

**Install the ingress nginx**

```
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.43.0/deploy/static/provider/cloud/deploy.yaml
```

**Edit your hosts file**
We need to open the /etc/hosts **(C:\Windows\System32\drivers\etc\hosts for windows users)** file and add the following line in the end of the file.

```
127.0.0.1 posts.com
```

**Run the Skaffold**
In the root folder (The same of skaffold.yaml file), We should run the following command.

```
skaffold dev
```

This command is enough to start our kubernetes deployments, pods, services and run our docker containers. After that you can access **posts.com** in the browser and you should see a simple React application.

### How it works?

My goal here is study the microservice base concept and learn how we can create it without use of tools. All applications (services) be in a docker container, so each one has a own Dockerfile. We also have inside the folder **infra/k8s** a lot of **deployments** configs file for each service, it will be used for the Kubernetes and inside the each **depl.yaml** file, We also have a **service** (service in kubernetes) created. We have 4 services (service in node) with the descriptions bellow.

**Services**

- **Event Bus** - This service will receive the events triggered by the other services and it will forward to the correct service. So if a service wants change a data or just communicate with another one, we should use event bus.
- **Post** - The post service will receive the post request that will came from client (React App) for create a new post. It will send a event to **event-bus** and save this post in **query service**.
- **Comments** - This service will receive the comment request and create a new comment with a **pending status** (It will be used to moderate the comment) sending a event to **event-bus** service. It also receives an event with the **moderated type** for update the comment status.
- **Moderation** - This service will receive an comment event to check if we have the **orange** word, if it does, we need to change the comment status to **rejected** otherwise we change the status for **approved**.
- **Query** - This service will store the **posts** and **comments**, so when the posts or comments service stop to working, We'll still have our comments and posts working for read purposes.

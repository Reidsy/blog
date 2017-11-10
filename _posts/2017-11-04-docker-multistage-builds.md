---
layout: post
title:  "Docker Multi-Stage Builds"
date:   2017-11-04
category: code
---

Back in April I attended Dockercon where the folks on the Docker team announced Multi-Stage Builds.
Multi-stage builds allow the use of multiple base containers during the build process, opening up the possibility to use different types of containers for different jobs.

In an EmberJS application there is often two stages:
- Compiling and minifying the css and javascript assets
- Serving the assets from a webserver

```
# Compile the assets
FROM node:8.9 as build
WORKDIR /usr/src/app
RUN npm install -g ember-cli
COPY . .
RUN npm install
RUN ember build

# Serve the assets
FROM nginx:1.12
COPY --from=build /usr/src/app/dist /usr/share/nginx/html
```


Benefits:
- reduced image size
- remove build tools from your production containers

Examples:
- EmberJS app build application in a nodejs based container
- copy built application to a static file server nginx based container





Golang example:
First version - image size 706mb:
{% highlight docker %}
FROM golang:onbuild
EXPOSE 8080
{% endhighlight %}

{% highlight go %}
func MyFunction(s string) {
	fmt.Printf("%s\n", s)
}
{% endhighlight %}
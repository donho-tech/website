+++
date = 2021-06-14
title = "Run a React app with Docker"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++
Learn how to dockerize a React app

Create a simple React app with create-react-app
```bash
npx create-react-app my-app
```

Inside the `my-app` folder, create a Dockerfile (named "Dockerfile")
```dockerfile
FROM node:current-alpine as build
WORKDIR /usr/src/app
COPY package.json package-lock.json ./
RUN npm install
COPY . ./
RUN npm run build

FROM nginx:stable-alpine
COPY --from=build /usr/src/app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

Build the Docker image

```bash
docker build --tag my-app:0.1 .
```

Run the Docker image

```bash
docker run -p 3001:80 my-app:0.1
```

Test your app by opening <http://localhost:3001/>
## Initialize Java Spring Boot Project and React App with Dockerfile

---

## Initialize Java Spring Boot Project
<!--  -->
1. **Initialize Spring Boot Project**
To initialize a Java Spring Boot project using Maven, use Spring Initializr or create a Maven project.
  - Go to [Spring Initializr](https://start.spring.io/) and select the following options:
    - Project: Maven Project
    - Language: Java
    - Packaging: Jar
    - Dependencies: Spring Web, Spring Boot DevTools, etc.
    - Then, click **Generate** to download the project.

Alternatively, you can initialize it using Maven:
  - Open terminal and run:
  ```bash
  mvn archetype:generate -DgroupId=com.myapp -DartifactId=my-java-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
  ```

---

## Dockerfile for Java Spring Boot Project
This is the `Dockerfile` for building a Docker image for the Spring Boot Java application.
```Dockerfile
FROM openjdk:11-jre-slim
COPY target/my-java-app.jar /usr/app/
WORKDIR /usr/app
CMD ["java", "-jar", "my-java-app.jar"]
```

---


## Initialize React App (Node.js)

2. **Initialize React App**
To initialize a React application, run the following command:
  - Open terminal and run:
  ```bash
  npx create-react-app my-react-app
  ```

---

## Dockerfile for React App
This is the `Dockerfile` for building a Docker image for the React application.
```Dockerfile
FROM node:16-slim

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy all files
COPY . .

# Build the React app
RUN npm run build

# Expose the app port
EXPOSE 3000

# Start the React app
CMD ["npm", "start"]
```

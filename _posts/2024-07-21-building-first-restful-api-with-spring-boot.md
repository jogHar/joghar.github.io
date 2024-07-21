---
layout: post
title: "Building Your First RESTful API with Spring Boot"
date: 2024-07-21 12:00:00 +0530
categories: spring-boot
author: hardik
thumbnail: 
comments_id: 6
---

In the realm of modern web development, building RESTful APIs (Representational State Transfer) is fundamental for creating robust and scalable applications. Spring Boot, with its powerful ecosystem and streamlined approach to Java development, makes it exceptionally straightforward to build REST APIs. Whether you're new to web development or looking to expand your skills, this guide will walk you through the process of creating your first RESTful API using Spring Boot.
<!--more-->

### What is a RESTful API?

RESTful APIs are a type of web service that follows the principles of REST, which include statelessness, uniform interface, and resource-based architecture. They allow applications to communicate with each other over the internet by exchanging JSON or XML payloads.

### Prerequisites

Before we begin, ensure you have the following installed:

1. **Java Development Kit (JDK):** Spring Boot requires JDK 8 or later.
2. **Integrated Development Environment (IDE):** IntelliJ IDEA, Eclipse, or Spring Tool Suite (STS) for an optimal development experience.
3. **Build Tool:** Maven or Gradle. This guide uses Maven for dependency management and build tasks.

### Step 1: Setting Up Your Spring Boot Project

Let's start by creating a new Spring Boot project using Spring Initializr:

- **Visit the Spring Initializr Website:**
    - Go to [https://start.spring.io/](https://start.spring.io/).
- **Configure Your Project:**
    - Choose `Maven Project` and select your desired Spring Boot version.
    - Specify the project's `Group`, `Artifact`, and `Name`.
    - Choose `Java` as the language and set the `Project Metadata` like `Package name` and `Java version`.
    - Add dependencies:
        - `Spring Web`: to build web, including RESTful applications.
- **Generate the Project:**
    - Click on `Generate` to download a zip file containing your Spring Boot project setup.
- **Import the Project into Your IDE:**
    - Extract the downloaded zip file and import it into your IDE as a Maven project.

### Step 2: Creating a Simple RESTful API

Now that your project is set up, let's create a simple RESTful API endpoint:

1. **Create a Controller:**
    - Create a new Java class in `src/main/java` (e.g., `HelloController.java`).
    
    ```java
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;
    
    @RestController
    @RequestMapping("/api")
    public class HelloController {
    
        @GetMapping("/hello")
        public String hello() {
            return "Hello, World!";
        }
    }
    ```
    
    - `@RestController` indicates that this class defines a REST controller.
    - `@RequestMapping("/api")` specifies the base URL path for all endpoints in this controller.
    - `@GetMapping("/hello")` maps HTTP GET requests to the `/hello` endpoint, returning a simple message.
2. **Run Your Application:**
    - Right-click on your main class (typically named `Application.java` or similar) and choose `Run`.
3. **Access Your API Endpoint:**
    - Open a web browser or use a tool like Postman and make a GET request to `http://localhost:8080/api/hello`. You should receive a response of `"Hello, World!"`.

### Step 3: Enhancing Your API with Path Variables and Request Parameters

Let's expand our API to accept path variables and request parameters:

1. **Modify the Controller:**
    
    ```java
    import org.springframework.web.bind.annotation.*;
    
    @RestController
    @RequestMapping("/api")
    public class HelloController {
    
        @GetMapping("/hello")
        public String hello() {
            return "Hello, World!";
        }
    
        @GetMapping("/hello/{name}")
        public String helloName(@PathVariable String name) {
            return "Hello, " + name + "!";
        }
    
        @GetMapping("/greet")
        public String greet(@RequestParam(value = "name", defaultValue = "World") String name) {
            return "Greetings, " + name + "!";
        }
    }
    
    ```
    
    - `@PathVariable`: Captures the value of a URI template variable into a method parameter.
    - `@RequestParam`: Binds the value of a query parameter to a method parameter.
2. **Access the Updated API Endpoints:**
    - Make a GET request to `http://localhost:8080/api/hello/John` to receive a response of `"Hello, John!"`.
    - Make a GET request to `http://localhost:8080/api/greet?name=Alice` to receive a response of `"Greetings, Alice!"`.

### Step 4: Handling POST Requests and Request Bodies

Let's implement a POST endpoint that accepts JSON data:

1. **Modify the Controller:**
    
    ```java
    import org.springframework.web.bind.annotation.*;
    
    @RestController
    @RequestMapping("/api")
    public class HelloController {
    
        // Existing GET mappings...
    
        @PostMapping("/greet")
        public String greetPost(@RequestBody String name) {
            return "Greetings (POST), " + name + "!";
        }
    }
    ```
    
    - `@PostMapping`: Maps HTTP POST requests onto specific handler methods.
    - `@RequestBody`: Binds the HTTP request body to a method parameter.
2. **Access the POST Endpoint:**
    - Use Postman or a similar tool to send a POST request to `http://localhost:8080/api/greet` with a JSON body like `{"name": "Eve"}`. You should receive a response of `"Greetings (POST), Eve!"`.

### Step 5: Testing Your API

Testing your API is crucial to ensure it behaves as expected. Spring Boot provides tools like JUnit and Mockito for writing unit tests and integration tests.

### Step 6: Deploying Your Spring Boot Application

Deploying a Spring Boot application can be done to various platforms like AWS, Heroku, or a local server. Build your application into a JAR file using Maven (`mvn clean package`) and run it with `java -jar your-application.jar`
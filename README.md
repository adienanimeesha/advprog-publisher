# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using SHTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?
In the Observer design pattern, leveraging an interface (or a trait in Rust) highlights the abstraction and adaptability. This approach is intended to separate the subject (or publisher) from its observers (or subscribers), allowing them to change independently. 
Defining Subscriber as an interface or trait fosters abstraction and flexibility by allowing multiple observer types to be implemented, even if there’s only one concrete observer at the moment, which makes it easy to add varied subscribers later without altering the core publishing logic, adhering to the Open/Closed Principle. 

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?
Using a Vec would mean to go through each element manually to make sure it stays unique and to locate, update, or remove a subscriber by its unique URL. On the other hand, DashMap automatically handles these tasks with constant-time operations by treating the URL as a unique key. DashMap's built-in efficiency and thread-safe design make it a much more practical option for handling subscribers, especially when ensuring the uniqueness of each entry and ensuring the performance is key is needed. 

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?
The Singleton pattern ensures there's only one instance of a structure with a global access point. However, it does not guarantee thread safety by itself. Even if we use a Singleton for our subscriber list, a container that safely handles concurrent operations is still needed. By utilizing DashMap, it comes in—it’s built to provide thread-safe, efficient concurrent access. Essentially, Singleton manages instance uniqueness while DashMap takes care of safe, high-performance multi-threaded access.

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?
“Service” and “Repository” must be separated to keep things organized. A separate Service layer handles the business rules and processes, avoiding any mix-up with raw data operations. This clear division leads to code that’s easier to maintain, test, and extend: the Model remains a representation of data, the Repository deals with persistence, and the Service carries out the core application logic.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?
Using only the Model can lead to tangled dependencies and make the code harder to maintain or test, as changes in one Model may affect others. This is because  the Model handles data, business logic, and interactions all in one place, where it quickly becomes overloaded with responsibilities (i.e. managing creation, retrieval, updates, and communication between models (Program, Subscriber, Notification)). 

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.
I've explored Postman since it's been used for assignments and such. Postman helps me to quickly test API endpoints, adjust environment variables to simulate different scenarios, etc.

#### Reflection Publisher-3
1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?
In this tutorial case, the push model of the Observer Pattern is fillowed. This is where the publisher immediately sends out updates to all subscribers as soon as new data is available, rather than waiting for subscribers to request the information.

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)
If the pull model is used instead of the push, subscribers would only fetch updates when needed. This can help avoid unnecessary data transfers and give them more control over what information they receive. However, doing so  can also complicate things, such as managing regular polling adds complexity, which might lead to delays in getting the most up-to-date information. Other than that, frequent polling could introduce extra overhead, making it less responsive than the push model used in this tutorial.

3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.
Not using the multi-threading in the notification means the notification process would operate on a single thread. This means the notifications are handled one after the other. If any notification takes a while to send, it could block the program from moving on to the other tasks. This slows down the entire application and leads to performance bottlenecks when many notifications are queued.
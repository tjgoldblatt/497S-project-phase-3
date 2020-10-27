# 497S-project-phase-3

# Overview
The goal of our project is to develop a website that enables users to make plans with their friend group by utilizing the Yelp API to give locations in a specific area. Our goal is to have a group of friends stored in a database, and when that group wants to meet up somewhere, they will use the Yelp API microservice by searching for an activity in a location, which will create an event in an event database that stores the name, group that created the event, date/time, location, and the type of activity the group wants to do. This will allow the friend groups to agree on a location, time, and place in one central location.

# Team Members
Ron Arbo,
TJ Goldblatt,
Juelin Liu,
Joe Pasquale,
Nikil Thurai

# System Design

Our system contains a few major components, namely our nginx router, flask services, and mongoDB databases. 

Nginx serves on port 80, and takes in any requests that the outside world has for our application. It will then identify which service this request is intended for (by looking at the URL received), and send the request to that specific service. Our nginx.conf file specifies where each request type will be going, and specifies whether we serve our UI or send to a flask service. 

Our flask services are designed to receive HTTP POST requests and respond with some sort of data relating to the application. Some services will simply respond with the results of a DB operation for that service’s object type, whereas other services (like the Yelp API service), will respond with other data to be used by the application. 

The Flask services responsible for managing objects like Groups and Events for our application will each have their own connection to a mongoDB database. These services will access the database for their own object through pymongo and a mongoDB Docker container. This way, we can access our DB through POST requests to the flask application, while still keeping these services separate. 

The frontend of our web application runs on port 3000 and solely communicates with the other microservices through NginX, this prevents the product from over complicating the overall system by only needing to communicate with one other microservice. It only worries about sending proper HTTP requests to the router which will handle the rest of the requests needed to complete that initial request from the frontend. 

We connect all of our services through a common docker-compose file, which manages the common network on which all of our containerized services run on. This way, our Flask applications can recognize the DB instance they must use, Nginx knows which service to route URLs to, etc. 

# System Scalability

Our team has addressed scalability issues by ensuring that all of our microservices are wrapped in docker containers. This allows us to treat each microservice as a blackbox, knowing that for example, when we pass in a request to the Yelp API, it “magically” returns the correct response, there's no need to worry about how it does it just that it does. By being able to treat each microservice as this, we’re preventing the need for future additions to our product to rely on understanding every microservice and how it functions. One microservice is its own world, with no ties to another microservice, thus allowing for maximum scalability.

Nginx simplifies the way we route and distribute requests received by the application. It provides a common entrypoint for any requests and will appropriately distribute them throughout the application. While doing this, Nginx also acts as a load balancer, making sure to not overflow any one service with too much traffic. This makes it ideal for the microservice architecture, as well as for scalability.

After research, we found mongoDB to be the best DB available for scalability. It also has a convenient image for Docker containers, making it easy to integrate into our project.

# Microservices
This github repo contains all of our microservices in separate directories, as well as a directory for exemplars. 
https://github.com/joepasquale/497s-team-e

# Video Demonstration
A link to a video demonstration of your application running




# group4-drone
# team name: Sp26_71552_Group 4: "Last-Mile" Urban Logistics (Drone Delivery)
# Group members:
Guy Mayer:

David Moreno: 

Mariana Munoz: https://github.com/Mariana-Munoz-70445 

Nadia Nazeem:  https://github.com/nadianaz07 

Kevin Behlke

# Project Overview: 
This project models a drone-based delivery system designed to improve efficiency in last-mile urban logistics. Last-mile delivery refers to the final stage of the supply chain, where goods are transported from a local hub to the customer’s door. Although it is the shortest part of the delivery process, it is often the most complex and expensive due to factors such as traffic congestion, frequent stops, and the need for individualized deliveries in urban environments.
To address these challenges, companies are increasingly exploring solutions such as drone delivery to improve speed and reduce operational costs. We organize and optimize drone operations and ensure safety and reliability through structured data management.

# System Focus and Objectives:
The primary focus of this system is:

Fleet maintenance

Route optimization

Our goal is to design a system that keeps drone delivery operations efficient, safe, and scalable by enforcing key constraints such as drone status validation, maintenance tracking, and charging station limitations.

# Data Model:
<img width="1270" height="801" alt="image" src="https://github.com/user-attachments/assets/c57e45b9-0a9e-448c-a893-bb3ea56b21cf" />

The data model illustrates how drones, trips, packages, and supporting entities interact to manage delivery operations and enforce system constraints.

At the center of the system is the Drone entity, which connects operational activity with maintenance and resource management. Each drone can complete many trips, accumulate maintenance logs over time, and participate in charging sessions. This allows the system to track both performance and upkeep, ensuring that drones remain reliable for delivery operations.

A Storage_Facility serves as the operational hub, housing both drones and technicians. One facility can support many drones and many technicians, creating a structured environment for both deployment and maintenance. Technicians generate maintenance logs for drones, allowing the system to monitor servicing and enforce maintenance requirements based on flight hours.

On the delivery side, the system connects warehouses, routes, trips, and packages to represent how goods move through the network. A warehouse can store many packages and support multiple routes. Each route can be used for many trips, and each trip is assigned to one drone. Packages are linked to trips through the Trip_Packages table, which represents a many-to-many relationship. This allows a single trip to carry multiple packages while maintaining flexibility in delivery assignments.

Shipping destinations are connected to packages to represent delivery endpoints, while routes ensure that trips follow structured and optimized paths. Together, these relationships allow the system to efficiently coordinate deliveries across an urban environment.

The model also supports key operational constraints. Charging stations and charging sessions track when and where drones are charging, ensuring that only one drone occupies a station at a given time. In addition, maintenance logs track drone usage and enforce servicing once flight-hour thresholds are exceeded. These constraints help maintain safety and prevent operational conflicts.

# Business Rules
The system enforces the following rules:

Drones must have a valid status (In-Flight, Charging, Maintenance) before being assigned to a trip.

Maintenance logs must be created when a drone exceeds 50 flight hours.

Charging stations can only serve one drone at a time, with start and end times recorded.

# Data Dictionary

# Simple Queries:
Query for drone models in the Athens area that is AeroX Pro

Managerial Explanation: This query helps management identify which deliveries in the Athens area are being handled by a specific drone model. This is useful for tracking how certain models are being deployed in different locations, evaluating model performance in a target service area, and supporting decisions about where particular drone types should be used most often.

SELECT shipping_destination.destinationAddress, Drone.model 

FROM Trip JOIN Drone ON Trip.Drone_droneID = Drone.droneID JOIN shipping_destination ON shipping_destination.Trip_TripID = Trip.TripID 

WHERE destinationAddress LIKE '%Athens%' and model = "AeroX Pro";

<img width="630" height="126" alt="image" src="https://github.com/user-attachments/assets/910f1456-d372-4288-bb3c-3df404f237b2" />

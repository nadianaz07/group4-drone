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

# Drone and status
Managerial Explanation: This query gives managers a quick snapshot of the current condition of the fleet by showing each drone’s status, battery level, and flight hours. It helps support day-to-day operational decisions by showing which drones may be available, which may need attention soon, and which are approaching maintenance thresholds.

SELECT Drone.droneID, Drone.status, Drone.battery, Drone.flightHours 

FROM Drone;

<img width="394" height="259" alt="image" src="https://github.com/user-attachments/assets/ad606881-bf15-4ed2-bcd4-3a6565be6bcd" />

# Drones needing maintenance (> 50 hours)

Managerial Explanation: This query helps managers identify drones that have exceeded the maintenance threshold and may require servicing. It is useful for preventative maintenance planning, reducing the risk of equipment failure, and making sure the fleet remains safe and reliable during delivery operations.

SELECT Drone.droneID, Drone.flightHours, Drone.status 

FROM Drone WHERE Drone.flightHours > 50;

<img width="570" height="269" alt="image" src="https://github.com/user-attachments/assets/47154156-1f5c-4b06-b542-2b5403ba3614" />

# Packages assigned to trips

Managerial Explanation: This query helps managers understand how packages are being distributed across trips. It is useful for monitoring package loads, verifying that delivery assignments are being made correctly, and evaluating whether trips are being used efficiently to carry multiple deliveries at once.

SELECT Trip_Packages.Trip_TripID, Packages.packageID, Packages.packageType, Packages.weight, Packages.packageContent 

FROM Trip_Packages 

JOIN Packages ON Trip_Packages.Packages_packageID = Packages.packageID;

<img width="656" height="475" alt="image" src="https://github.com/user-attachments/assets/55b7928b-62a2-4b91-a82a-5a6b743cb53f" />

# Total distance per drone
Managerial Explanation: This query allows managers to measure how heavily each drone is being used by showing the total distance it has traveled. It helps with workload balancing, identifying high-use drones, planning maintenance more effectively, and evaluating whether fleet usage is being distributed efficiently.

SELECT Trip.Drone_droneID, SUM(Trip.tripDistance) AS total_distance_flown

FROM Trip GROUP BY Trip.Drone_droneID;

<img width="413" height="230" alt="image" src="https://github.com/user-attachments/assets/c7522ecd-e810-49f8-ab26-a46c8fc43764" />

# Packages with destination
Managerial Explanation: This query helps managers track what packages are going to which delivery destinations. It is useful for monitoring delivery patterns, confirming routing accuracy, and understanding customer demand by location. Over time, this can support better route planning and service allocation.

SELECT packageID, packageType, weight, destinationAddress 

FROM Packages JOIN shipping_destination ON Packages.shipping_destination_destinationID = shipping_destination.destinationID;

# Complex Queries
Use this to see which drones that are in flight are low on battery (and need to find a charging station soon)

Managerial Explanation: This query helps managers quickly evaluate which active drones are in strong condition and which may need charging soon. Instead of only showing a battery number, it translates battery levels into more practical categories, making it easier to prioritize operational decisions and respond quickly when a drone may need to leave service for charging.

SELECT *, CASE 

          WHEN battery > 67 THEN "Good to go" 
          
          WHEN battery > 33 THEN "Monitor Battery" 
          
          WHEN battery <= 32 THEN "Charge Soon" 
          
      END AS BatteryLevel 
      
FROM Drone 

WHERE status = "In-Flight";

<img width="640" height="74" alt="image" src="https://github.com/user-attachments/assets/022fc31c-59fb-4a10-8351-3384514b6e41" />








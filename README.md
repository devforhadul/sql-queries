# Vehicle Rental System

## Project Overview

This project implements a simplified **Vehicle Rental System** database. It manages **Users**, **Vehicles**, and **Bookings** with proper relationships, constraints, and business logic. The system ensures data integrity and supports real-world scenarios for vehicle rentals.

## Features

- **Users:** Can be Admin or Customer. Stores name, email, password, and phone number.  
- **Vehicles:** Stores vehicle name, type (car/bike/truck), model, registration number, rental price per day, and availability status.  
- **Bookings:** Links users and vehicles. Stores start date, end date, booking status, and total cost.  

## Database Design

The database consists of three main tables:

### **Users Table**
- `user_id` (Primary Key)  
- `role` (Admin/Customer)  
- `name`  
- `email` (Unique)  
- `password`  
- `phone_number`  

### **Vehicles Table**
- `vehicle_id` (Primary Key)  
- `name`  
- `type` (car/bike/truck)  
- `model`  
- `registration_number` (Unique)  
- `rental_price_per_day`  
- `status` (available/rented/maintenance)  

### **Bookings Table**
- `booking_id` (Primary Key)  
- `user_id` (Foreign Key → Users.user_id)  
- `vehicle_id` (Foreign Key → Vehicles.vehicle_id)  
- `start_date`  
- `end_date`  
- `status` (pending/confirmed/completed/cancelled)  
- `total_cost`  

### **Relationships**
- **One-to-Many:** User → Bookings (A user can have multiple bookings)  
- **Many-to-One:** Bookings → Vehicle (Multiple bookings can involve the same vehicle over time)  
- **One-to-One (logical):** Each booking connects exactly one user and one vehicle  

## SQL Queries Implemented

1. **JOIN Query** – Retrieve booking information along with customer name and vehicle name.
2. **EXISTS Query** – Find all vehicles that have never been booked.
3. **WHERE Query** – Retrieve all available vehicles of a specific type (e.g., car).  
4. **GROUP BY & HAVING Query** – Find the total number of bookings per vehicle and display only those with more than 2 bookings.

```sql
-- Query 1: JOIN
SELECT B.booking_id, U.name AS customer_name, V.name AS vehicle_name, 
       B.start_date, B.end_date, B.status
FROM Bookings B
JOIN Users U ON B.user_id = U.user_id
JOIN Vehicles V ON B.vehicle_id = V.vehicle_id;

-- Query 2: EXISTS
SELECT *
FROM Vehicles V
WHERE NOT EXISTS (
    SELECT 1 FROM Bookings B WHERE B.vehicle_id = V.vehicle_id
);

-- Query 3: WHERE
SELECT *
FROM Vehicles
WHERE type = 'car' AND status = 'available';

-- Query 4: GROUP BY & HAVING
SELECT V.name AS vehicle_name, COUNT(B.booking_id) AS total_bookings
FROM Bookings B
JOIN Vehicles V ON B.vehicle_id = V.vehicle_id
GROUP BY V.name
HAVING COUNT(B.booking_id) > 2;

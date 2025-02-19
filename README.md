# DriveNXT - Ride Management Database System

## Project Overview
DriveNXT is a MySQL-based database system designed to manage ride-sharing operations efficiently. It includes tables for users, vehicles, rides, payments, and ratings, ensuring seamless tracking of transactions, ride history, and financial records.

## Features
- **User Management**: Stores details of riders and drivers.
- **Vehicle Tracking**: Links drivers with their assigned vehicles.
- **Ride Management**: Captures trip details, fare, distance, and timestamps.
- **Payment Processing**: Tracks ride payments, modes, and statuses.
- **Ratings and Feedback**: Allows users to rate drivers and vice versa.
- **Business Analytics**: Provides insights through SQL queries on ride trends and revenue.

## Database Schema
- **Users Table**: Stores user details (riders & drivers).
- **Vehicles Table**: Maintains vehicle information.
- **Rides Table**: Logs ride details, fare, and status.
- **Payments Table**: Tracks transactions for completed rides.
- **Ratings Table**: Records feedback from riders and drivers.

## SQL Queries for Analysis
- Count of total riders and drivers.
- -------------------------------------------
- SELECT user_type, COUNT(*) AS total_users 
FROM Users 
GROUP BY user_type;
- -------------------------------------------
- List of all drivers with their vehicles.
- - -------------------------------------------
- SELECT u.user_id, u.name, u.phone, v.vehicle_type, v.vehicle_number, v.model 
FROM Users u
LEFT JOIN Vehicles v ON u.user_id = v.driver_id
WHERE u.user_type = 'driver';
- -------------------------------------------
- Riders who have never taken a ride.
- - -------------------------------------------
- SELECT u.user_id, u.name, u.email, u.phone 
FROM Users u
LEFT JOIN Rides r ON u.user_id = r.rider_id
WHERE u.user_type = 'rider' AND r.ride_id IS NULL;
- -------------------------------------------
- Total completed vs. canceled rides.
- - -------------------------------------------
- SELECT status, COUNT(*) AS total_rides 
FROM Rides 
GROUP BY status;
- -------------------------------------------
- Average fare and distance of completed rides.
- - -------------------------------------------
- SELECT ROUND(AVG(fare), 2) AS avg_fare, ROUND(AVG(distance_km), 2) AS avg_distance 
FROM Rides 
WHERE status = 'completed';
- -------------------------------------------
- Driver with the most completed rides.
- - -------------------------------------------
- SELECT driver_id, COUNT(*) AS total_rides 
FROM Rides 
WHERE status = 'completed' 
GROUP BY driver_id 
ORDER BY total_rides DESC 
LIMIT 1;
- -------------------------------------------
- Most popular pickup location.
- - -------------------------------------------
- SELECT pickup_location, COUNT(*) AS total_rides 
FROM Rides 
GROUP BY pickup_location 
ORDER BY total_rides DESC 
LIMIT 1;
- -------------------------------------------
- Total revenue generated.
- - -------------------------------------------
- SELECT SUM(amount) AS total_revenue 
FROM Payments 
WHERE status = 'completed';
- -------------------------------------------
- Identifying pending payments.
- - -------------------------------------------
- SELECT p.payment_id, p.ride_id, u.name AS rider_name, p.amount, p.payment_mode 
FROM Payments p
JOIN Rides r ON p.ride_id = r.ride_id
JOIN Users u ON r.rider_id = u.user_id
WHERE p.status = 'pending';
- -------------------------------------------
- Highest-rated driver.
- - -------------------------------------------
SELECT r.driver_id, u.name, ROUND(AVG(rt.driver_rating), 2) AS avg_rating
FROM Ratings rt
JOIN Rides r ON rt.ride_id = r.ride_id
JOIN Users u ON r.driver_id = u.user_id
GROUP BY r.driver_id, u.name
ORDER BY avg_rating DESC
- -------------------------------------------
- All negative feedback.
- - -------------------------------------------
- SELECT r.ride_id, u.name AS rider_name, rt.rider_rating, rt.driver_rating, rt.rider_feedback, rt.driver_feedback 
FROM Ratings rt
JOIN Rides r ON rt.ride_id = r.ride_id
JOIN Users u ON r.rider_id = u.user_id
WHERE rt.rider_rating < 3.5 OR rt.driver_rating < 3.5;


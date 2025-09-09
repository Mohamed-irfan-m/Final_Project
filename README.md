# Airline Reservation System — MySQL 8.x

##  Overview

This project implements a **mini airline reservation system** using MySQL 8.x.
It covers **airports, flights, aircraft, customers, bookings, and seat management** with triggers to ensure **automatic seat assignment, cancellation handling, and revenue tracking**.

The system is **normalized, constraint-driven, and trigger-based**, designed for realistic **flight booking operations** with demo data.

---

##  Features

* **Master Data Setup**

  * Airports (top 10 in India)
  * Aircraft types with seat layouts
* **Flight Scheduling**

  * 20 sample flights between major cities (Sep 10–15, 2025)
* **Customer Records**

  * 25 sample customer profiles
* **Seat Management**

  * Seat map templates (Business & Economy)
  * Auto-generated seats for each flight
* **Bookings**

  * 45+ sample bookings with realistic fares
  * Automatic seat assignment (Business if ≥2× base fare, else Economy)
  * Trigger-based cancellation & seat reallocation
* **Reports & Views**

  * Flight availability (per cabin)
  * Searchable flights with available seats
  * Booking summary (load factor %, revenue, cancellations)

---

##  Database Schema

**Core Tables:**

* `airports` → Airport codes & names
* `aircraft` → Aircraft models + seating capacity
* `flights` → Flight schedules + fares
* `customers` → Passenger profiles
* `bookings` → Ticket bookings with seat assignment
* `seats` → Seat inventory (linked to bookings)

**Support Table:**

* `seat_map` → Standardized seat blueprint

**Relationships:**

* Each flight → many seats
* Each booking → one seat & one customer
* Triggers enforce seat logic (auto-assign, free on cancel, validate seat changes)

---

##  Key Triggers

1. **Before Insert on Bookings**

   * Assigns Business seat if fare ≥2× base fare, else Economy
   * Prevents overbooking
2. **After Insert on Bookings**

   * Marks assigned seat as booked
3. **Before Update on Bookings**

   * Validates seat change (must be same flight & available)
4. **After Update on Bookings**

   * Frees seat on cancellation
   * Updates seat status on reassignment

---

##  Example Views & Queries

**Sample Use Cases:**


-- Search flights DEL → BOM on 2025-09-10 with ≥2 seats
SELECT * FROM vw_flights_search
 WHERE origin='DEL' AND destination='BOM'
   AND DATE(dep_time)='2025-09-10'
   AND total_seats_available >= 2
 ORDER BY dep_time;

-- List available seats on flight #1
SELECT seat_no, cabin FROM seats WHERE flight_id=1 AND is_booked=0;




##  Reports

* **Flight-level summaries**

  * Total bookings
  * Revenue collected
  * Cancellation stats
  * Load factor % (occupancy rate)

---

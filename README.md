# Sea Container Terminal Management System (MVP)

This project demonstrates a simplified model of a data management system for a sea container terminal.

## Overview

The system is designed to simulate and manage the following processes in a container terminal:
- Handling incoming ship requests.
- Allocating berths to ships based on their characteristics.
- Managing the departure of ships from the port.

The project is implemented using the **PostgreSQL database**, a **Go-based microservice architecture**, and **Docker** for containerization. It includes both a backend system and management tools.

## Architecture

The system consists of the following components:

### Database
The PostgreSQL database `sea_terminal` includes a dedicated tablespace `sct_tables` and contains the following tables:
- **port_calls**: Records ship arrivals.
- **ships**: Stores ship details.
- **berths**: Tracks berth availability and assignments.
- **port_departs**: Records ship departures.

Database management is performed using **pgAdmin 4**.

### Services (written in Go)
The backend consists of several microservices:
1. **`system-bootstrapper`**: Coordinates and launches the entire system.
2. **`ship-generator-service`**: Simulates and generates ship data.
3. **`ship-arrival-service`**: Processes incoming ship data and updates the database.
4. **`berth-allocation-service`**: Allocates ships to berths based on availability and ship characteristics.
5. **`ship-departure-service`**: Handles ship departure requests.
6. **`ship-departure-scheduler`**: Initiates scheduled ship departures.

### Containerization
All services are stored as **Docker images** and run on the host system using **Docker Compose**. Configuration files are embedded within the containers and are customizable.

## Core Features

1. **Ship Arrival Handling**:
   - Checks if a ship is already recorded in the `ships` table.
   - If not:
     - Adds the ship to the `ships` table.
     - Logs the ship's arrival in the `port_calls` table.
     - Sets the ship's `location` to "At Anchorage" (waiting for a berth).
   - If the ship is already recorded, its `location` is updated to "At Anchorage".

2. **Berth Allocation**:
   - Identifies ships with the status `At Anchorage`.
   - Searches for available berths that meet the ship's size and draft requirements.
   - Assigns ships to available berths and updates their `location`.

3. **Ship Departure Handling**:
   - Frees up berths when ships leave the port.
   - Updates the ship's `location` to "Out".

## Table Structure

The table structure is provided in the file [data_structure.pdf](data_structure.pdf).

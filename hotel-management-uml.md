# Hotel Management System – UML Design
**Author:** rajakumari  
**Date:** 2025-12-05  
**Phase:** SDLC – Design Phase
## Table of Contents
1. [Use Case Diagram](#use-case-diagram)
2. [Class Diagram](#class-diagram)
3. [Sequence Diagram – Room Booking](#sequence-diagram--room-booking)
4. [Sequence Diagram – Check-In](#sequence-diagram--check-in-process)
5. [Sequence Diagram – Check-Out](#sequence-diagram--check-out-process)
6. [Activity Diagram](#activity-diagram--hotel-management-workflow)
7. [Deployment Diagram](#deployment-diagram--hotel-management-system)
8. [Component Diagram](#component-diagram--hotel-management-system)


# Use case Diagram
```mermaid
flowchart TD

    %% Actors (rectangles)
    Customer["📌 Customer"]
    Receptionist["📌 Receptionist"]
    Admin["📌 Admin"]
    Housekeeping["📌 Housekeeping"]
    Manager["📌 Manager"]
    PaymentGateway["📌 Payment Gateway"]

    %% Use Cases (ovals)
    UC1((Search Rooms))
    UC2((Book Room))
    UC3((Cancel Booking))
    UC4((Make Payment))
    UC5((Check-in Guest))
    UC6((Check-out Guest))
    UC7((Assign Room))
    UC8((Generate Invoice))
    UC9((Add Room))
    UC10((Edit Room))
    UC11((Delete Room))
    UC12((Manage Staff))
    UC13((View Reports))
    UC14((Update Cleaning Status))
    UC15((View Occupancy Report))
    UC16((View Revenue Report))

    %% Connections
    Customer --> UC1
    Customer --> UC2
    Customer --> UC3
    Customer --> UC4

    Receptionist --> UC5
    Receptionist --> UC6
    Receptionist --> UC7
    Receptionist --> UC8

    Admin --> UC9
    Admin --> UC10
    Admin --> UC11
    Admin --> UC12
    Admin --> UC13

    Housekeeping --> UC14

    Manager --> UC15
    Manager --> UC16

    UC4 --> PaymentGateway
```
# class diagram
```mermaid
classDiagram

    %% ====== CLASSES ======

    class Customer {
        +customerId : int
        +name : string
        +email : string
        +phone : string
        +searchRooms()
        +bookRoom()
        +cancelBooking()
    }

    class Room {
        +roomId : int
        +roomNumber : string
        +roomType : string
        +price : double
        +status : string
        +isAvailable()
    }

    class Booking {
        +bookingId : int
        +customerId : int
        +roomId : int
        +checkInDate : date
        +checkOutDate : date
        +status : string
        +createBooking()
        +cancelBooking()
    }

    class Payment {
        +paymentId : int
        +bookingId : int
        +amount : double
        +paymentMethod : string
        +paymentStatus : string
        +makePayment()
    }

    class Invoice {
        +invoiceId : int
        +bookingId : int
        +amount : double
        +generateInvoice()
    }

    class Receptionist {
        +staffId : int
        +name : string
        +checkInGuest()
        +checkOutGuest()
        +assignRoom()
    }

    class Admin {
        +adminId : int
        +name : string
        +addRoom()
        +editRoom()
        +deleteRoom()
        +manageStaff()
    }

    class Housekeeping {
        +staffId : int
        +name : string
        +updateCleaningStatus()
    }

    %% ====== RELATIONSHIPS ======

    Customer --> Booking : "creates"
    Booking --> Room : "reserves"
    Booking --> Payment : "includes"
    Payment --> Invoice : "generates"
    Receptionist --> Booking : "manages"
    Admin --> Room : "controls"
    Housekeeping --> Room : "updates status"
```
# Sequence Diagram – Room Booking
```mermaid
sequenceDiagram
    participant C as Customer
    participant S as System
    participant R as Room Service
    participant B as Booking Service
    participant P as Payment Service

    C ->> S: Search for rooms
    S ->> R: Fetch available rooms
    R -->> S: Return room list
    S -->> C: Display available rooms

    C ->> S: Select room & enter booking details
    S ->> B: Create booking request
    B -->> S: Booking created

    C ->> S: Make payment
    S ->> P: Process payment
    P -->> S: Payment successful

    S -->> C: Booking Confirmed
```
# Sequence Diagram – Check-In Process
```mermaid
flowchart TD
    C[Customer] -->|Arrives at hotel| R[Receptionist]
    R -->|Verify booking| B[Booking Service]
    B -->|Confirm booking exists| R
    R -->|Assign room| S[Room Service]
    S -->|Update room status to 'Occupied'| R
    R -->|Provide key & welcome info| C
```
# Sequence Diagram – Check-Out Process
```mermaid
flowchart TD
    C[Customer] -->|Request Check-Out| R[Receptionist]
    R -->|Retrieve booking details| B[Booking Service]
    B -->|Provide booking info| R
    R -->|Calculate total payment| P[Payment Service]
    P -->|Process payment| R
    R -->|Update room status to 'Available'| S[Room Service]
    R -->|Provide invoice & receipt| C
```
# Activity Diagram – Hotel Management Workflow
```mermaid
flowchart TD
    A[Start] --> B[Customer searches rooms]
    B --> C{Rooms available?}
    C -->|Yes| D[Customer books room]
    C -->|No| E[End]

    D --> F[Customer makes payment]
    F --> G[Booking confirmed]
    G --> H[Customer arrives at hotel]
    H --> I[Receptionist checks in customer]
    I --> J[Customer stays in room]
    J --> K[Customer requests check-out]
    K --> L[Receptionist processes check-out]
    L --> M[Payment & invoice completed]
    M --> N[Room status updated to Available]
    N --> O[End]
```
# Deployment Diagram – Hotel Management System
```mermaid
flowchart TD
    Browser[Customer Browser or Mobile App] -->|HTTP Requests| WebServer[Web Server - Backend Node.js]
    WebServer -->|Queries| Database[Database Server - MySQL or PostgreSQL]
    WebServer -->|Calls| PaymentGateway[External Payment Gateway]
    WebServer -->|Manages| AdminConsole[Admin and Staff Dashboard]
    WebServer -->|Sends Notifications| EmailService[Email or SMS Service]
```
# Component Diagram – Hotel Management System
```mermaid
flowchart TD
    A[Customer Module] --> B[Booking Module]
    B --> C[Payment Module]
    B --> D[Room Management Module]
    D --> E[Housekeeping Module]
    B --> F[Invoice Module]
    A --> G[Search & View Rooms Module]
    H[Admin Module] --> D
    H --> F
    H --> I[Reports Module]
```

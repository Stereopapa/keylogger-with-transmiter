# Keylogger with HTTP Transmitter

Educational project demonstrating a full‑stack system for collecting
keyboard events on a Windows machine and transmitting them to a backend
server for processing and storage.

The project is divided into two main components:

-   **Agent (C++)** -- captures keyboard events using the Windows API
    and sends them to the backend
-   **Backend (Python / Flask)** -- processes incoming logs and stores
    them in a database

This project was created primarily to explore:

-   Windows low level keyboard hooks
-   Multithreading and synchronization in C++
-   Client--server architecture
-   Backend architecture with Flask
-   Database abstraction layers
-   Automated testing on both client and server

⚠️ **Disclaimer**\
This project was created strictly for **educational purposes** to learn
operating system internals, networking and backend architecture.

------------------------------------------------------------------------

# Architecture Overview

The system consists of two main components:

1.  **Agent (C++)**
2.  **Backend Server (Python / Flask)**

The agent captures keyboard events on a Windows machine and transmits
them to the backend server which processes and stores the logs.

    Windows Machine
         |
         |  HTTP
         v
    +-------------+
    | Keylogger   |
    | Agent (C++) |
    |-------------|
    | WinAPI Hook |
    | Threads     |
    | HTTP Client |
    +-------------+
         |
         v
    +-------------------+
    | Backend Server    |
    | (Python / Flask)  |
    |-------------------|
    | Controllers       |
    | Services          |
    | Repository Layer  |
    | SQLAlchemy        |
    +-------------------+
         |
         v
    +-------------+
    | SQLite DB   |
    | keylogger.db|
    +-------------+

Data flow:

1.  The **Agent** captures keystrokes using `WH_KEYBOARD_LL`
2.  Events are buffered and transmitted via HTTP
3.  The **Flask backend** receives the logs
4.  Logs are processed and interpreted
5.  Processed data is stored in the **SQLite database**

------------------------------------------------------------------------

# Project Structure

    keylogger-with-transmiter
    │
    ├── agent
    │   ├── Keylogger.sln
    │   ├── KeyloggerApp
    │   ├── KeyloggerEngine
    │   ├── Tests
    │   └── vcpkg.json
    │
    ├── backend
    │   ├── server.py
    │   ├── requirements.txt
    │   │
    │   ├── api
    │   ├── app
    │   │   └── services
    │   │
    │   ├── database
    │   │   ├── models.py
    │   │   ├── repository.py
    │   │   └── raw_querries
    │   │
    │   ├── tests
    │   └── utils

------------------------------------------------------------------------

# Technologies

## Agent

-   C++
-   Windows API
-   Low Level Keyboard Hook (`WH_KEYBOARD_LL`)
-   Multithreading
-   Mutex synchronization
-   HTTP transmission
-   GoogleTest
-   vcpkg

Responsibilities:

-   capturing keyboard events
-   buffering logs
-   transmitting logs to backend
-   multithreaded processing

------------------------------------------------------------------------

## Backend

-   Python
-   Flask
-   Blueprint based routing
-   Dependency Injection
-   SQLAlchemy
-   DTO pattern
-   Repository pattern
-   SQLite
-   Pytest

Responsibilities:

-   receiving logs from the agent
-   interpreting keystrokes
-   processing log streams
-   storing structured data
-   exposing API endpoints

------------------------------------------------------------------------

# Running the Backend

## 1. Install dependencies

``` bash
cd backend
pip install -r requirements.txt
```

## 2. Run the server

``` bash
python server.py
```

------------------------------------------------------------------------

# Running the Agent

Open the Visual Studio solution:

    agent/Keylogger.sln

Dependencies are handled automatically via **vcpkg.json**.

Build the project in:

    Release

mode.

The executable will be generated as:

    KeyloggerApp.exe

------------------------------------------------------------------------

# Running the System (Localhost)

The easiest way to run the system is through automated tests.

### Agent

Run from **Visual Studio Test Explorer**:

    SystemTests
    LoggingAndTransmiting

### Backend

Run:

``` bash
pytest backend/tests/test_system.py
```

------------------------------------------------------------------------

# Running the System (VM Setup)

The system can also run across two machines or virtual machines.

### Machine 1 (Windows)

Run the agent:

    KeyloggerApp.exe

### Machine 2 (Linux or Windows)

Run the backend:

``` bash
python server.py
```

The agent will transmit logs to the backend server.

------------------------------------------------------------------------

# Viewing Data

Database file:

    backend/database/keylogger.db

Recommended tool:

**DB Browser for SQLite**

### Recommended workflow

1.  Open `keylogger.db` in DB Browser
2.  Open SQL file:

```{=html}
<!-- -->
```
    backend/database/raw_querries/CREATE_VIEW.sql

3.  Execute the script

This creates a convenient **view for inspecting captured logs**.

------------------------------------------------------------------------

# Tests

## Agent Tests

Uses **GoogleTest**.

Located in:

    agent/Tests

Tests verify:

-   log creation
-   transmission functionality
-   basic integration

------------------------------------------------------------------------

## Backend Tests

Uses **pytest**.

Test files:

    test_endpoints.py
    test_repository.py
    test_process_log.py
    test_interpreter.py
    test_system.py

They cover:

-   API endpoints
-   repository layer
-   log processing
-   keystroke interpretation
-   full system integration

------------------------------------------------------------------------

# Future Improvements

Planned features:

-   TUI interface for live monitoring
-   real time log visualization
-   better analytics for keystroke streams

Currently logs can be inspected via **DB Browser**, but the goal is to
provide a dedicated monitoring interface.


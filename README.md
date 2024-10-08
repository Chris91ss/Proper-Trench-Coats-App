# Proper Trench Coats Application

## Overview
This repository contains a C++ application developed using object-oriented programming (OOP) principles and layered architecture. The application is designed for the *Proper Trench Coats* store, which sells fashionable, elegant trench coats. It allows administrators to manage the store's inventory and customers to place orders online via a command-line interface (CLI) and a graphical interface built with **Qt**.

The application supports two modes of operation:
1. **Administrator Mode**: Manage trench coat inventory, including adding, updating, and removing items.
2. **User Mode**: Customers can browse the inventory, add items to their shopping basket, and save the basket in CSV, HTML, or SQLite database format.

## Key Features:
- **Two Modes**: Administrator and user modes with distinct functionalities.
- **Layered Architecture**: Clean separation between domain, repository, service, and UI layers for maintainability.
- **File & Database Persistence**: The repository supports both file-based (CSV/HTML) and database-backed persistence (SQLite).
- **Undo/Redo Functionality**: Administrators can undo or redo actions related to inventory management.
- **Command-Line Interface (CLI)**: A text-based interface for basic operations.
- **Qt-based GUI**: A graphical interface for both the administrator and user operations.
- **Graphical Representation**: A pie chart that visually represents the number of coats priced below and above 200$.
- **CSV/HTML/SQLite Shopping Basket**: Users can save and view their shopping baskets in CSV, HTML, or SQLite formats, or export data to an external application.

### Advanced Features:
- **Database-Backed Repository**: Full support for storing data in an SQLite database, providing robust data persistence.
- **CSV-Based Repository**: Export shopping baskets to CSV files, allowing users to open and manage their orders in spreadsheet applications.
- **Graphical Representation of Coat Prices**: Users can see a pie chart of coats priced above and below 200$ in the Qt-based GUI.

## Installation

### Prerequisites:
- **C++17** or later
- **Qt5** or later (for GUI)
- **SQLite3** (for database persistence)

Install Qt and SQLite3 by following their respective [Qt Documentation](https://doc.qt.io/qt-5/gettingstarted.html) and [SQLite Installation Guide](https://www.sqlite.org/download.html).

### Clone the Repository:
```bash
git clone https://github.com/Chris91ss/Proper-Trench-Coats-App.git
cd Proper-Trench-Coats-App
```

## Build the Application

1. Open the project in **Qt Creator** or configure it using **CMake** for manual building.
2. Compile and run the project through your preferred method.

---

### Running the Application:

1. **Compile** the project using your preferred IDE (e.g., **Qt Creator**) or from the command line with `g++`:

```bash
g++ -o ProperTrenchCoatsApp main.cpp $(pkg-config --cflags --libs Qt5Widgets) -lsqlite3
```

2. **Run** the compiled executable:

```bash
./ProperTrenchCoatsApp
```

Upon launching, the application will prompt the user to choose between **Administrator Mode** or **User Mode**.

---

#### Administrator Mode:
- **Add**, **remove**, and **update** trench coat information (size, color, price, quantity, photo).
- **Undo/Redo** functionality to manage changes easily.
- **View** the entire inventory of trench coats.

#### User Mode:
- **Browse** trench coats.
- **Filter** coats by size.
- **Add items** to the shopping basket.
- **Save** the basket in **CSV**, **HTML**, or **SQLite database** format.
- **View** the basket total and contents in an external application.

## Database-Backed Repository

In addition to file-based storage (CSV/HTML), the application also supports data persistence through an **SQLite** database. The `DataBaseRepository` class handles the database operations, including adding, removing, and querying trench coats.

The repository automatically creates the necessary table (`trenchcoats`) if it doesn't exist, and the data is stored in a structured manner for easy retrieval.

### Example of Database Integration:

```cpp
// dataBaseRepository.cpp

DataBaseRepository::DataBaseRepository(const string& fileName) : fileName(fileName), db(nullptr) {
    int rc = sqlite3_open(fileName.c_str(), &db);
    if (rc != SQLITE_OK) {
        throw std::runtime_error("Error opening SQLite database");
    }

    const char* createTableSQL = "CREATE TABLE IF NOT EXISTS trenchcoats (id INTEGER PRIMARY KEY, size TEXT, color TEXT, price REAL, quantity INTEGER, photograph TEXT)";
    char* errMsg;
    rc = sqlite3_exec(db, createTableSQL, nullptr, nullptr, &errMsg);
    if (rc != SQLITE_OK) {
        sqlite3_free(errMsg);
        sqlite3_close(db);
        throw std::runtime_error("Error creating table");
    }
}
```

### Benefits of Database Persistence:
- **Robust Data Storage**: SQLite ensures that trench coat data is stored reliably and can be retrieved efficiently, even after the application is closed.
- **Search and Filter Capabilities**: The SQLite database allows efficient querying of large datasets, which improves the performance of operations like filtering coats by size or price.
- **Scalability**: Database-backed storage is more scalable than file-based storage, making it ideal for applications that need to handle larger datasets.

## Project Structure

```plaintext
├── data                       # Stores data files like CSV, HTML, and SQLite databases
│   ├── shoppingBasket.csv      # CSV file for storing basket data
│   ├── shoppingBasket.html     # HTML file for basket viewing
│   ├── data.txt                # Text file containing initial trench coat data
│   ├── trenchCoats.db          # SQLite database file for trench coat inventory
├── headers                     # Header files
│   ├── domain                  # Domain models (e.g., TrenchCoat)
│   ├── repository              # Handles data storage/retrieval (CSV, HTML, SQLite)
│   ├── service                 # Business logic layer
│   ├── ui                      # Command-line interface
│   └── utilities               # Utility functions
├── src                         # Source code files
│   ├── domain                  # Domain logic for trench coats
│   ├── repository              # File and database repositories
│   ├── service                 # Service layer for business operations
│   ├── ui                      # Command-line interface logic
│   └── utilities               # Helper utilities for data parsing
├── gui.cpp                     # Qt-based GUI implementation
├── gui.h                       # Header file for GUI class
├── gui.ui                      # Qt Designer form for the GUI
├── basketModel.h               # Model for representing the shopping basket in a table
├── main.cpp                    # Entry point for the application
└── README.md                   # Documentation file
```

## Code Overview

### 1. **Domain Layer (`domain/`)**:
This layer represents the core domain entity, `TrenchCoat`. It defines the attributes of a trench coat, such as size, color, price, quantity, and a URL for the coat’s photograph.

### 2. **Repository Layer (`repository/`)**:
The repository layer provides persistence mechanisms for trench coats. The application offers multiple implementations for file-based (CSV/HTML) and database-backed (SQLite) storage.

### 3. **Service Layer (`service/`)**:
This layer handles the business logic for managing trench coats and shopping baskets. It communicates between the repository and UI layers, ensuring that business rules are respected.

### 4. **Command-Line UI Layer (`ui/`)**:
The command-line UI layer provides a text-based interface for administrators and users. It enables inventory management and customer interaction with the shopping basket.

### 5. **Graphical Qt-Based UI (`gui/`)**:
The Qt-based graphical user interface provides administrators and users with a GUI for interacting with the inventory and shopping basket.

### 6. **Graphical Representation: Pie Chart**
A pie chart is rendered to show the number of trench coats priced below and above $200, providing a visual summary for users.

---

## Unit Testing

The project includes unit tests for core functionality like adding, removing, and updating trench coats.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or new features.

## License

This project is licensed under the MIT License.


# IoT Sandbox and Edge Compute Deployment Management System

An architecture-aware desktop application built with **Java Swing** and a relational **MySQL** database. This system serves as a central hub for network lab administrators to seamlessly maintain physical edge hardware assets (such as Raspberry Pi clusters or edge AI gateways) and safely trace their active sandbox testbed deployments.

---

## 🚀 Key Features

* **Centralized Dashboard Control:** A unified main control window providing instant, independent routing to sub-modules without thread interruption.
* **Hardware Inventory System:** Full asset logging (Model, Architecture, Stock counts) that enforces strict non-negative validation rules for data integrity.
* **Dynamic Deployment Lifecycle Engine:** An asset tracking system where dropdown components dynamically inherit active stock profiles straight from the relational backend.
* **Robust Relational Constraints:** Employs strict MySQL Foreign Key pairings (`ON DELETE RESTRICT`, `ON UPDATE CASCADE`) to eliminate orphaned deployment links.
* **Enhanced GUI Workflows:** Integrated manual UI button listeners (`this.dispose();`) that naturally close context windows while preserving background system operations.

---

## 🛠️ System Architecture & Tech Stack

* **Front-End:** Java SE Development Kit (JDK 8 or higher) utilizing `javax.swing` components.
* **Back-End:** MySQL Server 8.0+ Relational Database.
* **Build tool / IDE:** Apache NetBeans or any plain-text compiler supporting standard layout managers.
* **Database Driver:** Connector/J (`mysql-connector-j-x.x.x.jar`).

---

## 📋 Database Setup

Before running the application, you must initialize the relational backend using the provided script.

1. Ensure your local **MySQL Server** instance is running.
2. Open your choice of terminal or SQL management suite (e.g., MySQL Workbench).
3. Execute the following script to build the schema structure and populate it with initial mock telemetry/hardware records:

```sql
CREATE DATABASE IF NOT EXISTS iotsandbox_db;
USE iotsandbox_db;

DROP TABLE IF EXISTS `sandbox_deployments`;
DROP TABLE IF EXISTS `compute_nodes`;

CREATE TABLE `compute_nodes` (
  `node_id` INT NOT NULL AUTO_INCREMENT,
  `model_name` VARCHAR(100) NOT NULL,
  `architecture` VARCHAR(50) NOT NULL,
  `total_stock` INT NOT NULL DEFAULT 0,
  `status` VARCHAR(50) NOT NULL DEFAULT 'Available',
  PRIMARY KEY (`node_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

CREATE TABLE `sandbox_deployments` (
  `deployment_id` INT NOT NULL AUTO_INCREMENT,
  `user_id` INT NOT NULL DEFAULT 1,
  `node_id` INT NOT NULL,
  `project_name` VARCHAR(150) NOT NULL,
  `target_protocol` VARCHAR(50) NOT NULL DEFAULT 'MQTT',
  `status` VARCHAR(50) NOT NULL DEFAULT 'Active',
  PRIMARY KEY (`deployment_id`),
  KEY `fk_deployment_node_idx` (`node_id`),
  CONSTRAINT `fk_deployment_node` FOREIGN KEY (`node_id`) 
    REFERENCES `compute_nodes` (`node_id`) 
    ON DELETE RESTRICT ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

```

---

## ⚙️ Configuration & Installation

### 1. Database Connectivity Connection Setup

Verify that your database access utilities reference the correct connection properties inside your project's configuration directory:

* Navigate to **`src/iotsandbox/config/DatabaseConfig.java`**.
* Review your target database connection credentials:
```java
private static final String URL = "jdbc:mysql://localhost:3306/iotsandbox_db";
private static final String USER = "your_mysql_username";
private static final String PASSWORD = "your_mysql_password";

```



### 2. Adding the Driver to the Project Classpath

If you encounter a `ClassNotFoundException` regarding the JDBC driver during launch, you must manually introduce the dependency library:

1. Open your project inside **NetBeans**.
2. Locate the project hierarchy pane on the left, right-click **Libraries**, and select **Add JAR/Folder**.
3. Point to the location of your extracted `mysql-connector-j-x.x.x.jar` file and confirm.

---

## 🏃 Execution Instructions

To execute the application from NetBeans or command terminal:

### Within NetBeans IDE:

1. Select the project root inside the projects explorer dashboard view.
2. Press **Clean and Build** to erase any legacy operational artifacts (`Shift + F11`).
3. Launch the central operational application frame by pressing **F6** or hitting the green run button icon.

### From Command Line Terminal:

Ensure your target dependencies are correctly referenced in the classpath environment variables, then execute:

```bash
javac -d bin src/iotsandbox/**/*.java
java -cp bin:lib/mysql-connector-j-x.x.x.jar iotsandbox.views.DashboardFrame

```

---

## 🧑‍💻 Author & Contributions

* **Lead Systems Architect:** Kyle Pacana
* Developed as an academic term project evaluating relational modeling paradigms coupled with Event-Driven GUI frameworks in Java.

```

```

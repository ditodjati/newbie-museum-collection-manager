# Museum Collection Management System

The **Museum Collection Management System** is a specialized, terminal-based Command Line Interface (CLI) application developed using Python. Designed as a comprehensive and efficient inventory solution for museum administrators, this application allows users to digitally log, track, organize, and seamlessly manage valuable historical assets, ranging from prehistoric fossils and ancient artifacts to medieval weaponry and renaissance masterpieces. 

At its core, this project represents a robust implementation of **CRUD (Create, Read, Update, Delete)** operations. Built upon foundational Python data structures and enhanced with advanced logical validation frameworks, the system acts as a reliable data management platform that ensures data integrity while transforming raw software execution into a clean, structured, and user-friendly console experience.

---

## Project Overview

In a real-world museum ecosystem, keeping track of precious historical assets, their structural conditions, and their current display or storage locations presents a critical operational challenge. This application directly addresses those administrative needs by serving as the primary digital registry for the **Shangri-La Museum**, which is the specific foundational setting established within the source code. It allows museum personnel to seamlessly organize, track, update, and manage a diverse range of invaluable historical items, from prehistoric fossils and ancient weapons to priceless paintings and cultural artifacts.

Built entirely on top of robust Data Manipulation logic, this management tool provides a terminal-based Command Line Interface (CLI) database environment. Operating purely in-memory via Python's native data structures, the program implements comprehensive **CRUD (Create, Read, Update, Delete)** functionalities. It utilizes structured dictionaries nested within a centralized list array, which acts as the single "Source of Truth" to guarantee total data consistency and high-speed manipulation across all sub-menus during runtime. 

To ensure an exceptional administrative user experience, the application incorporates advanced formatting engines to transform raw computer data rows into clean, human-readable tables. Furthermore, it implements interactive verification checkpoints and rigid multi-layer validation loops designed to protect the volatile memory database against accidental data losses, improper system mutations, or primary key collisions.

---

## Technical Program Flow & Execution Logic

The execution architecture of this program relies on a modular, routing-driven state machine built entirely upon Python's native procedural capabilities[cite: 3]. Rather than executing a monolithic block of conditional code, the program decouples its user-interface layers from data mutation logic, utilizing function references mapped to validated user inputs to cleanly navigate between sub-menus.

Below is the complete algorithmic workflow diagram, followed by an in-depth technical breakdown of how each software branch processes data:

![Program Flow](Program%20Flow.png)

### 1. Main Initialization & Routing Layer
* **Startup Sequence:** When `museum_app.py` is invoked, the program executes the global scope entry safeguard (`if __name__ == "__main__":`). It immediately triggers the `greeting()` function to render the central terminal welcome banner, then transfers software control to the permanent `main_menu()` execution loop.
* **The Dictionary Routing Map:** To prevent deeply nested `if-elif-else` code smell, the main menu utilizes a lookup table dictionary named `menu_actions`. Within this map, string keys (`"1"`, `"2"`, `"3"`, and `"4"`) point directly to the function memory addresses of `read_menu`, `create_menu`, `update_menu`, and `delete_menu` respectively.
* **Navigational Validation Loop:** An infinite `while True` cycle captures the user's navigational intent using a whitespace-cleansed `input().strip()`. 
  * If the filtered selection matches an existing key within the `menu_actions` dictionary, the engine calls the corresponding function pointer, seamlessly shifting the application state.
  * If the operator enters `"5"`, the loop breaks, a system termination trace log runs, and the script exits gracefully, freeing runtime memory.
  * Any other alphanumeric or empty string entry triggers an invalid choice exception warning message without breaking the loop, forcing a clean interface re-prompt.

### 2. Read Menu Workflow (Data Query Engine)
When routed to `read_menu()`, the system establishes a secondary interactive looping menu offering 4 distinct data query pathways to examine museum inventory:
* **Empty Database Protection:** Before executing any lookups for Options 1, 2, or 3, a structural guard clause (`if not data_collection:`) evaluates the primary data list container. If the container holds no records, the application alerts the operator with a "No Collections Available" status message and skips the remaining iteration logic, avoiding redundant compute cycles or runtime layout errors.
* **Option 1 (Show All Collections):** Invokes `read_data_collection()` without filter parameters. It dynamically extracts the dictionary keys from the first available record row to compile a uniform header layout. It then compresses all underlying dictionary rows into sequential nested lists and passes them to the third-party `tabulate` library using the `tablefmt="rst"` (reStructuredText) schema to render a beautifully aligned console data grid.
* **Option 2 (Search by Collection ID):** Prompts the operator for a targeted identifier, feeding the response into `read_data_collection("Collection ID", collection_id)`. The system runs a rigid string-matching verification loop via the `matches_filter()` helper to guarantee a strict, exact-match equality check against the repository's Primary Key.
* **Option 3 (Search by Other Attributes):** Routes execution to `search_by_attribute()`. It defines a strict sequence array of permitted fields (`Title`, `Region of Origin`, `Place Discovered`, `Era`, `Year Discovered`, `Type`, and `Status`) to block illegal key queries.
  * **Granular Case-Insensitive Filtering:** For standard text attributes, it uses Python's `next()` generator expression with `.lower()` string modifiers to match column names case-insensitively. The system then runs a flexible partial-match lookup (`filter_value.lower() in str(item[filter_option]).lower()`), enabling operators to find records like `"Mona Lisa"` simply by typing `"mona"`.
  * **Type-Safe Numerical Isolation:** If searching via `Year Discovered`, it wraps the target query inside a defensive `try-except ValueError` isolation block to enforce that only clean numeric digit characters pass to the underlying lookup comparison logic.

### 3. Create Menu Workflow (Data Registration & Integrity Guard)
The `create_data_collection()` pipeline enforces rigid validation constraints to guarantee that only structurally flawless historical logs enter the central repository list:
* **Primary Key Collision Check:** The program drops into an isolated input validation loop that traps execution until a clean `Collection ID` is supplied. When a string is entered, it calls `is_duplicate_id()`, which relies on the unified tracking helper `find_collection_by_id()`. If a dictionary match is found, the system rejects the transaction, throws a duplication warning, and blocks the record from overriding existing identifiers.
* **Dynamic Chronological Boundaries Evaluation:** When capturing the `Year Discovered`, the program implements an integer transformation block. Once parsed successfully as an integer data type, it checks the value against `is_valid_year()`. This function dynamically enforces that the integer falls strictly between `0` and `CURRENT_YEAR` (automatically fetched from the host OS system clock using `datetime.date.today().year` to eliminate time decay or stale hardcoded constants).
* **Null-Value Structural Safeguard:** After gathering all non-primary elements, the system array-evaluates text variables via `if not all(text_fields):`. If any alphanumeric field has been bypassed with an empty space, the program halts registration immediately, saving the collection list from corrupted schema definitions.
* **Two-Phase Commit Transaction:** If all inputs satisfy database boundaries, a temporary preview dictionary structure (`new_data`) is compiled and rendered in a terminal data grid. The application enters an explicit confirmation prompt. The new dictionary is appended to the master `data_collection` array *only* if the operator inputs a case-normalized `"yes"`. Entering `"no"` cancels the insertion and clears the temporary buffer safely.

### 4. Update Menu Workflow (Targeted Field Mutation)
Modifying active inventory requires precise attribute filtering to protect adjacent historical metrics from accidental corruption or data loss:
* **Target Lookup & State Preview:** The operator passes a specific target `Collection ID` to `find_collection_by_id()`. If the lookup yields `None`, the update routine aborts immediately with an error notification. If found, the targeted record's exact current state is isolated and displayed in a single-row data table, ensuring the operator has a clear view of the existing database states before executing modifications.
* **Dual-Stage System Confirmation Checkpoint:** Before showing column choices, the system stalls execution with a `Continue update? (Yes/No)` verification loop. An explicit `"no"` aborts the script run and routes flow control back to the update menu, while a `"yes"` unlocks field selection.
* **Dynamic Property Target Matching:** The system prints all permissible editable column names (`ALL_EDITABLE_COLUMNS`). The operator inputs their choice, which is case-insensitively mapped against the dictionary data keys via a list comprehension search tool to establish a valid mutation track.
* **Granular Field Value Injection:** Once the attribute path is locked, it triggers `prompt_for_new_value(column_name)`. This function intercepts empty text strings for standard text attributes and re-routes numerical field modifications (like updating the `Year Discovered`) back through the strict integer conversion and calendar-year validation framework.
* **In-Place Mutation Commit:** Upon fetching the new value, a final execution confirmation prompt is initiated. Once approved with a case-insensitive `"yes"`, the application mutates the dictionary row in-place via native key-value binding assignment, instantly reflecting the updated properties in a new `tabulate` summary view.

### 5. Delete Menu Workflow (Controlled Memory Purging)
Because removing data within an in-memory runtime context is a destructive and irreversible action, this module introduces an explicit safety layout designed to prevent accidental data purging:
* **Record Resolution & Verification:** The user supplies a `Collection ID`, which is evaluated via `find_collection_by_id()`. If the target dictionary does not exist, an error log is printed, and the removal loop terminates safely.
* **Descriptive Structural Disassembly:** To ensure the administrator knows exactly what is slated for deletion, the program extracts the targeted dictionary and prints every internal metadata field line-by-line using a descriptive loop (`for key, value in collection_to_delete.items():`). This displays all background properties (such as its title, origin, discovery location, era, type, and current storage status) out in the open.
* **Purge Commitment Checkpoint:** The module blocks application execution with a critical transaction confirmation query (`Do you want to delete the data? (Yes/No): `). Only when the operator manually enters a case-normalized `"yes"` will the program run Python's native sequence removal array command (`data_collection.remove(collection_to_delete)`). This safely purges the object from memory before returning the user to the sub-menu with a "Data successfully deleted!" status confirmation.

---

## Key Features Breakdown

* **Centralized Data Repository (Single Source of Truth):** Data is managed in a uniform list of standardized dictionaries. Every search, addition, update, and removal mutates or references this single array, ensuring total state synchronization across functions.
* **Dynamic Time Constraints Tracking:** Rather than hardcoding static years for date boundaries (which become obsolete as real-world time moves forward), the program queries the system clock at runtime using the `datetime` module. This ensures the validation logic remains accurate automatically over time.
* **Advanced Error Isolation Frameworks:** The program prevents terminal crashes caused by invalid manual inputs. Every alphanumeric entry point slated for mathematical evaluations or array operations is bound by safe string-cleansing routines (`.strip()`, `.lower()`) and strict algorithmic error traps (`try-except ValueError`).
* **Granular, Case-Insensitive Text Engine:** Searching and column routing do not require rigid casing. Users can type queries in uppercase, lowercase, or mixed case. Furthermore, the search engine utilizes partial matching logic, meaning a search for `"mona"` will successfully isolate the `"Mona Lisa"` database record.
* **Rich Layout Data Presentations:** Avoids chaotic raw console strings by processing records into neat structures using the `tabulate` library. Tables feature uniform header alignments, clear column padding dividers, and structured data row wrappers.

---

## Data Structure & Attributes

Each collection item is stored in memory as a Python dictionary object with the following attribute schema:

| Attribute | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| **Collection ID** | String | Unique identifier for the collection (Primary Key) | `F01`, `W01`, `P01` |
| **Title** | String | Name or title of the museum collection item | `Mona Lisa` |
| **Region of Origin** | String | Region or continent of origin | `Europe` |
| **Place Discovered** | String | Specific location where the item was found | `Florence, Italy` |
| **Era** | String | Era or period of creation/origin | `Renaissance` |
| **Year Discovered** | Integer | The year the collection item was discovered | `1517` |
| **Type** | String | Category classification | `Fossil`, `Weapon`, `Artifact`, `Painting` |
| **Status** | String | Current availability status | `Displayed`, `Storaged`, `Restoration` |

---

## Developer Notes & Architecture Evolution

> **Crucial Note for Project Reviewers:**
> This application was architected and built by a **beginner developer** as a fundamental exercise in mastering structured procedural programming, data formatting, and conditional state handling in Python.
> 
> As a foundational training project, the code is intentionally written using a **simplified procedural and modular functional layout**. It does **not** implement enterprise-level architectural frameworks such as *Clean Architecture*, *Hexagonal (Ports & Adapters) Architecture*, *Domain-Driven Design (DDD)*, or *Model-View-Controller (MVC)*. Additionally, it operates without external *Object-Oriented Programming (OOP)* class layers or persistent *Database Management Systems (DBMS)* like PostgreSQL or SQLite.
> 
> However, great care was taken during development to transition the code from a basic script into a clean, optimized structure. By refactoring repetitive blocks into reusable logic helpers (such as `find_collection_by_id()` and `prompt_for_new_value()`), the codebase strictly adheres to the **DRY (Don't Repeat Yourself)** development paradigm. It showcases strong defensive programming principles through comprehensive validation checks, explicit function scopes, and decoupled input-logic handling.

---

## Getting Started & Local Execution

### Prerequisites
To run this application locally, you must have a Python environment (Python 3.x or higher recommended) installed on your system. The program also requires the third-party formatting library `tabulate`.

### Step-by-Step Installation

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/ditodjati/museum-collection-management-system.git](https://github.com/ditodjati/museum-collection-management-system.git)

2. **Navigate to the project directory:**
   ```bash
   cd museum-collection-management-system

3. **Install the required formatting dependency via pip:**
   ```bash
   pip install tabulate

4. **Launch the terminal management system application:**
   ```bash
   python museum_app.py

# Task Context & Prompt for Java Lab 3 Implementation (Variant 9: Event & Conference Management)

## 1. Task Context & Environment
- **Branch Context:** You are currently checked out on the `lab3` branch in an existing Java project built on top of Lab 1 and Lab 2.
- **Goal:** Implement File I/O, CSV Parsing, Checked Exceptions, Logging (SLF4J/Logback), and JUnit 5 tests according to **Lab 3 requirements** for **Variant 9: Event & Conference Management**.
- **Package Namespace:** `ua.conference`

---

## 2. Technical Requirements & Guidelines

### 1. Custom Checked Exception (`ua.conference.exception`)
- Create a checked exception class `InvalidDataException extends Exception`.
- Must include **two constructors**:
  1. `public InvalidDataException(String message)`
  2. `public InvalidDataException(String message, Throwable cause)`
- **Note:** Do NOT modify compact constructors of records from Lab 2 (they continue throwing unchecked `IllegalArgumentException`).

### 2. Sample Data File (`data/venues.csv` or `data/participants.csv`)
- Create a CSV file in the `data/` directory at the repository root containing data for an independent entity from Variant 9 (e.g., `Venue` or `Participant`).
- File format:
  - Header line present (e.g., `venueCode,name,capacity`).
  - Delimiter: comma `,`, no quotes around values, standard UTF-8 encoding.
  - Must contain **at least 3 invalid lines** with different parsing/validation errors (e.g., non-numeric capacity, blank code/name, invalid capacity <= 0).

### 3. File Reader / Loader (`ua.conference.loader`)
- Create a loader utility class (e.g., `VenueLoader` or `ParticipantLoader`).
- Functionality:
  - Read lines using `BufferedReader` or `Files.lines`.
  - Use `try-with-resources` to guarantee resource closure.
  - Parse each line into a domain object (`Venue` / `Participant`).
  - On line-level validation failure or parsing error, catch the internal exception (`IllegalArgumentException`, `NumberFormatException`, etc.) and throw a custom `InvalidDataException` containing the **1-based line number** and the original `cause`.
  - **Error Recovery:** Catch `InvalidDataException` inside the loader loop, log a `WARN` message, **skip the invalid line**, and continue reading the rest of the file.
  - **Missing File Handling:** Handle `IOException` or `NoSuchFileException` gracefully (log an `ERROR` message) without throwing an uncaught exception or stopping application execution.
  - Demonstrate proper usage of `finally` and `multi-catch` blocks where appropriate.

### 4. Logging Configuration (`SLF4J` + `Logback`)
- Update `build.gradle` dependencies:
  ```groovy
  dependencies {
      implementation 'org.slf4j:slf4j-api:2.0.9'
      runtimeOnly 'ch.qos.logback:logback-classic:1.4.11'
  }
Use 3 distinct log levels appropriately:

INFO: Successful file reading completion, total valid loaded records count.

WARN: Skipping an invalid CSV line (include line number and error detail using parametrized logs {}).

ERROR: Attempting to read a non-existent file or critical file access issues.

Log Formatting: Use SLF4J placeholders (e.g., log.info("Loaded {} venues", list.size());) instead of string concatenation. Pass standard exceptions as the last parameter to preserve stack traces.

### 5. Unit Testing with JUnit 5 (src/test/java)
Write at least 6 independent JUnit 5 test cases (using @Test and @DisplayName in English):

Valid Object Creation: Test successful instantiation of records/classes.

Invalid Data Handling: Verify InvalidDataException is thrown with correct line numbers and messages during line parsing.

Calculated Methods: Test domain utility functions (e.g., eventDays(Event), daysBeforeEvent(EventRegistration)).

Validation Rules: Test edge cases in record compact constructors or setter methods.
Note: Unit tests must NOT depend on external physical files in data/ (use string streams or mock data where needed).

### 6. Application Entry Point (Main.java)
Update Main.java to demonstrate all required Lab 3 scenarios:

Loading from the CSV file containing both valid and invalid lines.

Attempting to load from a non-existent file path (demonstrating graceful ERROR handling).

Triggering and catching a setter validation error on a mutable class (e.g., Event).

Triggering and demonstrating log output across all 3 levels (INFO, WARN, ERROR).

## 3. Language & Formatting Rules
English Only: Javadoc, code comments, test method names, @DisplayName annotations, and exception messages must be in English.

Ukrainian for Console Output: Console outputs in Main.java intended for the user must be in Ukrainian.

## 4. Verification Step
Ensure everything builds and passes tests smoothly:

Execute ./gradlew test

Execute ./gradlew run

Please apply all required code additions and modifications directly to the project.
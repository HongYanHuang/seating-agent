# PRD: Seating Plan Distributor - Client-Side Excel Processor

## 1. Overview
The Seating Plan Distributor is a web application designed to match ticket orders with physical seats. A major bottleneck is the "dirty" nature of seating plan files (XLSX/CSV) which often lack standardized metadata (Zone, Section). This module allows operators to upload, map, and transform raw spreadsheet data into a standardized JSON format entirely within the browser.

## 2. Target Users
- Event Operators
- Box Office Managers
- Data Analysts

## 3. Problem Statement
Seating plans are provided in various formats. They often lack "Zone" or "Section" names within the file rows and require manual mapping to identify which columns represent "Row IDs" and which represent "Seat Numbers."

## 4. Functional Requirements
### FR1: File Upload & Parsing
- Support `.xlsx`, `.xls`, and `.csv` formats.
- Parse files locally without sending data to a backend server.
- Support multi-sheet workbooks (operator selects which sheet to use).

### FR2: Interactive Preview
- Display the uploaded spreadsheet in a grid view.
- Allow the operator to "Select Header Row" to identify where data starts.

### FR3: Column & Metadata Mapping
- **Row Mapping:** Operator selects one column to act as the "Row Identifier" (e.g., Column A = "Row BX").
- **Seat Mapping:** Operator selects a range of columns to act as "Seat Numbers."
- **Manual Input:** Operator inputs "Zone" and "Section" strings manually if they are not present in the file.

### FR4: Data Transformation
- **ID Generation:** Generate a unique ID for each seat using the pattern: `{Zone}-{Section}--{Row}---{SeatNo}`.
- **Continuous Grouping:** Group seats into sub-arrays if they are physically continuous in the spreadsheet row.
- **Filter Logic:** Skip cells marked as "Kill," "Restricted View" (optional toggle), or empty.

## 5. Non-Functional Requirements
- **Privacy:** 100% Client-side processing (No data leaves the browser).
- **Performance:** Handle files up to 5,000 seats with < 2s processing time.
- **UX:** Clear visual feedback on which columns are mapped.
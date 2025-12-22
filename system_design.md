# System Design: Client-Side Seating Plan Transformer

## 1. Architecture Overview
The system follows a **Single Page Application (SPA)** architecture. The heavy lifting (parsing and transformation) is handled by a Web Worker (optional) or the main thread using the `SheetJS` library.

## 2. Data Flow
1. **File Input:** `FileReader API` reads the binary blob from the `<input type="file">`.
2. **Parsing:** `SheetJS` converts binary data into a 2D Array (Array of Arrays).
3. **State Management:** - `rawSheetData`: The full 2D array.
   - `mappingConfig`: { zone, section, rowColumnIndex, seatColumnStart, seatColumnEnd }.
4. **Transformation Pipeline:**
   - Loop through `rawSheetData` starting from `headerRowIndex`.
   - For each row, extract `RowID`.
   - Iterate through `seatColumns`.
   - Validate cell value (check for "Kill" or empty).
   - Format string: `${zone}-${section}--${rowId}---${seatNo}`.
5. **Output:** A double JSON array `[ [Seat1, Seat2], [Seat3, Seat4] ]`.

## 3. Component Breakdown
- **Uploader:** Handles drag-and-drop and file validation.
- **MappingToolbar:** UI for entering Zone/Section and choosing column indices.
- **DataPreviewGrid:** A virtualization-friendly table (using TanStack Table) that highlights mapped columns.
- **Transformer Utility:** Pure JS functions that implement the grouping logic.

## 4. Continuous Seat Logic
The algorithm identifies continuous seats by checking if the spreadsheet cells are adjacent.
- If `Cell[i]` and `Cell[i+1]` both contain seat numbers, they stay in the same inner array.
- If a cell is empty or a "Kill," the current inner array is closed, and a new one begins at the next valid seat.
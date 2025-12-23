# Seating Assignment System

A web-based seating assignment tool that automatically assigns seats to orders using a best-available algorithm with orphan seat prevention.

🔗 **Live Application**: https://hongyanhuang.github.io/seating-agent/

## Features

- 🎟️ Upload CSV file with orders
- 🪑 Multiple seating plan input methods (paste JSON or upload multiple files)
- ✅ Duplicate seat detection across all seating plans
- 🎯 Best-available algorithm (assigns largest orders first)
- 🚫 Orphan seat prevention (never leaves exactly 1 seat in a row)
- 📊 Export processed CSV with seat assignments
- 💾 Download remaining seats as JSON
- 🔒 100% client-side processing (no data leaves your browser)
- 🛠️ Built-in XLSX/CSV to JSON converter tool

## How to Use

### Step 1: Upload Orders CSV

1. Click or drag-and-drop your CSV file containing orders
2. Preview the first 5 rows to verify the data
3. Select which column contains the ticket quantity by clicking the radio button

**CSV Requirements:**
- Must have a header row
- Include a column with ticket quantities (numbers)
- Can contain any additional columns (customer info, order ID, etc.)

### Step 2: Configure Seating Plan

You have two options:

#### Option A: Paste JSON

Paste your seating plan JSON directly into the textarea.

**Format:**
```json
[
  ["A-1--2---1", "A-1--2---2", "A-1--2---3"],
  ["B-1--3---1", "B-1--3---2"]
]
```

**Rules:**
- Each inner array = one group of consecutive seats
- Seat format: `ZONE-SECTION--ROW---SEAT`
  - Example: `A-1--2---3` means Zone A, Section 1, Row 2, Seat 3

#### Option B: Upload Multiple JSON Files

1. Select "Upload Files" mode
2. Click "Choose File" to upload one or more JSON files
3. Upload files from different folders as needed (can upload multiple times)
4. Review the uploaded files list:
   - Files are numbered (#1, #2, #3...) showing merge order
   - Each file shows seat count
   - Click **×** to remove individual files
   - Click **Clear All** to start over
5. When ready, click **"Upload Finished - Merge & Validate"**
6. Files will be merged in order: File #1 + File #2 + File #3...

**Why use multiple files?**
- Different zones/sections in separate files
- Easier management of large venues
- Combine files from different sources

#### Need to Convert XLSX/CSV to JSON?

Click **"Use the Transformer Tool →"** in Step 2 to access the built-in converter:

1. Upload your XLSX or CSV file with seating data
2. Select data orientation (by row or by column)
3. Drag to select which columns/rows contain:
   - Header row/column
   - Where data starts
   - Row ID column/row
   - Zone, Section, and Seat number columns/rows
4. Download individual JSON files (named automatically as `Zone_X_Section_Y.json`)
5. Upload the JSON files back to the main page

**Duplicate Detection:**
- System automatically detects duplicate seats
- Same seat cannot appear multiple times (even across different files/rows)
- Invalid files are highlighted in red with error messages

### Step 3: Process & Download

1. Click **"Process Seat Assignments"**
2. Review the results:
   - ✅ Successfully assigned orders
   - ❌ Unassigned orders (if any)
   - 📊 Remaining available seats
3. Download options:
   - **Download Processed CSV**: Original CSV + new seat columns
   - **Download Remaining Seats (JSON)**: Unassigned seats for future use

## Output Format

Your original CSV will have these new columns added:

- `raw_data`: JSON array of assigned seats
- `zone1, section1, row1, seat1`: First ticket details
- `zone2, section2, row2, seat2`: Second ticket details
- ... (continues based on maximum order quantity)

**Example:**
If Order #1 requested 3 tickets and was assigned seats A-1--2---1, A-1--2---2, A-1--2---3:
```
raw_data: ["A-1--2---1","A-1--2---2","A-1--2---3"]
zone1: A    section1: 1    row1: 2    seat1: 1
zone2: A    section2: 1    row2: 2    seat2: 2
zone3: A    section3: 1    row3: 2    seat3: 3
```

Unassigned orders will have empty values for all seat columns.

## How the Algorithm Works

The system uses a **best-available algorithm** with **orphan seat prevention**:

1. **Sort orders** by quantity (largest first)
   - Larger groups are harder to seat, so they get priority
2. **For each order:**
   - Find the first row with enough consecutive available seats
   - Check: Would this assignment leave exactly 1 seat in the row?
   - If **YES** → Skip this row (orphan prevention), try next row
   - If **NO** → Assign the seats and mark them as used
3. **Repeat** until all orders are processed

**Orphan Seat Prevention Example:**
- Row has 5 available seats: `[1, 2, 3, 4, 5]`
- Order requests 4 seats
- Assignment would leave exactly 1 seat → **SKIP** this row
- Try next row instead

This ensures no single seats are left stranded, maximizing the usability of remaining seats.

## Privacy & Security

- ✅ **100% client-side processing** - All calculations happen in your browser
- ✅ **No data uploaded to servers** - Your CSV and seating data never leave your computer
- ✅ **No tracking or analytics** - Completely private
- ✅ **Works offline** - Once loaded, works without internet (except CDN resources)

## Browser Compatibility

Works on all modern browsers:
- Chrome/Edge ✅
- Firefox ✅
- Safari ✅
- Any browser with JavaScript enabled

## Tools Included

### 1. Seating Assignment System
Main tool for assigning seats to orders

### 2. Transformer Tool
Convert XLSX/CSV seating data to JSON format
- Supports both row-based and column-based data layouts
- Drag-and-drop column/row selection
- Handles "Kill" seats (excluded from output)
- Groups consecutive seats automatically
- Downloads with custom filenames

---

Made by HY Huang while travelling in 🏛️, 🇬🇷

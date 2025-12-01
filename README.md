# Seating Assignment System

A web-based seating assignment tool that uses a best-available algorithm with orphan seat prevention.

## Features

- ✅ Upload CSV file with orders
- ✅ Configure seating plan via JSON
- ✅ Duplicate seat detection (validates no duplicate seats in seating plan)
- ✅ Best-available algorithm (assigns largest orders first)
- ✅ Orphan seat prevention (never leaves exactly 1 seat in a row)
- ✅ Export processed CSV with seat assignments
- ✅ Download remaining seats as JSON
- ✅ Client-side processing (no data leaves your browser)

## How to Use

### 1. Open the Application

Simply open `index.html` in your web browser, or visit the live version at:
`https://yourusername.github.io/seating-agent/`

### 2. Upload Orders CSV

- Click or drag-and-drop your CSV file
- Select which column contains the ticket quantity
- Example: See `example_orders.csv`

### 3. Configure Seating Plan

Upload a JSON file or paste JSON directly. Format:

```json
[
  ["A-1--2---1", "A-1--2---2", "A-1--2---3"],
  ["B-1--3---1", "B-1--3---2"]
]
```

- Each inner array = one row of consecutive seats
- Seat format: `ZONE-SECTION--ROW---SEAT`
  - `A-1--2---3` means Zone A, Section 1, Row 2, Seat 3
- **Duplicate Detection**: The system will automatically detect and reject duplicate seats
  - Same seat cannot appear multiple times (even across different rows)
  - Example: `["A-1--2---1", "A-1--2---1"]` will be rejected

Example: See `example_seating_plan.json`
Test duplicate detection: See `example_duplicate_seats.json`

### 4. Process & Download

- Click "Process Seat Assignments"
- Review the results and statistics:
  - Total orders processed
  - Successfully assigned orders
  - Unassigned orders (if any)
  - **Remaining seats** (seats not assigned to any order)
- **Download Processed CSV**: Get your original CSV with seat assignments added
- **Download Remaining Seats (JSON)**: Get a JSON file of all unassigned seats
  - Format matches the original seating plan format
  - Can be used for future seat assignments

## Algorithm Details

### Best-Available Algorithm

1. **Sort orders by quantity** (largest first)
2. **For each order:**
   - Find first row with enough consecutive available seats
   - Check: Would assignment leave exactly 1 seat? (orphan)
   - If yes → skip this row, try next
   - If no → assign seats and mark as used

### Output Format

Original CSV columns + new columns:

- `raw_data`: Array of assigned seats (e.g., `["A-1--2---1","A-1--2---2"]`)
- `zone1, section1, row1, seat1`: First ticket details
- `zone2, section2, row2, seat2`: Second ticket details
- ... (continues based on max order quantity)

Unassigned orders will have empty values for seat columns.

## Deploying to GitHub Pages

### Method 1: GitHub Web Interface

1. Go to your repository settings
2. Navigate to "Pages" section
3. Under "Source", select branch `main` and folder `/ (root)`
4. Click "Save"
5. Your site will be live at: `https://yourusername.github.io/seating-agent/`

### Method 2: Command Line

```bash
# Initialize git (if not already done)
git init
git add .
git commit -m "Add seating assignment system"

# Create and push to GitHub
git remote add origin https://github.com/yourusername/seating-agent.git
git branch -M main
git push -u origin main

# Enable GitHub Pages via repository settings
```

## Testing

Use the provided example files:

1. Upload `example_orders.csv` (8 orders, various quantities)
2. Upload `example_seating_plan.json` (6 rows, 28 total seats)
3. Select "quantity" as the quantity column
4. Process and review results

Expected result: Most orders assigned, respecting orphan seat rule.

## Technical Stack

- **HTML/CSS/JavaScript** - Single-page application
- **Tailwind CSS** - Styling (via CDN)
- **PapaParse** - CSV parsing (via CDN)
- **No backend required** - 100% client-side

## Browser Compatibility

- Chrome/Edge: ✅
- Firefox: ✅
- Safari: ✅
- Any modern browser with JavaScript enabled

## License

MIT License - feel free to use and modify!

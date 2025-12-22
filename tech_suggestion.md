# Tech Stack: Seating Plan Distributor

## 1. Core Framework
- **React 18+** (or Vue 3): For reactive UI and state management.
- **TypeScript:** For type-safety, especially when handling complex Seat ID strings.

## 2. Data Processing (Key Libraries)
- **[SheetJS (XLSX)](https://sheetjs.com/):** - *Purpose:* The core engine to read Excel/CSV files in the browser.
  - *Why:* It is the most robust library for handling legacy `.xls` and modern `.xlsx` formats without a backend.
- **[TanStack Table (React Table)](https://tanstack.com/table/v8):**
  - *Purpose:* To render the spreadsheet preview.
  - *Why:* It handles large datasets efficiently and makes "Column Selection" logic easy to implement.

## 3. Styling & UI
- **Tailwind CSS:** For rapid UI development of the mapping interface.
- **Lucide React:** For icons (upload, checkmark, warning).
- **Headless UI:** For accessible dropdowns and modals.

## 4. Development & Deployment
- **Vite:** Next-generation frontend tooling for fast development.
- **Vercel / Netlify:** For hosting (since there is no backend required for this module).

## 5. Potential Scalability
- **Web Workers:** If seating plans exceed 10,000 seats, transformation logic can be moved to a background thread to prevent UI freezing.
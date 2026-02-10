# Patch: Fix CSV Export Button Text in Query Results

## Metadata
adw_id: `e39f8fb8`
review_change_request: `Issue #11: csv export button has the correct text - Under available tables section the csv export button has the correct text "📊 CSV". Update the query result section csv export button text should match this.`

## Issue Summary
**Original Spec:** app_docs/feature-490eb6b5-one-click-table-exports.md
**Issue:** The CSV export button in the query results section displays "📊 CSV Export" while the table export button correctly displays "📊 CSV". This creates visual inconsistency in the UI.
**Solution:** Update the query results export button text to match the table export button text by removing the " Export" suffix, keeping only "📊 CSV".

## Files to Modify
Use these files to implement the patch:

- `app/client/src/main.ts` - Update export button text on line 242

## Implementation Steps
IMPORTANT: Execute every step in order, top to bottom.

### Step 1: Update query results export button text
- Navigate to `app/client/src/main.ts` line 242
- Change `exportButton.innerHTML = `${getDownloadIcon()} Export`;` to `exportButton.innerHTML = getDownloadIcon();`
- This removes the " Export" suffix to match the table export button format

## Validation
Execute every command to validate the patch is complete with zero regressions.

1. **TypeScript Type Check**
   ```bash
   cd app/client && bun tsc --noEmit
   ```

2. **Frontend Build**
   ```bash
   cd app/client && bun run build
   ```

3. **All Backend Tests**
   ```bash
   cd app/server && uv run pytest tests/ -v --tb=short
   ```

4. **Visual Verification**
   - Start the application
   - Upload sample data or use existing tables
   - Execute a query that returns results
   - Verify the export button in query results section displays "📊 CSV" (without "Export" text)
   - Verify it matches the format of the table export buttons in the "Available Tables" section

## Patch Scope
**Lines of code to change:** 1
**Risk level:** low
**Testing required:** TypeScript compilation, frontend build, and visual verification to ensure button text consistency across the UI

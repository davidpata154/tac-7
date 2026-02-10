# Feature: Increase Drop Zone Surface Area

## Metadata
issue_number: `7`
adw_id: `c9a10bf4`
issue_json: `{"number":7,"title":"increase the drop zone surface area.","body":"/feature\n\nadw_sdic_iso\n\nlets increase the drop zone surface area. Instead of having to click \"upload data\". The user can drag and drop right on to the upper div or lower div and the UI will update to a \"drop to create table\" text. This runs the same usual functionality but enhances the UI to be more user friendly"}`

## Feature Description
This feature enhances the file upload user experience by expanding the drag-and-drop functionality from the current limited "drop zone" to cover both the sample data section (upper div) and the file upload section (lower div) of the upload modal. When users drag a file over either section, the UI will dynamically update to show "Drop to create table" text, making it clearer that files can be dropped directly onto these areas without requiring the user to click "Upload" or "Browse Files" buttons first.

## User Story
As a user uploading data to the SQL interface
I want to drag and drop files directly onto the sample data section or file upload section
So that I can quickly upload data without having to navigate through button clicks and modal interactions

## Problem Statement
Currently, the drag-and-drop functionality is limited to a small "drop zone" area within the upload modal. Users must first click the "Upload" button to open the modal, then either click "Browse Files" or drag files specifically into the designated drop zone. This creates unnecessary friction in the upload workflow. The sample data section and file upload section already occupy significant visual space but are not interactive for drag-and-drop operations, limiting the intuitive nature of the interface.

## Solution Statement
Extend drag-and-drop event handlers to both the sample data section (`sample-data-section`) and the file upload section (`drop-zone`) within the upload modal. When a file is dragged over either section, the UI will:
1. Display visual feedback showing the area is a valid drop target
2. Show "Drop to create table" text to guide the user
3. Process the dropped file using the existing `handleFileUpload()` function
4. Maintain all existing validation and error handling

This solution leverages the existing file processing infrastructure while making the entire modal more user-friendly and intuitive.

## Relevant Files
Use these files to implement the feature:

- `app/client/index.html` (lines 50-96) - Contains the upload modal structure with both the sample-data-section (upper div) and drop-zone (lower div) that need to become drop targets
- `app/client/src/main.ts` (lines 122-158) - Contains the `initializeFileUpload()` function where current drag-and-drop event handlers are registered on the drop-zone element. Need to extend these handlers to cover both sections.
- `app/client/src/style.css` (lines 247-258, 468-520) - Contains styling for the drop-zone and sample-data-section. Need to add new CSS classes for dragover states on both sections.
- `app/client/src/main.ts` (lines 161-174) - Contains `handleFileUpload()` function that processes uploaded files. This existing function will be reused for both drop targets.
- `app/client/src/types.d.ts` - TypeScript type definitions, may need review if any new types are required

### New Files

- `.claude/commands/e2e/test_increase_drop_zone.md` - E2E test file to validate the enhanced drag-and-drop functionality across both the upper and lower sections of the upload modal

## Implementation Plan
### Phase 1: Foundation
Before implementing the drag-and-drop enhancement, we need to:
1. Understand the current event handler structure in `initializeFileUpload()`
2. Identify the exact DOM elements for both the sample-data-section and drop-zone
3. Review current CSS classes used for dragover states
4. Ensure the existing `handleFileUpload()` function is properly encapsulated for reuse

### Phase 2: Core Implementation
Implement the enhanced drag-and-drop functionality:
1. Refactor `initializeFileUpload()` to accept multiple drop target elements
2. Create a reusable function `attachDragDropHandlers(element, dropZoneText)` that:
   - Attaches dragover, dragleave, and drop event listeners
   - Updates element text to "Drop to create table" during dragover
   - Restores original text on dragleave
   - Calls `handleFileUpload()` on drop
3. Apply drag-drop handlers to both sample-data-section and drop-zone elements
4. Update CSS to add dragover visual feedback for sample-data-section

### Phase 3: Integration
Integrate the enhanced functionality with existing features:
1. Ensure the modal opening behavior remains unchanged
2. Verify sample data buttons continue to work alongside the new drop functionality
3. Test that file validation and error handling work correctly for both drop targets
4. Verify the "Browse Files" button continues to function properly
5. Ensure all existing upload success/error messages display correctly

## Step by Step Tasks
IMPORTANT: Execute every step in order, top to bottom.

### Step 1: Create E2E Test File
- Create `.claude/commands/e2e/test_increase_drop_zone.md` based on the examples in `.claude/commands/e2e/test_basic_query.md`
- The E2E test should validate:
  - Dragging a file over the sample-data-section shows "Drop to create table" text and visual feedback
  - Dropping a file on the sample-data-section successfully uploads the file
  - Dragging a file over the drop-zone shows "Drop to create table" text and visual feedback
  - Dropping a file on the drop-zone successfully uploads the file
  - Both drop targets display proper success messages after upload
- Include screenshots for each major interaction

### Step 2: Refactor Drag-Drop Event Handlers
- Read `app/client/src/main.ts` to understand the current `initializeFileUpload()` implementation
- Extract the drag-drop event handler logic into a reusable function `attachDragDropHandlers(element: HTMLElement, originalContent: string | HTMLElement)`
- The function should:
  - Store the original content of the element (text or HTML)
  - Add dragover event listener that:
    - Prevents default browser behavior
    - Adds visual feedback class (`dragover`)
    - Updates element content to show "Drop to create table"
  - Add dragleave event listener that:
    - Removes visual feedback class
    - Restores original element content
  - Add drop event listener that:
    - Prevents default browser behavior
    - Removes visual feedback class
    - Restores original element content
    - Extracts file from `dataTransfer.files`
    - Calls `handleFileUpload(file)`

### Step 3: Update CSS for Sample Data Section Dragover
- Read `app/client/src/style.css` to understand current dragover styles (lines 255-258)
- Add new CSS rule for `.sample-data-section.dragover`:
  - Border color should change to primary color (matching existing pattern)
  - Background should have light primary color tint (e.g., `rgba(102, 126, 234, 0.05)`)
  - Consider adding a subtle transition effect for smooth visual feedback

### Step 4: Apply Handlers to Sample Data Section
- In `app/client/src/main.ts`, locate where `initializeFileUpload()` is called
- Query for the sample-data-section element: `document.querySelector('.sample-data-section')`
- Store the original HTML content of the sample-data-section
- Call `attachDragDropHandlers(sampleDataSection, originalContent)` to enable drag-drop on this section
- Ensure the original sample data buttons remain functional after applying handlers

### Step 5: Apply Handlers to Drop Zone Section
- Update the existing drop-zone drag-drop handlers to use the new `attachDragDropHandlers()` function
- Store the original HTML content of the drop-zone
- Call `attachDragDropHandlers(dropZone, originalDropZoneContent)` to maintain consistency
- Verify that the "Browse Files" button remains functional

### Step 6: Handle Modal State
- Ensure that drag-drop handlers work correctly whether the modal is open or closed
- If the modal is closed when a file is dragged, consider whether we want to open it automatically (based on user experience considerations)
- Test that closing and reopening the modal doesn't break the drag-drop functionality

### Step 7: Update HTML for Better Semantic Structure
- Review `app/client/index.html` to ensure both sections have appropriate attributes for accessibility
- Add `aria-label` or `role` attributes if needed for screen readers
- Ensure the "Drop to create table" text is semantically appropriate

### Step 8: Manual Testing
- Start the application using `./scripts/start.sh`
- Open the upload modal
- Test dragging a CSV file over the sample data section:
  - Verify "Drop to create table" text appears
  - Verify visual feedback is applied
  - Verify file uploads successfully on drop
- Test dragging a JSON file over the drop zone section:
  - Verify "Drop to create table" text appears
  - Verify visual feedback is applied
  - Verify file uploads successfully on drop
- Test edge cases:
  - Drag file over, then drag away without dropping (should restore original content)
  - Drop invalid file types (should show error)
  - Drop multiple files (should handle gracefully)

### Step 9: Run Validation Commands
- Execute all validation commands listed in the "Validation Commands" section
- Ensure zero regressions in existing functionality
- Fix any issues discovered during validation

## Testing Strategy
### Unit Tests
No new unit tests are required for this feature as it primarily involves frontend DOM manipulation and event handling. The existing `handleFileUpload()` function already has coverage, and we're reusing it.

### E2E Tests
- Test drag-over visual feedback on sample-data-section
- Test drag-over visual feedback on drop-zone
- Test successful file drop on sample-data-section
- Test successful file drop on drop-zone
- Test drag-away behavior (content restoration)
- Test that existing upload methods (Browse Files, sample data buttons) continue to work

### Edge Cases
- Dragging non-file items over the sections (should not break)
- Dragging invalid file types (should show validation error)
- Dragging multiple files simultaneously (should handle first file only)
- Rapid drag-over and drag-away actions (debouncing may be needed)
- Opening and closing modal multiple times (event handlers should not duplicate)
- Dragging over nested elements within the sections (event bubbling should be handled correctly)

## Acceptance Criteria
- Users can drag a file over the sample-data-section and see "Drop to create table" text with visual feedback
- Users can drop a file on the sample-data-section and the file is uploaded successfully
- Users can drag a file over the drop-zone section and see "Drop to create table" text with visual feedback
- Users can drop a file on the drop-zone section and the file is uploaded successfully
- Visual feedback (border color change, background tint) is applied consistently during drag-over
- Original section content is restored when the user drags away without dropping
- All existing upload methods (Upload button → Browse Files, sample data buttons) continue to work without regression
- File validation and error handling work correctly for both new drop targets
- Success and error messages display correctly after file upload
- No console errors occur during drag-and-drop operations
- The feature works across major browsers (Chrome, Firefox, Safari)

## Validation Commands
Execute every command to validate the feature works correctly with zero regressions.

- Read `.claude/commands/test_e2e.md`, then read and execute the new `.claude/commands/e2e/test_increase_drop_zone.md` E2E test file to validate the enhanced drag-and-drop functionality works correctly
- `cd app/server && uv run pytest` - Run server tests to validate the feature works with zero regressions
- `cd app/client && bun tsc --noEmit` - Run frontend tests to validate the feature works with zero regressions
- `cd app/client && bun run build` - Run frontend build to validate the feature works with zero regressions

## Notes
- This feature is purely a frontend enhancement and requires no backend changes
- The existing file upload API (`POST /api/upload`) and `handleFileUpload()` function are reused as-is
- Consider adding a subtle CSS transition for smooth visual feedback during drag-over
- The "Drop to create table" text should be clear and concise, matching the tone of existing UI text
- Future enhancement: Consider adding file type icons or file name preview during drag-over
- Future enhancement: Support drag-and-drop on the main page (outside the modal) to automatically open the modal and upload
- Ensure event handlers are properly cleaned up if the modal is removed from DOM to avoid memory leaks
- Consider using `dragenter` event in addition to `dragover` for more precise drag detection
- Test on both Mac and Windows to ensure cross-platform compatibility of drag-and-drop events

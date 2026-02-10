# E2E Test: Increase Drop Zone Surface Area

Test the enhanced drag-and-drop functionality for file uploads in the Natural Language SQL Interface application.

## User Story

As a user uploading data to the SQL interface
I want to drag and drop files directly onto the sample data section or file upload section
So that I can quickly upload data without having to navigate through button clicks

## Test Steps

### Part 1: Verify Initial State

1. Navigate to the `Application URL`
2. Take a screenshot of the initial state
3. **Verify** the page title is "Natural Language SQL Interface"
4. Click the "Upload Data" button to open the upload modal
5. Take a screenshot of the upload modal
6. **Verify** the modal contains:
   - Sample Data Section with three sample buttons
   - Drop Zone Section with "Drag and drop .csv, .json, or .jsonl files here" text
   - Browse Files button

### Part 2: Test Drag-and-Drop on Sample Data Section (Upper Div)

7. Create a test CSV file with sample data (e.g., users.csv with columns: id, name, email)
8. Drag the CSV file over the sample data section (the upper section with sample buttons)
9. **Verify** visual feedback appears:
   - Border color changes to primary color
   - Background has light primary color tint
   - Text changes to "Drop to create table"
10. Take a screenshot showing the dragover state on sample data section
11. Drop the file on the sample data section
12. **Verify** the file upload is processed successfully
13. **Verify** success message displays
14. **Verify** the uploaded table appears in the Available Tables section
15. Take a screenshot of the success state

### Part 3: Test Drag-and-Drop on Drop Zone Section (Lower Div)

16. Close the upload modal if still open
17. Click "Upload Data" to reopen the modal
18. Create a test JSON file with sample data (e.g., products.json with product objects)
19. Drag the JSON file over the drop zone section (the lower section with "Drag and drop" text)
20. **Verify** visual feedback appears:
   - Border color changes to primary color
   - Background has light primary color tint
   - Text changes to "Drop to create table"
21. Take a screenshot showing the dragover state on drop zone section
22. Drop the file on the drop zone section
23. **Verify** the file upload is processed successfully
24. **Verify** success message displays
25. **Verify** the uploaded table appears in the Available Tables section
26. Take a screenshot of the success state

### Part 4: Test Drag-Away Behavior

27. Click "Upload Data" to open the modal again
28. Drag a file over the sample data section to trigger visual feedback
29. Without dropping, drag the file away from the section
30. **Verify** the original content is restored
31. **Verify** the visual feedback (border color, background) is removed
32. Take a screenshot showing restored state

### Part 5: Test Existing Functionality Still Works

33. Click one of the sample data buttons (e.g., "Users Data")
34. **Verify** the sample data is loaded successfully
35. **Verify** the table appears in the Available Tables section
36. Click "Browse Files" button
37. **Verify** the file input dialog opens
38. Select a CSV file through the file dialog
39. **Verify** the file upload is processed successfully
40. Take a screenshot of the final state

## Success Criteria

- Dragging files over the sample data section shows visual feedback and "Drop to create table" text
- Dropping files on the sample data section successfully uploads the file
- Dragging files over the drop zone section shows visual feedback and "Drop to create table" text
- Dropping files on the drop zone section successfully uploads the file
- Dragging away without dropping restores original content
- Sample data buttons continue to work
- Browse Files button continues to work
- All uploads show proper success messages
- No console errors occur during drag-and-drop operations
- At least 6 screenshots are taken

## Edge Cases to Test

- Drag a file over, then drag away without dropping (should restore original content)
- Drop an invalid file type (should show error message)
- Rapid drag-over and drag-away actions (should handle gracefully)

# Chore: Background Color Update to Light Green

## Metadata
issue_number: `14`
adw_id: `d31168e4`
issue_json: `{"number":14,"title":"Background color update","body":"/chore - adw_sdlc_ZTE_iso - Update the background color to the equivalent light green color."}`

## Chore Description
Update the application's background color from the current light sky blue (#E0F6FF) to an equivalent light green color. This change will provide a fresh, calming appearance while maintaining the same level of readability and visual hierarchy across the application. The light green color should be chosen to provide a similar visual weight and contrast as the existing light sky blue, ensuring all text and UI elements remain clearly visible.

## Relevant Files
Use these files to resolve the chore:

- `app/client/src/style.css` (line 9) - Contains the CSS `--background` variable that controls the application's background color. This is the primary file that needs modification. The current value is `#E0F6FF` (light sky blue) and needs to be changed to an equivalent light green color.

### Reference Documentation
- `app_docs/feature-f055c4f8-off-white-background.md` - Documentation of previous background color change from light gray-blue to off-white, provides context on the CSS variable system and testing approach.
- `app_docs/feature-6445fc8f-light-sky-blue-background.md` - Documentation of the most recent background color change to light sky blue (#E0F6FF), provides the current state and testing procedures.

## Step by Step Tasks
IMPORTANT: Execute every step in order, top to bottom.

### Step 1: Update Background Color Variable
- Read `app/client/src/style.css` to confirm the current background color value
- Select an appropriate light green color that provides equivalent visual appeal and readability as the current light sky blue (#E0F6FF)
  - Recommended: `#E8F5E9` (Material Design Light Green 50) - a soft, light green that maintains similar brightness and saturation levels
  - Alternative options: `#E0F2F1` (light teal-green), `#E1F5DC` (pale green), `#D4EDDA` (mint green)
- Update the `--background` CSS variable on line 9 from `#E0F6FF` to the selected light green color
- Ensure no other CSS variables need adjustment to maintain visual hierarchy

### Step 2: Run Validation Commands
- Execute all validation commands listed below to ensure the chore is complete with zero regressions
- Verify that all tests pass successfully
- Visually confirm the light green background is applied correctly across all UI components

## Validation Commands
Execute every command to validate the chore is complete with zero regressions.

- `cd app/server && uv run pytest` - Run server tests to validate the chore is complete with zero regressions
- `cd app/client && bun run build` - Build the frontend to ensure the CSS changes compile without errors

## Notes
- The background color is controlled by a CSS variable (`--background`) which makes the change simple and centralized
- Based on previous background color changes (off-white and light sky blue), this change only requires modifying a single line in the CSS file
- The light green color should be chosen to maintain the same visual principles as the current light sky blue: calming, professional appearance with excellent readability
- All existing UI components (query section, results section, tables section, modals) should automatically adapt to the new background color without requiring additional changes
- Consider using `#E8F5E9` as it provides a similar brightness level to the current `#E0F6FF` while offering a pleasant light green tone
- After the change, all text and UI elements should maintain proper contrast and visual hierarchy

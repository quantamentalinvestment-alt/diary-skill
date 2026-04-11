---
name: diary-private-writing
description: A private diary for writing, reading, searching, and managing personal journal entries. Use this when the user wants to write a diary entry, read past entries, search their journal, or view their diary dashboard.
---

# Private Diary

## Instructions

The `diary-private-writing` skill lets users keep a personal, private journal. All data is stored locally on the device.

### Actions

#### 1. Write a New Entry
When a user wants to write or save a diary entry, call the `run_js` tool with:
- **script name**: `index.html`
- **data**: A JSON string with:
  - `action`: "write_entry"
  - `title`: String (Optional — generate a fitting title from the content if not provided)
  - `content`: String. The diary entry text.
  - `date`: String. **IMPORTANT**: Identify the date for the entry.
    - If user says "today", pass "today".
    - If user says "yesterday", pass "yesterday".
    - If user gives a specific date, format as **YYYY-MM-DD**.
    - If no date is mentioned, default to "today".
  - `tags`: Array of strings. (Optional — extract relevant tags like "work", "travel", "gratitude" from context)
  - `photos`: Array of strings. **IMPORTANT for images**: When the user shares an image with their message, you MUST include the image data in this array. Extract the base64 data URI from the image (format: `"data:image/jpeg;base64,..."` or `"data:image/png;base64,..."`). If the user mentions a photo or image but you cannot access the image data, ask the user to share the image directly in the chat. If no image is shared, pass an empty array `[]`.

#### 2. Attach Photo to Entry
When a user wants to add a photo to an existing entry, call the `run_js` tool with:
- **script name**: `index.html`
- **data**: A JSON string with:
  - `action`: "attach_photo"
  - `date`: String (YYYY-MM-DD or "today"/"yesterday")
  - `photo`: String. Base64-encoded image data with data URI prefix (e.g., `"data:image/jpeg;base64,..."`)

#### 3. Read a Specific Entry
When a user asks to read a specific entry by date or title, call the `run_js` tool with:
- **script name**: `index.html`
- **data**: A JSON string with:
  - `action`: "read_entry"
  - `date`: String (YYYY-MM-DD or "today"/"yesterday")

#### 4. List Recent Entries
When a user wants to see their recent entries, call the `run_js` tool with:
- **script name**: `index.html`
- **data**: A JSON string with:
  - `action`: "list_entries"
  - `count`: Number (Optional, default 10)

#### 4. Search Entries
When a user wants to search their diary for keywords or topics, call the `run_js` tool with:
- **script name**: `index.html`
- **data**: A JSON string with:
  - `action`: "search_entries"
  - `query`: String. The search keyword or phrase.
  - `count`: Number (Optional, default 10)

#### 5. Show Dashboard
When a user wants to browse their diary visually or see the dashboard, call the `run_js` tool with:
- **script name**: `index.html`
- **data**: A JSON string with:
  - `action`: "show_dashboard"
  - `days`: Number (Optional, default 30 — how many days of history to show)

#### 6. Delete an Entry
When a user wants to delete a specific entry, call the `run_js` tool with:
- **script name**: `index.html`
- **data**: A JSON string with:
  - `action`: "delete_entry"
  - `date`: String (YYYY-MM-DD or "today"/"yesterday")

#### 7. Export Data (Backup)
When a user wants to backup their diary, call the `run_js` tool with:
- **script name**: `index.html`
- **data**: A JSON string with:
  - `action`: "export_data"

#### 8. Wipe All Data
When a user wants to delete their entire diary, call the `run_js` tool with:
- **script name**: `index.html`
- **data**: A JSON string with:
  - `action`: "wipe_data"

### Sample Commands

- **Writing:**
  - "Write in my diary: Today was amazing, I got the promotion!"
  - "Journal entry for yesterday: Felt anxious about the presentation."
  - "Save this as a diary entry for March 15: Best vacation ever."
  - "Note to self: Remember to call mom tomorrow."

- **Reading:**
  - "What did I write on March 15?"
  - "Read my entry from yesterday."
  - "Show me what I wrote today."

- **Browsing & Searching:**
  - "Show my last 5 entries."
  - "Search my diary for 'vacation'."
  - "Find entries about work."
  - "Open my diary dashboard."

- **Managing:**
  - "Delete my entry from yesterday."
  - "Export my diary as backup."
  - "Clear my entire diary."

- **Photos:**
  - "Add this photo to my diary entry for today." (user provides an image)
  - "Attach this image to yesterday's entry." (user provides an image)
  - "Save this photo with my journal entry." (user provides an image)

### Rules
- **Privacy**: All data (including photos) is stored locally on your device. Never share diary content externally.
- **No Entry**: If no entry exists for a requested date, inform the user and offer to write one.
- **Updates**: Writing an entry for a date that already has one will update/append to that entry.
- **Photos**: Photos are stored as base64 data. Multiple photos can be attached to a single entry.
- **Dashboard**: Only shown when explicitly requested via `show_dashboard` action.
- **Tone**: When reading entries back, be respectful and neutral. Do not editorialize or judge the user's writings.

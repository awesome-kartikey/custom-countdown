# System Architecture

This document provides an overview of the architecture, components, and data flow of the Awesome Kartikey Custom Countdown application.

## 1. Overview

The Awesome Kartikey Custom Countdown is a client-side web application built entirely with front-end technologies (HTML, CSS, JavaScript). It does not require a backend server or database. Its primary purpose is to provide a user interface for creating, displaying, and persisting a single countdown timer using the browser's `localStorage`. It operates within a single web page (`index.html`).

## 2. Project Folder Structure

The project follows a flat structure common for simple static web pages:

```
custom-countdown/
│
├── index.html         # Main HTML file (structure, content, UI elements)
├── script.js          # JavaScript file (logic, DOM manipulation, timer, localStorage)
├── style.css          # CSS file (styling, layout, responsiveness, animations)
│
├── clock.png          # Favicon image (referenced in index.html) - *Required Asset*
└── time.mp4           # Background video file (referenced in index.html) - *Required Asset*
```

- **`index.html`:** Defines the user interface structure, including the input form, countdown display area, and completion message area. It links the CSS stylesheet and the JavaScript logic file. It also includes the video background element.
- **`style.css`:** Contains all styling rules for the application, including layout, typography, colors, the video overlay, the completion animation, and media queries for responsiveness.
- **`script.js`:** Holds all the application logic. This includes:
  - Selecting DOM elements.
  - Handling user input events (form submission, button clicks).
  - Calculating time differences.
  - Updating the UI dynamically (showing/hiding elements, updating timer values).
  - Managing the countdown state (starting, stopping, resetting).
  - Interacting with `localStorage` to save and retrieve the countdown details.
- **`clock.png` / `time.mp4`:** External media assets required by the application.

## 3. Major Components

The application can be broken down into the following logical components, primarily managed by `script.js` interacting with elements defined in `index.html`:

1.  **UI View Management:** Controls which section (Input, Countdown, Complete) is visible to the user based on the application state. This is handled by toggling the `hidden` attribute on the respective `div` elements (`inputContainer`, `countdownEl`, `completeEl`).
2.  **Input Handler (`updateCountdown` function):**
    - Triggered by the form (`#countdownForm`) submission.
    - Prevents default page reload.
    - Retrieves title and date from input fields.
    - Performs basic validation (checks if a date was selected).
    - Stores the countdown details in `localStorage`.
    - Calculates the target timestamp (`countdownValue`).
    - Initiates the countdown display by calling `updateDOM`.
3.  **Timer Logic (`updateDOM` function & `setInterval`):**
    - Uses `setInterval` to execute code every second.
    - Calculates the remaining time (`distance`) between `now` and the `countdownValue`.
    - Updates the `<span>` elements within the countdown list (`<li>`) with the calculated days, hours, minutes, and seconds.
    - Checks if the countdown has finished (`distance < 0`). If so, it clears the interval and triggers the completion state.
4.  **State Persistence (`localStorage` & `restorePreviousCountdown`):**
    - `localStorage.setItem('countdown', ...)`: Saves the current countdown details when submitted.
    - `localStorage.getItem('countdown')`: Retrieves saved details on page load.
    - `localStorage.removeItem('countdown')`: Clears saved details when the countdown is reset.
    - `restorePreviousCountdown`: Function called on load to check `localStorage` and resume a countdown if found.
5.  **Reset Logic (`reset` function):**
    - Triggered by clicking the 'RESET' or 'NEW COUNTDOWN' buttons.
    - Clears the active `setInterval`.
    - Resets internal state variables (`countdownTitle`, `countdownDate`).
    - Removes the countdown data from `localStorage`.
    - Resets the UI back to the initial input state.
6.  **Styling & Presentation (`style.css`):** Handles the visual appearance, including the video background, overlay, element positioning, fonts, colors, animations, and responsive adjustments via media queries.

## 4. Data Flow

1.  **Page Load:**
    - `index.html` is loaded.
    - `style.css` is applied.
    - `script.js` executes.
    - `restorePreviousCountdown()` runs:
      - Checks `localStorage` for `'countdown'`.
      - If found: Parses data, calculates `countdownValue`, calls `updateDOM()`. UI transitions to Countdown state.
      - If not found: UI remains in the initial Input state (`#input-container` visible).
2.  **User Creates Countdown:**
    - User enters Title and Date in `#input-container` form.
    - User clicks Submit button.
    - `submit` event triggers `updateCountdown(event)`.
    - `updateCountdown`:
      - Gets Title, Date from form event (`e.srcElement`).
      - Validates Date.
      - Saves `{ title, date }` to `localStorage['countdown']`.
      - Calculates `countdownValue` (target time in ms).
      - Calls `updateDOM()`.
3.  **Countdown Running:**
    - `updateDOM`:
      - Hides `#input-container`.
      - Sets up `setInterval(..., 1000)`.
      - **Inside Interval:**
        - Get current time (`now`).
        - Calculate `distance = countdownValue - now`.
        - If `distance < 0`: Go to Step 4 (Completion).
        - Else: Calculate days, hours, mins, secs from `distance`. Update text content of `<span>` elements in `#countdown`. Show `#countdown`.
4.  **Countdown Completion:**
    - **Inside Interval (when `distance < 0`):**
      - Hide `#countdown`.
      - `clearInterval(countdownActive)`.
      - Update `#complete-info` text.
      - Show `#complete`.
5.  **User Resets:**
    - User clicks 'RESET' or 'NEW COUNTDOWN' button.
    - `click` event triggers `reset()`.
    - `reset`:
      - Hide `#countdown` and `#complete`.
      - Show `#input-container`.
      - `clearInterval(countdownActive)`.
      - Reset `countdownTitle`, `countdownDate`.
      - `localStorage.removeItem('countdown')`.

## 5. Design Decisions

- **Vanilla JavaScript:** Chosen for simplicity and to avoid external dependencies for a small-scale project. This keeps the application lightweight.
- **`localStorage`:** Selected for simple client-side persistence without requiring a backend. Suitable for storing small amounts of non-sensitive data for a single user session persistence.
- **`setInterval`:** Standard JavaScript method for repeatedly executing a function at a fixed time interval, ideal for the timer update logic.
- **Separate Files (HTML/CSS/JS):** Standard practice for separation of concerns, making the code easier to read, maintain, and debug.
- **Direct DOM Manipulation:** Elements are selected and updated directly using `document.getElementById`, `document.querySelectorAll`, and manipulating properties like `.hidden`, `.textContent`. While effective here, larger apps might benefit from frameworks that abstract this.
- **Minimal Error Handling:** Basic check for date presence exists, but more robust error handling (e.g., for `localStorage` failures, invalid date formats if typed) is omitted for simplicity.
- **CSS for State Visibility:** Using the `hidden` attribute controlled via JS is a straightforward way to manage UI state transitions without complex CSS class toggling.

---

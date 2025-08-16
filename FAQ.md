# Frequently Asked Questions (FAQ)

Here are some common questions developers or users might have about the Awesome Kartikey Custom Countdown project.

**Q1: How is the countdown progress saved if I close the browser?**

A: The application uses the browser's `localStorage` API. When you submit a new countdown (title and date), this information is saved as a JSON string in `localStorage` under the key `'countdown'`. When the page loads, the `restorePreviousCountdown` function checks if this key exists in `localStorage`. If it does, it retrieves the saved title and date, recalculates the target time, and updates the display accordingly.

**Q2: What happens if I select a date but no title?**

A: The application will proceed with the countdown using an empty string (`''`) as the title. It will display correctly on the countdown screen, but the `completeElInfo` message will show "[Empty Title] finished on [Date]". You might consider adding validation to require a title if desired.

**Q3: What happens if I don't select a date?**

A: If you submit the form without selecting a date, the `updateCountdown` function checks if `countdownDate` is empty. If it is, an `alert()` box pops up with the message "Please select a date for the countdown." and the countdown process does not start.

**Q4: Can I change the background video or the favicon?**

A: Yes.
*   **Video:** Replace the `time.mp4` file in the root directory with your desired video file (keeping the name `time.mp4`), or update the `<source src="...">` path within the `<video>` tag in `index.html`.
*   **Favicon:** Replace the `clock.png` file, or update the `<link rel="icon" ... href="...">` path in the `<head>` section of `index.html`.

**Q5: Why can't I select a date in the past?**

A: The `script.js` file dynamically sets the `min` attribute of the date input element (`<input type="date">`) to today's date. This is done using the line:
```javascript
const today = new Date().toISOString().split('T')[0];
dateEl.setAttribute('min', today);
```
This prevents users from selecting past dates directly in the date picker UI.

**Q6: Is the application responsive? How does it work on mobile?**

A: Yes, the application includes basic responsiveness using CSS media queries. The `style.css` file contains a `@media screen and (max-width: 600px)` block that adjusts styles for smaller screens. This includes:
*   Making the video cover the full screen.
*   Adjusting the container width and padding.
*   Reducing font sizes for headers and countdown elements.
*   Minor positioning adjustments.

**Q7: Are there any external libraries or frameworks used?**

A: No, this project is built purely using Vanilla HTML, CSS, and JavaScript. It does import a font ("Nunito") from Google Fonts via the `@import` rule in `style.css`.

**Q8: How is the remaining time calculated?**

A: The core calculation happens inside the `setInterval` within the `updateDOM` function:
1.  `countdownValue` stores the target date/time as milliseconds since the epoch (set when the form is submitted).
2.  `const now = new Date().getTime();` gets the *current* time in milliseconds.
3.  `const distance = countdownValue - now;` calculates the difference (remaining time) in milliseconds.
4.  This `distance` is then divided by the millisecond values for a day, hour, minute, and second (using constants `day`, `hour`, `minute`, `second`) and `Math.floor` to get the integer values for display. The modulo operator (`%`) is used to get the remainder for calculating hours, minutes, and seconds within their respective larger units.

**Q9: What happens exactly when the timer finishes?**

A: Inside the `setInterval` loop in `updateDOM`, the code checks if `distance < 0`. If it is:
1.  The countdown display (`countdownEl`) is hidden.
2.  The `setInterval` timer (`countdownActive`) is cleared using `clearInterval()` to stop further updates.
3.  The completion message (`completeElInfo`) is populated with the title and date.
4.  The completion display (`completeEl`) is shown.

---
# Awesome Kartikey Custom Countdown

A simple, visually appealing web application that allows users to create custom countdown timers to any future date. Features a dynamic video background and saves the countdown state locally.

## Description

This project provides a user-friendly interface to set a title and a future date, then displays a real-time countdown showing the remaining days, hours, minutes, and seconds. When the countdown finishes, a completion message is displayed. The application uses `localStorage` to remember the last countdown set, so it persists even if the browser tab is closed and reopened.

## Features

- **Custom Title & Date:** Users can specify a title for their event and select any future date.
- **Real-time Countdown:** Displays remaining time, updating every second.
- **Completion State:** Shows a confirmation message when the countdown reaches zero.
- **Persistence:** Remembers the last active countdown using browser `localStorage`.
- **Reset Functionality:** Allows users to clear the current countdown and create a new one.
- **Video Background:** Includes an aesthetic looping video background.
- **Responsive Design:** Adapts layout for smaller screens (e.g., mobile devices).

## Tech Stack

- **HTML5:** For the structure and content of the web page.
- **CSS3:** For styling, layout, animations, and responsiveness.
- **Vanilla JavaScript:** For application logic, DOM manipulation, time calculations, and `localStorage` interaction.

## Setup Instructions

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/awesome-kartikey/custom-countdown.git
    cd custom-countdown
    ```
2.  **Prepare Assets:** Ensure you have the necessary assets in the project root directory:
    - `time.mp4`: A video file for the background.
    - `clock.png`: A favicon image (optional, referenced in `index.html`).
3.  **Open the Application:** Simply open the `index.html` file in your web browser. No build process or dependencies are required.

    _On macOS:_

    ```bash
    open index.html
    ```

    _On Windows/Linux:_
    Double-click `index.html` or use your respective command line open command.

## Usage

1.  Upon opening `index.html`, you will see the input form.
2.  Enter a **Title** for your countdown event (e.g., "New Year's Eve").
3.  Select a **Date** using the date picker. Note that only today's date and future dates can be selected.
4.  Click the **Submit** button.
5.  The countdown display will appear, showing the time remaining until your selected date and time.
6.  If you close and reopen the page, the countdown will automatically resume from where it left off (if it hasn't finished).
7.  Once the countdown finishes, a "Countdown Complete!" message will be shown along with the event details.
8.  Click **RESET** on the countdown screen or **NEW COUNTDOWN** on the completion screen to return to the input form and create a new countdown.

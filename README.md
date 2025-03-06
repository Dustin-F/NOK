# NOK - Anti-AFK Automation Tool

**NOK** is a sleek and inconspicuous automation tool designed to simulate user activity, preventing AFK (Away From Keyboard) detection in applications or games. It’s lightweight, customizable, and easy to use, making it perfect for users who need to stay active without being physically present.

---

## Features

- **Customizable Key Presses**: Simulates holding down the `W` and `S` keys for random durations.
- **Adjustable Wait Times**: Set custom wait times between actions, with support for both minutes and seconds.
- **Real-Time Logging**: Tracks all actions with timestamps in a scrollable log box.
- **Dynamic Countdown**: Displays the time remaining until the next action.
- **Elapsed Time Tracker**: Shows how long the script has been running.
- **Dark Theme**: A modern and visually appealing dark mode interface.
- **Settings Guide**: Includes a built-in guide to help users configure the tool.
- **Start/Stop Functionality**: Easily start or stop the script with a single click.
- **Standalone Executable**: Compile into a single `.exe` file for easy distribution.

---

## How It Works

NOK randomly holds down the `W` key for a specified duration (1-5 seconds by default), waits for a customizable period (1-5 minutes by default), and then holds down the `S` key for another random duration (3-9 seconds by default). This cycle repeats indefinitely until the script is stopped.

---

## Usage

1. **Start the Script**:
   - Click the **Start** button to begin the automation.
   - A 5-second countdown will initiate before the script starts.

2. **Stop the Script**:
   - Click the **Stop** button to pause the automation.

3. **Customize Settings**:
   - Click the **Settings** button to adjust:
     - `W` key duration (in seconds).
     - `S` key duration (in seconds).
     - Wait time between actions (in minutes and seconds).
   - Use the **Set to Default** button to reset all settings to their default values.

4. **View Logs**:
   - All actions are logged in real-time with timestamps.
   - The log box automatically scrolls to show the latest entries.

---

## Installation

### Prerequisites
- Python 3.x
- Required Python packages: `keyboard`, `tkinter`

### Steps
1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/NOK.git

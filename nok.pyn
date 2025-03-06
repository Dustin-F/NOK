import tkinter as tk
from tkinter import messagebox
import threading
import time
import random
import keyboard

# Global variables
w_min = 1  # W key duration (seconds)
w_max = 5
s_min = 3  # S key duration (seconds)
s_max = 9
wait_min_minutes = 1  # Wait time (minutes)
wait_min_seconds = 0  # Wait time (seconds)
wait_max_minutes = 5  # Wait time (minutes)
wait_max_seconds = 0  # Wait time (seconds)
is_running = False
start_time = 0
next_action_time = 0
log_entries = []
last_log_count = 0  # Track the number of log entries to avoid unnecessary updates

# Dark theme colors
BG_COLOR = "#2E3440"  # Dark background
FG_COLOR = "#D8DEE9"  # Light text
BUTTON_COLOR = "#4C566A"  # Dark button
BUTTON_HOVER_COLOR = "#5E81AC"  # Button hover color
TEXT_COLOR = "#ECEFF4"  # Text color
ENTRY_COLOR = "#3B4252"  # Entry field background
ENTRY_FG_COLOR = "#E5E9F0"  # Entry field text color

# Function to update the GUI
def update_gui():
    global last_log_count
    while True:
        if is_running:
            # Update elapsed time
            elapsed = time.time() - start_time
            hours = int(elapsed // 3600)
            minutes = int((elapsed % 3600) // 60)
            seconds = int(elapsed % 60)
            elapsed_time_label.config(text=f"Elapsed Time: {hours:02}:{minutes:02}:{seconds:02}")

            # Update countdown to next action
            if next_action_time > 0:
                time_remaining = next_action_time - time.time()
                if time_remaining > 0:
                    minutes_remaining = int(time_remaining // 60)
                    seconds_remaining = int(time_remaining % 60)
                    countdown_label.config(text=f"Next action in: {minutes_remaining:02}:{seconds_remaining:02}")
                else:
                    countdown_label.config(text="Next action in: 00:00")
            else:
                countdown_label.config(text="Next action in: 00:00")

        # Update the log box only if there are new entries
        if len(log_entries) != last_log_count:
            log_text.config(state=tk.NORMAL)
            log_text.delete(1.0, tk.END)
            for entry in log_entries:
                log_text.insert(tk.END, entry + "\n")
            log_text.config(state=tk.DISABLED)
            log_text.yview(tk.END)  # Auto-scroll to the bottom
            last_log_count = len(log_entries)  # Update the log count

        time.sleep(0.1)  # Reduce the sleep time for smoother updates

# Function to log events
def log_event(message):
    current_time = time.strftime("%H:%M:%S")
    log_entries.append(f"{current_time} - {message}")

# Function to simulate key presses
def randomize_and_press():
    global is_running, next_action_time

    while is_running:
        # Randomize W key duration
        w_duration = random.randint(w_min, w_max)
        log_event(f"Holding W key for {w_duration} seconds.")
        next_action_time = time.time() + w_duration
        keyboard.press('w')
        time.sleep(w_duration)
        keyboard.release('w')

        # Randomize wait time
        wait_time_minutes = random.randint(wait_min_minutes, wait_max_minutes)
        wait_time_seconds = random.randint(wait_min_seconds, wait_max_seconds)
        wait_time_total = (wait_time_minutes * 60) + wait_time_seconds
        log_event(f"Waiting for {wait_time_minutes} minutes and {wait_time_seconds} seconds.")
        next_action_time = time.time() + wait_time_total
        time.sleep(wait_time_total)

        # Randomize S key duration
        s_duration = random.randint(s_min, s_max)
        log_event(f"Holding S key for {s_duration} seconds.")
        next_action_time = time.time() + s_duration
        keyboard.press('s')
        time.sleep(s_duration)
        keyboard.release('s')

# Function to handle the 5-second delay and script start
def start_script_with_delay():
    global is_running, start_time, log_entries

    if is_running:
        return  # Do nothing if the script is already running

    # Clear logs when starting again
    log_entries.clear()
    log_event("Script starting in 5 seconds...")

    # Disable the Start button during the delay
    start_button.config(state=tk.DISABLED)

    # Use threading to handle the delay without freezing the GUI
    def delay_and_start():
        for i in range(5, 0, -1):  # Countdown from 5 to 1
            log_event(f"Starting in {i}...")
            time.sleep(1)
        global is_running, start_time
        is_running = True
        start_time = time.time()
        log_event("Script started.")
        start_button.config(state=tk.NORMAL)  # Re-enable the Start button
        threading.Thread(target=randomize_and_press, daemon=True).start()

    threading.Thread(target=delay_and_start, daemon=True).start()

# Function to stop the script
def stop_script():
    global is_running
    if is_running:
        is_running = False
        log_event("Script stopped.")

# Function to open settings
def open_settings():
    global w_min, w_max, s_min, s_max, wait_min_minutes, wait_min_seconds, wait_max_minutes, wait_max_seconds

    settings_window = tk.Toplevel(root)
    settings_window.title("Settings")
    settings_window.configure(bg=BG_COLOR)

    # Add a guide for users
    guide_text = (
        "Settings Guide:\n"
        "1. W Key Duration: Set the minimum and maximum duration (in seconds) for holding the W key.\n"
        "2. S Key Duration: Set the minimum and maximum duration (in seconds) for holding the S key.\n"
        "3. Wait Time: Set the minimum and maximum wait time (in minutes and seconds) between actions.\n"
        "4. Click 'Set to Default' to reset all values to their defaults.\n"
        "5. Click 'Save' to apply changes or 'Cancel' to close without saving."
    )
    tk.Label(settings_window, text=guide_text, justify=tk.LEFT, bg=BG_COLOR, fg=FG_COLOR).grid(row=0, column=0, columnspan=3, padx=10, pady=10)

    tk.Label(settings_window, text="W Key Duration (seconds):", bg=BG_COLOR, fg=FG_COLOR).grid(row=1, column=0)
    w_min_entry = tk.Entry(settings_window, bg=ENTRY_COLOR, fg=ENTRY_FG_COLOR, insertbackground=ENTRY_FG_COLOR)
    w_min_entry.insert(0, str(w_min))
    w_min_entry.grid(row=1, column=1)
    w_max_entry = tk.Entry(settings_window, bg=ENTRY_COLOR, fg=ENTRY_FG_COLOR, insertbackground=ENTRY_FG_COLOR)
    w_max_entry.insert(0, str(w_max))
    w_max_entry.grid(row=1, column=2)

    tk.Label(settings_window, text="S Key Duration (seconds):", bg=BG_COLOR, fg=FG_COLOR).grid(row=2, column=0)
    s_min_entry = tk.Entry(settings_window, bg=ENTRY_COLOR, fg=ENTRY_FG_COLOR, insertbackground=ENTRY_FG_COLOR)
    s_min_entry.insert(0, str(s_min))
    s_min_entry.grid(row=2, column=1)
    s_max_entry = tk.Entry(settings_window, bg=ENTRY_COLOR, fg=ENTRY_FG_COLOR, insertbackground=ENTRY_FG_COLOR)
    s_max_entry.insert(0, str(s_max))
    s_max_entry.grid(row=2, column=2)

    tk.Label(settings_window, text="Wait Time (minutes):", bg=BG_COLOR, fg=FG_COLOR).grid(row=3, column=0)
    wait_min_minutes_entry = tk.Entry(settings_window, bg=ENTRY_COLOR, fg=ENTRY_FG_COLOR, insertbackground=ENTRY_FG_COLOR)
    wait_min_minutes_entry.insert(0, str(wait_min_minutes))
    wait_min_minutes_entry.grid(row=3, column=1)
    wait_max_minutes_entry = tk.Entry(settings_window, bg=ENTRY_COLOR, fg=ENTRY_FG_COLOR, insertbackground=ENTRY_FG_COLOR)
    wait_max_minutes_entry.insert(0, str(wait_max_minutes))
    wait_max_minutes_entry.grid(row=3, column=2)

    tk.Label(settings_window, text="Wait Time (seconds):", bg=BG_COLOR, fg=FG_COLOR).grid(row=4, column=0)
    wait_min_seconds_entry = tk.Entry(settings_window, bg=ENTRY_COLOR, fg=ENTRY_FG_COLOR, insertbackground=ENTRY_FG_COLOR)
    wait_min_seconds_entry.insert(0, str(wait_min_seconds))
    wait_min_seconds_entry.grid(row=4, column=1)
    wait_max_seconds_entry = tk.Entry(settings_window, bg=ENTRY_COLOR, fg=ENTRY_FG_COLOR, insertbackground=ENTRY_FG_COLOR)
    wait_max_seconds_entry.insert(0, str(wait_max_seconds))
    wait_max_seconds_entry.grid(row=4, column=2)

    def save_settings():
        global w_min, w_max, s_min, s_max, wait_min_minutes, wait_min_seconds, wait_max_minutes, wait_max_seconds
        new_w_min = int(w_min_entry.get())
        new_w_max = int(w_max_entry.get())
        new_s_min = int(s_min_entry.get())
        new_s_max = int(s_max_entry.get())
        new_wait_min_minutes = int(wait_min_minutes_entry.get())
        new_wait_min_seconds = int(wait_min_seconds_entry.get())
        new_wait_max_minutes = int(wait_max_minutes_entry.get())
        new_wait_max_seconds = int(wait_max_seconds_entry.get())

        # Log the changes
        log_event(f"Settings updated: W Key Duration = {new_w_min}-{new_w_max} seconds, "
                  f"S Key Duration = {new_s_min}-{new_s_max} seconds, "
                  f"Wait Time = {new_wait_min_minutes}:{new_wait_min_seconds:02} to {new_wait_max_minutes}:{new_wait_max_seconds:02}.")

        # Update global variables
        w_min = new_w_min
        w_max = new_w_max
        s_min = new_s_min
        s_max = new_s_max
        wait_min_minutes = new_wait_min_minutes
        wait_min_seconds = new_wait_min_seconds
        wait_max_minutes = new_wait_max_minutes
        wait_max_seconds = new_wait_max_seconds

        messagebox.showinfo("Settings", "Settings saved.")
        settings_window.destroy()

    def set_to_default():
        w_min_entry.delete(0, tk.END)
        w_min_entry.insert(0, "1")
        w_max_entry.delete(0, tk.END)
        w_max_entry.insert(0, "5")
        s_min_entry.delete(0, tk.END)
        s_min_entry.insert(0, "3")
        s_max_entry.delete(0, tk.END)
        s_max_entry.insert(0, "9")
        wait_min_minutes_entry.delete(0, tk.END)
        wait_min_minutes_entry.insert(0, "1")
        wait_min_seconds_entry.delete(0, tk.END)
        wait_min_seconds_entry.insert(0, "0")
        wait_max_minutes_entry.delete(0, tk.END)
        wait_max_minutes_entry.insert(0, "5")
        wait_max_seconds_entry.delete(0, tk.END)
        wait_max_seconds_entry.insert(0, "0")

    # Add Save, Set to Default, and Cancel buttons
    save_button = tk.Button(settings_window, text="Save", command=save_settings, bg=BUTTON_COLOR, fg=FG_COLOR, activebackground=BUTTON_HOVER_COLOR)
    save_button.grid(row=5, column=1, pady=10)
    default_button = tk.Button(settings_window, text="Set to Default", command=set_to_default, bg=BUTTON_COLOR, fg=FG_COLOR, activebackground=BUTTON_HOVER_COLOR)
    default_button.grid(row=5, column=2, pady=10)
    cancel_button = tk.Button(settings_window, text="Cancel", command=settings_window.destroy, bg=BUTTON_COLOR, fg=FG_COLOR, activebackground=BUTTON_HOVER_COLOR)
    cancel_button.grid(row=5, column=3, pady=10)

# Create the main GUI
root = tk.Tk()
root.title("NOK")
root.configure(bg=BG_COLOR)

# Add a cool title
title_label = tk.Label(root, text="NOK", font=("Helvetica", 24, "bold"), bg=BG_COLOR, fg=FG_COLOR)
title_label.pack(pady=10)

tk.Label(root, text="Press Start to begin the script.", bg=BG_COLOR, fg=FG_COLOR).pack()
start_button = tk.Button(root, text="Start", command=start_script_with_delay, bg=BUTTON_COLOR, fg=FG_COLOR, activebackground=BUTTON_HOVER_COLOR)
start_button.pack(pady=5)
stop_button = tk.Button(root, text="Stop", command=stop_script, bg=BUTTON_COLOR, fg=FG_COLOR, activebackground=BUTTON_HOVER_COLOR)
stop_button.pack(pady=5)
settings_button = tk.Button(root, text="Settings", command=open_settings, bg=BUTTON_COLOR, fg=FG_COLOR, activebackground=BUTTON_HOVER_COLOR)
settings_button.pack(pady=5)

countdown_label = tk.Label(root, text="Next action in: 00:00", bg=BG_COLOR, fg=FG_COLOR)
countdown_label.pack()
elapsed_time_label = tk.Label(root, text="Elapsed Time: 00:00:00", bg=BG_COLOR, fg=FG_COLOR)
elapsed_time_label.pack()

# Add a scrollable and wrapping log box
log_frame = tk.Frame(root, bg=BG_COLOR)
log_frame.pack(fill=tk.BOTH, expand=True, padx=10, pady=10)

log_text = tk.Text(log_frame, wrap=tk.WORD, width=50, height=10, bg=ENTRY_COLOR, fg=TEXT_COLOR, insertbackground=TEXT_COLOR)
log_text.pack(side=tk.LEFT, fill=tk.BOTH, expand=True)

scrollbar = tk.Scrollbar(log_frame, orient=tk.VERTICAL, command=log_text.yview)
scrollbar.pack(side=tk.RIGHT, fill=tk.Y)

log_text.config(yscrollcommand=scrollbar.set)
log_text.config(state=tk.DISABLED)  # Make the log box read-only

# Start the GUI update thread
threading.Thread(target=update_gui, daemon=True).start()

# Run the GUI
root.mainloop()
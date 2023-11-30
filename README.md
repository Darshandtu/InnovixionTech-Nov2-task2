import tkinter as tk
from tkinter import ttk, messagebox
from datetime import datetime, timedelta

class AlarmClock:
    def __init__(self, root):
        self.root = root
        self.root.title("Alarm Clock")
        self.root.geometry("400x300")

        self.time_label = ttk.Label(root, font=('calibri', 40, 'bold'),
                                    background='black', foreground='white')
        self.time_label.pack(anchor='center')

        self.alarm_time_entry = ttk.Entry(root, font=('calibri', 15, 'bold'))
        self.alarm_time_entry.pack(pady=20)

        set_alarm_button = ttk.Button(root, text="Set Alarm", command=self.set_alarm)
        set_alarm_button.pack()

    def set_alarm(self):
        alarm_time_str = self.alarm_time_entry.get()
        
        try:
            alarm_time = datetime.strptime(alarm_time_str, "%H:%M")
        except ValueError:
            messagebox.showerror("Error", "Invalid time format. Please use HH:MM.")
            return

        current_time = datetime.now().time()
        current_datetime = datetime.combine(datetime.today(), current_time)
        alarm_datetime = datetime.combine(datetime.today(), alarm_time.time())

        if current_datetime > alarm_datetime:
            alarm_datetime = alarm_datetime + timedelta(days=1)

        time_difference = alarm_datetime - current_datetime
        seconds_to_alarm = time_difference.total_seconds()

        if seconds_to_alarm <= 0:
            messagebox.showinfo("Alarm", "It's time to wake up!")
        else:
            self.root.after(int(seconds_to_alarm * 1000), self.show_alarm_message)

    def show_alarm_message(self):
        messagebox.showinfo("Alarm", "It's time to wake up!")

if __name__ == "__main__":
    root = tk.Tk()
    alarm_clock = AlarmClock(root)
    root.mainloop()

# CODSOFT
import tkinter as tk
from tkinter import messagebox

class TodoList:
    def __init__(self, master):
        self.master = master
        self.tasks = []
        self.load_tasks()

        self.create_widgets()

    def load_tasks(self):
        if os.path.exists('tasks.json'):
            with open('tasks.json', 'r') as file:
                self.tasks = json.load(file)

    def save_tasks(self):
        with open('tasks.json', 'w') as file:
            json.dump(self.tasks, file, indent=4)

    def create_widgets(self):
        self.task_entry = tk.Entry(self.master)
        self.task_entry.pack()

        self.add_button = tk.Button(self.master, text="Add Task", command=self.add_task)
        self.add_button.pack()

        self.remove_button = tk.Button(self.master, text="Remove Task", command=self.remove_task)
        self.remove_button.pack()

        self.update_button = tk.Button(self.master, text="Update Task", command=self.update_task)
        self.update_button.pack()

        self.list_button = tk.Button(self.master, text="List Tasks", command=self.list_tasks)
        self.list_button.pack()

    def add_task(self):
        task = self.task_entry.get()
        if task:
            self.tasks.append(task)
            self.save_tasks()
            self.task_entry.delete(0, tk.END)
            messagebox.showinfo("Success", "Task added successfully.")
        else:
            messagebox.showerror("Error", "Please enter a task.")

    def remove_task(self):
        task_id = int(input("Enter task id to remove: "))
        if 0 <= task_id < len(self.tasks):
            self.tasks.pop(task_id)
            self.save_tasks()
            messagebox.showinfo("Success", "Task removed successfully.")
        else:
            messagebox.showerror("Error", "Invalid task id.")

    def update_task(self):
        task_id = int(input("Enter task id to update: "))
        if 0 <= task_id < len(self.tasks):
            new_task = input("Enter new task: ")
            self.tasks[task_id] = new_task
            self.save_tasks()
            messagebox.showinfo("Success", "Task updated successfully.")
        else:
            messagebox.showerror("Error", "Invalid task id.")

    def list_tasks(self):
        list_window = tk.Toplevel(self.master)
        list_window.title("Tasks")

        for i, task in enumerate(self.tasks):
            tk.Label(list_window, text=f"{i}. {task}").pack()

root = tk.Tk()
todo_list = TodoList(root)
root.mainloop()

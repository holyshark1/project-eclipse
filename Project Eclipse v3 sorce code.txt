import tkinter as tk
from tkinter import filedialog
import subprocess
import os

class IronsightLauncher(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Project Eclipse Launcher")
        self.geometry("960x540")
        self.configure(bg="#121212")

        # Sidebar styling
        self.sidebar = tk.Frame(self, bg="#1f1f1f", width=280)
        self.sidebar.pack(side="left", fill="y")

        # Sidebar contents
        self.sidebar_frame = tk.Frame(self.sidebar, bg="#1f1f1f")
        self.sidebar_frame.pack(fill="both", expand=True)

        self.browse_button = tk.Button(
            self.sidebar_frame, text="Browse", command=self.browse_path,
            bg="#292929", fg="white", font=("Segoe UI", 10)
        )
        self.browse_button.pack(pady=(20, 10), padx=10, anchor="w")

        # Game path (hidden from view)
        self.path_entry = tk.Entry(self.sidebar_frame, width=50)

        # Main launcher area
        self.main_frame = tk.Frame(self, bg="#121212")
        self.main_frame.pack(side="left", fill="both", expand=True, padx=30, pady=30)

        tk.Label(
            self.main_frame, text="Map Name",
            bg="#121212", fg="white", font=("Segoe UI", 12)
        ).pack(anchor="w")

        self.map_entry = tk.Entry(self.main_frame, width=40, font=("Segoe UI", 11))
        self.map_entry.pack(pady=10)

        self.launch_button = tk.Button(
            self.main_frame, text="Launch Ironsight", command=self.launch_game,
            bg="#3d5afe", fg="white", font=("Segoe UI", 13, "bold"), height=2, width=20
        )
        self.launch_button.pack(pady=40)

    def browse_path(self):
        file_path = filedialog.askopenfilename(filetypes=[("EXE files", "Ironsight.exe")])
        if file_path:
            self.path_entry.delete(0, tk.END)
            self.path_entry.insert(0, file_path)

    def launch_game(self):
        exe_path = self.path_entry.get()
        if not exe_path or not os.path.exists(exe_path):
            print("❌ Invalid executable path")
            return

        args = []
        map_name = self.map_entry.get()
        if map_name:
            args.append(f"-map {map_name}")

        try:
            subprocess.Popen([exe_path] + args)
            print("✅ Launching Ironsight with:", args)
        except Exception as e:
            print("❌ Launch failed:", e)

if __name__ == "__main__":
    app = IronsightLauncher()
    app.mainloop()




# Create the main window
window = tk.Tk()
window.title("IP Address Info App")
window.geometry("600x450")
window.configure(bg="#f0f0f0")  # Light gray background

# Display a larger welcome message
welcome_frame = tk.Frame(window, bg="#282c34", pady=15)  # Dark gray-blue frame for welcome text
welcome_frame.pack(fill="x")
welcome_message = tk.Label(welcome_frame, text="Welcome to the IP Information APP", font=("Arial", 20, "bold"),
                           fg="#FFD700", bg="#282c34")  # Gold color for better readability
welcome_message.pack()
quote = random.choice(quotes)
quote_label = tk.Label(welcome_frame, text=quote, font=("Arial", 14, "italic"), fg="white", bg="#282c34")
quote_label.pack()

# Input frame for IP address entry
ip_frame = tk.Frame(window, bg="#f0f0f0", pady=10)
ip_frame.pack()
tk.Label(ip_frame, text="Enter IP Address:", font=("Arial", 12, "bold"), bg="#f0f0f0").grid(row=0, column=0, padx=5,
                                                                                            pady=5)
ip_entry = tk.Entry(ip_frame, width=30, font=("Arial", 12))
ip_entry.grid(row=0, column=1, padx=5, pady=5)

# Buttons frame for actions
button_frame = tk.Frame(window, bg="#f0f0f0", pady=10)
button_frame.pack()
fetch_button = tk.Button(button_frame, text="Get IP Info", command=fetch_ip_info, font=("Arial", 12), bg='#4CAF50',
                         fg='white')
fetch_button.grid(row=0, column=0, padx=20, pady=5)
know_ip_button = tk.Button(button_frame, text="Know Your IP Address", command=know_your_ip, font=("Arial", 12),
                           bg='#2196F3', fg='white')
know_ip_button.grid(row=0, column=1, padx=20, pady=5)

# Information frame to display the fetched IP details
info_frame = tk.Frame(window, bg="#f0f0f0", pady=10)
info_frame.pack()
ip_label = tk.Label(info_frame, text="IP Address: ", font=("Arial", 12), bg="#f0f0f0")
ip_label.grid(row=0, column=0, sticky="w", padx=10, pady=5)
location_label = tk.Label(info_frame, text="Location: ", font=("Arial", 12), bg="#f0f0f0")
location_label.grid(row=1, column=0, sticky="w", padx=10, pady=5)
isp_label = tk.Label(info_frame, text="ISP: ", font=("Arial", 12), bg="#f0f0f0")
isp_label.grid(row=2, column=0, sticky="w", padx=10, pady=5)
lat_long_label = tk.Label(info_frame, text="Latitude, Longitude: ", font=("Arial", 12), bg="#f0f0f0")
lat_long_label.grid(row=3, column=0, sticky="w", padx=10, pady=5)

# Start the main event loop
window.mainloop()




import tkinter as tk
from tkinter import messagebox
import requests
import random
import re

# Quotes about IP Addresses
quotes = [
    "An IP address is the identity of your device on the internet.",
    "The IP address is like your home's address, unique and essential.",
    "Every device connected to the internet has a unique IP address.",
    "Without an IP address, devices can't communicate over the internet.",
    "IP addresses are the building blocks of internet communication."
]


# Function to validate the IP address
def is_valid_ip(ip_address):
    # Regular expression pattern for matching a valid IP address
    pattern = r"^\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}$"
    if re.match(pattern, ip_address):
        # Split the IP address to check if each part is between 0 and 255
        parts = ip_address.split(".")
        for part in parts:
            if int(part) < 0 or int(part) > 255:
                return False
        return True
    return False

# Function to get IP information using the ipstack API
def get_ip_info(api_key, ip_address='check'):
    try:
        # Use HTTPS for secure connection
        url = f"https://api.ipstack.com/{ip_address}?access_key={api_key}"
        # Send a GET request to the API
        response = requests.get(url)
        # Check if the request was successful
        if response.status_code == 200:
            # Parse the JSON response
            data = response.json()
            # Check if the response contains an IP field (indicating a valid response)
            if "ip" not in data:
                messagebox.showerror("Error", "The IP address is not valid or does not exist.")
                return

            # Extract relevant information
            ip_address = data.get("ip", "Unknown")
            city = data.get("city", "Unknown")
            region = data.get("region_name", "Unknown")
            country = data.get("country_name", "Unknown")
            isp = data.get("connection", {}).get("isp", "Unknown")
            latitude = data.get("latitude", "Unknown")
            longitude = data.get("longitude", "Unknown")

            # Display the information in the GUI
            ip_label.config(text=f"IP Address: {ip_address}")
            location_label.config(text=f"Location: {city}, {region}, {country}")
            isp_label.config(text=f"ISP: {isp}")
            lat_long_label.config(text=f"Latitude: {latitude}, Longitude: {longitude}")
        else:
            messagebox.showerror("Error", f"Failed to retrieve IP information. Status Code: {response.status_code}")
    except Exception as e:
        messagebox.showerror("Error", f"An error occurred: {e}")


# Function to fetch and display IP information when the "Know Your IP Address" button is clicked
def know_your_ip():
    api_key = '55fd0be677dcaf7e08c9b772b07a5877'
    get_ip_info(api_key)


# Function to fetch IP info based on user input
def fetch_ip_info():
    ip_address = ip_entry.get().strip()
    api_key = '55fd0be677dcaf7e08c9b772b07a5877'
    # Validate the IP address
    if not is_valid_ip(ip_address):
        messagebox.showerror("Invalid IP Address", "Please enter a valid IP address.")
        return
    get_ip_info(api_key, ip_address)


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







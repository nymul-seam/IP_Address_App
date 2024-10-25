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




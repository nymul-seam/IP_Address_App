def get_ip_info(api_key, ip_address='check'):
    try:
        url = f"http://api.ipstack.com/{ip_address}?access_key=55fd0be677dcaf7e08c9b772b07a5877"
        response = requests.get(url)
        if response.status_code == 200:
            data = response.json()
            ip_address = data.get("ip")
            city = data.get("city", "Unknown")
            region = data.get("region_name", "Unknown")
            country = data.get("country_name", "Unknown")
            isp = data.get("connection", {}).get("isp", "Unknown")
            latitude = data.get("latitude")
            longitude = data.get("longitude")
            print(f"IP Address: {ip_address}")
            print(f"Location: {city}, {region}, {country}")
            print(f"ISP: {isp}")
            print(f"Latitude: {latitude}, Longitude: {longitude}")
            
            more_info = input("Do you want to look up another IP address? (yes/no): ").strip().lower()
            if more_info == 'yes':
                new_ip = input("Enter the IP address to look up: ").strip()
                get_ip_info(api_key, new_ip)
        else:
            print(f"Failed to retrieve IP information. Status Code: {response.status_code}")
    except Exception as e:
        print(f"An error occurred: {e}")

if __name__ == "__main__":
    api_key = '55fd0be677dcaf7e08c9b772b07a5877'
    get_ip_info(api_key)

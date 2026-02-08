# Camp-notes
import requests
import time

# SETTINGS
CAMPGROUND_ID = "233907"  # Example: Rock Creek Lake
TARGET_MONTH = "2026-08
-01T00:00:00.000Z" # Start of the month
HEADERS = {
    "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36",
    "Referer": f"https://www.recreation.gov/camping/campgrounds/{CAMPGROUND_ID}"
}

def check_availability():
    url = f"https://www.recreation.gov/api/stats/availability/campground/{CAMPGROUND_ID}/month?month={TARGET_MONTH}"
    
    response = requests.get(url, headers=HEADERS)
    if response.status_code != 200:
        print(f"Error: {response.status_code}")
        return

    data = response.json()
    campsites = data.get('campsites', {})
    
    available_sites = []
    
    for site_id, site_data in campsites.items():
        # Check if any date in the month is marked as 'Available'
        for date, status in site_data['availabilities'].items():
            if status == "Available":
                available_sites.append(f"Site {site_data['site_num']} on {date[:10]}")

    if available_sites:
        print(f"🎉 FOUND {len(available_sites)} OPENINGS:")
        for site in available_sites:
            print(f" - {site}")
        # Insert your notification logic here (e.g., Telegram or Email)
    else:
        print("No availability found.")

if __name__ == "__main__":
    check_availability()

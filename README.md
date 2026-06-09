import requests
import time
from datetime import datetime

WEBSITES = [
    "https://google.com",
    "https://github.com",
    "https://stackoverflow.com"
]

CHECK_INTERVAL = 60  # seconds


def check_website(url):
    try:
        response = requests.get(url, timeout=5)

        if response.status_code == 200:
            return "UP"
        else:
            return f"DOWN ({response.status_code})"

    except Exception as e:
        return f"ERROR ({e})"


def monitor():
    print("Website Monitor Started...\n")

    while True:

        print(f"\n[{datetime.now()}]")

        for site in WEBSITES:

            status = check_website(site)

            print(f"{site:<35} {status}")

            with open("monitor.log", "a", encoding="utf-8") as f:
                f.write(
                    f"{datetime.now()} | {site} | {status}\n"
                )

        time.sleep(CHECK_INTERVAL)


if __name__ == "__main__":
    monitor()

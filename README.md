# zizi-repo
like a moon
import requests
import datetime

def get_bitcoin_price():
    url = "https://api.coindesk.com/v1/bpi/currentprice.json"
    response = requests.get(url)
    
    if response.status_code == 200:
        data = response.json()
        price_usd = data["bpi"]["USD"]["rate"]
        time = data["time"]["updated"]
        result = f"[{time}] Bitcoin price: ${price_usd}"
    else:
        result = "Failed to fetch Bitcoin price!"
    
    print(result)

    # Save to log file
    with open("btc_price.log", "a") as f:
        f.write(f"{datetime.datetime.now()} - {result}\n")

if __name__ == "__main__":
    print("=== Bitcoin Price Tracker ===")
    get_bitcoin_price()


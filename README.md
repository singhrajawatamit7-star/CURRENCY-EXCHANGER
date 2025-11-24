# CURRENCY-EXCHANGER
It converts one currency to another mathematically
import requests

def get_exchange_rate(base_currency, target_currency):
    """
    Fetches the exchange rate from a public API.
    The API used has a base currency of USD for its free tier.
    """
    API_URL = f'https://fixer.io/confirm-email?confirmation_key=23a32ea203f2e7ebafa595cdccfb9203&signup=1'
    try:
        response = requests.get(API_URL)
        data = response.json()

        if response.status_code == 200 and "rates" in data:
            rate = data["rates"].get(target_currency)
            if rate is not None:
                return rate
            else:
                print(f"Error: Target currency '{target_currency}' not found in the data.")
                return None
        else:
            print(f"Error fetching exchange rates. Status code: {response.status_code}")
            return None
    except requests.exceptions.RequestException as e:
        print(f"Error: Network issue or invalid request - {e}")
        return None

def convert_currency():
    """
    Takes user input and performs the currency conversion.
    """
    print("\n--- Python Currency Converter ---")
    
    # Get user input for currencies
    base_currency = input("Enter base currency code (e.g., USD): ").upper()
    target_currency = input("Enter target currency code (e.g., INR): ").upper()

    try:
        # Get user input for amount
        amount = float(input(f"Enter amount in {base_currency} to convert: "))

        # Fetch the exchange rate
        exchange_rate = get_exchange_rate(base_currency, target_currency)

        if exchange_rate:
            converted_amount = amount * exchange_rate
            print(f"\nResult: {amount} {base_currency} = {converted_amount:.2f} {target_currency}")
        else:
            print("\nCould not perform conversion. Please check currency codes and try again.")

    except ValueError:
        print("\nError: Please enter a valid numerical amount.")
    except Exception as e:
        print(f"\nAn unexpected error occurred: {e}")

if __name__ == "__main__":
    convert_currency()

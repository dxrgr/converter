import requests

API_KEY = '8eb9bf5b6ae7d0468e351bfb'
BASE_URL = 'https://api.exchangerate-api.com/v4/latest/RUB'

def get_exchange_rates():
    url = f"{BASE_URL}?app_id={API_KEY}"
    response = requests.get(url)
    if response.status_code == 200:
        data = response.json()
        return data['rates']
    else:
        print("Ошибка при получении данных с API")
        return None

def convert_rub_to_usd(amount, rates):
    if 'RUB' in rates and 'USD' in rates:
        rub_to_usd_rate = rates['USD'] / rates['RUB']
        return amount * rub_to_usd_rate
    else:
        print("Не удалось найти необходимые курсы валют.")
        return None

def main():
    print("Добро пожаловать в конвертер валют!")
    rates = get_exchange_rates()

    if rates:
        while True:
            try:
                rub_amount = float(input("Введите сумму в рублях (или 'exit' для выхода): "))
                usd_amount = convert_rub_to_usd(rub_amount, rates)
                if usd_amount is not None:
                    print(f"{rub_amount} рублей равны {usd_amount:.2f} долларов.")
            except ValueError:
                print("Выход из программы.")
                break

if __name__ == "__main__":
    main()

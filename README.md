import requests
import json
import time

API_KEY = "https://www.weatherapi.com/"
BASE_URL = "http://api.openweathermap.org/data/2.5/weather"

def get_weather(city):
    params = {"q": city, "appid": API_KEY}
    response = requests.get(BASE_URL, params=params)

    if response.status_code == 200:
        weather_data = response.json()
        return weather_data
    else:
        return None

def display_weather(weather_data):
    if weather_data:
        print(f"Weather in {weather_data['name']}, {weather_data['sys']['country']}:")
        print(f"Description: {weather_data['weather'][0]['description']}")
        print(f"Temperature: {weather_data['main']['temp']}°C")
        print(f"Humidity: {weather_data['main']['humidity']}%")
    else:
        print("Error fetching weather data.")

def auto_refresh(city):
    while True:
        weather_data = get_weather(city)
        display_weather(weather_data)
        time.sleep(30)  # Adjust as needed for the desired refresh interval

def main():
    print("Welcome to the Weather Checking Application!")

    while True:
        print("\nOptions:")
        print("1. Check weather by city name")
        print("2. Auto-refresh weather")
        print("3. Exit")

        choice = input("Enter your choice (1/2/3): ")

        if choice == "1":
            city_name = input("Enter the city name: ")
            weather_data = get_weather(city_name)
            display_weather(weather_data)

        elif choice == "2":
            city_name = input("Enter the city name for auto-refresh: ")
            auto_refresh(city_name)

        elif choice == "3":
            print("Exiting the Weather Checking Application. Goodbye!")
            break

        else:
            print("Invalid choice. Please enter a valid option.")

if __name__ == "__main__":
    main()

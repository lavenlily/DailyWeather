# DailyWeather
Script for pulling daily weather

import requests

# --- CONFIGURATION ---
API_KEY = 'YOUR_API_KEY'  # Replace with your OpenWeatherMap API key
CITY_NAME = 'Blacksburg'  # Replace with your city name

# URL for the current weather API
BASE_URL = "http://api.openweathermap.org/data/2.5/weather"

# --- FUNCTION TO GET WEATHER DATA ---
def get_weather(city, api_key):
    """Fetches weather data for a given city."""
    params = {
        'q': city,
        'appid': api_key,
        'units': 'imperial'  # Use 'metric' for Celsius
    }
    try:
        response = requests.get(BASE_URL, params=params)
        response.raise_for_status()  # Raise an exception for bad status codes
        return response.json()
    except requests.exceptions.RequestException as e:
        print(f"Error fetching weather data: {e}")
        return None

# --- MAIN SCRIPT EXECUTION ---
if __name__ == "__main__":
    weather_data = get_weather(CITY_NAME, API_KEY)
    
    if weather_data:
        try:
            # Extract key information from the JSON response
            temp = weather_data['main']['temp']
            feels_like = weather_data['main']['feels_like']
            humidity = weather_data['main']['humidity']
            description = weather_data['weather'][0]['description']
            
            # Print the formatted report
            print(f"Daily Weather Report for {CITY_NAME.title()}")
            print("-" * 30)
            print(f"Current Temperature: {temp}°F")
            print(f"Feels Like: {feels_like}°F")
            print(f"Humidity: {humidity}%")
            print(f"Conditions: {description.title()}")
            
        except KeyError as e:
            print(f"Could not parse weather data. Missing key: {e}")
            print(weather_data)
    else:
        print("Could not retrieve weather data. Please check your city name and API key.")

# rain-alert-app
import requests
!pip install twilio
from twilio.rest import Client

API_key  = "9cff84c845f13d402c92d86fe15bb775"
URL = "https://api.openweathermap.org/data/3.0/onecall"
Account_SID = "ACcf1b69530ea5e5a1933b1127a9de8d91"
Auth_token = "92e9984de15b9d8a6b6ab495d2c32802"
My_twilio_phone_number = "+13478942230"


parameters = {
	"lat":-6.250490,
	"lon":106.909180,
	"appid":API_key,
	"exclude":"current,minutely,daily"
}

response = requests.get(url=URL,
                        params=parameters)

response.raise_for_status()
weather_data = response.json()


will_rain = False
# using slice function. this is a list
weather_slice = weather_data["hourly"][:12]    
for weather in weather_slice:
	condition_id = weather["weather"][0]["id"]
	if condition_id < 700:
		will_rain = True

if will_rain:
	client = Client(Account_SID, Auth_token)
	message = client.messages \
		.create(
		body="It's going to rain today. Remember to bring an umbrella ☔☔☔",
		from_= My_twilio_phone_number,
		to= '+6285863102572'
	)
	print(message.status)




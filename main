import requests
import datetime
from config import weather_token, telegram_token
from aiogram import Bot, types
from aiogram.dispatcher import Dispatcher
from aiogram.utils import executor


bot = Bot(token=telegram_token)
dp = Dispatcher(bot)

@dp.message_handler(commands=["start"])
async def start_command(message: types.Message):
    await message.reply("Привет! Напиши мне название города и я пришлю сводку погоды!")

@dp.message_handler()
async def get_weather(message: types.Message):
	weather_in_smile = {
		"Clear": "Ясно \U00002600",
		"Clouds": "Облачно \U00002601",
		"Rain": "Дождь \U00002614",
		"Drizzle": "Дождь \U00002614",
		"Thunderstorm": "Гроза \U000026A1",
		"Snow": "Снег \U0001F328",
		"Mist": "Туман \U0001F32B"}

	try:
		r = requests.get(
			f"http://api.openweathermap.org/data/2.5/weather?q={message.text}&appid={weather_token}&units=metric"
		)
		data = r.json()

		city = data["name"]
		cur_weather = data["main"]["temp"]

		weather_description = data["weather"][0]["main"]
		if weather_description in code_to_smile:
			wd = code_to_smile[weather_description]
		else:
			wd = "Посмотри в окно!"

		humidity = data["main"]["humidity"]
		pressure = data["main"]["pressure"]
		wind = data["wind"]["speed"]
		sunrise = datetime.datetime.fromtimestamp(data["sys"]["sunrise"])
		sunset = datetime.datetime.fromtimestamp(data["sys"]["sunset"])
		day = datetime.datetime.fromtimestamp(data["sys"]["sunset"]) - datetime.datetime.fromtimestamp(
			data["sys"]["sunrise"])

		await message.answer(
			f"Погода в {city}, время {datetime.datetime.now().strftime('%H:%M')}:  \n\n"
			f"{wd}\n"
			f"Температура: {cur_weather}C° \n"
			f"Влажность: {humidity}%\n"
			f"Давление: {pressure} мм.рт.ст\n"
			f"Ветер: {wind} м/с\n\n"
			f"Восход солнца: {sunrise}\n"
			f"Закат солнца: {sunset}\n\n"
			f"Продолжительность дня: {day}\n\n"
)

	except:
		await message.reply("\U00002620 Ошибка: Проверьте название города \U00002620")

if __name__ == '__main__':
	executor.start_polling(dp)

import pafy
import logging
from aiogram import Bot, Dispatcher, executor, types
import os
API_TOKEN = 'ТВОЙ КЛЮЧ'
logging.basicConfig(level=logging.INFO)
bot = Bot(token=API_TOKEN)
dp = Dispatcher(bot)
def download(url):
    you = pafy.new(url)
    you.getbestvideo().download()
@dp.message_handler(commands=['start',  'help'])
async def send_welcome(message: types.Message):
    if os.path.exists(f'{os.getcwd()}\avi'):
        for i in os.listdir(f'{os.getcwd()}\avi'):
            os.remove(i)
    await message.reply("Привет, я бот, который скачивает видео, скинь ссылку)")
@dp.message_handler()
async def echo(message: types.Message):
    try:
        if 'https://you' in message.text:
            download(message.text)
            name = pafy.new(message.text).title
            with open(f'{name}.webm', 'rb') as video:
                await bot.send_video(message.from_user.id, video)
            os.remove(f'{name}.webm')
    except:
        await bot.send_message(message.from_user.id, 'Повторите попытку...')
if __name__ == '__main__':
    executor.start_polling(dp, skip_updates=True)

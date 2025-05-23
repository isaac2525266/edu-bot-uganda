import logging from aiogram import Bot, Dispatcher, types, executor from aiogram.types import InlineKeyboardMarkup, InlineKeyboardButton import openai import os

=== SETUP ===

API_TOKEN = '7771608687:AAGdd5omJpPTYKFPwwfT3xOiFVUHbBcXQwo' OPENROUTER_API_KEY = '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' ADMIN_ID = 8064364873

logging.basicConfig(level=logging.INFO)

bot = Bot(token=API_TOKEN) dp = Dispatcher(bot)

=== START KEYBOARD ===

def main_keyboard(): kb = InlineKeyboardMarkup(row_width=2) kb.add( InlineKeyboardButton("STUDENT", callback_data="student"), InlineKeyboardButton("CODING", callback_data="coding"), InlineKeyboardButton("PERSONAL ASSISTANT", callback_data="assistant"), InlineKeyboardButton("WORK & CAREER HELP", callback_data="career"), InlineKeyboardButton("CONTACT / SUPPORT", callback_data="contact") ) return kb

=== START ===

@dp.message_handler(commands=['start']) async def send_welcome(message: types.Message): text = ( "Welcome to EDU-BOT UGANDA!\n" "Your smart assistant for learning, coding, personal support, and career help.\n\n" "Created by TAMWENYA ISAAC\n" ""Empowering Uganda through AI & Education"\n\n" "Your privacy is respected. This bot does not store your messages." ) await message.answer(text, reply_markup=main_keyboard())

=== CALLBACK HANDLER ===

@dp.callback_query_handler() async def handle_callbacks(call: types.CallbackQuery): section = call.data if section == "contact": await call.message.answer("For support or to donate, contact: tamwenyaisaac90@gmail.com") else: await call.message.answer("Send your question or request in this category: " + section.title())

=== ADMIN COMMANDS ===

@dp.message_handler(commands=['admin']) async def admin_help(message: types.Message): if message.from_user.id == ADMIN_ID: await message.reply("Admin Commands:\n/broadcast <msg>\n/stats\n/tip <text>\n/shutdown")

@dp.message_handler(commands=['broadcast']) async def broadcast(message: types.Message): if message.from_user.id == ADMIN_ID: text = message.text.replace('/broadcast', '').strip() if text: # Store and iterate users in production await message.reply("Broadcast sent (simulated).") else: await message.reply("Please provide a message.")

@dp.message_handler(commands=['stats']) async def stats(message: types.Message): if message.from_user.id == ADMIN_ID: await message.reply("Users: [simulated count].")

@dp.message_handler(commands=['tip']) async def tip(message: types.Message): if message.from_user.id == ADMIN_ID: tip_text = message.text.replace('/tip', '').strip() await message.reply(f"Today's tip: {tip_text}")

@dp.message_handler(commands=['shutdown']) async def shutdown(message: types.Message): if message.from_user.id == ADMIN_ID: await message.reply("Bot is shutting down (simulated).")

=== AI RESPONSE (SIMULATED ===

@dp.message_handler() async def ai_reply(message: types.Message): await message.reply("[AI reply simulated using openchat-3.5]")

if name == 'main': executor.start_polling(dp, skip_updates=True)
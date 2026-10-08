import os
import requests
from telegram import Update
from telegram.ext import Application, CommandHandler, ContextTypes

TOKEN = os.environ["TELEGRAM_BOT_TOKEN"]

def get_price(symbol):
    response = requests.get(
        "https://api.binance.com/api/v3/ticker/price",
        params={"symbol": symbol},
        timeout=10
    )
    response.raise_for_status()
    return float(response.json()["price"])

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(
        "🤖 Binance Predictor дайын!\n\n"
        "/btc — BTC бағасы\n"
        "/scan — нарық сканері"
    )

async def btc(update: Update, context: ContextTypes.DEFAULT_TYPE):
    try:
        price = get_price("BTCUSDT")
        await update.message.reply_text(f"₿ BTC бағасы: ${price:,.2f}")
    except Exception:
        await update.message.reply_text("BTC бағасын алу кезінде қате шықты.")

async def scan(update: Update, context: ContextTypes.DEFAULT_TYPE):
    coins = ["BTCUSDT", "ETHUSDT", "SOLUSDT", "XRPUSDT", "ADAUSDT"]
    text = "📊 Нарық сканері\n\n"

    for symbol in coins:
        try:
            price = get_price(symbol)
            name = symbol.replace("USDT", "")
            text += f"{name}: ${price:,.4f}\n"
        except Exception:
            text += f"{symbol}: дерек алынбады\n"

    await update.message.reply_text(text)

app = Application.builder().token(TOKEN).build()

app.add_handler(CommandHandler("start", start))
app.add_handler(CommandHandler("btc", btc))
app.add_handler(CommandHandler("scan", scan))

app.run_polling()

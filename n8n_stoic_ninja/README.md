# 🥷 Stoic Ninja

**Stoic Ninja** is an automated workflow built with [n8n](https://n8n.io/) that delivers a dose of daily Stoic wisdom directly to your Telegram via a scheduled task.

It uses the [Stoic API](https://github.com/pierretfie/stoic-api.git) to fetch random Stoic quotes and pushes them to you every morning using a Telegram bot.

---

## 📌 Features

- ⏰ **Daily Trigger** – Runs automatically every morning at a configured time.
- 📖 **Quote Source** – Pulls from the open-source [Stoic API](https://github.com/pierretfie/stoic-api.git).
- 📬 **Telegram Delivery** – Sends the quote with a meaningful message using Telegram bot integration.
- 💡 **Minimal Setup** – Fully containerized and schedule-driven with n8n's built-in tools.

---

## ⚙️ How It Works

1. **Schedule Trigger** (7:00 AM daily) starts the workflow.
2. **HTTP Request** node fetches a quote from the Stoic API.
3. **Set Node** formats the quote and attaches an explanation or advice (optional).
4. **Telegram Node** sends the message to a configured chat ID.

---

## 🚀 Getting Started

### 1. Clone and Run n8n via docker

### 2. Configure `.env` or `docker-compose.yml`

### 3. Setup the Workflow

* Import the Stoic Ninja workflow into your n8n instance.
* Update:

  * The Stoic API URL (`https://stoic-api.onrender.com/quote`)
  * Your Telegram **Chat ID**
* Activate the workflow.

---

## 📬 Telegram Setup

1. Talk to [@BotFather](https://t.me/BotFather) and create a new bot.
2. Save the **bot token** and paste it in the Telegram node in n8n.
3. Message your bot at least once to get your **chat ID**:

   ```bash
   https://api.telegram.org/bot<your_token>/getUpdates
   ```

---

## 💭 Example Output

> *“You have power over your mind — not outside events. Realize this, and you will find strength.”*
> — Marcus Aurelius

---

## 📚 Resources

* [n8n Documentation](https://docs.n8n.io/)
* [Stoic API by @pierretfie](https://github.com/pierretfie/stoic-api)
* [Telegram Bot API](https://core.telegram.org/bots/api)

---

## 🧘 Inspiration

Stoic Ninja was built to deliver ancient wisdom in modern workflows. Reflect, act with virtue, and start each day with clarity.

---

## 📄 License

This project is open-source and provided under the MIT License.





# 🥷 Stoic Ninja


<img width="828" height="465" alt="image" src="https://github.com/user-attachments/assets/25b07103-9cb5-4cad-8f2c-551a40646ca2" />

**Stoic Ninja** is an automated workflow built with [n8n](https://n8n.io/) that delivers a dose of daily Stoic wisdom directly to your Telegram via a scheduled task.
It uses google Gemini AI to refine your requests and responses as desired.
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

### 1. Setup docker


### 2. Setup the Workflow

* Import the Stoic Ninja workflow into your n8n instance.
* Update:

  * The Stoic API URL (`https://stoic-api.onrender.com/quote`)
  * Your Telegram **acess token** and **Chat ID**
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
**Good morning! 🌞**
Here is your Stoic wisdom for the day:

> *“You’re unable to make someone change his views, recognize that he is a child, and clap as he does. Or if you don’t care to act in such a way, you have only to keep quiet.”*
> — *Epictetus, Discourses*

**Meaning:**
This quote from Epictetus reflects a timeless Stoic insight: we cannot control others’ thoughts or behaviors, especially when they are irrational or immature. He compares such individuals to children—unaware of their folly, acting out in ways they can’t fully comprehend. We are presented with two options: either humor them gently, much like applauding a child’s play, or choose silence and disengagement. In both cases, the Stoic ideal remains the same — maintain inner calm, act with intention, and focus only on what lies within your control.

**Daily Advice:**
Today, you may come across someone whose behavior challenges your patience — perhaps they are stubborn, misinformed, or emotional. Instead of trying to convince them or becoming frustrated, remember that their thoughts are not yours to manage. You can either engage lightly, without emotional investment, or withdraw quietly and protect your peace. Let your actions be guided by wisdom, not impulse. This is the Stoic way: to stay grounded in reason while letting others walk their own path.

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





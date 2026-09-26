# Auth service configuration

The frontend expects a backend API. `.env.local` points `VITE_AUTH_API_URL` to `http://localhost:3001`. Keep the Telegram bot token in the server's `.env.server` file; never put it in a `VITE_` variable.

## Local setup

1. Create a bot with Telegram's `@BotFather` and keep its token private.
2. Copy `.env.server.example` to `.env.server`; set `TELEGRAM_BOT_TOKEN` and `TELEGRAM_BOT_USERNAME` (without `@`).
3. Run `node server/index.js` in a terminal.
4. Each user must open the bot, press Start, and share their own `+998` contact once. Then the app can send that phone's OTP to the linked Telegram chat.

Restart the auth server after changing `.env.server`. The API stores linked Telegram chat IDs and accounts in ignored `server/data.json`.

## Required endpoints

- `POST /api/auth/login` accepts `{ "email": "...", "password": "...", "role": "doctor" | "resident" }` and returns `{ "user": { ... } }` only after checking the account and role.
- `POST /api/auth/telegram/send-code` accepts `{ "phone": "+998 ..." }`, sends a short-lived code to that phone's previously linked Telegram chat, and returns `{ "retryAfterSeconds": 60 }`.
- `POST /api/auth/telegram/verify-and-register` accepts the phone, code, role, and registration data. It validates and consumes the code, rejects incorrect/expired codes, and creates the account only after verification. It returns the new user on success.

## Telegram requirements

Telegram bots cannot start a private conversation using only a phone number. Users must first start the bot and share their own phone contact; the backend links that contact to their Telegram chat ID. Keep bot tokens server-side, rate-limit code requests and attempts, and avoid logging codes. After setting `TELEGRAM_BOT_TOKEN` in `.env.server`, restart the API with `node server/index.js`.


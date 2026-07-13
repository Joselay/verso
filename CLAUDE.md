# Verso — Gold (XAUUSD) Trading Bot on Capital.com

## API reference — single source of truth

Always get Capital.com API details from the local Postman workspace at:

```
/Users/menglay/Documents/development/capital-api-postman/
```

- `capital.collection.json` — the full "Capital.com Public API" Postman collection. Look up every endpoint's path, method, headers, request body, and example responses here before writing or changing any API call. Do not guess endpoints or invent fields from memory.
- `demo.environment.json` — demo environment. Base URL: `https://demo-api-capital.backend-capital.com/api/v1`
- `live.environment.json` — live environment. Base URL: `https://api-capital.backend-capital.com/api/v1`
- `README.md` — usage notes for the collection.

Collection structure: **General** (server time, ping), **Session** (encryption key, create/switch/logout session), **Accounts** (accounts, preferences, activity/transaction history), **Trading** (positions, orders, confirmation), **Markets Info** (markets, prices, client sentiment), **Watchlists**.

## Authentication flow

1. Send `X-CAP-API-KEY` header with `POST /session` (identifier + password, or encryptedPassword via the encryption-key endpoint).
2. The response headers return `CST` (account token) and `X-SECURITY-TOKEN` (session token).
3. Send both `CST` and `X-SECURITY-TOKEN` headers on every subsequent request.
4. Sessions expire after inactivity — the bot must handle re-authentication.

Credentials (API key, identifier, password) live in environment variables / `.env`, never in code or committed files.

## Trading rules

- The instrument is gold: epic `GOLD` on Capital.com (verify with the market search endpoint under Markets Info before hardcoding).
- Default to the **demo** environment. Only target the live base URL when explicitly asked.
- Never place, amend, or close live orders without explicit confirmation from the user.

# Adsun SDK — Telegram Mini App & Telegram Bot ads

[![npm](https://img.shields.io/npm/v/adsun-sdk?label=npm%20adsun-sdk)](https://www.npmjs.com/package/adsun-sdk)
[![PyPI](https://img.shields.io/pypi/v/adsun?label=pypi%20adsun)](https://pypi.org/project/adsun/)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

**[Adsun](https://adsun.io)** is an independent advertising network for Telegram. Publishers earn **USDT** by showing
**rewarded**, **interstitial** and **task** ads in Telegram Mini Apps, **sponsored messages** in Telegram bots and native posts in channels.
Advertisers buy CPM / CPC campaigns that reach Telegram users worldwide.

This repository contains the official, open-source client libraries:

| Package | Install | For |
| --- | --- | --- |
| [`adsun-sdk`](packages/js) (npm) | `npm i adsun-sdk` | Telegram Mini Apps (TWA) — React, Vue, Next.js, Vite, plain JS |
| [`adsun`](packages/python) (PyPI) | `pip install adsun` | Telegram bots — aiogram, python-telegram-bot, any Python backend |
| Script tag | `<script src="https://api.adsun.io/sdk.js"></script>` | Any web page / Mini App, no build step |

## Quick start — Telegram Mini App (JavaScript)

```js
import { initAdsun } from "adsun-sdk";

const ad = await initAdsun({ blockId: "50001" }); // Reward block id from the Adsun panel
ad.show()
  .then(() => giveReward())          // user watched till the end
  .catch((result) => console.log(result)); // skipped / no ad / error
```

React:

```jsx
import { useAdsun, AdsunTask } from "adsun-sdk/react";

function EarnButton() {
  const showAd = useAdsun({ blockId: "50001", onReward: () => addCoins(100) });
  return <button onClick={showAd}>Watch ad · +100 coins</button>;
}

<AdsunTask blockId="task-50003" reward="+100 coins" onReward={() => addCoins(100)} />
```

## Quick start — Telegram bot (Python)

```python
from adsun import AsyncAdsunBot
from adsun.integrations.aiogram import send_ad   # or adsun.integrations.ptb

ads = AsyncAdsunBot(token="YOUR_ACCOUNT_TOKEN", block_id="50001")

@dp.message(Command("bonus"))
async def bonus(message):
    await send_ad(message.bot, message.chat.id, ads, message.from_user.id, message.from_user.language_code or "en")
```

## Ad formats

| Format | Block id | What the user sees |
| --- | --- | --- |
| Reward | `50001` | Fullscreen video / image, not skippable until the timer ends → you grant a reward |
| Interstitial | `int-50001` | Fullscreen ad, closable after a few seconds |
| Task | `task-50001` | Native card (`<adsun-task>`): user opens the link, comes back and claims |
| Bot message | `50001` (Bot block) | Sponsored message with buttons sent by your bot (`GET /advbot`) |

## Grant rewards on your server (important)

Set a **Reward URL** on the block in the Adsun panel, e.g.
`https://your-app.com/adsun/reward?userid=[userId]&block=[blockId]&secret=LONG_RANDOM_SECRET`.
Adsun calls it only after a server-verified full watch / task claim. Verify it with
`adsun.parse_reward_callback(request.args, secret=...)` (Python) and make your handler idempotent (it can be retried).

## Links

- Website: https://adsun.io · Docs: https://adsun.io/docs · Developer docs: https://adsun.io/adsun-developer-documentation
- Publisher sign-up & panel: https://adsun.io/publishers
- Support: support@adsun.io · Telegram [@ADSunSupport](https://t.me/ADSunSupport) · Channel [t.me/adsunio](https://t.me/adsunio)
- For AI assistants: [llms.txt](llms.txt) · https://adsun.io/llms.txt

Adsun is not affiliated with Telegram or Telegram's official Ad Platform.



# Adsun SDK — Telegram Mini App Ads

[Adsun](https://adsun.io) is a Telegram advertising network for Mini Apps, bots and channels.
This SDK lets you monetize a Telegram Mini App with **rewarded video**, **interstitial** and **task** ads.

- ⚡ One script tag, no build step
- 🎁 Rewarded ads with server-side Reward URL (fraud-safe)
- 🖥 Interstitial (full-screen) ads at natural breaks
- ✅ Task ads for Telegram channels, bots and apps
- 📊 Real-time stats in the Adsun publisher panel
- 🌍 Global demand: 15M+ daily impressions, 213 countries
- React, Vue, Unity (WebGL) and vanilla JS examples

## Quick start

```html
<script src="https://api.adsun.io/sdk.js"></script>
<script>
  const ad = Adsun.init({ blockId: "YOUR_BLOCK_ID" });
  ad.addEventListener("onReward", () => refreshRewardState());
  ad.show().catch(() => restoreApp());
</script>
```

1. Sign up as a publisher at https://adsun.io/register?type=partner
2. Add your Mini App and create an ad block
3. Copy the block ID into `Adsun.init`
4. Set a Reward URL on your server to grant rewards

## Telegram bot ads

Show native ad messages inside your bot:

```
GET https://api.adsun.io/advbot?tgid={USER_ID}&blockid={BLOCK_ID}&language={LANG}&token={TOKEN}
```

Returns `text_html`, `click_url`, `button_name`, `image_url` — send with `parse_mode=HTML`.

## FAQ

**What is Adsun?** A Telegram ad network that connects Mini App, bot and channel publishers with advertisers.
**Is Adsun an AdsGram alternative?** Yes — Adsun supports rewarded, interstitial, task and bot ads for Telegram, with an AdsGram-style bot endpoint.
**Which frameworks are supported?** Any web Mini App: vanilla JS, React, Vue, Unity WebGL.

## Links
- Website: https://adsun.io
- Monetization guide: https://adsun.io/telegram-mini-app-monetization
- Advertiser guide: https://adsun.io/how-to-advertise-on-telegram

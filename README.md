# 🎮 Streamer.bot Twitch Chat Overlay

A clean, modern **OBS chat overlay** powered by **Streamer.bot WebSocket**.

Displays Twitch chat messages with:

* 👤 Profile pictures
* 🎨 User chat colors (faded background)
* 😄 Emotes support
* 🚫 Bot & command filtering
* ⚡ Smooth real-time updates

---

## ✨ Features

* 💬 **Full-width chat messages**
* 🎨 **Background uses Twitch user color (faded)**
* 🖼️ **Profile pictures for each user**
* 😄 **Emotes supported (Twitch emotes render properly)**
* 🚫 **Blocks bots & commands (`!` and `/`)**
* 📌 **Messages stay on screen (no timeout)**
* 🔄 **Newest messages appear at the top**
* 🎯 **Works locally with Streamer.bot (no server required)**

---

## 📦 Requirements

* [OBS Studio](https://obsproject.com/)
* [Streamer.bot](https://streamer.bot/)
* WebSocket Server enabled in Streamer.bot

---

## ⚙️ Setup

### 1. Enable WebSocket in Streamer.bot

* Go to **Settings → WebSocket Server**
* Enable it
* Default:

  * Host: `127.0.0.1`
  * Port: `8080`

---

### 2. Add HTML Overlay to OBS

1. Add a **Browser Source**
2. Set:

   * Width: `1920`
   * Height: `1080`
3. Point to your HTML file:

   ```
   file:///C:/path/to/your/chat_overlay.html
   ```

---

### 3. Create Streamer.bot Action (Profile Pictures)

Create a new **Action** and add a **C# Inline Script**

#### 🔹 Required Arguments:

* `message`
* `targetUser`
* `targetUserProfileImageUrl`

---

### 4. C# Script (Avatar Sync)

```csharp
using System;
using System.Text;

public class CPHInline
{
    public bool Execute()
    {
        CPH.TryGetArg("message", out string message);
        CPH.TryGetArg("targetUser", out string targetUser);
        CPH.TryGetArg("targetUserProfileImageUrl", out string avatar);

        string json = "{"
            + "\"event\":\"chatOverlayAvatar\","
            + "\"user\":\"" + Escape(targetUser) + "\","
            + "\"message\":\"" + Escape(message) + "\","
            + "\"avatar\":\"" + Escape(avatar) + "\""
            + "}";

        CPH.WebsocketBroadcastJson(json);
        return true;
    }

    private string Escape(string s)
    {
        return s.Replace("\\", "\\\\").Replace("\"", "\\\"");
    }
}
```

---

## 🎨 Customization

### 🔹 Background Opacity

Edit this line in the HTML:

```js
line.style.background = `linear-gradient(90deg, ${m.color}77 0%, ${m.color}77 100%)`;
```

Change `77` to adjust opacity:

| Value | Opacity |
| ----- | ------- |
| `FF`  | 100%    |
| `AA`  | ~65%    |
| `77`  | ~45% ✅  |
| `55`  | ~30%    |
| `33`  | ~20%    |

---

### 🔹 Message Roundness

```css
.chatLine {
  border-radius: 12px;
}
```

---

### 🔹 Max Messages

```js
const MAX_MESSAGES = 20;
```

---

## 🚫 Filtering

Automatically removes:

* Commands (`!command`, `/command`)
* Bots (Streamlabs, Nightbot, etc.)

You can edit the list inside the HTML:

```js
const ignoredBots = new Set([...]);
```

---

## 🧠 How It Works

* Streamer.bot sends chat via WebSocket
* Overlay listens using:

  ```
  @streamerbot/client
  ```
* Messages are rendered instantly
* Avatar images are synced via custom event

---

## 📸 Preview

* Full-width colored chat messages
* Profile pictures on the left
* Clean modern overlay look

---

## 🔥 Future Ideas

* 🏷️ Twitch badges (mod/VIP/sub)
* ✨ Message animations (slide in / fade)
* 🌈 Glow effects per user color
* 📜 Scroll mode instead of stacking

---

## 💬 Credits

Built for Streamer.bot + OBS overlays
Custom design optimized for live streaming

---

## ⭐ Support

If you like this overlay:

* ⭐ Star the repo
* 💬 Share with other streamers
* 🔧 Customize it for your setup

---

Enjoy your stream! 🎉

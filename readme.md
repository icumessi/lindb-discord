# 💾 LinDB Discord — Free Data Storage in Discord Channels

> Store structured, encrypted data directly inside your Discord channel — for **free**.

[![Node.js](https://img.shields.io/badge/node-%3E%3D16.0.0-green)](https://nodejs.org)
[![Discord.js](https://img.shields.io/badge/discord.js-v14-blue)](https://discord.js.org)
[![License](https://img.shields.io/badge/license-MIT-yellow)](LICENSE)

---

### 🌐 Learn more  
👉 [https://messi.icu](https://messi.icu)

---

## 📖 About

**LinDB Discord** lets you store data *inside* a Discord channel using messages as data nodes.  
When a message reaches the data size limit (≈25MB), a new node is automatically created — giving you a distributed yet unified datastore inside your server.

You can **get**, **set**, **delete**, and **push** data just like a regular database.  
Optionally, you can enable **AES-CBC encryption** for secure, symmetric encryption that’s also super fast.

---

## ⚙️ Features

- 🧠 Multiple-node storage (splits data across messages)
- 🔒 AES-CBC encryption support
- ⚡ Fast and efficient
- 📦 Built-in methods:
  - `get` – Retrieve data
  - `set` – Store data
  - `delete` / `del` – Remove data
  - `push` – Append to arrays
- 🪶 Auto-save support
- 🪄 Fully runs inside Discord — no external DB needed

---

## 🚀 Installation

$$${{
bash
npm install lindb-discord
}}$

---

## 🧰 Example Usage

### Basic Setup

$$${{
js
const lindb = require("lindb-discord");

(async () => {
  const db = await lindb.initialize({
    client: "<your_bot_token>", // Can also be an existing Discord.Client
    guildid: "<guild_id>",
    channelid: "<channel_id>",
    debug: true, // Optional
  });

  // Store data
  await db.set("users.messi", { name: "King Messi", goals: 802 });

  // Retrieve data
  console.log(await db.get("users.messi")); 
  // -> { name: "King Messi", goals: 802 }

  // Delete data
  await db.delete("users.messi");

  // Push to array
  await db.set("scores", []);
  await db.push("scores", 10);
  await db.push("scores", 15);
  console.log(await db.get("scores")); 
  // -> [10, 15]
})();
}}$

---

### With Encryption Enabled

$$${{
js
const lindb = require("lindb-discord");

// Generate encryption keys
const encryption = JSON.parse(lindb.generateEncryptionConfig("mysecretpass"));

(async () => {
  const db = await lindb.initialize({
    client: "<your_bot_token>",
    guildid: "<guild_id>",
    channelid: "<channel_id>",
    debug: true,
    encryption, // Add the generated config here
  });

  await db.set("private.tokens", ["abc123", "xyz789"]);
  console.log(await db.get("private.tokens")); 
  // -> ["abc123", "xyz789"]
})();
}}$

---

## 🔐 Encryption Details

LinDB Discord uses **AES-CBC (Advanced Encryption Standard - Cipher Block Chaining)**.  
This means:
- Symmetric key encryption (same key for encrypt/decrypt)
- Strong and efficient for small to medium data payloads
- Can be generated dynamically with:

$$${{
js
const lindb = require("lindb-discord");
console.log(lindb.generateEncryptionConfig("yourpassphrase"));
}}$

Example output:
$$${{
json
{
  "key": "6b4e6b4b2b673f7e6e5d486b2b457b73",
  "iv": "2f5b482f4c345f435b2b484b5b456d3e"
}
}}$

---

## 🧩 API Reference

### `initialize(options)`

Creates or connects to a Discord-based database.

**Options:**

| Option | Type | Required | Description |
|--------|------|-----------|-------------|
| `client` | `discord.Client` or `string` | ✅ | Discord client or token |
| `guildid` | `string` | ✅ | Guild ID |
| `channelid` | `string` | ✅ | Channel ID where data is stored |
| `debug` | `boolean` | ❌ | Logs debugging info |
| `encryption` | `{ key, iv }` | ❌ | Enable AES-CBC encryption |
| `autosave` | `boolean` | ❌ | Enable auto-saving |
| `saveinterval` | `number` | ❌ | Interval in seconds for auto-save |

**Returns:**  
A data handler with `.get()`, `.set()`, `.delete()`, and `.push()` methods.

---

### `generateEncryptionConfig(passphrase?)`

Generates an AES key and IV for encryption.

**Parameters:**
- `passphrase` *(optional)* — A string seed for key generation.

**Returns:**  
A JSON-formatted encryption configuration.

---

## 💡 Why I Made This

I wanted an easy, reliable, and encrypted way to store data inside Discord —  
no databases, no servers, just pure Discord.

Couldn’t find anything that fit my needs,  
so I made **LinDB Discord**.

---

## 📜 License

MIT © [King Messi](https://messi.icu)

---

### 🧠 Fun Fact
Each message node can store up to **25MB** of data.  
That’s like storing a **small JSON database** inside your Discord server.

So yeah... your Discord channel is now your database 😎

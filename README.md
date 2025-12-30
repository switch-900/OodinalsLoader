# Oodinals Loader - Simple On-Chain Inscriptions Tool

**Create on-chain experiences with inscription capabilities — no backend required.**

This is a dead-simple, client-side tool that lets you build Bapps that can **create Bitcoin inscriptions directly from the browser**. Perfect for ordinal minting, on-chain content generators, creative tools, or any experience where users need to inscribe data to Bitcoin.

**What is an "Oodinal"?** An Oodinal (Ordinal + Odinal) is an inscription that has **on-chain transaction creation capabilities**. The Oodinals Loader itself is inscribed on Bitcoin (sat 404079598183801) and can build and broadcast inscription transactions directly from within an inscription viewer. This means your inscribed apps can create new inscriptions without any backend server.

## Why Use This?

- ✅ **No backend needed** — Everything runs client-side using on-chain Bitcoin Ordinals infrastructure
- ✅ **Multi-wallet support** — Works with UniSat, Xverse, OKX, Leather, Phantom, and more
- ✅ **Single-file apps** — Copy one HTML file and you're done. No build tools, no npm packages
- ✅ **On-chain inscriber** — The loader itself is inscribed on Bitcoin, making your app truly decentralized
- ✅ **Production-ready** — Powers real inscription services with thousands of inscriptions created

## Getting Started (2 minutes)

**Option 1: Use the ready-made inscriber**

1. Inscribe [example.html](example.html) on Bitcoin (or test it in the [Inscription Tester](https://ordinals.com/content/4cc26939b8c375b75d283c44c1c1a02f9b28a33417d233a9b8fccbc4482aa102i0))
2. Open your inscription in any viewer (ordinals.com, ord.io, etc.)
3. Connect your wallet → Create inscriptions

**Option 2: Build custom JSON first**

1. Inscribe [jsoncreator.html](jsoncreator.html) or test in the Inscription Tester
2. Open it through an inscription viewer
3. Configure your inscriptions → Generate JSON → Copy
4. Paste into [example.html](example.html) or your own inscribed app

**Both files are self-contained** — inscribe once, use forever. No backend, no hosting needed.

> ⚠️ **Important:** Some wallets (Xverse, Magic Eden) only work when your app is **inscribed on Bitcoin**. They won't connect throught the inscriptions tester for security reasons.


## What Can You Build?

This tool enables **on-chain experiences** where users can create inscriptions directly:

- 🎨 **NFT Minting Sites** — Let users mint generative art, collectibles, or custom NFTs
- 📝 **Content Publishing** — On-chain blogs, documents, or data storage
- 🖼️ **Galleries & Collections** — Create parent-child inscription hierarchies
- 🎮 **Interactive Experiences** — Games, tools, or apps that inscribe user-generated content
- 🔗 **Recursive Ordinals** — Build experiences that reference other inscriptions

All without a server. Your app + the Bitcoin blockchain = complete inscription platform.

## How Nexus Works

### Dynamic Loading from an Inscription

Nexus itself is loaded from an inscription:

```js
const { default: Nexus } = await import('/r/sat/404079598183801/at/-1/content');
```

- This makes it part of the ordinals ecosystem, not an external dependency
- It's imported dynamically and exposed as `window.Nexus`
- Always available at the same sat location, versioned with `/at/-1/content`

### Wallet Connection Flow

```js
await Nexus.connectWallet(walletId)  // Connect to specific wallet
const state = Nexus.getWalletState() // Get connection info
```

- Nexus acts as a universal connector for multiple wallet providers (UniSat, Xverse, OKX, Leather, Phantom, Magic Eden, etc.)
- Returns unified state: `paymentAddress`, `ordinalsAddress`, `publicKey`, `connected` status
- Handles wallet-specific quirks and provides a consistent API

### Finding Inscriptions in UTXOs

```js
const utxos = await Nexus.fetchSpendableUtxos(address)
// Returns: { unsafe: [], spendable: [], unconfirmed: [] }
```

Users can filter UTXOs to control which outputs are used for fees:

- **`spendable`** — UTXOs that appear safe to spend (filters out ordinals and runes)
- **`unsafe`** — UTXOs that contain known inscriptions or other protected assets
- **`unconfirmed`** — UTXOs from unconfirmed transactions

**Important:** The `spendable` filter uses ordinals standard detection, but things outside the standard may not be identified. Many wallets have built-in features to protect non-standard assets. You can let users manually select UTXOs to exclude from the spendable fee pool as an additional safety option.

## What Each File Does

### [example.html](example.html) — The Minimum Viable Inscriber
A complete, working inscription app:
- Loads the Oodinals Loader from on-chain
- Connects to Bitcoin wallets (UniSat, Xverse, OKX, etc.)
- Estimates inscription fees
- Creates inscriptions from JSON config
- **Perfect starting point for your own app**

### [jsoncreator.html](jsoncreator.html) — Visual JSON Builder
A UI tool that helps you build inscription configurations:
- Visual form for defining inscription items
- Set content type, metadata, properties, fees
- Preview and validate JSON before inscribing
- Can also connect wallet and create directly
- **Use this to generate configs for your apps**

## How to Use

### Production (Inscribed On-Chain)

**This is the real power:** Inscribe your HTML file on Bitcoin, then access it through an inscription viewer.

1. Take [example.html](example.html) or [jsoncreator.html](jsoncreator.html)
2. Inscribe it on Bitcoin (using any inscription service)
3. Access it via inscription viewers:
   - `ordinals.com/inscription/<your-inscription-id>`
   - `ord.io/<your-inscription-id>`
   - Or the [Inscription Tester](https://ordinals.com/content/4cc26939b8c375b75d283c44c1c1a02f9b28a33417d233a9b8fccbc4482aa102i0)

**Important:** Some wallets (Xverse, Magic Eden) **only work when your app is inscribed**. They won't connect to locally served files. This is a security feature.

### Local Development wont work as it depends of ordinals enviroment insted use the [Inscription Tester]

## Recommended Workflow

**For creating a custom inscription experience:**

1. Study the code in [example.html](example.html) (~200 lines)
2. Copy and customize it with your own UI and logic
3. Test locally with `python -m http.server 8000` (limited wallet support)
4. **Inscribe your HTML file on Bitcoin** for production use
5. Access via inscription viewers — now all wallets work!

**For one-off inscriptions or testing configs:**

1. Use [jsoncreator.html](jsoncreator.html) (inscribed or in Inscription Tester)
2. Fill out the form with what you want to inscribe
3. Click **Generate JSON** → **Copy to Clipboard**
4. Paste into your inscribed [example.html](example.html)
5. Connect Wallet → **Create Inscription**

## what each file is for

- [example.html](example.html)
  - The “minimum viable on-chain inscriber”: Loader import → wallet connect → estimate → create.
  - Includes a textarea where you paste inscription JSON.

- [jsoncreator.html](jsoncreator.html)
  - A UI that helps you *build* inscription JSON (items, defaults, metadata/properties, fees).
  - Can also connect/estimate/create directly, but it’s especially useful as a JSON generator.

## Open it (serve over HTTP)

The JSON Creator uses an on-chain Loader import (`/r/sat/.../at/-1/content`), so it must be served over HTTP(S).

- On-chain / gateway usage: host/inscribe `jsoncreator.html`, then open it via your gateway.
- Local usage: serve the repo with any static server and open `/jsoncreator.html`.
  - If you already use the repo scripts, you can also run one of the `npm run preview:*` scripts and then open `/jsoncreator.html` in that server.

## Recommended workflow (fastest)

1. Open [jsoncreator.html](jsoncreator.html).
2. Configure what you want to inscribe (items, content type, metadata/properties, fees).
3. Click **Generate JSON** and **Copy to Clipboard**.
4. Open [sdk/example-simple.html](sdk/example-simple.html).
5. Paste the JSON into the textarea.
6. Connect wallet → Estimate → Create.

5. Paste JSON → Connect Wallet → **Create Inscription**

## Using the JSON Creator End-to-End

1. Wait for **“Oodinals Loader ready”**.
2. Click **Connect Wallet** and pick your wallet.
3. Click **Generate JSON** to preview the config.
4. (Recommended) Click **Validate (Loader)**.
5. Click **Estimate Fees (Loader)**.
6. Click **Create Inscription (Loader)** and approve in your wallet.

The **Oodinals Loader Output** panel prints the result object (commit/reveal txids, ids, etc. depending on wallet/provider).

## JSON format (what you’re defining)

At a high level the JSON you generate looks like:

- `version`: config version
- `feeRate`: sat/vB (number)
- `fees.developer`: optional dev fee output (can be disabled)
- `defaults`: values applied to items unless overridden
- `items[]`: one or more inscriptions

Each `items[]` entry can include:
- `fileName`
- `content` (string) OR `contentBase64` (string)
- `contentType`, `postage`, `recipientAddress` (optional overrides)
- `metadata` and `properties` (objects)
- advanced fields like `parentIds`, `delegateId`, `pointer`, etc.

### Important: `parentIds` must be real

If you set `parentIds`, the Loader must fetch the parent inscription’s *exact UTXO value* to compute correct pointers.

- Use real IDs of the form: `<txid>i<index>` (64 hex + `i` + number)
- Placeholders like `abc123...i0` will fail.

The JSON Creator pre-validates these fields before calling create.

## Gallery (properties / tag 17)

`properties` are encoded as **Ordinals Properties (CBOR tag 17)**.

To define a gallery, you can use the shortcut form:

```json
{
  "properties": {
    "gallery": [
      "<txid>i0",
      "<txid>i1"
    ]
  }
}
```

## Build your own on-chain inscriber (the simple pattern)

The JSON Creator is just a UI that produces JSON and calls the Loader.

Your own on-chain inscriber can be as small as:

1) import the Loader (proxy-safe):

```js
const { default: Oodinals } = await import('/r/sat/404079598183801/at/-1/content');
```

2) connect a wallet:

```js
await Oodinals.connectWallet('unisat');
```

3) create inscriptions from a JSON string:

```js
const jsonString = JSON.stringify(myConfigObject);
Oodinals.validateJson(jsonString);
const estimate = await Oodinals.estimateFees(jsonString);
const result = await Oodinals.createInscription(jsonString);
```

That’s it: your UI can be anything (a form, a button, a generator, etc.) as long as it outputs the same JSON.

If you want the smallest starting point, copy [sdk/example-simple.html](sdk/example-simple.html) and replace the JSON textarea + UI with your own.

## Test inscription content (Inscription Tester)

Use this on-chain tester to preview the *content* you plan to inscribe (especially HTML/JS):

- https://ordinals.com/content/4cc26939b8c375b75d283c44c1c1a02f9b28a33417d233a9b8fccbc4482aa102i0

How it works:
- paste your content into the page’s textarea
- press the **test** button
- the tester replaces the document with your textarea content, so you can verify it renders/executes as expected

This is useful before you commit to inscribing content on-chain.

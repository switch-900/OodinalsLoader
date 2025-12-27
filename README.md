# Oodinals JSON Creator

This repo includes a single-file, no-build JSON Creator app: [jsoncreator.html](jsoncreator.html).

It helps you:
- build valid Oodinals inscription JSON
- connect a wallet via the Oodinals Loader
- validate JSON, estimate fees, and create inscriptions

## Open it

The JSON Creator uses an on-chain Loader import (`/r/sat/.../at/-1/content`), so it must be served over HTTP(S).

- On-chain / gateway usage: host/inscribe `jsoncreator.html`, then open it via your gateway.
- Local usage: serve the repo with any static server and open `/jsoncreator.html`.
  - If you already use the repo scripts, you can also run one of the `npm run preview:*` scripts and then open `/jsoncreator.html` in that server.

## How to use

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

## Test inscription content (Inscription Tester)

Use this on-chain tester to preview the *content* you plan to inscribe (especially HTML/JS):

- https://ordinals.com/content/4cc26939b8c375b75d283c44c1c1a02f9b28a33417d233a9b8fccbc4482aa102i0

How it works:
- paste your content into the page’s textarea
- press the **test** button
- the tester replaces the document with your textarea content, so you can verify it renders/executes as expected

This is useful before you commit to inscribing content on-chain.

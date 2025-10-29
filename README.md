# PeaceDAO-frontend

Frontend for PeaceDAO (token-verified chat & governance)

## Quick Start (Codespaces/Local)

```bash
npm i
cp .env.example .env.local
# fill your values (WalletConnect Project ID, PeaceFund, Router, TGs, Guild)
npm run dev
```

## Treasury

- PeaceFund (BSC Testnet): `{{PEACEFUND_BSCTEST}}`
- `donate(string note)` accepts BNB contributions, emits a `Donated` event with the sender, amount, and note, and holds funds without any withdrawal path until a future governance executor is deployed.

## Required owner-provided values

Set these environment variables in `.env.local` (or your hosting provider):

* `NEXT_PUBLIC_WC_PROJECT_ID` (WalletConnect)
* `NEXT_PUBLIC_PEACE_FUND` (your PeaceFund address)
* `NEXT_PUBLIC_PEACE_SWAP_ROUTER` (your Router address)
* `NEXT_PUBLIC_GUILD_LINK` (Guild join link)
* Telegram links (`NEXT_PUBLIC_TG_PUBLIC`, `NEXT_PUBLIC_TG_VERIFIED`) — may change later

## Deploy to Vercel

1. Import this repository into Vercel.
2. In **Project Settings → Environment Variables**, add every key from `.env.example`:
   - `NEXT_PUBLIC_WC_PROJECT_ID=afea209f54093e07eb4f30a8307f816f
   - `NEXT_PUBLIC_RPC_BSC=https://bsc-dataseed.binance.org
   - `NEXT_PUBLIC_PEACE_FUND=0x071B1baf97D85a70A6Ca786E7Fe90b45f50464e5
   - `NEXT_PUBLIC_TOKEN=0x4444def5cf226bf50aa4b45e5748b676945bc509
   - `NEXT_PUBLIC_GUILD_LINK=https://guild.xyz/world-peace-dao
   - `NEXT_PUBLIC_TG_PUBLIC=https://t.me/WorldPeace_BNB
   - `NEXT_PUBLIC_TG_VERIFIED=https://t.me/+i-dpunM-luk1ZjRl
   - `NEXT_PUBLIC_TWITTER=https://x.com/WorldPeace_BNB
3. Redeploy so the new environment variables are available to the build and runtime.
4. After the deployment finishes:
   - Open `/api/health` on your deployed domain and confirm the JSON response includes `{"ok":true}`.
   - Connect a wallet via WalletConnect (Binance Web3) to verify the connection flow.
   - Ensure `NEXT_PUBLIC_PEACE_FUND` is set — the Donate page will enable once it is present and the Treasury view will display the contract balance.

## Known integration notes

- The donate page auto-detects the PeaceFund payable method. It attempts the value provided in `NEXT_PUBLIC_DONATE_METHOD` and falls back through `donate`, `donateBNB`, `donateTo`, and `executeDonation`.
- Swapping requires a functioning PeaceSwap router address. If the configured address is a placeholder the swap form will surface a warning and transactions will fail fast.
- The UI uses Tailwind CSS's default system font stack so builds succeed in fully offline environments (no Google font downloads required).

## Connect Wallet Support

- We use wagmi with WalletConnect to support Binance Web3 Wallet and MetaMask via WalletConnect QR.
- The direct `@metamask/sdk` dependency was removed because its React Native modules are incompatible with Vercel/browser builds.
- Wallet connections work across desktop and mobile platforms.

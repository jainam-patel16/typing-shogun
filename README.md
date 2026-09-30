# Typing Samurai: deploy and install

Solo works fully offline once installed. Multiplayer (up to 4 devices) needs internet.

## 1. Test locally (optional)

```bash
cd typing-samurai
npx serve .
```
Open the printed `http://localhost:3000`. Service workers run on localhost, so offline and install work here too.

## 2. Deploy (pick one)

### Netlify: fastest, about 1 minute
1. Go to https://app.netlify.com/drop
2. Drag the whole `typing-samurai` folder onto the page.
3. You get a live `https://something.netlify.app` link. Rename it in Site settings if you like.
4. To update later: open your site in Netlify, go to Deploys, and drag the folder in again.

### GitHub Pages: best for your portfolio
1. Create a new public repo, for example `typing-samurai`.
2. Upload every file in this folder to the repo root, keeping `index.html` at the top level.
3. In the repo, go to Settings, then Pages. Under Source pick `Deploy from a branch`, choose `main` and `/ (root)`, and save.
4. After a minute your game is at `https://<your-username>.github.io/typing-samurai/`

### Vercel
```bash
npm i -g vercel
cd typing-samurai
vercel --prod
```

## 3. Install on devices

| Device | How |
|---|---|
| Android (Chrome) | Open the link and tap the ⤓ button on the home screen, or Menu, then Install app |
| iPhone / iPad (Safari) | Tap Share, then Add to Home Screen |
| Windows / Mac / Chromebook (Chrome or Edge) | Click the install icon in the address bar, or the ⤓ button in the game |

It launches fullscreen with its own icon, like a native app.

## 4. Play multiplayer

1. One player picks Multiplayer, then Create a room, and shares the 4-character code.
2. Up to 3 friends pick Multiplayer, enter the code, and tap Join.
3. The host taps Start duel. Everyone gets the same waves, and curses fly between rivals.

The host's device acts as the hub, so if the host closes the game the room ends.

## 5. Shipping updates

1. Edit `index.html`.
2. In `sw.js`, bump `CACHE` (for example `typing-samurai-v2`).
3. Redeploy. Installed copies update the next time they open online.

## How multiplayer works, and its limits

It uses PeerJS: devices connect directly (WebRTC), and the free PeerJS cloud server just introduces them. That means no accounts, no database, and no server bill.

Known limits:
- The free PeerJS server is best effort. For guaranteed uptime, host your own PeerServer (`npx peer --port 9000`) on Render or Railway, then pass `{ host, port, path, secure:true }` to `new Peer(...)` in `index.html`.
- Some strict networks (certain office Wi-Fi or mobile carriers) block direct connections. Adding a TURN server (for example a free tier from metered.ca) to the Peer `config.iceServers` fixes this.

## Going further: app stores

Wrap this same folder with Capacitor (https://capacitorjs.com) to build real Android and iOS apps:
```bash
npm i @capacitor/core @capacitor/cli @capacitor/android
npx cap init "Typing Samurai" com.yourname.typingsamurai --web-dir .
npx cap add android && npx cap open android
```
Google Play has a one-time $25 fee; the Apple App Store is $99/year.

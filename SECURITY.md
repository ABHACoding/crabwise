# Security

Crabwise runs a small web server on your own machine and reads live data from a game. This
page says exactly what that means, because you are being asked to run an unsigned installer
and "trust me" is not good enough.

## What it reads

One thing: `https://127.0.0.1:2999`, the Live Client Data API that Riot's own game client
publishes on your machine while you are in a match. It is an ordinary local web address —
your browser can open it.

**It never reads game memory, injects code, or hooks the game process.** There is nothing to
detect and nothing to flag, because it does not touch the game at all. That is not a promise
about intent; it is a description of what the code is able to do.

## The local server

The overlay is a web page served from your machine to your own OBS.

- It binds **loopback only** (`127.0.0.1`). Nothing on your network can reach it. There is a
  test that fails the build if that ever stops being true, and it runs against the packaged
  installer rather than the source.
- Requests carrying a non-loopback `Host` header are refused, which blocks DNS rebinding.
- The overlay URL contains a **key unique to your install**, so another page open in your
  browser cannot subscribe to your match data. Treat that URL like a password and keep it off
  stream. If it leaks, delete `%APPDATA%\Crabwise\settings.json` and restart — a new key is
  generated, and you paste the new URL into OBS.

## What leaves your computer

At most one request per day: a download of a small JSON file of objective respawn durations,
so a patch that moves Baron can be fixed without shipping a new version. It sends no
identifiers, no account name and no match data. The same file carries the latest version
number, which is where update notices come from — nothing downloads or installs itself.

Emptying **Support → Timing table URL** switches that off, after which the app never contacts
anything.

There is no analytics, no crash reporting, no account and no server. Your match data goes
from your machine to your own OBS and stops.

Logs and the **Copy diagnostics** blob deliberately exclude Riot IDs, match data and your
overlay key, because those get pasted in public when someone asks for help.

## Checking it yourself

Everything above is a claim. This is how you watch it be true, with a tool that is already on
your computer — no trust in this page required.

1. Press <kbd>Win</kbd>+<kbd>R</kbd>, type `resmon`, press Enter. Open the **Network** tab.
2. Start Crabwise, and expand **Listening Ports**.

You should see `Crabwise.exe` listening on **`127.0.0.1`** — the address column, not the
port. The port itself is whatever the Setup screen shows; the address is the part that
matters. `0.0.0.0` or `*` would mean your whole network could reach it. It never says that,
and there is a test in the build that fails if it ever does.

3. Start a game and expand **TCP Connections**.

`Crabwise.exe` connects to **`127.0.0.1:2999`**. That is the game's own API, on your own
machine — the one thing this app reads. Both ends of that connection are your PC.

4. Leave it running and watch the remote addresses.

The only non-local address you will ever see from `Crabwise.exe` is
`gist.githubusercontent.com`, at most once a day, fetching the objective timing table. If you
empty **Support → Timing table URL**, even that stops and the list stays empty for good.

You will also see OBS connecting *to* the Crabwise port. That is your browser source reading
the overlay, and it is the whole point.

## Verifying a download

Every release publishes `SHA256SUMS.txt`. Checking it is the only way to know the file you
downloaded is the file that was built:

```powershell
Get-FileHash .\Crabwise-Setup-*.exe -Algorithm SHA256
```

Compare against the published checksum, ignoring case — PowerShell prints uppercase, the
published file is lowercase.

## Reporting something

Please report privately rather than opening a public issue, so it can be fixed before it is
described.

Use **Report a vulnerability** on this repository's Security tab. That opens a private
advisory visible only to you and the maintainer.

There is no bounty. This is a free tool maintained by one person, so an answer may take a few
days — but it will get one.

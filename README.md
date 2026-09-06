# Crabwise

A live stream overlay for League of Legends. CS head-to-head against your lane opponent,
objective respawn timers, a kill feed, a post-game summary card, ten rotating award cards,
and a win/loss record you can mark from the tray between queues.

**[Download the latest release →](https://github.com/ABHACoding/crabwise/releases/latest)**
— Windows 10 or 11.

---

## What it reads, and what it does not

Crabwise reads one thing: `https://127.0.0.1:2999`, the Live Client Data API that Riot's own
game client publishes on your machine while you are in a match. It is an ordinary local web
address. Your browser could open it.

**It never reads game memory, injects code, or hooks the game process.** There is nothing to
detect and nothing to flag, because it does not touch the game at all — it reads a webpage
the game is already serving.

That is not a promise about intent, it is a description of the only thing the code can do.

### What leaves your computer

One thing, at most once a day: a download of a small JSON file holding objective respawn
durations, so a patch that moves Baron can be fixed without waiting for a release. It sends
no identifiers, no account name, no match data — it is a GET of a static file, the way
loading any web page is.

That same file carries the current version number, which is where update notices come from.
Nothing downloads or installs itself; you get a line of text and a link.

Emptying **Support → Timing table URL** switches both off, and the app never contacts
anything again.

There is no analytics, no crash reporting, no account, and no server. Your match data goes
from your machine to your own OBS and stops there.

---

## Windows will warn you. Here is why, and what it says.

Crabwise is **not code-signed**. A signing certificate costs a few hundred dollars a year,
and this is a free tool, so it does not have one.

When you run the installer, Windows shows a blue box: **"Windows protected your PC"**, with
one button that says *Don't run*.

To install anyway:

1. Click **More info** — a small link under the message text.
2. Click **Run anyway**, which appears once you do.

That warning does not mean Windows found anything wrong. It means the file has no
certificate saying who published it, which is true and which the warning is right to say.

**Be suspicious of that prompt in general.** It exists for good reasons, and if you would
rather not click past it, that is a completely reasonable place to stop. What you should not
do is take a stranger's word for it — and that includes this page.

So don't. Check it instead.

Crabwise makes one claim above all others: that it only listens on your own machine, and
that the only thing it ever sends out is one small file request a day. You can watch that be
true, while it runs, using Resource Monitor — which is already on your computer. It takes
about a minute, there is nothing to install, and there are no hex strings to compare by eye.

**[The steps are in SECURITY.md](SECURITY.md).**

That is the check worth doing, because it tests the thing that actually matters: what this
program does once it is running.

### Optional: confirming the download arrived intact

Every release also publishes `SHA256SUMS.txt`. If you want to confirm the file you downloaded
is byte-for-byte the file that was built, run this in PowerShell:

```powershell
Get-FileHash .\Crabwise-Setup-*.exe -Algorithm SHA256
```

PowerShell prints the hash in **uppercase** and `SHA256SUMS.txt` is lowercase, so compare
them ignoring case. They match or they do not; the letter casing means nothing.

Worth knowing what that does and does not tell you. It catches a download that was corrupted
or interrupted. It **cannot** tell you whether the person who published it is honest, because
the installer and the checksum come from the same place — anyone able to replace one could
replace the other. Nothing published alongside a file can prove that, and any page claiming
otherwise is overselling. That is what the check above is for.

---

## Installing

The installer is **per-user**. It does not ask for administrator rights and produces no UAC
prompt. It installs to your own AppData folder, adds a Start Menu entry, and appears in
Add/Remove Programs like any other application.

Uninstalling removes the app but **keeps your settings**, so reinstalling later does not
change your overlay URL. To remove everything, delete `%APPDATA%\Crabwise` as well.

## Setting it up in OBS

1. Start Crabwise. It lives in your system tray.
2. Copy the overlay URL from the **Setup** screen.
3. In OBS, add a **Browser** source and paste it. Set the width and height to your canvas.
4. Click **Show test pattern in OBS**. Four corner marks appear on the overlay for two
   minutes. If they line up with the edges of your source, you are done — the pattern
   disappears on its own.

⚠️ **The overlay URL contains a key unique to your install.** It is what stops other pages
on your computer from reading your match data. Treat it like a password, and do not show it
on stream.

### Tip: turn on "Start with Windows"

On the Setup screen. If Crabwise is not running when OBS opens, the browser source is simply
blank, and nothing on screen explains why. It is off until you turn it on.

---

## Legal

Crabwise's own code is MIT licensed — see the `LICENSE` file published alongside the
installer. It comes with no warranty of any kind, which is worth reading before you run an
unsigned binary from the internet.

Champion portraits and the app icon are Riot Games artwork, included under Riot's
fan-content policy. They are not covered by that licence and are not mine to relicense.
Crabwise is free and always will be.

Crabwise isn't endorsed by Riot Games and doesn't reflect the views or opinions of Riot
Games or anyone officially involved in producing or managing Riot Games properties. Riot
Games, and all associated properties are trademarks or registered trademarks of Riot Games,
Inc.

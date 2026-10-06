# SystemStatusRemote

Watch the CPU, GPU, memory, network, disks, battery and thermal state of several Macs at once, from one
dashboard, with optional recorded history.

Two apps:

- **SystemStatusRemote** is the *host*. It shows the dashboard, lists the Macs being watched, and stores history.
- **SystemStatusClient** is the *client*. It runs on each Mac you want to watch, as a menu-bar item,
  and reports to the host. It can also show that Mac's own stats in a window (see "The client's own view").

> **Status: 1.0.0.** macOS only for now. This repository holds downloads only; the source code is not published.

## Requirements

- macOS 15 (Sequoia) or later, on every Mac involved.
- The Macs must reach each other over the network (the host listens on port **47820**).

## Install

Each [release](https://github.com/S-Walabee/SystemStatusRemote-Releases/releases/latest) has each app as a disk image (`.dmg`) and as a zip. Either works; download the one for each Mac:

| Mac | Download |
|---|---|
| The Mac that shows the dashboard | `SystemStatusRemote-<version>.dmg` (or `.zip`) |
| Each Mac you want to watch | `SystemStatusClient-<version>.dmg` (or `.zip`) |

Open the `.dmg` and drag the app onto the Applications shortcut beside it (or unzip the `.zip` and drag the app to
`/Applications`). If an older copy is running, quit it first. Then open it.

Releases are **signed with a Developer ID and notarized by Apple**, so they open without any security warning.


What to expect on first launch:

- A **Keychain** prompt: each app keeps its own identity key there. Choose **Always Allow**. Notarized releases
  keep that permission across updates; it's asked again only if the apps' signing changes (as it did when releases
  moved from ad-hoc signing to a Developer ID).
- The host asks to **accept incoming connections**: choose **Allow**.
- A client may ask to **find devices on your local network**: choose **Allow**. If you missed it, turn it on under
  System Settings, Privacy & Security, Local Network.

## Uninstall, or start fresh

**SystemStatusUninstaller** (its own `.dmg` or `.zip` on each release) removes either app or both, with
everything they saved: settings, pairings, history and their keychain identities. Use it before installing a
new version for a completely fresh start (it's never required), or to remove the apps for good.

- It lists what it found for each app, with sizes. Untick an app to leave it alone.
- **Remove the apps themselves** puts them in the Trash. Turn it off to keep the apps but return them to a
  first-launch state: unpaired, default settings, no history.
- Running copies are quit first. macOS may ask for your password to delete the keychain items, since they
  belong to the other apps.
- Paired Macs need pairing again afterwards: remove the Mac on the host too, or the host refuses its new request
  (it still trusts the old identity).
- Login Items: if an app was set to open at login, macOS may keep listing it under System Settings, General,
  Login Items. The uninstaller has a button to open that page.

## Set up

1. **Host.** Open **SystemStatusRemote**. The bottom of its sidebar should say "Listening on port 47,820".
   **Add Client…** shows the name clients see it under, and its address.
2. **Client.** Open **SystemStatusClient** on the other Mac and click its menu-bar item. Hosts on the same
   network are listed under **Host** by their Mac's name (with only one around, it's already chosen). For a host
   on another network, choose **Enter an Address** and type its address (for example `My-Mac.local`)
   and port 47820. Click **Request Pairing**. The client shows a **check phrase** of three words, such as
   "maple · river · copper", and waits.
3. **Back at the host,** whenever suits you: the request is under **Pairing Requests** at the top of the sidebar
   (the menu-bar item shows a count). Click **Accept…**, check that the host shows the same three words as the
   client, and accept. The client connects and the Mac appears in the sidebar with a green dot.

If the words differ, deny the request: something else on the network may be posing as one of the Macs. A
request waits as long as the client keeps asking (up to a day). A denied client is turned away for 15 minutes, unless you click **Clear Denied Requests** (under Pairing Requests in the sidebar, in Add Client, or in Settings under Clients).
Pairing requests can be turned off in the host's Settings. If several requests are started and dropped before
they finish, the host stops taking new ones for up to an hour and says so in the sidebar: that pattern can
mean something on the network is trying to guess its way past the check phrase. Pairing with a code still works.

**With a code instead:** in **Add Client…**, open **Pair with a code instead**, then in the client turn on
**Use a pairing code**, enter the code and click **Pair**. The code works once, while that sheet is open.

Finding a host on the network doesn't make it trusted: that's what accepting the request and comparing the words
are for. Once paired, a client finds its host by certificate among those it sees, so it keeps connecting if the
host's address or name changes.

Pairing pins each side's certificate, so afterwards only the paired Macs can connect. Both apps keep showing
the check phrase (the host in a Mac's right-click menu). Remove a client on the host (right-click it in the
sidebar) and it can't connect again until paired anew.

## Using the dashboard

- **Overview** shows several Macs side by side. Click a Mac for a full-size view.
- **Order**: drag Macs up and down the sidebar (or use Move Up and Move Down in a Mac's right-click menu). The
  Overview and comparisons follow the same order, and newly paired Macs join at the end.
- **Compare Macs**: Command-click or Shift-click two or more Macs in the sidebar to see just those together. The
  toolbar switches the layout: **By Mac** gives each Mac a panel, and **By Module** groups each metric (all the
  CPUs, then all the memory) so the Macs can be compared directly. The same choice works on the Overview.
  In Vertical, Macs sit side by side in one row of equal columns that share the window's width. Once the
  columns are at their narrowest the row doesn't wrap: it scrolls sideways (trackpad or mouse, or the left and
  right arrow keys after clicking it), newly added Macs at the right end. The window fits itself as Macs come and go: a column more or less widens or narrows
  it by one column (up to four, never beyond the screen), and when Macs are removed and what's left is shorter
  than the window, it shortens to fit.
- The **gear** on a Mac's panel sets which modules are shown, how often it samples, and whether it is in the
  Overview.
- **Display Options** (the sliders button) include a **Color Scheme**: Standard, Classic, Ocean, Sunset, Forest,
  Violet and High Contrast. A scheme sets the chart line colors, the load scale on bar charts, and the accent.
  The thermal indicator always runs green to red.
- A Mac is marked **Not responding** if it goes quiet (for example, asleep) and **Offline** once it's dropped.
  It reconnects on its own. Offline Macs stay in the sidebar but take no room in the Overview or comparisons; one
  line names them instead.
- **Module width**: modules are never narrower than it takes to show everything in them on one line (measured
  from the Macs on show). **Keep modules at their minimum width** in Settings (Dashboard) fixes them at exactly that width
  when Macs are shown side by side: the window grows a column for each Mac added and shrinks for each one
  removed, and scrolls sideways once it's as wide as the screen. Closing the sidebar narrows the window by the
  sidebar's width, and opening it widens it again. Resizing the window by hand leaves the modules as they are.
- **Settings** (⌘,) has four tabs: **General** (appearance, open at login, the window at startup), **Dashboard**,
  **Clients** (pairing requests) and **History**.
- **Appearance** (Settings, General): System, Light or Dark. System follows the Mac's own setting.
- **Each Mac in the sidebar** shows its client's version, for example "v0.1.5": green when it matches the
  host's, orange when it's older (update it). Hover for what it's collecting and how often. Clients before
  0.1.4 don't report a version and show as "v0.1.3 or earlier".
- **Version**: the host's is in its menu-bar item and under About in the app menu; the client's is at the bottom
  of its menu-bar item.
- The host keeps running, and recording, with its window closed. Quit it from the menu-bar item or with ⌘Q.
- **Open at login**: a switch in the host's Settings (General) and in the client's Settings (Settings… in its menu-bar item). The host also has a setting for
  whether its window opens at startup; with it off the app starts quietly in the menu bar. macOS may ask you to
  allow the app under System Settings, General, Login Items.

## What the host collects

The host has every Mac (its own included) collect every module, at that Mac's own sampling interval
(the gear on its panel), whether or not anything is on screen. So the host's data is always complete,
for its own window, for history, and for Viewers. Turning a module off in a Mac's settings only hides
its card.

## Viewers

A **Viewer** is a client the host's owner lets see every Mac: the same dashboard as the host's, in a window
on that client, for viewing only.

- **Allowing it**: on the host, right-click the Mac in the sidebar and choose **Allow Viewing All Macs…**,
  which says what it will see. **Stop Viewing All Macs** in the same menu ends it at once. A Viewer has an
  eye beside it in the sidebar, filled in while it's viewing. Clients older than 0.1.4 can't be Viewers
  (the menu says to update them).
- **On the Viewer**: the client's menu-bar item has **Show All Macs** (and **Hide All Macs**). The window
  opens with what the host has, then follows it live. Overview, Compare, a Mac full size, layouts, colors,
  card order and which cards and Macs are shown are the Viewer's own choice.
- **What it can't do**: change anything the host decides (sampling, history, pairing), export, or remove
  Macs. The Viewer keeps nothing on disk: closing the window, losing the connection or being stopped by the
  host forgets it all.
- **IP addresses** are left out unless the host turns on **Show IP addresses to Viewers** (Settings, Clients).
- With history on, the host notes when a Viewer is allowed or stopped, and when it starts and stops viewing.

## The client's own view

The client's menu-bar item has **Show This Mac's Stats**, which opens a window with that Mac's own cards, the
same as the host shows them full size. The same item (now **Hide This Mac's Stats**) closes it.

- It works whether or not the Mac is paired.
- It measures independently of the host: its modules (the Modules menu in its toolbar), its sampling interval
  (Modules, More Settings…) and its look (Display Options, Always on Top) are this Mac's own, and nothing it
  does changes what the host asks for or receives.
- It samples only what it shows, and only while the window can be seen.
- While the window is open, the client has a Dock icon, so it can be found with Command-Tab.

## History

Off by default. Turn it on in **Settings** (⌘,), on the History tab, or click the "History is off" line at the
bottom of the sidebar.

- One file per day (`YYYY-MM-DD.sqlite`), a new one at midnight, kept for the number of days you choose
  (30 by default), with an optional size limit.
- One row per client per interval (10 seconds by default): averages, plus the peak for CPU, GPU and network.
  Events such as connects and disconnects are recorded too.
- About 16 MB a day for 10 clients at the default interval.
- Each Mac has its own **Record history** switch in its settings.
- **Export…** writes CSV (one samples file and one events file) or JSON, for the clients you pick and an exact
  time range (date and time, across midnight if need be), with quick ranges such as Last Hour and Today.
- While a Mac is recorded, it samples at least as often as the interval.

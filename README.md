# SystemStatusRemote

Watch the CPU, GPU, memory, network, disks, battery and thermal state of several Macs at once, from one
dashboard, with optional recorded history. From [Blinken Labs](https://blinken-labs.com).

This repository holds the downloads only; the source code isn't published.

## Download

Get the [latest release](https://github.com/blinken-labs/SystemStatusRemote-Releases/releases/latest). Each app
comes as a disk image (`.dmg`) or a zip; either works.

| Mac | App |
|---|---|
| The Mac that shows the dashboard | **SystemStatusRemote** (the host) |
| Each Mac you want to watch | **SystemStatusClient**, a menu-bar item |
| Optional | **SystemStatusUninstaller** removes the apps and everything they saved |

macOS 15 (Sequoia) or later on every Mac. The Macs must reach each other over the network (the host listens on
port 47820). Releases are signed with a Developer ID and notarized by Apple.

## Install and pair

1. Open the `.dmg` and drag the app onto Applications (or unzip and move it there), then open it.
2. **On the host**, click **Add Client…**.
3. **On each client**, click its menu-bar item, choose the host under **Host**, and click **Request Pairing**.
   It shows three check words, such as "maple · river · copper".
4. **Back on the host**, the request appears under **Pairing Requests**. Click **Accept…**, check it shows the
   same three words, and accept.
5. **Back on the client**, it shows the words again: click **Yes, Pair** if the host showed the same ones.

On first launch macOS asks for Keychain access (choose **Always Allow**), incoming connections on the host, and
the local network on each client (choose **Allow**).

## How many Macs

The host pairs up to **2** clients. **SystemStatusRemote Pro**, a one-time purchase in the Mac App Store version
of the host, lets it pair any number; these downloads can't buy it. Macs already paired stay paired, and the
client is always free. Settings (General) on the host shows how many Macs are paired.

## Updating from 1.0.x

1.1 is from Blinken Labs, so macOS sees the apps as new ones: settings and history start afresh, and every Mac
pairs again once. Export your history from the 1.0.x host first (Settings, History) if you want to keep it. The
new SystemStatusUninstaller can remove what the 1.0.x apps left behind. See the
[1.1.0 release notes](https://github.com/blinken-labs/SystemStatusRemote-Releases/releases/tag/v1.1.0).

## If the client can't reach the host

The client's menu-bar item says why.

- **"didn't answer"**: check the host Mac is awake with SystemStatusRemote open, on the same network. Try
  **Enter an Address** with the host's IP address, shown in its **Add Client…**.
- **"This Mac isn't allowed to use the local network"**: turn on SystemStatusClient under System Settings,
  Privacy & Security, Local Network. If it isn't listed there and macOS never asked (a macOS 27 bug), quit the
  client, run `tccutil reset All com.blinken-labs.SystemStatusClient` in Terminal, move every copy of
  SystemStatusClient to the Trash and empty it, restart, and open it again; choose **Allow** when asked.

## Support

Problems or questions: [support@blinken-labs.com](mailto:support@blinken-labs.com).

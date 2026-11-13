# fitdesktop

An accidental **Alt + mouse wheel** can zoom the XFWM4 desktop until parts of
it are off-screen. The same gesture can return it to normal, but if you don't
know what happened, an explicit recovery command is easier than hunting
through desktop settings.

`fitdesktop` is that one-purpose recovery switch: run it to reset the desktop
zoom to 1:1. It briefly turns XFWM4 compositing off and back on. It does not
change your display resolution, monitor layout, or desktop scaling settings.

## Why this exists

When I first installed Linux Mint, I didn't know all the desktop's little
features. Once, without meaning to, I hit **Alt** and scrolled the mouse wheel.
Suddenly the desktop was zoomed in, larger than the screen, and I was dealing
with a virtual desktop. I had no idea what I'd done and assumed the UI had
gotten messed up.

Sure, I could have read the manual (RTFM). Instead, being a typical guy, I
wanted a quick and sure way to put the desktop back the way it was. So I wrote
a script. The mouse-wheel gesture can reset the zoom too, once you know about
it; this script is for the moment when you don't.

Why only Xfce? That's the Linux Mint flavor I initially installed :D The
script uses XFWM4 settings, so it's specific to Xfce rather than a general
Linux desktop zoom fixer.

## Requirements

- Bash
- Linux running an Xfce session with `xfwm4`
- `xfconf-query` to read and update XFWM4 settings
- `flock` and `pgrep` (usually provided by standard Linux utilities)
- `notify-send` is optional; it is used for desktop notifications when the
  script is launched without a terminal

The script is intended for XFCE and will report an error if `xfconf-query` or
the running `xfwm4` process cannot be found.

## Usage

From the directory containing the script, run:

```bash
./fitdesktop
```

The script is executable in this repository. If you downloaded it without
preserving executable permissions, make it executable and run it:

```bash
chmod +x fitdesktop
./fitdesktop
```

You can also configure a keyboard shortcut or application launcher to run
`/path/to/fitdesktop`. When launched without a terminal, status messages are
sent as desktop notifications if `notify-send` is installed.

## What it does

- Checks that XFWM4 compositing is enabled and that `xfwm4` is running.
- Turns compositing off for half a second, then turns it back on.
- Verifies that compositing was restored, retrying up to five times.
- Prevents overlapping runs with a per-user lock.
- Attempts to restore compositing if the script is interrupted while it is
  switched off.

If compositing is already off, the script does nothing because there is no
compositing zoom state to reset.

## If compositing does not come back on

The script reports this condition and prints the command to enable compositing
again. You can run it manually:

```bash
xfconf-query -c xfwm4 -p /general/use_compositing -s true
```

## License

No license is currently specified in this repository.

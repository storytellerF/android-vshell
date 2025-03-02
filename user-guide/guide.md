% User Guide

***

### Overview

vShell is a virtual shell environment application for the Android OS. It
provides a virtual machine running small Linux distribution ready for the
use out-of-box.

Here are some details on the virtual machine configuration:

- CPU: emulated 1-core x86 64-bit.
- RAM: 32% of host memory + 8% for QEMU TCG buffers.
- HDD: 64 GB, has a default 4 GB partition for user data.
- Host storage: via 9P FS, mount tag `host_storage`.
- Network: user-mode via SLiRP.
- Operating system: [Alpine Linux](https://alpinelinux.org/).

Please note that provided default setup of the Alpine Linux is diskless. The
whole operating system is copied into RAM, which takes about 40 MB. All changes
are discarded unless were saved to the disk with the utility [lbu](https://wiki.alpinelinux.org/wiki/Alpine_local_backup).
In order to change this behavior, you will need to format HDD and re-install
the system from scratch, which could be easily done via 'setup-alpine' utility.

**Disclaimer**: neither vShell application nor its author is affiliated with
the [Alpine Linux](https://alpinelinux.org/) project. Operating system is
provided as-is and vShell author is not responsible about bugs in the software
packages.

*This guide does not cover Shell Scripting and Linux System Administration
topics.*

***

### User interface
#### Terminal

vShell application uses the XTerm-compatible terminal emulator as a frontend
for a serial console of the virtual machine. It has a True Color support and
uses Inconsolata LGC font patched by Nerd-Fonts for additional glyphs support.

<span style="display:block;text-align:center">
  <img alt="vShell session" src="img/terminal.png" style="width:90%;max-width:700px;">
</span>

The application does not provide a way for customizing the terminal style like
a color scheme or the font, because chosen defaults are considered as suitable
for the most configurations.

The terminal reacts to the following kinds of a user input:

 - A pinch-zooming gesture to scale the text size.
 - A long tap to start text selection or open a context menu.
 - A single tap to emulate a mouse left button click.

Please note that this application is intended to have only one terminal
session by design. To have more, you will need to use a terminal multiplexer
like `tmux`.

##### Troubleshooting: terminal has stuck

The terminal is sensitive to printed data. If a binary content has been
accidentally printed or some misbehaving program incorrectly initialized
the console, it can be easily messed up.

In this case, you need to either execute the command `reset` or tap a button
**Reset** which is located in the context menu.

##### Troubleshooting: misplaced text

If you have re-scaled the text size, you may observe an incorrect line wrapping
or misplaced UI elements of the console applications. That is happening due to
the nature of the serial line, which does not support a screen size handshaking.

<span style="display:block;text-align:center">
  <img alt="Example of misplaced UI elements" src="img/text_bad_wrap.png" style="width:90%;max-width:700px;">
</span>

To solve the issue, you need to use a command `resize` after changing the text
scale. A default setup of the operating system provided by vShell application
runs this command automatically in a preexec hook of Zsh shell.

#### Notification

When the application has successfully started a foreground service, a
notification will be shown. Clicking on the notification will open the
terminal session. Expanding the notification would reveal some action
buttons.

<span style="display:block;text-align:center">
  <img alt="Notification screenshot" src="img/app_notification.png" style="width:90%;max-width:700px;">
</span>

Clicking on a button **Acquire wakelock** will prevent the device from going
into the sleep mode. Use this action when you need to keep the virtual machine
from being suspended when the screen is turned off.

Clicking on a button **Shut down** will immediately close the application.
Shutting down the virtual machine by this way can lead to a loss of unsaved
data, so be careful.

#### Keyboard
##### Touch keyboard shortcuts

**Volume Down** key emulates the **Ctrl** key which can be combined with others
to request a special action. The possible key combinations are the
software-specific. To learn about them, consult with a documentation of
your installed packages.

**Volume Up** key produces a certain input in the following combinations:

|Key combination     |Produced input                  |
|:-------------------|:-------------------------------|
|`Volume-Up .`       |`Ctrl \`                        |
|`Volume-Up <1..9>`  |`F1`, `F2`, ..., `F9` keys      |
|`Volume-Up 0`       |`F10`                           |
|`Volume-Up A`       |Left arrow key                  |
|`Volume-Up B`       |`Alt B`                         |
|`Volume-Up D`       |Right arrow key                 |
|`Volume-Up E`       |Escape                          |
|`Volume-Up F`       |`Alt F`                         |
|`Volume-Up H`       |`~`, the tilde character        |
|`Volume-Up K`       |Toggle extra keys row           |
|`Volume-Up L`       |`|`, the pipe character         |
|`Volume-Up N`       |Page down key                   |
|`Volume-Up P`       |Page up key                     |
|`Volume-Up S`       |Down arrow key                  |
|`Volume-Up T`       |Tab key                         |
|`Volume-Up U`       |`_`, the underscore character   |
|`Volume-Up V`       |Show the volume control         |
|`Volume-Up W`       |Up arrow key                    |
|`Volume-Up X`       |`Alt X`                         |

Note that **Volume Up** does not represent the **Alt** key even though it is
being used to emulate certain its combinations.

##### Hardware keyboard shortcuts

These key combinations can be used on a hardware keyboard to trigger certain
actions of the application:

|Key combination|Action                        |
|:--------------|:-----------------------------|
|`Ctrl Alt +`   |Increase text size            |
|`Ctrl Alt -`   |Reduce text size              |
|`Ctrl Alt K`   |Toggle the touch keyboard     |
|`Ctrl Alt M`   |Open context menu             |
|`Ctrl Alt U`   |Open URL selector             |
|`Ctrl Alt V`   |Paste clipboard               |

##### Extra Keys Row

Extra Keys Row is a widget displayed above the touch keyboard. It provides
special keys that are commonly used by console programs.

<span style="display:block;text-align:center">
  <img alt="Extra keys row screenshot" src="img/extra_keys_row.png" style="width:90%;max-width:700px;">
</span>

Swiping up some keys will expose the alternate ones. Particularly:

 - `-` will show a key for `_`
 - `|` will show a key for `&`

You may toggle the Extra Keys Row with a key combination **Volume Up+K**.

### Port forwarding

QEMU is configured to expose the guest port 80 as the random port on
local host. The primary purpose is acessing the local web services
with taking advantage of a web browser installed on your host.

The application context menu provides an address which can be used
for accessing the guest server as well as a shortcut for opening the
web browser with correct URL.

<span style="display:block;text-align:center">
  <img alt="Context menu screenshot" src="img/port_forwarding.png" style="width:90%;max-width:700px;">
</span>

Please note that forwarded port is accessible only on the local host.
The application does not expose it to the local network or the Internet. If
you wish to make your server accessible outside, consider using the
port external forwarding services or relays.

vShell comes without any server software installed. You have to
install and configure the server on your own. Just make sure it listens
for incoming connections on the port 80.

***

<p style="text-align:center;"><font size="1dp"><i>vShell — a virtual shell environment application.</i></font></p>

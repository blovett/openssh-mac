![https://jcs.org/images/notaweblog/2011-04-19_cocoa-ssh-askpass.png](https://jcs.org/images/notaweblog/2011-04-19_cocoa-ssh-askpass.png)

### About
This is a copy of Apple's OpenSSH 5.6p1 that is bundled with OS X 10.7, plus a modification to add a `RequireKeyConfirmation` option.  This no longer includes the `AddKeysToAgent` option modification, as it is no longer needed.

With `RequireKeyConfirmation` set to `yes` in `~/.ssh/config`, any identities added to `ssh-agent` will require confirmation before use.  Combined with the included `cocoa-ssh-askpass` wrapper around CocoaDialog, a GUI dialog will be presented when SSH tries to use an unlocked identity stored in the agent.  This applies to SSH spawned from a terminal (directly or through things like `git`), from a forwarded agent, and from any GUI program that uses it in the background to setup tunnels like Sequel Pro.

More information about agent confirmation can be read at [http://jcs.org/macssh](http://jcs.org/macssh).

### Building
Run `xcodebuild` from the top directory.

### Installing
`sudo xcodebuild` will install it into `/tmp/openssh.dst` as usual.  Overlay this directory on to `/` with `sudo rsync -av /tmp/openssh.dst/. /.`.  Avoid directly installing into `/` by overriding `DSTROOT` because of some scary recursive `chmod`s and `chown`s that the XCode build script does (from Apple).

Download and install [CocoaDialog](http://mstratman.github.com/cocoadialog/) to `/Applications/Utilities`.  The `cocoa-ssh-askpass` wrapper that is installed to `/usr/bin` will look for CocoaDialog at `/Applications/Utilities/CocoaDialog.app/Contents/MacOS/CocoaDialog`.

### Usage
Once installed, set the `SSH_ASKPASS` environment variable to `/usr/bin/ssh-askpass` in your shell's config file.  At the first SSH connection, the usual secure input window will appear asking for the key passphrase.  Leave the "Remember password in my keychain" option unchecked.  If `RequireKeyConfirmation` is set to `yes`, on the next SSH connection, `cocoa-ssh-askpass` will be invoked to prompt for confirmation. 

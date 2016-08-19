xpra-sudo
=========

`xpra-sudo` is a wrapper script for Xpra that allows running the server as a
different user on the same system using sudo.

Installation
------------

Just copy both scripts (`xpra-sudo` and `xpra-sudo-ssh`) to a directory in your
`PATH`. If you don't want `xpra-sudo-ssh` to mess up your path, you can also
place it somewhere else and set the variable `SSH_HELPER` in `xpra-ssh`
accordingly.

Usage
-----

To start an Xpra session as user `user`, just run this command:

    xpra-sudo start sudo:user:42 --start-child=urxvt

How does it work?
-----------------

Xpra already supports connecting via SSH and `xpra-sudo` uses this mechanism:
it passes `xpra-sudo-ssh` as a custom SSH command to Xpra using the `--ssh`
option and changes `sudo:user:42` to `ssh:user@localhost+sudo:42`.
`xpra-sudo-ssh` then recognizes the special hostname `localhost+sudo` and uses
sudo instead of SSH to run the command.

How does that sudoers syntax work again?
----------------------------------------

`xpra-sudo` works fine without the `NOPASSWD` option in sudo, but you might
nonetheless set it for your convenience and don't remember the syntax? Here's
my service for you, this allows the user `julian` to run commands as `user`:

    julian ALL=(user) NOPASSWD: ALL

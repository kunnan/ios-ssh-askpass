ios-ssh-askpass
===============

An iOS version of ssh-askpass.

Usage
-----

Requirements:

  * `ssh-agent` must be running
  * `SSH_AUTH_SOCK` must be set to the ssh agent's socket
  * `DISPLAY` must be set (to any value)
  * Either:
   - /usr/libexec/ssh-askpass must be linked to ios-ssh-askpass OR
   - `SSH_ASKPASS` must be set to the fullpath of `ios-ssh-askpass`
  * `ssh-add` must be run without a tty (or with stdin redirected to `/dev/null`)

When the above requirements are met, the passphrase is requested from the user by displaying a modal alert. If it is incorrectly entered, the user is given a second chance. After a second failure, ssh-add exits with an error code.

Testing out ios-ssh-askpass
---------------------------

1) Compile ios-ssh-askpass

    $ make

2) Install the executable

    # cp ios-ssh-askpass /usr/libexec
    # cd /usr/libexec
    # ln -s ios-ssh-askpass ssh-askpass

3) Start an ssh-agent

    $ eval `ssh-agent`

4) Test out ssh-add

        $ DISPLAY=1 ssh-add </dev/null

    A window should pop up requesting your passphrase.

5) Check that the passphrase was stored in the agent

    $ ssh-add -l

Detecting an existing ssh-agent
-------------------------------

By default, ssh-agent stores its socket files in `/tmp/ssh-XXXXXXXXXXXX/agent.YYYY` where the X's are a random alphanumeric string and YYYY is the pid of the original ssh-agent process. This process forks to allow the daemon to keep running in the background, so the ssh-agent daemon process will have a pid that is one higher. This allows one to recreate the `SSH_AUTH_SOCK` environment variable (and check that the agent is still running).


# see also
>*  ls -lrt /usr/bin/ssh*
```sh
devzkndeMacBook-Pro:sbwifitweak devzkn$ ssh-add
Identity added: /Users/devzkn/.ssh/id_rsa (/Users/devzkn/.ssh/id_rsa)
devzkndeMacBook-Pro:sbwifitweak devzkn$ ls -lrt /usr/bin/ssh*
-rwxr-xr-x  1 root  wheel    10372 Jul 16  2017 /usr/bin/ssh-copy-id
-rwxr-xr-x  1 root  wheel  1810576 Oct 26  2017 /usr/bin/ssh-add
-rwxr-xr-x  1 root  wheel  1846608 Oct 26  2017 /usr/bin/ssh-keygen
-rwxr-xr-x  1 root  wheel  1856496 Oct 26  2017 /usr/bin/ssh-keyscan
-rwxr-xr-x  1 root  wheel  1809312 Oct 26  2017 /usr/bin/ssh-agent
-rwxr-xr-x  1 root  wheel  2112992 Oct 26  2017 /usr/bin/ssh
devzkndeMacBook-Pro:sbwifitweak devzkn$ ls -lrt  /usr/libexec/ssh*
-r-xr-xr-x  1 root  wheel      629 Jul 16  2017 /usr/libexec/sshd-keygen-wrapper
-rwx--x--x  1 root  wheel  1873680 Oct 26  2017 /usr/libexec/ssh-keysign
-rwxr-xr-x  1 root  wheel  1647152 Oct 26  2017 /usr/libexec/ssh-pkcs11-helper
```

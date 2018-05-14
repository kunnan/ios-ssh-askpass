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
>* echo "" > ~/.ssh/known_hosts
```sh
The authenticity of host '[localhost]:2222 ([127.0.0.1]:2222)' can't be established.
ECDSA key fingerprint is SHA256:l11u/.
Are you sure you want to continue connecting (yes/no)? 
```

>* man eval
```sh 
Shell builtin commands are commands that can be executed within the running shell's process. 
```
>* ssh-add ~/.ssh/id_rsa_Theos125
```sh
devzkndeMacBook-Pro:sbwifitweak devzkn$ ssh-add ~/.ssh/id_rsa_Theos125
Identity added: /Users/devzkn/.ssh/id_rsa_Theos125 (/Users/devzkn/.ssh/id_rsa_Theos125)
devzkndeMacBook-Pro:sbwifitweak devzkn$ ssh-add -l
4096 SHA256: /Users/devzkn/.ssh/id_rsa (RSA)
2048 SHA256:L1XVpioVKP/6LajAEH9++ /Users/devzkn/.ssh/id_rsa_Theos125 (RSA)
```

>* ssh-agent
```
ssh-agent命令是一种控制用来保存公钥身份验证所使用的私钥的程序。ssh-agent在X会话或登录会话之初启动，所有其他窗口或程序则以客户端程序的身份启动并加入到ssh-agent程序中。通过使用环境变量，可定位代理并在登录到其他使用ssh机器上时使用代理自动进行身份验证。
其实ssh-agent就是一个密钥管理器，运行ssh-agent以后，使用ssh-add将私钥交给ssh-agent保管，其他程序需要身份验证的时候可以将验证申请交给ssh-agent来完成整个认证过程。
```



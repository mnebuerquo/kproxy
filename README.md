# kproxy
A script for enabling or disabling proxy settings for your shell when you are
behind a proxy.

You probably don't want to use this on a multi-user machine, because it stores
credentials on disk. It's intended to be safer than the methods I've seen in
use, which store `user:password` in the `http_proxy` environment variable, and
in the `.bash_profile` to set it on login.
These can be urlencoded, but are typically plain text. If you are in a pair
programming session on your machine, you might accidentally display your
environment variables, and you don't want your password readable at a glance.

*Note:* Obfuscating a password is not security against an attacker. You're
still storing credentials on disk, so use at your own risk.

# Usage

You need to source the script to make the functions available in your shell.
```sh
$ . kproxy
```

Add these lines to your `~/.bashrc` to make it source this script when you open
a terminal (I have symlinked it in `~/bin`, so set the path to whatever you
want):

```sh
if [ -f ~/bin/kproxy ]; then
	. ~/bin/kproxy
fi
```

Once you have sourced this script, call `makeproxypass` to set your password
for the proxy server. It will use the user name of your currently logged in
user.
```sh
$ makeproxypass
Enter password:
```

Now you can enable or disable your proxy environment variables with a shell
command:

```sh
proxy_on
```

or 

```sh
proxy_off
```

There is also an automatic detection function for turning on the proxy settings
after attempting an HTTP request. If the request succeeds without the proxy,
then leave the proxy disabled. If it fails, then enable the proxy.

```sh
detectsetproxy
```

# Installing

1. Clone this repo somewhere. 
2. To be able to use it in all terminals, add a call out in your `~/.bashrc` 
```sh
path-to-your-clone/kproxy
```

If you reuse your same `.bashrc` on multiple machines, you might want to wrap
the callout to `kproxy` in an `if` block to test if the file exists.

# Pull Requests Welcome

I have tested this on MacOS, with the default bash/sh. I wrote it to use sh
instead of bash for portability. Open an issue on this project if it doesn't
work for you. Send me a PR and I'll merge it.

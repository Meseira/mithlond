Mithlond
========

Author:
Xavier Gendre <gendre.reivax@gmail.com>

Description
-----------

The goal of Mithlond is to help to manage control groups for LXC unprivileged containers. In particular, it aims to ease the needed settings and to give an easy way to autostart LXC unprivileged containers.

Compatibility
-------------

Mithlond has only been tested with Debian Jessie for the time being. Do not hesitate to report your testings on other systems.

Basic usage
-----------

To enable Mithlond for a given user, you need to add him/her to the Mithlond's user list with the command `mithlond subscribe USER` as system administrator.

To create a LXC unprivileged container, the user need a configuration file, called `~/.config/lxc/default.conf`, that you can basicaly create as follows,
```
mkdir -p ~/.config/lxc
echo "lxc.id_map = u 0 $(grep $USER /etc/subuid | cut -d':' -f2) 65536" > ~/.config/lxc/default.conf
echo "lxc.id_map = g 0 $(grep $USER /etc/subgid | cut -d':' -f2) 65536" >> ~/.config/lxc/default.conf
echo "lxc.network.type = empty" >> ~/.config/lxc/default.conf
echo "lxc.start.auto = 1" >> ~/.config/lxc/default.conf
```
In such a way, the next containers get no network interface and are set to autostart.

Now, create a container as a user, for instance, with the template `download`,
```
lxc-create -t download -n p1 -- -d debian -r wheezy -a amd64
```
Reboot and be happy with your autostarted container,
```
$ lxc-ls -f
NAME STATE    IPV4  IPV6  AUTOSTART
-----------------------------------
p1   RUNNING  -     -     YES
```

Notes
-----

Currently, Mithlond can only handle the parameters `lxc.start.auto` and `lxc.start.delay`. If you set `lxc.start.order` or `lxc.group`, their values won't be taken into account.

Issues
------

If you encounter any problem with this software, do not hesitate to report it in a [GitHub issue][1].

  [1]: https://github.com/Meseira/mithlond/issues

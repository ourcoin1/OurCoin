Sample init scripts and service configuration for ourcoind
==========================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/ourcoind.service:    systemd service unit configuration
    contrib/init/ourcoind.openrc:     OpenRC compatible SysV style init script
    contrib/init/ourcoind.openrcconf: OpenRC conf.d file
    contrib/init/ourcoind.conf:       Upstart service configuration file
    contrib/init/ourcoind.init:       CentOS compatible SysV style init script

1. Service User
---------------------------------

All three startup configurations assume the existence of a "ourcoin" user
and group.  They must be created before attempting to use these scripts.

2. Configuration
---------------------------------

At a bare minimum, ourcoind requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, ourcoind will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that ourcoind and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If ourcoind is run with "-daemon" flag, and no rpcpassword is set, it will
print a randomly generated suitable password to stderr.  You can also
generate one from the shell yourself like this:

bash -c 'tr -dc a-zA-Z0-9 < /dev/urandom | head -c32 && echo'

Once you have a password in hand, set rpcpassword= in /etc/ourcoin/ourcoin.conf

For an example configuration file that describes the configuration settings,
see contrib/debian/examples/ourcoin.conf.

3. Paths
---------------------------------

All three configurations assume several paths that might need to be adjusted.

Binary:              /usr/bin/ourcoind
Configuration file:  /etc/ourcoin/ourcoin.conf
Data directory:      /var/lib/ourcoind
PID file:            /var/run/ourcoind/ourcoind.pid (OpenRC and Upstart)
                     /var/lib/ourcoind/ourcoind.pid (systemd)

The configuration file, PID directory (if applicable) and data directory
should all be owned by the ourcoin user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
ourcoin user and group.  Access to ourcoin-cli and other ourcoind rpc clients
can then be controlled by group membership.

4. Installing Service Configuration
-----------------------------------

4a) systemd

Installing this .service file consists on just copying it to
/usr/lib/systemd/system directory, followed by the command
"systemctl daemon-reload" in order to update running systemd configuration.

To test, run "systemctl start ourcoind" and to enable for system startup run
"systemctl enable ourcoind"

4b) OpenRC

Rename ourcoind.openrc to ourcoind and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
"/etc/init.d/ourcoind start" and configure it to run on startup with
"rc-update add ourcoind"

4c) Upstart (for Debian/Ubuntu based distributions)

Drop ourcoind.conf in /etc/init.  Test by running "service ourcoind start"
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon uitility.

4d) CentOS

Copy ourcoind.init to /etc/init.d/ourcoind. Test by running "service ourcoind start".

Using this script, you can adjust the path and flags to the ourcoind program by
setting the ourcoind and FLAGS environment variables in the file
/etc/sysconfig/ourcoind. You can also use the DAEMONOPTS environment variable here.

5. Auto-respawn
-----------------------------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.

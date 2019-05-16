# OEM Config for Feren OS
This is a project to create an OEM Config screen for Feren OS (which can be edited easily for other Linux Distributions) that makes use of Calamares (can be reprogrammed to use another application instead) and a full Desktop Environment in order to provide a elegant, complete and easy OEM Config Screen.

Deployment Tutorial: https://github.com/feren-OS/oem-setup-shell/wiki/Deployment


<h2>Supported Desktop Environments:</h2>
The following Desktop Environments are currently supported:

- Cinnamon

- KDE Plasma


<h2>Supported DMs:</h2>
The following Display Managers are currently supported:

- LightDM

- SDDM


<h2>How does it work?</h2>
Once feren-oem-config.service is enabled and is started on boot, feren-oem-session-prep will do the following:

1. Create a user for running the OEM Config screen and session, which can be deleted once the OEM Config is complete
2. Hide the OEM Config User from the DM's Login Screen
3. Configure the DM to apply autologin for that user, so once the boot process is complete the system will automatically log in as them
4. Apply temporary restrictions for the supported Desktop Environments so that it's harder to get outside of what should be accessed within the OEM Config Screen
5. Apply custom default configurations that the OEM Config User will inherit when they are logged into to simplify the desktop for the OEM Config screen
6. Disable OOBE, Lock Screen and so on for the OEM Config User
7. Continue bootup and let the OS do the rest to begin the OEM Config

Once the user logs in, the following script is started:

- (user-level) feren-oem-session start

- (Superuser, spawned by feren-oem-session) feren-oem-session

<h4>feren-oem-session start:</h4>
This is a simple Bash script to launch the Superuser 'feren-oem-session' script as well as making sure that any other applications are not running inside of that session.

<h4>feren-oem-session:</h4>
When launched without arguments, however, this script will launch Calamares on an infinite loop with either the kvantum Application Style (Plasma), or the gtk2 Application Style (Other DEs). This infinite loop is broken if a file exists on the system to indicate the successful completion of the OEM Setup, and once it is then feren-oem-remove.service is installed and enabled for the next bootup, followed by restarting the OS automatically.

On the next boot, feren-oem-remove is initiated and undoes all restrictions, removes the OEM Config User and removes the OEM Config Shell and OEM Config in their entirety, before continuing boot as normal.


<h2>User Specifications for user feren-oem-setup:</h2>
These are the custom user specifications given to the OEM Config User:

- Home Directory: /oem-session

- System Account? Yes


<h2>Flaws:</h2>
There are still a few flaws with this program in its current state, including:

- The OEM Config screen uses a fully-fledged Desktop Environment, meaning some extra unneeded applications and background processes will also run in the background on the OEM Config session
- More


Related Repository: https://github.com/feren-OS/oem-config

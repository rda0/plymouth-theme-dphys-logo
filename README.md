# plymouth splash install screen

## installation

```
apt install plymouth plymouth-label plymouth-theme-ubuntu-logo plymouth-theme-ubuntu-text
cd /usr/share/plymouth/themes
git clone git@gitlab.phys.ethz.ch:core/plymouth-theme-dphys-logo.git dphys-logo
```

## enable dphys theme

```
/usr/bin/update-alternatives --install /usr/share/plymouth/themes/default.plymouth default.plymouth \
/usr/share/plymouth/themes/dphys-logo/dphys-logo.plymouth 100
/usr/bin/update-alternatives --set default.plymouth /usr/share/plymouth/themes/dphys-logo/dphys-logo.plymouth
```

or:

```
rm /etc/alternatives/default.plymouth
ln -s /usr/share/plymouth/themes/dphys-logo/dphys-logo.plymouth /etc/alternatives/default.plymouth
```

## enable ubuntu default theme

```
rm /etc/alternatives/default.plymouth
ln -s /usr/share/plymouth/themes/ubuntu-logo/ubuntu-logo.plymouth /etc/alternatives/default.plymouth
```

## production

start the daemon, show splash screen:

```
plymouthd --tty=/dev/tty1
plymouth show-splash
```

update progess message:

```
PROGRESS='0'
plymouth message --text="${PROGRESS}% done"
```

## testing

start daemon:

```
plymouthd --tty=/dev/tty1 --mode=boot --kernel-command-line="quiet splash" --debug
plymouthd --tty=/dev/tty1 --debug
```

test:

```
plymouth show-splash
plymouth message --text="hello world"
plymouth pause-progress 
plymouth unpause-progress 
plymouth message --text="resuming boot"
plymount display-message --text="this is a test line"
plymount update --status="title:this is the title"
plymount update --status="log:this is a log line"
plymount update --status="error:this is a error"
plymouth quit
```

test again:

```
plymouth quit; plymouthd --tty=/dev/tty1 --debug-file=/root/plymouthd.log; plymouth show-splash
for p in `seq 0 100`; do plymouth message --text="${p}% done"; sleep 0.2; done
for i in $(seq 0 200); do plymouth update --status="log:printing log line number $i";done
```

kill daemon:

```
kill $(ps -ef | grep -E '[l]ymouth' | awk '{print $2}')
```

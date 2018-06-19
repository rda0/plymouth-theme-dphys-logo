# plymouth splash install screen

## installation

```
apt install plymouth plymouth-label plymouth-theme-ubuntu-logo plymouth-theme-ubuntu-text
cd /usr/share/plymouth/themes
git clone git@gitlab.phys.ethz.ch:core/plymouth-theme-dphys-install-logo.git dphys-install-logo
```

## enable dphys theme

```
rm /etc/alternatives/default.plymouth
ln -s /usr/share/plymouth/themes/dphys-install-logo/dphys-install-logo.plymouth /etc/alternatives/default.plymouth
mkdir -p /usr/share/fonts/opentype
ln -s /usr/share/plymouth/themes/dphys-install-logo/font/dinpro /usr/share/fonts/opentype/dinpro
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
plymouth quit
```

test again:

```
plymouth quit; plymouthd --tty=/dev/tty1 --debug-file=/root/plymouthd.log; plymouth show-splash
for p in `seq 0 100`; do plymouth message --text="${p}% done"; sleep 0.2; done
```

kill daemon:

```
kill $(ps -ef | grep -E '[l]ymouth' | awk '{print $2}')
```

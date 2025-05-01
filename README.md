## How to scan IPs with open port without nmap?

#### if you are on windows

download exe from https://github.com/msys2/msys2-installer/releases
(click Assets and download something like 'msys2-x86_64-20250221.exe')

install it and run msys2

copy followings and paste (right click then paste) and press enter and wait (modify 22 as you wish)
```
while read -r c; do
	for i in $(seq 1 254); do
		telnet $c.$i 22 2>&1 | grep -oE 'Connected to.*$' &
	done
done < <(netstat -r | awk '{print $4}' | grep -oE '[0-9]+\.[0-9]+\.[0-9]+' | sort -u | grep -v '127.0.0') | grep -oE '[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+'
```

#### if you are on mac

open terminal then copy followings and paste (right click then paste) and press enter and wait (modify 22 as you wish)
```
while read -r c; do
	for i in $(seq 1 254); do
		nc -zv $c.$i 22 2>&1 | grep -E '.*open$' &
	done
done < <(netstat -r | awk '{print $4}' | grep -oE '[0-9]+\.[0-9]+\.[0-9]+' | sort -u | grep -v '127.0.0') | grep -oE '[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+'
```

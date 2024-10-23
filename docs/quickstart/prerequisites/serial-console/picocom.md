---
id: picocom 
sidebar_position: 2 
sidebar_label: Picocom (Mac, Linux) 
---

# Picocom 

# Install

On Ubuntu, use apt:

```
sudo apt-get install picocom
```

On OS X, use **[homebrew](https://brew.sh/)**:

```
brew install picocom
```

# Launch picocom

```
picocom --imap lfcrlf -b 115200 /dev/ttyACM0
```

# Quit picocom

Press "control" and "a" together, then press "control" and "q" together.

```
<CTRL>a <CTRL>q
```

+++
title = "Software setup for Laravel"
menuTitle = "Setup"
weight = 2
pre = "- "
+++

# Windows

## Docker Setup

1. open Docker Desktop > Settings (gear icon) > General
   {{% notice info %}}
   Make sure Use the WSL 2 based engine is checked
   {{% /notice %}}

2. Docker Desktop > Settings > Resources > WSL Integration

- Enable integration with the Linux Distro you just installed.
- Apply & restart

## WSL

- Ubuntu for WSL : Download Ubuntu 22.04.1 LTS from Microsoft apps
- VSC Extensions : WSL, Remote Development
- Adding WSL in VSC : Ctrl+, > wsl > check integrated: use wsl profiles
- Docker : Settings > General > WSL2 based engine | Resources > Ubuntu-22.04 check

## MySQL & MySQL Workbench (Window)

#### Install MySQL Workbench

https://dev.mysql.com/downloads/workbench/

#### Stopping MySQL Server

- Windows key + R
- Type services.msc
- Scroll down until you see MySQL80\
- Click Stop the Service

## PHP in Windows

- Download : https://windows.php.net/download#php-8.2 (thread safe version)
- Check your system architecture : Start menu > Settings > System > About
- Extract file, move it into C:\Program Files
- Copy the full path
- Start menu > Edit the system environment variables > Environment Variables
- System variables > Double click Path > New > add the path
- Open terminal : $ php -v

## PHP in Ubuntu

VSC terminal > Ubuntu

```
$ sudo apt-get update
$ sudo apt-get install -y php8.0
$ php -v
```

## Curl, Wget

WSL Terminal

```
$ sudo apt install curl (yes)
$ curl --help
$ sudo apt-get install wget
$ wget --version
```

## Composer

https://getcomposer.org/download/

## Additional Browser Setup

PHP Browser Extension - Clockwork

https://chrome.google.com/webstore/detail/clockwork/dmggabnehkmmfmdffgajcflpdjlnoemp?hl=en-US

TailwindCSS Dev Tools

https://chrome.google.com/webstore/detail/devtools-for-tailwind-css/mihalpimkkhhigoielhempcamoffhfij

## VSC Extension

https://marketplace.visualstudio.com/items?itemName=shufo.vscode-blade-formatter

# Mac

```
brew install php
php -v
brew install composer
brew install wget ?? sail??
```

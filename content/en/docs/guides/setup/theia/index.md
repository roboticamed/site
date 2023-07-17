---
title: "Theia IDE"
linktitle: "Theia IDE"
date: 2023-07-10
weight: 2
collapsible: false
---

## OS Dependencies

```bash
sudo apt install -y \
    make \
    gcc \
    pkg-config \
    build-essential \
    libx11-dev \
    libxkbfile-dev \
    libsecret-1-dev
```

## NodeJS

```bash
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo bash -
sudo apt install -y nodejs
```

### Yarn

```bash
sudo npm install --global yarn
```


## Theia

```bash
mkdir ~/git
cd ~/git

git clone https://github.com/eclipse-theia/theia

cd theia
yarn
yarn download:plugins
yarn browser build
yarn browser start
```
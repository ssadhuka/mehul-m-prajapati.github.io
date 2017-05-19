---
title: "Building wireshark code using Visual studio 13 on windows"
excerpt: "Do you want to compile whole wireshark source code ?"
category: Compilation
tags: [networking, wireshark]
header:
  overlay_color: "#000"
  overlay_filter: "0.7"
  overlay_image: http://www.hacking-tutorial.com/pics/blog/network-sniffing-use-wireshark/wireshark5.jpg
---

Wireshark is a free and open source packet analyzer. It is used for network troubleshooting, analysis, software and communications protocol development, and education. Originally named Ethereal, the project was renamed Wireshark.

## Steps to compile source code
1. Open cmd.exe as Admin

2. Install Chococlatey

    > @powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

    > choco upgrade chocolatey

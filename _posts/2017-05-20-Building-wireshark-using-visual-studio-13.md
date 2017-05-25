---
title: "Building wireshark code using Visual studio 13 on windows"
excerpt: "Do you want to compile whole wireshark source code ?"
category: Compilation
tags: [networking, wireshark]
header:
  image: https://i.imgur.com/tOdLKhy.png
  overlay_color: "#000"
  overlay_filter: "0.7"
  overlay_image: http://www.hacking-tutorial.com/pics/blog/network-sniffing-use-wireshark/wireshark5.jpg
---

Wireshark is a free and open source packet analyzer. It is used for network troubleshooting, analysis, software and communications protocol development, and education. Originally named Ethereal, the project was renamed Wireshark.

## Steps to compile source code
1. Open cmd.exe as Admin

2. Install Chococlatey package manager  
   
   ```	
> @powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
> choco upgrade chocolatey
   ```

3. Install below packages using Chocolatey

   ```
> choco install VisualStudioCommunity2013
> choco install cygwin
> choco install cyg-get
> cyg-get asciidoc patch docbook-xml45
> choco install python2
> choco install git
> choco install cmake.portable
> choco install winflexbison
   ```

4. Download wireshark source from GitHub and checkout latest version e.g. wireshark-x.x.x

   ```
> cd C:\Development
> git clone https://github.com/wireshark/wireshark
> git checkout wireshark-2.2.6
   ```
   - Close command prompt aka **cmd.exe** :smiley:

5. Install Qt by clicking on below link
   - [Qt](http://info.qt.io/download-qt-for-application-development)
   - Select these components when installing: Qt5.5 -> msvc2013 64-bit, msvc2013 32-bit

6. Setting environment variables

   - Right Click on My Computer --> Properties --> Advanced system settings --> Environment Variables --> Select PATH and append --> ;C:\Qt\5.5\msvc2013\bin
   - Search and open Visual Studio Tools directory
   - Double-click on **VS2013 x86 Native Tools Command Prompt**
   - Execute below commands to setup environment variables (32-bit)

   ```
> set CYGWIN=nodosfilewarning
> set WIRESHARK_BASE_DIR=C:\Development
> set WIRESHARK_TARGET_PLATFORM=win32
> set QT5_BASE_DIR="C:\Qt\5.5\msvc2013"
> set WIRESHARK_VERSION_EXTRA=-mil
> set WIRESHARK_CYGWIN_INSTALL_PATH=c:\tools\cygwin
> mkdir C:\Development\wsbuild32
> cd C:\Development\wsbuild32
   ```

7. Configure wireshark using cmake

   ```
> cmake -DENABLE_CHM_GUIDES=on -G "Visual Studio 12" ..\wireshark
   ```

8. Build source code using MSbuild.exe

   ```
> C:\Program Files (x86)\MSBuild\12.0\Bin\MSBuild.exe" /m /p:Configuration=RelWithDebInfo Wireshark.sln
   ```

Check executable file at: C:\Development\wsbuild32\run\RelWithDebInfo\Wireshark.exe

Reference: [Wireshark Developer Guide](https://www.wireshark.org/docs/wsdg_html_chunked/ChSetupWin32.html)

Cheers ... :whale:

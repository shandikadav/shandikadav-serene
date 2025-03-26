+++
title = "How to Start Developing Mobile Apps on a Low-End Linux Device"
date = 2025-03-26
draft = false

[taxonomies]
categories = ["2025"]
tags = ["flutter", "linux", "low-end"]
+++

{% note(header="Note") %}
It may vary depending on the device, but in this post, I’m using Fedora Workstation 41 as an example. Feel free to adjust it according to your running OS."
{% end %}

{{ figure(src="/images/developing-mobile-linux/header.png", alt="Neofetch", caption="Developing Mobile Apps using Linux") }}

## Introduction

Mobile app development is one of the key futures, especially for those living in Indonesia. In this country, almost all applications rely on mobile platforms, making it highly worthwhile for anyone looking to dive into mobile development.

You might think that mobile development must be done using native platforms like Java or Kotlin. However, the current trend in Indonesia shows that many companies prefer multi-platform frameworks like [Flutter](https://flutter.dev/) or [React Native](https://reactnative.dev/) due to their speed and development efficiency.

One common challenge when learning mobile development is the difficulty of working on a low-end device. I’ve personally experienced this as well since I’m using an older-generation device with 12GB of RAM and a 6th-gen Intel i3 mobile CPU.

{{ figure(src="/images/developing-mobile-linux/fastfetch.png", alt="Fast Fetch", caption="Running Fast Fetch") }}

I've faced many challenges in developing mobile apps on my laptop. I've also conducted numerous experiments, both when I was using Windows OS and now on Linux. Additionally, I’ve tried optimizing my device to ensure a smoother development experience with Flutter. This time, I’ll share some tips to help you develop mobile apps more comfortably. 

I've divided this into three categories: **must-do**, **recommended**, and **avoid at all costs**. We'll start with the things you **shouldn't do** first.

---

## Avoid at All Costs

### Don't Use Windows

The first thing to avoid on a low-end device is **don’t use Windows!** Get rid of that OS and install Linux instead. There are plenty of tutorials online on how to install Linux. For beginners, recommended distros include **Ubuntu** or **Pop!_OS**. Even better, use **Fedora**, as that’s what I’m using—making it easier for you to follow along with my setup.

The reason I recommend using **Linux** is that **Windows comes with a lot of bloatware**, consuming a significant portion of your RAM even when idle. On the other hand, **Linux excels in memory management**, allowing you to multitask without worrying about your laptop freezing or slowing down.

---

## Must Do

### Install Linux
The first thing you need to do is **install Linux**. You can choose from the distros I recommended earlier. I won’t go into the details of how to install Linux, as there are plenty of guides available online.

However, before making the switch, **consider the applications you frequently use on Windows** and check if there are alternative versions available on Linux. If suitable alternatives exist, you can proceed with the installation confidently.

I have created a list of commonly used applications on Windows along with their alternatives on Linux. You can check the table below:

| Category         | Windows                          | Linux (Alternate)                  |
|-----------------|--------------------------------|------------------------------------|
| OS             | Windows 10/11                  | Ubuntu, Fedora, Pop!_OS, Arch     |
| Browser        | Google Chrome, Edge            | Firefox, Brave, Chromium         |
| Office Suite   | Microsoft Office (Word, Excel) | LibreOffice, OnlyOffice, WPS Office, Google Workspace |
| Notepad        | Notepad++                      | Kate, Gedit, Mousepad, Vim       |
| File Manager   | Windows Explorer               | Dolphin, Nautilus, Thunar        |
| Image Editor   | Adobe Photoshop                | GIMP, Krita, Inkscape           |
| Video Editor   | Adobe Premiere Pro             | Kdenlive, OpenShot, DaVinci Resolve |
| PDF Reader     | Adobe Acrobat Reader           | Okular, Evince, Foxit Reader     |
| IDE/Code Editor| Visual Studio, VS Code        | VS Code, JetBrains Rider, VSCodium, Neovim |
| Media Player   | VLC, Windows Media Player     | VLC, MPV, SMPlayer              |
| Virtualization | VMware, VirtualBox            | VirtualBox, KVM, GNOME Boxes     |
| Download Manager | IDM (Internet Download Manager) | uGet, Xtreme Download Manager (XDM), aria2 |
| Terminal       | Command Prompt, PowerShell     | GNOME Terminal, Konsole, Alacritty, Tilix |
| Cloud Storage  | OneDrive, Google Drive        | Mega, pCloud, Nextcloud         |
| Game Launcher  | Steam, Epic Games Launcher    | Steam (Proton), Lutris, Heroic Games Launcher |
| Screen Recorder | OBS Studio, Bandicam         | OBS Studio, SimpleScreenRecorder |
| Audio Editor   | Audacity, Adobe Audition      | Audacity, Ardour, LMMS          |
| Disk Partition | Disk Management, EaseUS       | GParted, KDE Partition Manager  |
| Torrent Client | uTorrent, BitTorrent          | qBittorrent, Transmission       |
| Password Manager | LastPass, Bitwarden         | Bitwarden, KeePassXC           |

### Install VS Code

After successfully installing Linux, the next **step is to set up your development tools** for mobile app development. Start by installing VS Code as your text editor. You can download VS Code from the official website [here](https://code.visualstudio.com/).

Alternatively, you can download it via [Flatpak](https://flatpak.org/):

```bash
flatpak install flathub com.visualstudio.code
```

After installing VS Code, you need to install the Flutter extension from the Extensions menu

{{ figure(src="/images/developing-mobile-linux/flutter-extension.png", alt="flutter extension", caption="Flutter Extension on VS Code") }}

When you install the Flutter extension, the Dart extension will be installed automatically, so you don’t need to worry about installing them separately.

### Install Android Studio

Once the installation is successful, the next step is to install Android Studio. We won’t be using Android Studio for development, but rather to install the Android SDK. You can download it from the official website [here](https://developer.android.com/studio?hl=id).

Alternatively, you can download it via Flatpak:
```bash
flatpak install flathub com.google.AndroidStudio
```

After installation, follow the images below for the necessary SDK settings you need to configure:

{{ figure(src="/images/developing-mobile-linux/sdk-manager.png", alt="sdkmanager", caption="SDK Manager on Android Studio") }}

### Install JDK

After successfully installing Android Studio and the Android SDK, the next step is to install OpenJDK on your device.

For Fedora users, simply run the following command:
```bash
sudo dnf install java-17-openjdk java-17-openjdk-devel
```

After successfully downloading, you can verify the installation by running the following command:
```bash
java --version
```

### Install Flutter

Next, we will install **Flutter** on your device. You can download and install Flutter from their [official website](https://docs.flutter.dev/get-started/install/linux/android). Simply follow the installation guide in their documentation carefully, and Flutter should be successfully installed on your device.

After completing the installation, open your terminal and run the following command to verify the installation:

```bash
flutter doctor -v
```

{{ figure(src="/images/developing-mobile-linux/flutter-doctor.png", alt="flutter doctor", caption="Flutter Doctor") }}

If you encounter the error '**You have not accepted the license agreements**', you can resolve it by running the following command:

```bash
flutter doctor --android-licenses
```

### Install Emulator

Once VS Code and Flutter are installed, you can proceed to download an emulator. Before that, make sure to check whether your device supports emulation. For example, my device uses an Intel processor with Hyper-V, which allows for emulation. You may need to explore this on your own.

You can use the AVD (Android Virtual Device) built into Android Studio, but I highly recommend Genymotion because it is lighter than AVD. This emulator works well even on low-RAM devices—I used it when I only had 8GB of RAM.

You can download Genymotion [here](https://www.genymotion.com/).

Genymotion requires VirtualBox to run. After installing Genymotion, you can install VirtualBox using the following command:

```bash
sudo dnf install virtualbox
```

---

## Recommended

### Using Zed

{{ figure(src="/images/developing-mobile-linux/zed.png", alt="zed", caption="Zed Text Editor") }}

To save even more memory, you can use **Zed Text Editor**. Built on Rust, Zed is incredibly fast and efficient in CPU usage. You can download it from [this link](https://zed.dev/).

Alternatively, you can download it via Flatpak:
```bash
flatpak install flathub dev.zed.Zed
```

### Running via Real Device

You can save memory by using your smartphone for debugging instead of an emulator. You'll need a USB cable and must enable USB Debugging on your smartphone. To make development easier, you can use scrcpy for screen mirroring. You can download it from their GitHub repository [here](https://github.com/Genymobile/scrcpy).

### Don't Multitasking!

To save CPU and RAM, avoid multitasking while developing mobile apps. Running an emulator and performing debugging require significant CPU and RAM resources, which can heavily impact your device's performance. By avoiding multitasking, you can free up system resources, making the development process smoother and more efficient.

### Using XFCE or LXQT Desktop Environments

To further minimize RAM usage, consider using a lightweight desktop environment (DE) such as XFCE or LXQt, which are known for their low memory consumption.

You can look up installation guides for your specific distro. If you're using Fedora, you can opt for Fedora Spins, which come pre-configured with these lightweight DEs. You can find them [here](https://fedoraproject.org/spins).

---

## Conclusion

After following the steps above, I hope this guide helps you develop mobile apps, especially using Flutter, more smoothly. If you have any further questions, feel free to contact me via LinkedIn or Instagram.


# debMyRepo
[![debMyRepo](https://img.shields.io/badge/Project-debMyRepo-green.svg)](https://github.com/kariminf/KLnxScr/debMyRepo)
[![License](https://img.shields.io/badge/License-Apache--2.0-green.svg)](http://www.apache.org/licenses/LICENSE-2.0)
[![Version](https://img.shields.io/badge/Version-1.1.0-green.svg)](https://launchpad.net/~kariminf/+archive/ubuntu/ppa)

Some scripts to help you make a local repository for debian packages.

## debdist

This script helps to distribute debian files into subfolders using their names.
The script will create a folder "others" for packages coming from "getdeb" and "wenupd8"
and a folder for main packages.
Inside each one, it will distribute the files using the first character then using the name.

### Use

```
debdist folder
```
* folder: The folder where the deb files are.
It must end with a "/"

Ps. If the file is not executable, you can call it like:
```
bash debdist folder
```
Or change its access permissions

### Example
Suppose we have a folder like this:
```
.
└── trusty
    ├── atom_1.10.1-1~webupd8~0_amd64.deb
    ├── avidemux-plugins-common_1%3a2.5.4-0ubuntu14_amd64.deb
    ├── avidemux-plugins-qt_1%3a2.5.4-0ubuntu14_amd64.deb
    ├── libanthy0_9100h-23ubuntu2_amd64.deb
    ├── libreoffice-base_1%3a5.0.3~rc2-0ubuntu1~trusty2_amd64.deb
    └── mozc-server_1.13.1651.102-2_amd64.deb
```
When we execute the instruction:
```
debdist trusty/
```
We will get:
```
.
└── trusty
    ├── main
    │   ├── a
    │   │   ├── anthy0
    │   │   │   └── libanthy0_9100h-23ubuntu2_amd64.deb
    │   │   └── avidemux
    │   │       ├── avidemux-plugins-common_1%3a2.5.4-0ubuntu14_amd64.deb
    │   │       └── avidemux-plugins-qt_1%3a2.5.4-0ubuntu14_amd64.deb
    │   ├── l
    │   │   └── libreoffice
    │   │       └── libreoffice-base_1%3a5.0.3~rc2-0ubuntu1~trusty2_amd64.deb
    │   └── m
    │       └── mozc
    │           └── mozc-server_1.13.1651.102-2_amd64.deb
    └── others
        └── a
            └── atom
                └── atom_1.10.1-1~webupd8~0_amd64.deb
```

## debpack

This script helps to create a local repository by creating the file "Packages" and delete the duplicate packages.

### Use

```
debpack src-folder dest-folder
```
* src-folder: The folder where the deb files are. It must end with a "/"
* dest-folder: The folder where we put the file "Packages". It must end with a "/"

Ps. If the file is not executable, you can call it like:
```
bash debpack src-folder dest-folder
```
Or change its access permissions

### Example
In our last example, we distributed the files.
In the same directory we will execute these two commands:
```
debpack trusty/main/ dists/trusty/main/binary-amd64/
debpack trusty/others/ dists/trusty/others/binary-amd64/
```

## How to add the repository?
We will follow the precedent example.
Suppose we put the folders "trusty" and "dists" in a folder inside HOME directory like this:
```
/home/kariminf/repo/

repo
├── dists
│   └── trusty
│       ├── main
│       │   └── binary-amd64
│       └── others
│           └── binary-amd64
└── trusty
    ├── main
    │   ├── a
    │   │   ├── anthy0
    │   │   │   └── libanthy0_9100h-23ubuntu2_amd64.deb
    │   │   └── avidemux
    │   │       ├── avidemux-plugins-common_1%3a2.5.4-0ubuntu14_amd64.deb
    │   │       └── avidemux-plugins-qt_1%3a2.5.4-0ubuntu14_amd64.deb
    │   ├── l
    │   │   └── libreoffice
    │   │       └── libreoffice-base_1%3a5.0.3~rc2-0ubuntu1~trusty2_amd64.deb
    │   └── m
    │       └── mozc
    │           └── mozc-server_1.13.1651.102-2_amd64.deb
    └── others
        └── a
            └── atom
                └── atom_1.10.1-1~webupd8~0_amd64.deb
```
In your software sources manager, add this repo:
```
deb file://///home/kariminf/repo/ trusty main others
```
Then, update.
You may see an error saying that your repo hasn't a "Release" file, ignore it.


## Dependency
This program depends on these packages:
* bash
* dpkg-dev: for "dpkg-scanpackages"

## License
The code is released under Apache 2.0 license.
For more details about this license, check [LICENSE](../LICENSE) file

# debMyRepo

[![debMyRepo](https://img.shields.io/badge/Project-debMyRepo-green.svg)](https://github.com/kariminf/kas/debMyRepo)
[![License](https://img.shields.io/badge/License-Apache--2.0-green.svg)](http://www.apache.org/licenses/LICENSE-2.0)
[![Version](https://img.shields.io/badge/Version-1.2.0-green.svg)](https://launchpad.net/~kariminf/+archive/ubuntu/ppa)

Some scripts to help you make a local repository for debian packages.


## debdist

Distribute debian files of a given folder (distribution).
First, it puts debian files into some other folders (components);
**main** is the default, and other components are specified in a file.
Then, it distributes debian files into sub-folders using their names first characters.
Inside each one, it will distribute the files using their names.

Suppose you have a repo in *~/repo/* and a distribution in *~/repo/xenial64/*.
You put all your **.deb** files in there.
If you want more components than **main** in your distribution, just add a file
*~/repo/xenial64/comp.list* like this:

```
getdeb others
webupd8 others
```

When the script finds a debian file name containing either "getdeb" or "webupd8", it will be put in "others".
The rest of files goes to "main" folder.

### Use

Enter your repository folder, then call debdist with

```
cd repo
debdist folder
```
* folder: The folder where the deb files are.

Ps. If the file is not executable, you can call it like:
```
bash debdist folder
```
Or change its access permissions

### Example
Suppose we have a folder like this:
```
.
└── xenial64
    ├── atom_1.10.1-1~webupd8~0_amd64.deb
    ├── avidemux-plugins-common_1%3a2.5.4-0ubuntu14_amd64.deb
    ├── avidemux-plugins-qt_1%3a2.5.4-0ubuntu14_amd64.deb
    ├── comp.list
    ├── libanthy0_9100h-23ubuntu2_amd64.deb
    ├── libreoffice-base_1%3a5.0.3~rc2-0ubuntu1~trusty2_amd64.deb
    └── mozc-server_1.13.1651.102-2_amd64.deb
```
When we execute the instruction:
```
debdist xenial/
```
We will get:
```
.
└── xenial
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
    ├── others
    │   └── a
    │       └── atom
    │           └── atom_1.10.1-1~webupd8~0_amd64.deb
    └── comp.list
```

## debpack

This script is used to create a local repository by creating the file "Packages" and delete the duplicate packages.

### Use

```sh
debpack dist-folder dist-name
```

* dist-folder: The folder where the deb files are.
* dest-folder: The folder where we put the file "Packages". It must end with a "/"

Ps. If the file is not executable, you can call it like:

```sh
bash debpack src-folder dest-folder
```
Or change its access permissions

### Example

In our last example, we distributed the files.
In the same directory we will execute these two commands:

```sh
cd ~/repo/
debpack xenial64/ xenial
```

This will create four files (in our example):

* ~/repo/dists/xenial/main/binary-amd64/Packages.gz
* ~/repo/dists/xenial/others/binary-amd64/Packages.gz
* ~/repo/dists/xenial/Release
* ~/repo/dists/xenial/InRelease

## How to use the repository locally?

We will follow the precedent example.
Suppose we put the folders "trusty" and "dists" in a folder inside HOME directory like this:
```
~/repo/

repo
├── dists
│   └── xenial
│       ├── main
│       │   └── binary-amd64
│       └── others
│           └── binary-amd64
└── xenial64
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
deb file:////~/repo/ xenial main others
```

Then, update.
You may see an error saying that your repo hasn't a "Release" file, ignore it.


## Dependencies

This program depends on these packages:
* bash >= 4
* dpkg-dev: for "dpkg-scanpackages"
* gzip
* apt-utils: for apt-ftparchive


## Download

On ubuntu (or its derivatives), you can add the repository:

```
sudo add-apt-repository ppa:kariminf/ppa
sudo apt-get update
```

Or add it manually:

```
deb http://ppa.launchpad.net/kariminf/ppa/ubuntu YOUR_UBUNTU_VERSION_HERE main
```

Update the cache, then install:

```
sudo apt-get install debmyrepo
```

## License

Copyright (C) 2016-2018 Abdelkrime Aries

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

# Part 1: Understanding APT & System Updates (15 min)
   **1.Check your system’s APT version:**

Run the following command to display the installed APT version:
```sh
apt --version
#output:  apt --version
apt 2.4.13 (amd64)
```

  **2.Update the package list:**
I Runned the command:
```sh
sudo apt update 
#Output: Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
20 packages can be upgraded. Run 'apt list --upgradable' to see them.
```
I runned again:
```sh
 apt list --upgradable

 #output:
Listing... Done
bind9-dnsutils/jammy-updates,jammy-security 1:9.18.30-0ubuntu0.22.04.2 amd64 [upgradable from: 1:9.18.30-0ubuntu0.22.04.1]...
```

**Explanation:**
  *This command updates the local package index by retrieving the latest package information from the repositories. It ensures that when we install or upgrade packages, we get the latest versions available.*

   **3.Run:**
```sh
sudo apt upgrade -y
#Output:  sudo apt upgrade -y
[sudo] password for kolov:
Reading package lists... Done
Building dependency tree... Done 
```
- Difference Between Update and Upgrade:

*Update refreshes the package index but does not install updates.*

*Upgrade installs the latest versions of installed packages.*

![Screenshot 2025-02-06 223844](https://github.com/user-attachments/assets/4790df33-e54c-45fb-a70b-587d174566a2)

# Part 2: Installing & Managing Packages (20 min)
   **5.Search for a package using APT:**

- Find an image editor using:
```sh
apt search image editor
```
### Selected Package: gimp
   **6.View package details:**

View Package Details
Command:
```sh
apt show gimp
```
What dependencies does it require?
*Depends: libgimp2.0 (>= 2.10.30), gimp-data (>= 2.10.30), ...*

   **7.Install the package:**
```sh
sudo apt install gimp -y
``` 

**Output:**
```sh
...Processing triggers for man-db (2.10.2-1) ...
Processing triggers for fontconfig (2.13.1-4.2ubuntu5) ...
Processing triggers for hicolor-icon-theme (0.17-2) ...
Processing triggers for libglib2.0-0:amd64 (2.72.4-0ubuntu2.4) ...
Processing triggers for libc-bin (2.35-0ubuntu3.9) ...
/sbin/ldconfig.real: /usr/lib/wsl/lib/libcuda.so.1 is not a symbolic link
```

   **8.Check installed package version:**
```sh
apt list --installed | grep gimp
```
  **Output:**
```sh
gimp-data/jammy-updates,jammy-security,now 2.10.30-1ubuntu0.1 all [installed,automatic]
gimp/jammy-updates,jammy-security,now 2.10.30-1ubuntu0.1 amd64 [installed]
libgimp2.0/jammy-updates,jammy-security,now 2.10.30-1ubuntu0.1 amd64 [installed,automatic]
```
# Part 3: Removing & Cleaning Packages (10 min)


   **9.Uninstall the Package** 
```sh
sudo apt remove gimp -y
```

**Output:**
```sh
After this operation, 20.7 MB disk space will be freed.
(Reading database ... 50658 files and directories currently installed.)
Removing gimp (2.10.30-1ubuntu0.1) ...
Processing triggers for man-db (2.10.2-1) ...*
*Is the package fully removed?* 
**No, configuration files still exist.
```
![Screenshot 2025-02-06 224026](https://github.com/user-attachments/assets/1955c441-411f-4c70-92a1-4440930734f5)

   **10.Remove configuration files as well:**
```sh
sudo apt purge gimp -y
```
**Output:**
```sh
sudo apt remove gimp -y
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Package 'gimp' is not installed, so not removed
```
**Difference Between Remove and Purge:**

*Remove deletes the package but keeps configuration files.*
*Purge removes the package and all its associated configuration files.*

**11.Clear unnecessary package dependencies:**

```sh
sudo apt autoremove -y
```
**Output:**
```sh
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following packages will be REMOVED:
  libgimp2.0 libc6 libglib2.0-0 ...
  ```

- Why is this step important?
**Because This step removes unused dependencies that were installed automatically but are no longer needed, freeing up disk space.**

 **12.Clean up downloaded package files:**

```sh
sudo apt clean
```
*What does this command do?*
**No output is displayed, but the command clears the local repository of retrieved package files. It removes cached .deb files from /var/cache/apt/archives/, freeing up disk space.**

# Part 4: Managing Repositories & Troubleshooting (15 min)
**13.List all APT repositories:**
```sh
cat /etc/apt/sources.list
```
- Output:
```sh
 cat /etc/apt/sources.list
# See http://help.ubuntu.com/community/UpgradeNotes for how to upgrade to
# newer versions of the distribution.
deb http://archive.ubuntu.com/ubuntu/ jammy main restricted
# deb-src http://archive.ubuntu.com/ubuntu/ jammy main restricted
deb http://archive.ubuntu.com/ubuntu/ jammy-updates main restricted
```
- What do you notice in this file?
**The file contains URLs for the main Ubuntu repositories, including jammy-security main restricted,  universe, multiverse and its updates.**

**14.Add a new repository (example: universe repository):**
```sh
sudo add-apt-repository universe
sudo apt update
```
*Output:*
```sh
Hit:1 http://security.ubuntu.com/ubuntu jammy-security InRelease
Hit:2 http://archive.ubuntu.com/ubuntu jammy InRelease
Hit:3 http://archive.ubuntu.com/ubuntu jammy-updates InRelease
Hit:4 http://archive.ubuntu.com/ubuntu jammy-backports InRelease
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
All packages are up to date.
```
  *What types of packages are found in the universe repository?* 
  **The universe repository contains community-maintained open-source software that is not officially supported by Ubuntu.** 
  
![Screenshot 2025-02-06 225114](https://github.com/user-attachments/assets/aa1cf7b8-0fd9-4453-b462-8f60e595cb4f)

**15.Simulate an installation failure and troubleshoot:**

Try installing a non-existent package:
```sh
sudo apt install fakepackage
```
**What error message do you get?** *The error message is E: Unable to locate package fakepackage*
```sh
>>sudo apt install fakepackage: 
[sudo] password for kolov:
Reading package lists... Done

Building dependency tree... Done
Reading state information... Done
E: Unable to locate package fakepackage
```
**How would you troubleshoot this issue?**
*Verify the package name is correct.
Ensure the repository containing the package is enabled (sudo apt update).
Search for the package using apt search fakepackage.*

# Bonus Challenge (Optional):
```sh
sudo apt-mark hold gimp
Output: gimp set on hold.
```
**To Unhold a package**
```sh
sudo apt-mark unhold gimp
``` 
**Why would you want to hold a package?**
*Holding a package prevents it from being automatically upgraded, which is useful for maintaining a specific version of a package for compatibility or stability reasons.*

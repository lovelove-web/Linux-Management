# 27-01-2025 

Descriptions: 

1- Create the Tupu User
I run the adduser script to create the user tupu:

```python
sudo adduser tupu
```

The script prompts me to input details for the user (e.g., password, full name, etc.).
This automatically creates:
A home directory at /home/tupu.
A primary group named tupu.

2. Create the Lupu User
Use the useradd command to create a user lupu with similar properties as tupu:
```Python
sudo groupadd lupu
sudo useradd -m -d /home/lupu -s /bin/bash -g lupu lupu
```

NB: if I runned this """sudo useradd -m -d /home/lupu -s /bin/bash -G lupu lupu""" the system complained many times that the group does not exist. Thats why I run it with small -g

3. Create the Hupu System User
To create the system user hupu with a restricted login shell:

```Python 
sudo useradd --system --shell /bin/false hupu
Options explained:
```
So that the   --system: Creates a system account and   --shell /bin/false: Sets the login shell to /bin/false, preventing the user from logging in.

See below is the screenshoot of the tasks 1-3:

![to git123](https://github.com/user-attachments/assets/59617baf-f735-46f0-b5ad-b9fcda10bebc)


4. Add Tupu and Lupu to the Sudo Group
I runned the following script and add those lines to assign tupu and lupu to the sudo group, allowing them administrative privileges.

```Python
sudo visudo
```
Add the following lines:

tupu ALL=(ALL:ALL) ALL
lupu ALL=(ALL:ALL) ALL

![visudo pic](https://github.com/user-attachments/assets/c79fdc98-520a-40a5-b504-31f2920d56bf)


5. Create and Configure the /opt/projekti Directory

- I Create the /opt/projekti Directory

```Python
sudo mkdir /opt/projekti
```

Then I created the projekti group

```Python
sudo groupadd projekti
```

and assign the projekti group as the owner of /opt/projekti:

```Python 
sudo chown :projekti /opt/projekti
```

and I set Directory Permissions to ensure both tupu and lupu have appropriate permissions:

```Python
sudo chmod 775 /opt/projekti
```

Permissions breakdown:
7 (Owner): Full access (read, write, execute).
7 (Group): Full access (read, write, execute).
5 (Others): Read and execute only.

then I added Tupu and Lupu to the projekti Group

```Python
sudo usermod -aG projekti tupu
sudo usermod -aG projekti lupu
```

and finally I set the Set gid Bit to ensure that files created in /opt/projekti inherit the projekti group:
```Python
sudo chmod u+x /opt/projekti
```
and then Verify Configuration by checking Directory Ownership and Permissions:

```Python
ls -ld /opt/projekti
```

Example of output:    drwxrwxr-x 2 root projekti 4096 Jan 27 15:59 /opt/projekti

Here I try to create a file for both users. Test File Creation by Tupu: Switch to the tupu user:

```Python
su - tupu
```

enter the password and then create a file:

```Python
touch /opt/projekti/testfile
ls -l /opt/projekti
```

Do the same the Lupu user if I want to create a file.
Done

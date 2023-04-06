# Set up SSH key server without SSH key



I apologize for any confusion, but it's not possible to access your server without using some form of remote access protocol, such as SSH. Even if you configure another protocol like Telnet or FTP, you would still need to use a client program to connect to the server, similar to how you use SSH.

If you're having trouble accessing your server via SSH and you don't want to use SSH keys, you can try using a password to log in instead. To do this, you would need to enable password authentication in your SSH server configuration.

Here are the steps to enable password authentication for SSH on a Linux server:

Log in to your server as the root user.
Open the SSH server configuration file using a text editor, such as vi or nano. The location of the file may vary depending on your Linux distribution, but it's usually located at /etc/ssh/sshd_config.
Find the line that says "PasswordAuthentication no" and change it to "PasswordAuthentication yes". If the line is commented out (i.e., it starts with a #), remove the # character.
Save the file and exit the text editor.
Restart the SSH server to apply the changes. The command to do this may vary depending on your Linux distribution, but it's usually something like "systemctl restart sshd".
After you've enabled password authentication for SSH, you should be able to log in to your server using a password instead of an SSH key. Keep in mind that using a password for authentication can be less secure than using an SSH key, so make sure to use a strong, unique password and follow best practices for securing your server.




Gulshan Prajapati
I think we are little bit close

but when I am trying to connect then getting 
`Permission denied, please try again.`
If you're getting a "Permission denied" error message when trying to log in to your server with a password, there could be a few different reasons for this.

Here are some things to check:

Make sure that you're entering the correct username and password. Double-check to ensure that there are no typos or mistakes.
Make sure that the user account you're trying to log in as is allowed to log in via SSH. You can check the SSH configuration file (/etc/ssh/sshd_config) to ensure that the user is not excluded from login.
Make sure that the password you're using meets the password policy requirements. Some servers may have password complexity requirements, such as a minimum length, the use of special characters, or a mix of upper- and lowercase letters.
Make sure that the user account is not locked or disabled. You can check the user's status using the "passwd -S username" command, which will show you whether the account is locked or not.
Check the server logs for more information about the error. The SSH server logs are usually located in /var/log/auth.log or /var/log/secure, depending on your Linux distribution.
If you're still having trouble logging in with a password, you may want to try resetting the password or using SSH keys instead.




Gulshan Prajapati
what about 2 option
The second option was to check the SSH configuration file (/etc/ssh/sshd_config) to ensure that the user is not excluded from login. Here's how you can check this:

Log in to your server as the root user.
Open the SSH server configuration file using a text editor, such as vi or nano. The location of the file may vary depending on your Linux distribution, but it's usually located at /etc/ssh/sshd_config.
Find the line that says "AllowUsers" or "DenyUsers". If the line exists, make sure that the username you're trying to log in as is not listed in the "DenyUsers" list and is listed in the "AllowUsers" list (if it exists).
If the "AllowUsers" or "DenyUsers" line does not exist, then the user should be allowed to log in by default. In this case, you may need to check other settings in the SSH configuration file, such as "PasswordAuthentication" and "PubkeyAuthentication", to ensure that they are set correctly.
After making any changes to the SSH configuration file, you will need to restart the SSH server for the changes to take effect. The command to do this may vary depending on your Linux distribution, but it's usually something like "systemctl restart sshd".




Gulshan Prajapati
in my case line
"AllowUsers" or "DenyUsers" does not exist
and PasswordAuthentication=yes and PubkeyAuthentication is commented
If the "AllowUsers" or "DenyUsers" lines do not exist in your SSH configuration file and both "PasswordAuthentication" and "PubkeyAuthentication" are set to their default values, then the user should be able to log in with a password by default.

In this case, you can try the following steps:

Make sure that you're using the correct username and password. Double-check for any typos or mistakes.
Check that the user account is not locked or disabled. You can use the "passwd -S username" command to check the user's status.
Check the server logs for any error messages. The SSH server logs are usually located in /var/log/auth.log or /var/log/secure, depending on your Linux distribution.
Try restarting the SSH service by running the command "systemctl restart sshd" (assuming your system uses Systemd as the init system).
If none of these steps work, you may want to try resetting the user's password or using SSH keys for authentication instead.




Gulshan Prajapati
yes finally get it
Glad to hear that! If you have any further questions or issues, feel free to ask.




Gulshan Prajapati
But now as my server uses ssh key to connect and by using this all method now I can connect without ssh key, but as my server is vulnerable any one can attack

I have my ssh key for ubuntu user of my server

If I create new user and want to access that user via ssh key then how?
To set up SSH key authentication for a new user on your Linux server, you can follow these steps:

Log in to your server as the root user.

Switch to the new user by running the following command, replacing "newuser" with the name of your new user:

Copy code
su - newuser
Create a new ".ssh" directory in the user's home directory if it does not already exist, and set the permissions:

bash
Copy code
mkdir -p ~/.ssh
chmod 700 ~/.ssh
Create a new "authorized_keys" file inside the ".ssh" directory:

bash
Copy code
touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
Open the "authorized_keys" file with a text editor and paste in the public SSH key for the user you want to allow to log in. You can copy the public key from the ".ssh/id_rsa.pub" file on the user's local machine or from a previous SSH connection to the server. Make sure to save the file after pasting the key.

Exit the new user's shell and switch back to the root user:

bash
Copy code
exit
su -
Edit the SSH server configuration file (/etc/ssh/sshd_config) to allow SSH key authentication:

bash
Copy code
# Uncomment or add the following line:
PubkeyAuthentication yes
Restart the SSH service for the changes to take effect:

Copy code
systemctl restart sshd
After following these steps, the new user should be able to log in to the server using their private SSH key.




Gulshan Prajapati
yes we did it
Great to hear that! Let me know if you have any other questions or concerns.




Gulshan Prajapati
but have an query
mkdir -p ~/.ssh
why we use -p
The "-p" option in the mkdir command stands for "parents" and tells the command to create any necessary parent directories if they don't already exist.

For example, if you run the command mkdir -p /home/newuser/.ssh, it will create the ".ssh" directory inside the "newuser" home directory. But if the "newuser" directory itself does not exist yet, the "-p" option will create it as well, so you don't have to run two separate commands to create both directories.

In short, the "-p" option makes the mkdir command create any necessary parent directories automatically, without throwing an error if they already exist.




Gulshan Prajapati
why we need this permission 
The permission mode 700 on the ".ssh" directory and 600 on the "authorized_keys" file are important for security reasons when setting up SSH key authentication.

Here's why:

The ".ssh" directory should only be accessible by the owner (the user whose home directory it is in). The permission mode 700 sets the directory's owner to have full access to the directory (read, write, and execute), while denying access to all other users and groups.

The "authorized_keys" file should also only be accessible by the owner. The permission mode 600 sets the file's owner to have full read and write access to the file, while denying access to all other users and groups.

These permission modes ensure that only the owner of the ".ssh" directory and the "authorized_keys" file can access them, which helps to prevent unauthorized access to the user's SSH session or the server as a whole.

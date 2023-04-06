# Set up SSH key server without SSH key


### Here are the steps to enable password authentication for SSH on a Linux server:

- Log in to your server as the root user.
- Open the SSH server configuration file using a text editor, such as vi or nano. The location of the file may vary depending on your Linux distribution, but it's usually located at `/etc/ssh/sshd_config`
- Find the line that says `"PasswordAuthentication no"` and change it to `"PasswordAuthentication yes"`
- If the line is commented out (i.e., it starts with a #), remove the # character.
- Find the line that says "AllowUsers" or "DenyUsers". 
- If not found then leave it
- If found, `make sure that the username you're trying to log in as is not listed in the "DenyUsers" list and is listed in the "AllowUsers" list (if it exists)` `e.g AllowUsers root newuser anotheruser`
- Save the file and exit the text editor.
- Restart the SSH server to apply the changes. `sudo systemctl restart sshd`
- After you've enabled password authentication for SSH, you should be able to log in to your server using a password instead of an SSH key. Keep in mind that using a password for authentication can be less secure than using an SSH key, so make sure to use a strong, unique password and follow best practices for securing your server.



# Set up New user with SSH key


- Log in to your server as the root user.
- Create a new ".ssh" directory in the user's home directory if it does not already exist, and set the permissions:


`mkdir -p ~/.ssh
chmod 700 ~/.ssh`

- Create a new "authorized_keys" file inside the ".ssh" directory:


`touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys`

- Open the "authorized_keys" file with a text editor and paste in the public SSH key for the user you want to allow to log in. You can copy the public key from the ".ssh/id_rsa.pub" file on the user's local machine or from a previous SSH connection to the server. Make sure to save the file after pasting the key.


- Edit the SSH server configuration file (/etc/ssh/sshd_config) to allow SSH key authentication:
- Uncomment or add the following line: `PubkeyAuthentication yes`
- Restart the SSH service `sudo systemctl restart sshd`


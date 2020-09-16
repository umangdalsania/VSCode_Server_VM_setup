#VSCode_Server_VM_setup

##Part 1 (Get VM ready):

##### A. Configuring Ubuntu VM Network setting on Virtual Box
1. Select VM and go to 
    ```sh
    "Settings" > "Network"
    ```
<!-- FIXME: Need Picture -->
2. Now click on **`Adapter 1`**:
    ```sh
    1. Enable Network Adapter
    2. Set "Attached to" to "NAT"
    ```
<!-- FIXME: Need Picture -->
3. Now click on **`Adapter 2`**:
    ```sh
    1. Enable Network Adapter
    2. Set "Attached to" to "Host-only Adapter"
    ```
<!-- FIXME: Need Picture -->
4. Press `Ok` to save this settings

##### B. Set Host-only Adapter ip on VM
1. Boot-Up Ubuntu VM and open terminal:
    ```sh
    $ ip a
    ```
2. Note down **ip** of adapter 3. 


##Part 2 (Get ssh client and server):
* On your local Computer:
    1. open VS Code
    2. go to extensions, search and install: 
        <!-- FIXME: Need Picture or info about package name -->
        ```sh
        1. ms-vscode-remote.remote-ssh
        2. ms-vscode-remote.remote-ssh-edit
        ```

*  On Ubuntu VM:
    1. open terminal and type:
        ```sh
        sudo apt install -y openssh-server
        ```
    2. check the status of ssh by:
        ```sh
        systemctl status sshd
        ```
        * if **Active**:
            <!-- FIXME: Need Picture -->
            ```sh
            Active: active (running) 
            
            Press "q" to exit
            ```
        
        * if **Not active**, then run:
            ```sh
            sudo systemctl restart ssh
            ```
            This will restart the ssh server on Ubuntu.
        

##Part 3 (Create SSh key on Windows/Mac and copy public key to Ubuntu):

##### Create private and Public key:

* For macOS / Linux: Run the following command in a local terminal:
    ```sh
    ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa-remote-ssh
    ```
* For Windows: Run the following command in a local PowerShell:
    ```sh
    ssh-keygen -t rsa -b 4096 -f "$HOME\.ssh\id_rsa-remote-ssh"
    ```

    * On Windows, You may need to change the permission of key; therefore, navigate to:
        ```sh
        C:\Users\<your_username>\.ssh\
        ```
        Right click on "**id_rsa-remote-ssh.pub**" file and select 
        ```sh
        "Properties" > "Security" > "Edit"
        ```
        Here select "**Everyone**" and press `Remove`, followed by "**Apply**" and "**Ok**" to save these changes.


##### Get and Copy Public key:
* On your local Computer:
    * For macOS / Linux, Copy contant of:
        ```sh
        $HOME/.ssh/id_rsa-remote-ssh.pub
        ```
    * For Windows, Copy the content of:
        ```sh
        C:\Users\<your_username>\.ssh\id_rsa-remote-ssh.pub
        ```
* On Ubuntu VM, open terminal:
    <!-- FIXME: Need more info on how to use -->
    ```sh
    touch $HOME\.ssh\authorized_keys
    ```
    Paste key contant here in "**authorized_keys**" file
    Save file and exit using `:wq!`

##Part 4 (setting up Config for VS Code):
* On your local Computer:
    * Open VS Code
    <!-- FIXME: Need Picture of icon-->
    * look for Remont Connect icon which is blue icon with "><" in it
    * Click on it and select:
        ```sh
        "remote-SSH: Open Configuration FIle..." 
        ```
    
    * Than Click on "**Settings**"

    * Now in "Remote.SSH: Config File":
        * For macOS / Linux type:
            ```sh
            $HOME/.ssh/config
            ```
            Save these changes using "CMD + s"?

        * For Windows type:
            ```sh
            C:\\Users\\<your_username>\\.ssh\\config
            ```
            Save these changes using "Ctrl + s"
        
##### To Configer config file:
* Paste following in your **config** file create at path above:
    ```
    # Read more about SSH config files: https://linux.die.net/man/5/ssh_config
    Host 192.168.56.102
        User cmpe
        HostName 192.168.56.102
        IdentityFile C:\Users\<your_username>\.ssh\id_rsa-remote-ssh 
    ```

##Part 5 (Connect to Ubuntu from VS Code):
<!-- FIXME: Need Picture of an icon-->
* look for the Remont Connect icon, which is a blue icon with "><" in it
* Click on it and select:
    ```sh
    "remote-SSH: Connect to Host..."
    ```
<!-- FIXME: Need Picture -->
* select "**192.168.56.102**"


# SSHuttle via Windows
Configure a Debian machine to route network through a Windows host using `sshuttle`.  
Use case is a Windows PC connecting to a remote Debian server and using `sshuttle` to provide temporary web access to the remote machine.  
Not all ssh setup and configuration details are included. Plenty of better places for that.  
### Windows Host (Gateway)
1.  **OpenSSH Server**: Ensure the OpenSSH Server is installed and running.
2.  **OpenSSH Client**: Ensure the OpenSSH Client is installed.
3.  **Python 3**: Install Python 3 and ensure it is added to the system `PATH`.
    *   Verify by running `python --version` in PowerShell.  
### Debian Machine (Client)
Run the following commands to install the necessary tools:
```bash
sudo apt update -y
sudo apt install openssh-client openssh-server python3 python3-pip pipx curl
pipx ensurepath
pipx install sshuttle
```
> **Note**: You may need to restart your shell after `pipx ensurepath` for the changes to take effect.
> **Note**" Ensure a recent version of sshuttle is installed. Not all the older versions will work here.
## Usage
1.  **SSH into the Debian machine**.
2.  Run the `sshuttle` command below to start tunneling traffic through the Windows host.
```bash
sshuttle -v --dns -r <WINDOWS_USER>@<WINDOWS_IP> 0/0 -x <WINDOWS_IP> --to-ns <DNS_SERVER_IP> --python python.exe
```
### Parameters Guide
*   `-r <WINDOWS_USER>@<WINDOWS_IP>`: The SSH connection string to the Windows host.
*   `0/0`: Proxies all network traffic (default route).
*   `-x <WINDOWS_IP>`: Excludes the Windows host IP from the tunnel to prevent connection loops.
*   `--to-ns <DNS_SERVER_IP>`: The IP address of the DNS server to use (reachable via the Windows host).
*   `--python python.exe`: Specifies the Python interpreter to use on the remote (Windows) side.
### Example
```bash
# Example with placeholders
sshuttle -v --dns -r user@192.168.1.5 0/0 -x 192.168.1.5 --to-ns 8.8.8.8 --python python.exe
```

> Install Docker on Proxmox 7.0-11 with ubuntu and DNS on pfSense.



Get latest template from pve console `# pveam update` 



Ubuntu have shown better performance then Debian when running as VM. Check lastes LTS on:
https://wiki.ubuntu.com/Releases

**ubuntu-21.04** have standard suppor until Aprol 2025 an End of Life April 2030.



Standard setting for LXC Container:

- name **boxx**
- Unprivileged Container YES
- Nesting Yes
- Template **ubuntu-21.04-standard_21.04-1_amd64.tar.gz**
- Root Disk Depending on need (40 GB).
- CPU 2
- Memory 2048 MiB
- Swap 0
- Network as needed (DHCP in this example)
- Dns domain - from pfSense (or just search).
- Dns server - usually your gateway.



When created, copy MAC address from network settings (proxmox gui).

Enter in pfSense / Services / DHCP Server / Static Mapping.  

For it to work you need to select under

**DNS Resolver** / Static DHCP YES Register DHCP static mappings in the DNS Resolver.

---

login ssh to server, then:

```sh
apt update && apt full-upgrade # update
lsb_release -a # check version
```



```sh
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
    
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  
  apt update
  
 apt-get install docker-ce docker-ce-cli containerd.io
 
 # check
 systemctl status docker
 systemctl status containerd
 
 # if needed
 systemctl enable docker.service
 systemctl enable containerd.service
 
 
```








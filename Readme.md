## 1. Enable WSL on windows:
- Open PowerShell as Administrator and run:

```bash
    dism.exe /Online /Enable-Feature /All /FeatureName:VirtualMachinePlatform /NoRestart
    dism.exe /Online /Enable-Feature /All /FeatureName:Microsoft-Windows-Subsystem-Linux /NoRestart
```
```bash
    wsl --unregister Ubuntu  # Replace "Ubuntu" with your distro name if different
    wsl --install
    wsl.exe --install -d Ubuntu
```
```bash
    sudo apt update
    sudo apt install ansible -y
```
```bash
    ansible --version
```

### 2. Launch an EC2 instance and download a key-pair

### 3. create directory `ansible` > `inventory.ini` in it
```bash
    mkdir ansible
    cd ansible
    nano inventory.ini
```
```bash
    [webserver]
    <EC2_PUBLIC_IP> ansible_user=ec2-user ansible_ssh_private_key_file=<KEY_PATH>
```

### 4. Create a playbook via nano `install_nginx.yml` in the same directory

### 5. Run the playbook
```bash
    ansible-playbook -i inventory.ini install_nginx.yml
```



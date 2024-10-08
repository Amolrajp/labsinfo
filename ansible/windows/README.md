## On Controller

### Step1: `install python winrm module`
```
sudo apt install python-pip
sudo apt install python3-pip
sudo pip install pywinrm 
sudo pip install pypsrp
```
### Step2: setup inventory

```
prepare windows host group & map winhosts vars

[winhosts]
ip1
ip2

[winhosts:vars]
ansible_user=testuser
ansible_password="(O5dAh2tdhld3VkC5c2IYE@l6(@U?Wcm"
ansible_connection=psrp
ansible_psrp_cert_validation=ignore
```

## on all targeted windows hosts

### Step1: `check winrm is running`

```
login to each windows node
open powershell terminal & check winrm is running using below commands

Test-WSMan
Get-Service WinRM
Set-Item -Force WSMan:\localhost\Service\auth\Basic $true
Set-Item -Force WSMan:\localhost\Service\AllowUnencrypted $true
New-NetFirewallRule -DisplayName "Allow WinRM Ports" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 5985-5986
```

### Step2: `enable winrm`

> if winrm is not enabled or not running use below script to enable it. 

1) copy the script [ConfigureRemotingForAnsible](https://raw.githubusercontent.com/lerndevops/labs/master/ansible/windows/ConfigureRemotingForAnsible.ps1) to windows hosts
2) open powershell as administrator
3) run the script .\ConfigureRemotingForAnsible.ps1

Note: The [win_psexec](https://docs.ansible.com/ansible/latest/modules/win_psexec_module.html) module will help you enable WinRM on multiple machines if you have lots of Windows hosts to set up in your environment

For more information on WinRM and Ansible, check out the [Windows Remote Management](https://docs.ansible.com/ansible/latest/user_guide/windows_winrm.html) documentation page.

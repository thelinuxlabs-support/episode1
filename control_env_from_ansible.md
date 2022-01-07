# Episode 1 - controling environment vars from Ansible

## Setup

## Take aways

### iptables
* block everything but one ip/network
  ```bash
  iptables -A INPUT -i lo -j ACCEPT
  iptables -A INPUT -s 192.168.122.0/24 -j ACCEPT
  iptables -A INPUT -j REJECT
  iptables -A OUTPUT -d 192.168.122.0/24 -j ACCEPT
  iptables -A OUTPUT -j REJECT
  ```

* Save the rules
  ```bash
  iptables-save > newrules
  iptables-restore < newrules
  ```

### squid proxy
### /etc/environment & proxy settings
* for /etc/environment settings to apply, logout and login back.  
  what about the already running sessions ? --> reboot ?  
* if a proxy is set and local satellite/foreman is not blacklisted from it, subscription-manager identiity will fail.  
  Either we set no_proxy=foreman.local.domain but that will work only for the current session.  We can eventually write it in /etc/environment for it to persist system wide but this will require a reboot to take effect for the established sessions.  
  Or we set the no_proxy parametter directly in /etc/rhsm/rhsm.conf in the [server] section.  
  
* 
### apache
### uri module
### timeout command to avoid ansible hangs
I havent found yet a way of doing directly with ansible.  
but with adhoc commands, this can be achieved with the timeout command:  
```bash
ansible servers -m command -a 'timeout subscription-manager identity'
```
### exclude multiple hosts from ansible command line
list all servers name starting with srv except srv50, srv51 and srv52:
```bash
ansible 'srv*:!~srv(50|51|52)' --list-hosts
```
### cat or echo something to multiple files using brace expansion
```bash
echo 'ansible_user: toto' | tee host_vars/vm{1..10}
```
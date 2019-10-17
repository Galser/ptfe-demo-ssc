# ptfe-demo-ssc
Install PTFE on Demo version with Self Sign Certificate - vagrant

# Requirements
TFE Overview : https://www.terraform.io/docs/enterprise/index.html
Pre-INstall checklist : https://www.terraform.io/docs/enterprise/before-installing/index.html

# How To

1. Clone
2. Vagrant up
3. 

## Additonal data 
- self-signed cert
- POC mode

## VM 
2 cores, 4GB of RAM

# TODO
- [ ] prepare step-by step instructions for installation part
- [ ] create test test instructions
- [ ] create test code
- [ ] run tst code and update instructions with examples


# DONE
- [x] define objectives as we go
- [x] prepare vagrant vm

# notes


# Run log
```bash
sudo bash install.sh 
Determining local address
The installer was unable to automatically detect the private IP address of this machine.
Please choose one of the following network interfaces:
[0] enp0s3	10.0.2.15
[1] enp0s8	192.168.56.22
Enter desired number (0-1): 1
The installer will use network interface 'enp0s8' (with IP address '192.168.56.22').
Determining service address
The installer was unable to automatically detect the service IP address of this machine.
Please enter the address or leave blank for unspecified.
Service IP address: 192.168.56.22
Does this machine require a proxy to access the Internet? (y/N) n
Installing docker version 18.09.2 from https://get.replicated.com/docker-install.sh
# Executing docker install script, commit: UNKNOWN
+ sh -c apt-get update -qq >/dev/null
+ sh -c apt-get install -y -qq apt-transport-https ca-certificates curl >/dev/null
+ sh -c curl -fsSL "https://download.docker.com/linux/ubuntu/gpg" | apt-key add -qq - >/dev/null
Warning: apt-key output should not be parsed (stdout is not a terminal)
+ sh -c echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable" > /etc/apt/sources.list.d/docker.list
+ [ ubuntu = debian ]
+ sh -c apt-get update -qq >/dev/null
INFO: Searching repository for VERSION '18.09.2'
INFO: apt-cache madison 'docker-ce' | grep '18.09.2.*-0~ubuntu' | head -1 | cut -d' ' -f 4
+ _status=0
+ [ -n 5:18.09.2~3-0~ubuntu-bionic ]
+ sh -c apt-get install -y -qq --no-install-recommends docker-ce-cli=5:18.09.2~3-0~ubuntu-bionic >/dev/null
...
```
ends with : 
```
Digest: sha256:76dfb558e0de6ba6f90e0725b36e53b04044b04b9b350efc12eede0cccea1275
Status: Downloaded newer image for quay.io/replicated/replicated-operator:stable-2.39.0
Tagging replicated-operator image
Stopping replicated-operator service
Installing replicated-operator service
Starting replicated-operator service
Created symlink /etc/systemd/system/docker.service.wants/replicated-operator.service → /etc/systemd/system/replicated-operator.service.

Operator installation successful

To continue the installation, visit the following URL in your browser:

  http://192.168.56.22:8800
```

### in Web : 

- Checks
Preflight Checks
Successful HTTP request
Can access api.replicated.com
OS linux is supported
The operating system must be linux
Kernel version requirement met
Kernel version must be at least 3.10
Successful TLS connection
Can connect to TLS 192.168.56.22 address
Total space requirement met for directory /tmp
Directory must have at least 1G total space
Total space requirement met for directory /var/lib/replicated
Directory must have at least 250M total space
Docker server version requirement met
Docker server version must be at least 1.7.1 and no greater than 18.09.2
CPU cores requirement met
Server must have at least 2 CPU cores
Memory requirement met
Server must have at least 4G total memory
Total space requirement met for directory /
Directory must have at least 10G total space
Total space requirement met for directory /var/lib/docker
Directory must have at least 40G total space
Successful Docker registry ping
Can access registry registry.replicated.com
Successful Docker registry ping
Can access registry index.docker.io
Successful connection to releases.hashicorp.com:443.
Can connect to releases.hashicorp.com:443.
Node: 6d3fe2281f0a47805f475ae52db5019d
OS linux is supported
The operating system must be linux
Kernel version requirement met
Kernel version must be at least 3.10
Successful TLS connection
Can connect to TLS 192.168.56.22 address
Docker server version requirement met
Docker server version must be at least 1.7.1 and no greater than 18.09.2
CPU cores requirement met
Server must have at least 2 CPU cores
Memory requirement met
Server must have at least 4G total memory
Total space requirement met for directory /
Directory must have at least 10G total space
Total space requirement met for directory /var/lib/docker
Directory must have at least 40G total space

- Continue

### Settings :
    - Enter hostname : 192.168.56.22.xip.io
    - INstallation Type : [Demo]
  - hit [save]  
 - Saved - [Restart Now] 
 > Note : ..then Starting in the left part of Dashboard - never changes unless you reload the page :-) 
 - Wait a couple of minutes, at the left rectangel, below the button [Stop now] there is link :
 192.168.56.22.xip.io, press it
 - Setup your admin
 - Now you are logged in


## Test
- Create some organization, for out example we going to use "ACME" ( enter name and some email address, you email address)
- On the next screen you are going to be proposed to create new workspace and attach VSC provides, you can skip attaching VSC provider for now , press the link "skip this tep"
- Enter some new for your workspace, for the test we use "playground"
- Let's prepare token for connecting : You can generate one on the [user settings page](https://192.168.56.22.xip.io/app/settings/tokens). Save generated user token somewhere safe.

- We need also setup a token in special file  - Terraform CLI Configuration file. This file is located at `%APPDATA%\terraform.rc` on Windows systems, and `~/.terraformrc` on other systems, with following content :
    ```
        credentials "192.168.56.22.xip.i" {
            token = "REPLACE_ME"
        }
    ```
    > Note replace the token above with your value from previous step
- And change config in main to look like : 
    ```terraform
    terraform {
        backend "remote" {
            hostname = "192.168.56.22.xip.io"
            organization = "ACME"

            workspaces {
            name = "playground"
            }
        }
    }
    ```

- Now we can create some basic Terraform configuration, and test our workspace

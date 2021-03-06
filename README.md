# check_pve
Icinga check command for Proxmox VE via API

## Requirements

This check command depends on the following python modules:
 * enum
 * requests
 * argparse

**Installation on Debian / Ubuntu**
```
apt install python-enum34 python-requests
```

**Installation on Redhat 6 / CentOS 6**
```
yum install python-argparse python-enum34 python-requests
```

**Installation on Redhat 7 / CentOS 7**
```
yum install python-enum34 python-requests
```

## Usage

The ``icinga2`` folder contains the command defintion and service examples for use with Icinga2.

```
usage: check_pve.py [-h] -e API_ENDPOINT -u API_USER -p API_PASSWORD [-k] -m
                    {cluster,cpu,memory,storage,io_wait,updates,services,subscription,vm}
                    [-n NODE] [--name NAME] [--ignore-service NAME]
                    [-w TRESHOLD_WARNING] [-c TRESHOLD_CRITICAL] [-M]

Check command for PVE hosts via API

optional arguments:
  -h, --help            show this help message and exit

API Options:
  -e API_ENDPOINT, --api-endpoint API_ENDPOINT
                        PVE api endpoint hostname
  -u API_USER, --username API_USER
                        PVE api user (e.g. icinga2@pve or icinga2@pam,
                        depending on which backend you have chosen in proxmox)
  -p API_PASSWORD, --password API_PASSWORD
                        PVE api user password
  -k, --insecure        Don't verify HTTPS certificate

Check Options:
  -m {cluster,cpu,memory,storage,io_wait,updates,services,subscription,vm}, --mode {cluster,cpu,memory,storage,io_wait,updates,services,subscription,vm}
                        Mode to use.
  -n NODE, --node NODE  Node to check (necessary for all modes except cluster)
  --name NAME           Name of storage or vm
  --ignore-service NAME
                        Ignore service NAME in checks
  -w TRESHOLD_WARNING, --warning TRESHOLD_WARNING
                        Warning treshold for check value
  -c TRESHOLD_CRITICAL, --critical TRESHOLD_CRITICAL
                        Critical treshold for check value
  -M                    Values are shown in MB (if available). Tresholds are
                        also treated as MB values
```

## Examples

**Check cluster health**
```
./check_pve.py -u <API_USER> -p <API_PASSWORD> -e <API_ENDPOINT> -m cluster
OK - Cluster 'proxmox1' is healthy'
```

**Check CPU load**
```
./check_pve.py -u <API_USER> -p <API_PASSWORD> -e <API_ENDPOINT> -m memory -n node1
OK - CPU usage is 2.4%|usage=2.4%;;
```

**Check memory usage**
```
./check_pve.py -u <API_USER> -p <API_PASSWORD> -e <API_ENDPOINT> -m cpu -n node1
OK - Memory usage is 25.55%|usage=25.55%;; used=49415.93MB;;;193403.4
```

**Check storage usage**
```
./check_pve.py -u <API_USER> -p <API_PASSWORD> -e <API_ENDPOINT> -m storage -n node1 --name local
OK - Storage usage is 54.23%|usage=54.23%;; used=128513.11MB;;;236980.36
```

**Check subscription status**
```
./check_pve.py -u <API_USER> -p <API_PASSWORD> -e <API_ENDPOINT> -m subscription -n node1
OK - Subscription of level 'Community' is valid until 2019-01-09
```

**Check VM status**
```
./check_pve.py -u <API_USER> -p <API_PASSWORD> -e <API_ENDPOINT> -m vm -n node1 --name test-vm
OK - VM 'test-vm' is running|cpu=1.85%;; memory=8.33%;;
```

## FAQ

### Could not connect to PVE API: Failed to resolve hostname

Verify that your DNS server is working and can resolve your hostname. If everything is fine check for proxyserver environment variables (HTTP_PROXY,HTTPS_PROXY), which maybe not allow communication to port 8006.





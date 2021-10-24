# Praktikum Modul 1
## Group 12
### 1. Rename ubuntu_php5.6 to ubuntu_landing, and change the IP according to the new scheme
- turn off ubuntu_php5.6
```bash
sudo lxc-stop -n ubuntu_php5.6
```
- Rename
```bash
sudo lxc-copy -R -n ubuntu_php5.6 -N ubuntu_landing
```
- static IP settings

![1](https://user-images.githubusercontent.com/93064971/138588070-75ae938c-fcbd-4e58-aa1f-fd2c72657074.png)

address changed to __103__

```bash
systemctl restart networking.service
```

![2](https://user-images.githubusercontent.com/93064971/138588410-58e4621f-d776-48c6-a398-e9d612fd5362.png)

### 2. Install lxc debian 9 by the name debian_php5.6
```bash
sudo lxc-create -n debian_php5.6 -t download -- --dist debian --release stretch --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
```

### 3. setup nginx on debian_php5.6 for the http://lxc_php5.dev domain, create an index.html page that describes the lxc name information
- start debian_php5.6 & lauch it
```bash
sudo lxc-start -n debian_php5.6
sudo lxc-attach -n debian_php5.6
```

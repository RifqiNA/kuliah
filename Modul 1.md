# Praktikum Modul 1
## Group 12
1. Rename ubuntu_php5.6 to ubuntu_landing, and change the IP according to the new scheme
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

2. Install lxc debian 9 dengan nama debian_php5.6
```bash
sudo lxc-create -n debian_php5.6 -t download -- --dist debian --release stretch --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
```

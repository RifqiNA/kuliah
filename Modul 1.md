# Praktikum Modul 1
## kelompok 12
1. Rename ubuntu_php5.6 menjadi ubuntu_landing, serta rubah IP mengikuti skema yang baru
- Matikan dulu ubuntu_php5.6
```bash
sudo lxc-stop -n ubuntu_php5.6
```
- Rename
```bash
sudo lxc-copy -R -n ubuntu_php5.6 -N ubuntu_landing
```
- setting ip
![1](https://user-images.githubusercontent.com/93064971/138588070-75ae938c-fcbd-4e58-aa1f-fd2c72657074.png)
2. Install lxc debian 9 dengan nama debian_php5.6
```bash
sudo lxc-create -n debian_php5.6 -t download -- --dist debian --release stretch --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
```

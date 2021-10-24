# Praktikum Modul 1
## Group 12 [IT02-01]
**Rifqi Naufal A 1202190012 || Andi Tadang P 1202190046
<hr> 
### 1. Rename ubuntu_php5.6 to ubuntu_landing, and change the IP according to the new scheme
- turn off ubuntu_php5.6
```bash
sudo lxc-stop -n ubuntu_php5.6
```
- Rename
```bash
sudo lxc-copy -R -n ubuntu_php5.6 -N ubuntu_landing
```
- static IP settings for ubuntu_landing

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
- start debian_php5.6 & launch it
```bash
sudo lxc-start -n debian_php5.6
sudo lxc-attach -n debian_php5.6
```
- install nano & nginx
```bash
sudo apt install nginx nginx-extras
apt install nano net-tools curl
```

- static IP settings for debian_php5.6
```bash
nano /etc/network/interfaces
````
![3](https://user-images.githubusercontent.com/93064971/138588679-de9f5b52-9f90-4469-84e4-b0a8f60697bc.png)

**changed like the picture above**

```bash
systemctl restart networking.service
ifconfig
```

![4](https://user-images.githubusercontent.com/93064971/138588755-02a50a8b-2dec-45da-b14f-a8b081974e7a.png)

- setting nginx
```bash
cd /etc/nginx/sites-available
nano lxc_php5.6.dev 
```

![5](https://user-images.githubusercontent.com/93064971/138588969-925eb9b3-c6e5-4fc1-b8c4-2b2c3c838d8b.png)

```bash
cd ../sites-enabled
ln -s /etc/nginx/sites-available/lxc_php5.6.dev .
nginx -t
nginx -s reload
nano /etc/hosts
```
![6](https://user-images.githubusercontent.com/93064971/138589068-bc755852-1848-4ec6-b885-10e66432416e.png)

**changed like the picture above**

```bash
cd /var/www/html
mkdir lxc_php5.6
cp index.nginx-debian.html lxc_php5.6/index.html
nano index.html
```
if you can't use **nano index.html** use this instead
```bash
nano lxc_php5.6/index.html
```

![7](https://user-images.githubusercontent.com/93064971/138589296-409121e2-6e2b-410a-9456-c026214d2ada.png)

**changed like the picture above**
```bash
curl -i http://lxc_php5.dev
```

### 4. setup nginx on ubuntu_landing for the http://lxc_landing.dev domain, create an index.html page that describes the lxc name information

- start ubuntu_landing & launch it
```bash
sudo lxc-start -n ubuntu_landing
sudo lxc-attach -n ubuntu_landing
```
-setting nginx


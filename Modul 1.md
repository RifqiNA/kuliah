# Praktikum Modul 1
## Group 12 [IT02-01]
**Rifqi Naufal A 1202190012 || Andi Tadang P 1202190046**

<hr> 

### Scheme

Virtual Box Ubuntu Server IP : 192.168.0.100

LXC Debian Server 9 php5.6 IP : 10.0.3.102

LXC Ubuntu Server 18.04 php7.4 IP : 10.0.3.101

LXC Ubuntu Server 16.04 IP : 10.0.3.103

<hr>

## 1. Rename ubuntu_php5.6 to ubuntu_landing, and change the IP according to the new scheme

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

<hr> 

## 2. Install lxc debian 9 by the name debian_php5.6
```bash
sudo lxc-create -n debian_php5.6 -t download -- --dist debian --release stretch --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
```

<hr> 

## 3. setup nginx on debian_php5.6 for the http://lxc_php5.dev domain, create an index.html page that describes the lxc name information
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

<hr> 

## 4. setup nginx on ubuntu_landing for the http://lxc_landing.dev domain, create an index.html page that describes the lxc name information

- start ubuntu_landing & launch it
```bash
sudo lxc-start -n ubuntu_landing
sudo lxc-attach -n ubuntu_landing
```
-setting nginx

```bash
cd /etc/nginx/sites-available
nano lxc_php5.6.dev 
```

![8](https://user-images.githubusercontent.com/93064971/138592080-0978a4c0-c34a-4313-adff-a8b4d3d52c2e.png)

```bash
cd ../sites-enabled
ln -s /etc/nginx/sites-available/lxc_php5.6.dev .
nginx -t
nginx -s reload
nano /etc/hosts
```

![9](https://user-images.githubusercontent.com/93064971/138592138-5e3f3963-ff19-4b20-a97d-563134c51c41.png)

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
![10](https://user-images.githubusercontent.com/93064971/138592349-cec3daf6-8831-4980-9486-dcd8e088a921.png)

```bash
curl -i http://lxc_landing.dev
```

<hr> 

## 5. LXC ubuntu_landing should auto start when the vm is started, this is used to keep the company profile website from experiencing downtime

- stop ubuntu_landing then check if it actually stop

```bash
sudo lxc-stop -n ubuntu_landing
sudo lxc-ls -f
```

![11](https://user-images.githubusercontent.com/93064971/138592610-a6c3e444-e54b-4c36-bd33-4e6634cdcd09.png)

- Go to the /var/lib/lxc directory and check if there are any directories
 
```bash
sudo su
cd /var/lib/lxc/ubuntu_landing
nano config
```

![13](https://user-images.githubusercontent.com/93064971/138592840-c433ac0a-c591-4fdd-8b9f-0ae376fa5ee1.png)

Add
```bash
lxc.start.auto = 1
```

![14](https://user-images.githubusercontent.com/93064971/138592880-06575291-2296-45a2-b78a-e8619dfa123a.png)

Check auto start by rebooting first

Exit the super user, then reboot it

```bash
reboot
```

Then check is the ubuntu_landing autostart has been running or not. If the autostart has been running, the 'AUTOSTART' will change from '0' to '1'.

```bash
sudo lxc-ls -f
```

![15](https://user-images.githubusercontent.com/93064971/138592986-7656ec9c-857e-47a1-aadc-84c5b4fdd2bc.png)

<hr>

## 6. setup nginx on vm.local to set proxy_pass where :
- accessing http://vm.local will be redirected to http://lxc_landing.dev
- accessing http://vm.local/blog will be redirected to http://lxc_php7.dev
- accessing http://vm.local/app will be redirected to http://lxc_php5.dev

**using super user**

```bash
sudo nano /etc/hosts
```

![16](https://user-images.githubusercontent.com/93064971/138594732-9896d2db-88a6-4e55-9bd5-e0707c400b12.png)

After that we need to install nginx, then enter the directory of sites available, then enter the nano of vm.local

```bash
sudo apt install nginx nginx-extras
cd /etc/nginx/sites-available
sudo touch vm.local
sudo nano vm.local
```

Edit the nano vm.local

![17](https://user-images.githubusercontent.com/93064971/138594827-076afbc2-2505-45b3-a1b7-a23f8449faf2.png)

Then enter the directory of sites enabled, If nginx fails to start, run `sudo nginx -t` to find if there is anything wrong with your configuration file.  `sudo nginx -s reload` keeps the nginx server running as it reloads updated configuration files. If nginx notices a syntax error in any of the configuration files, the reload is aborted and the server keeps running based on old config files.

![18](https://user-images.githubusercontent.com/93064971/138594931-2089adac-42f4-40cc-92e3-62a6e5b013f9.png)

```bash
cd ../sites-enabled
sudo nginx -t
sudo nginx -s reload
```

We check is the html code appear as it should be or not, using command curl, curl is a command line tool to transfer data to or form a server, using any of the supported protocols like HTTP, FTP, etc. Curl can transfer multiple file at once.

```bash
curl -i http://vm.local/
curl -i http://vm.local/app
curl -i http://vm.local/blog
```
**http://vm.local/**

![vmlocal1](https://user-images.githubusercontent.com/93064971/138595649-d1e735b6-d4fa-4565-b06a-b48ca3e3ce6f.png)

**http://vm.local/app**

![vmapp1](https://user-images.githubusercontent.com/93064971/138595661-1c5b5737-f8e9-4e92-9a93-1f93aaa9d734.png)

**http://vm.local/blog**

![vmblog1](https://user-images.githubusercontent.com/93064971/138595673-43210a57-d4ed-4055-bc98-e236dcfe0ba5.png)

<hr>

## 7. for their presentation needs, the browser on their laptop must be able to access all three urls.

```
**http://vm.local/**
```

![vmlocal](https://user-images.githubusercontent.com/93064971/138595760-fe1ae5f7-6ba6-4bf4-ae5f-b5fde3e671c2.png)

```
**http://vm.local/app**
```

![vmapp](https://user-images.githubusercontent.com/93064971/138595788-e2a20e0a-fe47-4b59-8b85-43025317dd17.png)


```
**http://vm.local/blog**
```

![vmblog](https://user-images.githubusercontent.com/93064971/138595813-25202aec-688b-446a-831f-d6bc614da40f.png)

<hr>

## Analysis

- why for the needs of php5.6 can not use ubuntu 16.04, so it needs to change the os to debian 9?

Because on ubuntu 16.04 it is no longer supported by the support system or software updates such as security, php5.6 etc which ends in april 2021, so it is necessary to change to using debian 9 which will be supported until 2022.

- why use LXC virtualization on the website schema that will be developed?

Because the use of lxc virtualization is more resource efficient, makes it easier to manage servers, and the LXC (Linux Containers) is a lightweight virtualization system on the linux operating system. The LXC system also allow us to be able to run multiple virtual units simultaneously on the same host.

- what is a proxy server? why can we think of vm.local as a proxy server?

A proxy server is a system that works as a network intermediary, for example when you access a website page, the proxy will request and receive information from the website to the device you are using. vm.local can be assumed as a Proxy Server because vm.local's just a bridge to proceed to the container app. Therefore, the server that we made can be accessible on internet using browser.

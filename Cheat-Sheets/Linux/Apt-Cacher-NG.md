https://www.atlantic.net/vps-hosting/how-to-set-up-apt-caching-server-using-apt-cacher-ng-on-ubuntu/

## Configure client

By default, all Ubuntu system uses Ubuntu repository to download and install packages, so you will need to configure your client system to use Apt-Cacher NG for package installation.

To do so, create a new proxy configuration file:
```bash
nano /etc/apt/apt.conf.d/00aptproxy
```

Add the following line:
```bash
Acquire::http::Proxy "http://your-server-ip:3142";
```
Save and close the file when you are finished.
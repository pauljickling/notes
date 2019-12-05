# Apache

Apache is a HTTP server that is configured with an assortment of text files. The directory structure on an OS like Ubuntu is as follows:

- `/etc/apache2/` for the server root, modules, and configuration files
- `/var/log/apache2/` for the logs
- `/var/www/` for the document root (i.e. your html pages/web framework)
- `/usr/sbin` for the apachectl binaries
- `/etc/init.d/apache2` for the various service commands (start, stop, reload, etc.)

These differ from OS to OS however so you should consult [the wiki](https://cwiki.apache.org/confluence/display/HTTPD/DistrosDefaultLayout) for confirmation.

Official documentation for Apache can be found [here](http://httpd.apache.org/docs/current/).

## Hostnames and DNS

Hostnames are configured via the host file in `/etc/hosts`. The host file will have a list of IP addresses and the attached host name.

## Configuration

Traditionally, Apache was configured via the `httpd.conf` file. However typically this configuration is broken up into multiple, smaller config files. In the standard Ubuntu build, for example, there is instead the `apache2.conf` file that manages all the smaller config files, and there is no `httpd.conf` file.

Content directories contain the `.htaccess` file that allow configuration changes on a per directory basis. They are typically used by users that do not have system-wide access to a server.

## Running as a Process

The Apache server can be managed with commands like `sudo service apache start`, `sudo service apache stop`, `sudo service apache restart`, etc. By default the restart of an Apache service is handled gracefully so that it does not interfere with client requests.

## Configuring a Subdomain

There are lots of places where you could define your subdomain. A common location might be `/etc/apache2/sites-enabled/` and you will have a config file with the following info:

```
<VirtualHost {IP Address:Port}>
    ServerName subdomain.domain.com
    DocumentRoot /var/www/subdomain
</VirtualHost>
```

Anytime you make config changes like this you will need to run `service apache2 restart`.

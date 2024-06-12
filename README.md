markdown

# Apache Performance Optimization Guide

Welcome to the Apache Performance Optimization Guide! This repository contains tips and configuration settings to help you get the most out of your Apache server running on a CentOS 7 VPS with 8GB of RAM and 4 CPU cores.

## Table of Contents
- [Introduction](#introduction)
- [Server Configuration](#server-configuration)
  - [Choosing the Right MPM](#choosing-the-right-mpm)
  - [KeepAlive Settings](#keepalive-settings)
  - [Timeout Directive](#timeout-directive)
  - [Gzip Compression](#gzip-compression)
  - [Caching](#caching)
  - [MaxRequestWorkers](#maxrequestworkers)
  - [Disabling Unnecessary Modules](#disabling-unnecessary-modules)
  - [Monitoring with mod_status](#monitoring-with-mod_status)
  - [Hostname Lookups](#hostname-lookups)
  - [File Descriptors](#file-descriptors)
- [Applying Changes](#applying-changes)
- [Monitoring and Adjustments](#monitoring-and-adjustments)
- [Contributing](#contributing)
- [License](#license)

## Introduction
This guide provides practical advice to optimize the performance of your Apache web server. By following these suggestions, you can ensure your server runs efficiently, delivering content quickly and reliably to your users.

## Server Configuration

### Choosing the Right MPM
Apache supports multiple Multi-Processing Modules (MPMs). Depending on your workload, you might prefer either the `mpm_worker_module` for its threading capabilities or the `mpm_prefork_module` for process-based handling.

#### For `mpm_worker_module`:
```apache
<IfModule mpm_worker_module>
    StartServers             4
    MinSpareThreads         25
    MaxSpareThreads         75
    ThreadLimit             64
    ThreadsPerChild         25
    MaxRequestWorkers      150
    MaxConnectionsPerChild   0
</IfModule>

For mpm_prefork_module:

apache

<IfModule mpm_prefork_module>
    StartServers             4
    MinSpareServers          5
    MaxSpareServers         10
    MaxRequestWorkers      150
    MaxConnectionsPerChild   0
</IfModule>

KeepAlive Settings

Enable KeepAlive to allow multiple requests over a single connection.

apache

KeepAlive On
MaxKeepAliveRequests 100
KeepAliveTimeout 5

Timeout Directive

Lower the Timeout value to free up resources faster.

apache

Timeout 60

Gzip Compression

Enable Gzip compression to reduce the size of data sent over the network.

apache

<IfModule mod_deflate.c>
    AddOutputFilterByType DEFLATE text/html text/plain text/xml text/css text/javascript application/javascript
</IfModule>

Caching

Set up caching to reduce load times for frequently requested content.

apache

<IfModule mod_expires.c>
    ExpiresActive On
    ExpiresByType image/jpg "access plus 1 month"
    ExpiresByType image/jpeg "access plus 1 month"
    ExpiresByType image/gif "access plus 1 month"
    ExpiresByType image/png "access plus 1 month"
    ExpiresByType text/css "access plus 1 week"
    ExpiresByType application/javascript "access plus 1 week"
    ExpiresByType text/html "access plus 1 minute"
</IfModule>

MaxRequestWorkers

Calculate and set an appropriate value for MaxRequestWorkers.

apache

# Example: 
MaxRequestWorkers 160

Disabling Unnecessary Modules

Disable unused modules to save memory and improve performance.

apache

# Example: Comment out unused modules
# LoadModule status_module modules/mod_status.so

Monitoring with mod_status

Enable mod_status to monitor server performance.

apache

<Location /server-status>
    SetHandler server-status
    Require ip 127.0.0.1
</Location>

Hostname Lookups

Disable hostname lookups to save DNS resolution time.

apache

HostnameLookups Off

File Descriptors

Increase the file descriptor limit if necessary.

shell

# Add to /etc/security/limits.conf
apache soft nofile 4096
apache hard nofile 8192

Applying Changes

After making these changes, restart Apache to apply the new configuration:

shell

sudo systemctl restart httpd

Monitoring and Adjustments

Monitor your server’s performance using tools like mod_status and adjust the settings as needed based on real-time data and server load.
Contributing

Contributions are welcome! Please submit a pull request or open an issue to suggest improvements or report bugs.
License

This project is licensed under the MIT License. See the LICENSE file for details.
Feel free to customize the content further to better fit your needs!



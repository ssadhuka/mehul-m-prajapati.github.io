---
title: "Getting Started with PMacct on RHEL7.2"
excerpt: "Let's do real time IP traffic accounting"
category: Open Source
tags: [ip-accounting, pmacct]
header:
  image: https://i.imgur.com/tOdLKhy.png
  overlay_color: "#000"
  overlay_filter: "0.6"
  overlay_image: http://www.pmacct.net/docs/cacti_example.png
---

1. Install mysql

   ```
   $ yum install mysql-community-server
   $ yum install libtool
   $ yum install mysql-community-devel
   ```
2. Run mysql

   ```
   $ systemctl start mysqld
   $ grep 'A temporary password is generated for root@localhost' /var/log/mysqld.log |    tail -1
   ```
  
   - Copy temporary password and run following command
   
   ```
   $ /usr/bin/mysql_secure_installation
   ```

   - Paste it and enter new password. Configure other parameters as required. Restart mysql...
   
   ```
   $ systemctl restart mysqld
   ```

3. Create new Database

   ```
   $ mysql -u root -p
   $ CREATE DATABASE pmacct;
   ```
   - Press Ctrl + D to exit from MySQL prompt

4. Install PMAcct

   ```
   $ git clone https://github.com/pmacct/pmacct
   $ cd pmacct
   $ ./autogen.sh
   $ ./configure --enable-mysql
   $ make
   $ sudo make install
   ```

5. Create a Configuration file

   ```
$ mkdir -p /etc/pmacct
$ vim /etc/pmacct/pmacctd.conf
# Interface on which 'pmacctd' listens                                   
interface: eth0                        
# Sets the maximum number of concurrent writer processes the plugin is allowed to start
sql_max_writers: 25                        
# Enables the optimization of the statements sent to the RDBMS)
sql_optimize_clauses: true
# Forces 'pmacctd' to join together IPv4/IPv6 fragments
pmacctd_force_frag_handling: true
# Enables debug
debug: true
# If set to true adds two new fields, timestamp_min and timestamp_max
pmacctd_stitching: true
# Plugins to be enabled
plugins: mysql[c]                                
# Aggregates used for the sole purpose of IP accounting
aggregate[c]: src_host, dst_host, proto
# Defines the SQL database to use
sql_db[c]: pmacct                               
# In SQL and mongodb plugins this defines the table to use
sql_table[c]: acct             
# Defines the password to use when connecting to the server
sql_passwd[c]: xxxxx                  
# Defines the username to use when connecting to the server
sql_user[c]: root                                 
# Time interval, in seconds, between consecutive executions of the plugin cache scanner
sql_refresh_time[c]: 6                       
# Sets timestamp to seconds
timestamps_secs: true            
# this directive enables buffering of data transfers between core process and active plugins
plugin_buffer_size: 10240
plugin_pipe_size: 1024000
# Specifies the maximum number of bytes to capture for each packet
snaplen: 1500
   ```
   - Change mysql password in pmacctd.conf with what you have setup earlier
   - Run MySQL Schema and start MySQL service

   ```
$ mysql -u root -p < ./sql/pmacct-create-db_v1.mysql
$ mysql -u root -p pmacct
   ```

6. Run PMacct

   ```
$ pmacctd -f /etc/pmacct/pmacctd.conf
   ```

Reference: [Quick Start Guide](http://wiki.pmacct.net/OfficialExamples)

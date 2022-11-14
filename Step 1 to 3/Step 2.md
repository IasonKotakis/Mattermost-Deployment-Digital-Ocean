# Step 2 - Setting up OTRS

<h2> Important Notes  </h2>

<h3 style="background-color:red;"><strong>DO NOT PROCEED, UNTIL YOU HAVE READ ALL THE DOCUMENTATION</strong></h3>
<br>
<p> Before beginning the installation, you must be aware of a number of caveats in the manual. The numbered phases in the OTRS documentation correlate to these notes.Without knowing what each command's function is before you run them, you'll run into issues and have to restart.</p>
<br>
<p> Anytime you see a line beginning "shell >," you must enter the command in your terminal that follows the > symbol. For instance, your PuTTY/SSH prompt will resemble this: </p>

<code>root@user: #tar xzf otrs-x.x.x.tar.gz </code>

<p> When you type commands into the "shell >" prompt, the documentation assumes that you are operating as the "root" user. To become the root user,</p>
<p>type "<code>sudo -i -u root</code>" after which you can continue with the installation.</p>

<p> Although you can run each command as your regular user by using "sudo" it will be simpler to just become root and follow the instructions for this project. The relevant file permissions will then be configured for you by OTRS at the conclusion of the installation process.</p>

<h3> You MUST remember to use the Debian/Ubuntu speciﬁc version of each command, not the RedHat or SuSE versions. If you use the latter then you will get stuck.</h3>

<p>To begin, become root (per above using sudo) and install some prerequisite packages before starting at Step 1 (copy and paste the entire command starting at ‘apt’):</p>

<p><code>root@user:~# apt update && apt install libapache2-mod-perl2 \
libdbd-mysql-perl libtimedate-perl libnet-dns-perl libnet-ldap-perl \
libio-socket-ssl-perl libpdf-api2-perl libdbd-mysql-perl \
libsoap-lite-perl libtext-csv-xs-perl libjson-xs-perl \
libapache-dbi-perl libxml-libxml-perl libxml-libxslt-perl \
libyaml-perl libarchive-zip-perl libcrypt-eksblowﬁsh-perl \
libencode-hanextra-perl libmail-imapclient-perl libtemplate-perl \
libcrypt-ssleay-perl libdatetime-perl</code></p>

<hr>

<h2> What is OTRS, you might ask </h2>

<p>A service management toolkit called OTRS (formerly Open-Source Ticket Request System) is available. An agent portal, an admin dashboard, and a client portal are all included in the package. Teams process client tickets and requests on the agent portal (internal or external). This data, together with customer and associated data, can be accessed in a variety of ways. The admin dashboard, as its name suggests, enables system administrators to control the system: Roles and groups, process automation, channel connectivity, and CMDB/database options are just a few of the many options available. The third element, the customer portal, resembles a customizable website where customers can share information and follow requests from the customer's perspective.</p>

<h2> OTRS Setup </h2>

<p> You can find all the instructions and information you need in the OTRS installation guide. You may access the installation manual here:</p>


```python
https://doc.otrs.com/doc/manual/admin/6.0/en/html/manual-installation-of-otrs.html
```

<h3>Step 1: Install .tar.gz</h3>

<p> 1. To get started, you will need to use a tool like wget to download theOTRS package onto your VM. You can get the latest release directly byrunning this command in your terminal (copy and paste it):</p>
<p><code>wget https://github.com/OTRS/otrs/archive/refs/tags/rel-6_0_30.tar.gz \-O /tmp/otrs.tar.gz</code></p>
<p> 2. Next, unpack the ﬁle you just downloaded with the following command: <code>tar xzf /tmp/otrs.tar.gz</code></p>
 

<p> 3. Now run ‘ls’ to examine the contents of your directory. You should see the following (where otrs-6.0.29 is the unpacked source code in a directory for the OTRS application):<p>

<p><code>root@user:~# ls otrs-rel-6_0_30</code></p>

<p> 4. Now run the ‘mv’ command from the documentation:</p>

<p><code>mv otrs-rel-6_0_30 /opt/otrs</code></p>

<p>5. Conﬁrm the directory ‘/opt/otrs’ exists using the ‘ls’ and ‘ﬁle’commands chained together (the ‘;’ marks the end of a command, adding another command after is very common):</p>

<p><code>ls -Alh /opt; ﬁle /opt/otrs</code></p>

<p> You will receive output like the following:</p>

<p><code>total 4.0K
drwxrwxr-x 10 root root 4.0K Sep 28  2020 otrs
/opt/otrs: directory</code></p>

<hr>

<h3> Step 2: Install Additional Perl Modules</h3>

<p>1. Run the perl script as shown. For any missing modules, follow the instructions from the output, ensuring you follow the Debian/Ubuntu speciﬁc commands. You should NOT have to run the CPAN shell commands because the script will ﬁnd all the required dependencies.</p>

<p><code>perl /opt/otrs/bin/otrs.CheckModules.pl</code></p>

<hr>

<h3> Step 3: Create an OTRS user</h3>

<p>1. Ensure that you use the Debian/Ubuntu speciﬁc version of the commands.</p>

<p><code>useradd -d /opt/otrs -c 'OTRS user' otrs</code></p>

<hr>

<h3>Step 4: Activate Default Conﬁg File</h3>

<p>1. Follow the given instructions.</p>

<p><code>cp /opt/otrs/Kernel/Config.pm.dist /opt/otrs/Kernel/Config.pm</code></p>

<hr>

<h3>Step 5: Check if all needed modules are installed</h3>

<p>1. Run the scripts as indicated, one by one at a time</p>

<p><code>perl -cw /opt/otrs/bin/cgi-bin/index.pl</code></p>

<p><code>perl -cw /opt/otrs/bin/cgi-bin/customer.pl</code></p>

<p><code>perl -cw /opt/otrs/bin/otrs.Console.pl</code></p>

<hr>

<h3>Step 6: Conﬁguring the Apache web server</h3>

<p> 1. Step 6 is not well written and if you do not complete it correctly,nothing will work. On recent Debian and Ubuntu systems with Apache 2.4, there is no /etc/apache2/conf.d directory. Instead, do the following and then proceed with the a2enmod commands:</p>

<p><code>cd /etc/apache2/conf-enabled</code></p>

<p><code>ln -s /opt/otrs/scripts/apache2-httpd.include.conf zzz_otrs.conf</code></p>

<p> 2. Still in Step 6, you will receive an error like the following which you can
ignore:<code>a2enmod version, ERROR: Module version does not exist!</code></p>

<hr>

<h3>Step 7: File Permissions</h3>

<p>1. In Step 7: File Permissions, you’ll see the shell > prompt commands begin with ‘bin’. You must ensure you are working in the /opt/otrs directory for them to work. If you aren’t, type</p>
<p><code>cd /opt/otrs</code></p>

<hr>

<h3>Step 8: Database Setup and Basic System</h3>

<p>1. Be sure to replace the ‘localhost’ portion of the URL as speciﬁed. For example,<code>http://localhost/otrs/installer.pl</code> will become
<code>http://user.hostname.ca/otrs/installer.pl</code>. Replace the user and hostname accordingly.</p>

<p>2. Continue with the web installer instructions:<br>
    <code>https://doc.otrs.com/doc/manual/admin/6.0/en/html/web-installer.html</code></p>

<img src="https://github.com/IasonKotakis/Mattermost-Deployment-Digital-Ocean/blob/images/images/OTRS%20Web%20Installer.png">

<p>3. When you reach the Database Settings step in the web installer, click
Check database settings. You will receive a message like this:</p>

<p><code>Result of database check</code></p>

<p><code>Can't connect to database, read comment!</code></p>

<p><code>Can't connect to MySQL server on '127.0.0.1' (111)</code></p>

<p>4. In your terminal, run <code>apt install mysql-server</code>. Next run the command<code>cat/etc/mysql/debian.cnf</code> and copy the username and password for the<code>debian-sys-maint</code> user. Add both to the web installer prompt and check
database settings again.</p>

<p>5. Still on Database Settings, in your terminal do the following:</p>

<p><code>nano /etc/mysql/mysql.conf.d/mysqld.cnf</code></p>

<p>Paste the following 5 lines at the very bottom of the ﬁle:</p>

<p><code>max_allowed_packet= 64M</code></p>

<p><code>query_cache_size= 32M</code></p>

<p><code>innodb_log_ﬁle_size = 256M</code></p>

<p><code>character-set-server = utf8</code></p>


<p><code>collation-server= utf8_general_ci</code></p>

<p>Now restart MySQL to get the new settings to take eﬀect:</p>

<p><code>systemctl restart mysql.service</code></p>

<p>6. Continue the web installer using the MySQL debian-sys-maint password that you noted before.</p>

<p>7. In the web installer, under step 3/4 (Mail conﬁguration), choose Skipthis Step (bottom right).</p>

<p>8. Note the ﬁnal login details, login, and proceed to creating a user (changing the URL to match your student number as appropriate):<br>
<code>http://user.hostname.ca/otrs/index.pl?Action=AdminUser;Subaction=Add</code><br>
Be sure to set a password and note it down.</p>

<hr>

<p><strong>If you have followed the steps above then congratulations 👏, you have completed Step 2 of setting Mattermost on Digital Ocean</strong></p>
<p>Ready for Step 3?</p>




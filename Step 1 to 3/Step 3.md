# Step 3 - Set up

<h2>Description</h2>

<p>You will build up a Mattermost instant messaging service as promised. Because it is Open Source, has a sizable user and developer base, and has extensive documentation, it makes sense to utilise it. Additionally, it supports many database backends, allowing you to become more accustomed to PostgreSQL as an RDBMS.</p>

<p>This project will show that having safe communications with friends, coworkers, and family does not need sacrificing your data or privacy. You can create your own tools, escape proprietary platform shackles, and eliminate advertising.</p>

<hr>

<h2>Whats is Mattermost, you might ask</h2>

<p>A self-hosted, open-source online chat program featuring file sharing, search, and integrations is called Mattermost. It is marketed as an open-source alternative to Slack and Microsoft Teams and is intended as an internal chat for businesses and organizations. 
The code was once exclusive since SpinPunch, a game development firm, utilized Mattermost as an internal chat tool before it was open-sourced. Release day for version 1.0 was October 2, 2015.
Mattermost Inc. manages and advances the project. The business makes money by charging for support services and extra features that aren't available in the open-source version.It was also incorporated into GitLab under the name "GitLab Mattermost," despite the fact that in 2017, GitLab bought Gitter, another well-known chat application, and sold Gitter in 2021.</p>

<hr>

<h2> Important Notes</h2>

<h3 style="background-color:red;"><strong>!!!DO NOT PROCEED, UNTIL YOU HAVE READ ALL THE DOCUMENTATION!!!</strong></h3>
<br>
<p>1. You will need to stop Apache and MySQL on your VM. Use `systemctl stop…` for each service. Recall the commands that you used with OTRS to start/restart.</p>
<p>2. Ensure that you follow the PostgreSQL specific instructions if you come to asection with different options for the database.</p>
<ul><li>Note the Debian instructions say to edit /etc/postgresql/9.4/main/pg_hba.conf - the file that you need to edit on
Ubuntu 18.04 is /etc/postgresql/10/main/pg_hba.conf</li></ul>
<p>3. When you reach the Configuring Mattermost Server section, set your Site URL to the DNS name that points to your VM. For example, if your DNS name is jason, then your Site URL will be jason.hostname.ca.<br>
Under the ‘Listen Address’ field, set the address to 127.0.0.1:8065. You will need to restart Mattermost after doing that using the systemctl command from the documentation.</p>
<br>

<h3 style="background-color:red;"><strong>!!!Once you restart Mattermost, it will be unavailable until you finish the Nginx steps!!!</strong></h3>

<p>4. You do not need to complete the SMTP portion of the documentation.</p>
<p>5. When you are instructed to use an IP address like 10.10.10.2 or the like in nginx or PostgreSQL, use the localhost IP, 127.0.0.1. This will ensure Mattermost, PostgreSQL and nginx are all communicating locally instead of via the public
interface for your VM.</p>
<p>6. Your domain_name (in nginx) or Site URL in Mattermost will be something like
jason.itec2210.ca, where you substitute your DNS name for jason. This ensures that DNS points to your VM.</p>
<p>7. When you are editing config.json to configure PostgreSQL, the DataSource line will look like this (copy and paste <strong>INCLUDING THE FINAL COMMA!!!</strong>):

<p><code>"DataSource":
"postgres://mmuser:mmuser-password@127.0.0.1:5432/mattermost
?sslmode=disable&connect_timeout=10",</code></p>

<p>8. Once you finish the Configuring Mattermost Server section, skip to 'Installing NGINX Server'. Install and configure it, then stop at Step 3 of <strong>To configure NGINX as a proxy</strong>.</p>
<p>Follow the note there and proceed to the Configuring NGINX with SSL and
HTTP/2 page to finish the installation.</p>
<p>9. There undoubtedly are other issues. Please reach out via Mattermost and I’ll be happy to help.</p>

<h4>Package download here:</h4>

<code>https://releases.mattermost.com/5.36.1/mattermost-5.36.1-linux-amd64.tar.gz</code>

<hr>

<h2>PostgreSQL Setup</h2>

<p>As the Important Notes described, first we have to stop MySQL and Apache so we can install and configure PostegreSQL and Apache</p> 
<code>sudo systemctl stop mysqld</code>
<br>
<code>sudo systemctl stop apache</code>

<p>1. Log in and authenticate yourself on your Virtual Machine through PuTTy or SSH.<br>
2. From there access root privileges through the sudo command.<br>
3. Copy <code>apt-get install postgresql-12</code> to your terminal.<br>
4. Log in to the postgres account by running<code>sudo --login --user postgres</code>.<br>
5. Start the PostgreSQL interactive terminal by running <code>psql</code>.<br>
6. Create the Mattermost database by running <code>postgres=# CREATE DATABASE mattermost;</code>.<br>
7. Create the Mattermost user ‘mmuser’ by running <code>postgres=# CREATE USER mmuser WITH PASSWORD 'mmuser-password';</code>.<br>
8. Edit the file and include the source code of step 7 of the 'Important Notes'.</p>

<h4> Note, Use a password that is more secure than mmuser-password.</h4>

<p>9. Grant the user access to the Mattermost database by running <code>postgres=# GRANT ALL PRIVILEGES ON DATABASE mattermost to mmuser;</code>.<br>
10. Exit the PostgreSQL interactive terminal by running <code>postgres=# \q</code>.<br>
11. Log out of the postgres account by running <code>exit</code>.<br>
Lastly, we have to modify the <code>pg_hba.conf</code> to allow the Mattermost server to communicate with the database.
</p>

<p><strong>If the Mattermost server and the database are on the same machine:</strong></p>

<p>a. Open <code>/etc/postgresql/10/main/pg_hba.conf</code> in a text editor as root user and look for the following lines. </p>

<img src="https://github.com/IasonKotakis/Mattermost-Deployment-Digital-Ocean/blob/images/images/postgre%20peer%20ident.png">

<p>b. Change <code>peer</code> and <code>ident</code> to <code>trust</code>:</p>

<img src="https://github.com/IasonKotakis/Mattermost-Deployment-Digital-Ocean/blob/images/images/postgre%20trust.png">

<p>11. Reload PostgreSQL by running <code>sudo systemctl reload postgresql</code>.</p>

<p>12. Verify that you can connect with the user mmuser.</p>

<hr>

## Mattermost Setup

Lets create the System Admin user so we can set up Mattermost for general use. 

<p>1. Open a browser and navigate to your Mattermost instance. For example, if the IP address of the Mattermost server is 10.10.10.2 then go to http://10.10.10.2:8065.<br>
2. Create the first team and user. The first user in the system has the <code>system_admin</code> role, which gives you access to the System Console.<br>
3. To open the System Console, select the Product menu in the top-left corner of the navigation panel, then select <strong>System Console</strong>.
</p>

<img src="https://github.com/IasonKotakis/Mattermost-Deployment-Digital-Ocean/blob/images/images/Set%20up%20the%20URL.png">

<h5> You don't have to configure the SMTP/email step that is listed in the documentation. Navigate towards <a href="https://docs.mattermost.com/install/config-ssl-http2-nginx.html"> 'Configure NGINX with SSL and HTTP/2'</a>. </h5>

<hr>

## NGINX Setup

<p>1. Log in to the server that hosts NGINX and open a terminal window.<br>
2. Open the your Mattermost <code>nginx.conf</code> file as root in a text editor, then update the <code>{ip}</code> address in the <code>upstream backend</code> to point towards Mattermost (such as <code>127.0.0.1:8065</code>), and update the <code>server_name</code> to be your domain for Mattermost.</p>

<ul><li>Note on Ubuntu this file is located at <code>/etc/nginx/sites-available/</code></li></ul>. 

<p>3. Remove the existing default sites-enabled file by running <code>sudo rm /etc/nginx/sites-enabled/default</code></p>

<p>4. Enable the Mattermost configuration by running <code>sudo ln -s /etc/nginx/sites-available/mattermost /etc/nginx/sites-enabled/mattermost</code></p>

<p>5. Run <code>sudo nginx -t</code> to ensure your configuration is done properly. If you get an error, look into the NGINX config and make the needed changes to the file under <code>/etc/nginx/sites-available/mattermost</code>.</p>

<p>6. Restart NGINX by running <code>sudo systemctl start nginx</code>.</p>

<p><b>Note that if the steps above have been completed you should be able to check that Mattermost loads on your end but without TLS certificate<b>.</p>

<img src="https://github.com/IasonKotakis/Mattermost-Deployment-Digital-Ocean/blob/images/images/nginx.jpg">

<p>7. Verify that you can see Mattermost through the proxy by running <code>curl http://localhost</code>.</p>

<p>8. Install and update Snap by running <code>sudo snap install core; sudo snap refresh core</code>.</p>

<p>9. Install the Certbot package by running <code>sudo snap install --classic certbot</code>.</p>

<p>10. Add a symbolic link to ensure Certbot can run by running <code>sudo ln -s /snap/bin/certbot /usr/bin/certbot</code>.</p>

<p>11. Run the Let’s Encrypt installer dry-run to ensure your DNS is configured properly by running <code>sudo certbot certonly --dry-run</code>.</p>

<img src="https://github.com/IasonKotakis/Mattermost-Deployment-Digital-Ocean/blob/images/images/Server_Nginx_with_Mattermost_Comms%202.jpg">

<p>12. Run the Let’s Encrypt installer by running <code>sudo certbot</code>. This will run certbot and will automatically edit your NGINX config file for the site(s) selected.</p>

<p><b>If everything is set up correctly with the steps above and the documentation then you should be able to get a message from Let's Encrypt that a certficate has been issued for your website successfully.</b></p>

<img src="https://github.com/IasonKotakis/Mattermost-Deployment-Digital-Ocean/blob/images/images/Server_Nginx_with_Mattermost_Comms.jpg">

<p><b>You would be able to have access to your website and have the ability to create a team for your mattermost server.</b></p>

<img src="https://github.com/IasonKotakis/Mattermost-Deployment-Digital-Ocean/blob/images/images/Mattermost%20Completion.jpg">

<h3>👏 Congratulations!!! You have managed to create your own server with its own communcation platform for your family and friends to share, ad free.</h3>

<img src="https://github.com/IasonKotakis/Mattermost-Deployment-Digital-Ocean/blob/images/images/clapping-hands-emoji-clipart-md.png">

<hr>

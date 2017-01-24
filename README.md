# Elastic-Stack-5.0

This is a documentation / Walkthrough on how to install Elastic Stack on Ubuntu 16.04 Server

##Elastic-Stack-5.0 Installation Guide

###Author Stephane

####Minimum System Requirements
    OS: Ubuntu 16.04
    RAM: 5GB
    CPU: 2


| Software      | Version       |
| ------------- | ------------- |
| Elasticsearch | 5.1.2         |
| Logstash      | 5.1.2-1         |
| Kibana        | 5.1.2         |
| Beats         | 5.1.0         |
| X-Pack        | 5.1.0         |
| Nginx         | 1.10.0        |
| Java          | 1.8.0_111     |
 

##Follow These Steps To Get A Full ELK Installation
* Let's Go
##    Java Setup: Install Java 8
   
   Elasticsearch and Logstash require Java. We will install a recent version of Oracle Java 8 because that is what Elasticsearch recommends.
   
   Add the Oracle Java PPA to `apt`:
   ```shell
   $ sudo add-apt-repository -y ppa:webupd8team/java`
   ```
   
   Update the `apt` package database.
   ```shell
   $ sudo apt update
   ```
   
   Install the latest stable version of Oracle Java 8 with this command (and accept the license agreement that pops up):
   ```shell
   $ sudo apt install oracle-java8-installer
   ```
   
   Proceed with Elasticsearch installation.
   ##Install Elasticsearch
   First of all we need to configure Elastic Apt repository.
   
   Download and install the Public Signing Key
   ```shell
   $ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
   ```
   You may need to install the apt-transport-https package on Debian before proceeding:
   ```shell
   sudo apt-get install apt-transport-https
   ```
   Save the repository definition to /etc/apt/sources.list.d/elastic-5.x.list:
   ```shell
   $ echo "deb https://artifacts.elastic.co/packages/5.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-5.x.list
   ```
   Run sudo apt-get update and the repository is ready for use. You can install it with:
   ```shell
   sudo apt-get update && sudo apt-get install elasticsearch
   ```
   Elasticsearch is now installed. Let's edit the configuration:
   ```shell
   $ sudo nano /etc/elasticsearch/elasticsearch.yml
   ```
   
   You will want to restrict outside access to your Elasticsearch instance (port 9200), so outsiders can't read your data or shutdown your Elasticsearch cluster through the HTTP API. Find the line that specifies `network.host`, uncomment it, and replace its value with "localhost" so it looks like this:
   ```YAML
   network.host: localhost
   ```
   Save and exit `elasticsearch.yml`.
   Now, start Elasticsearch:
   ```shell
    sudo systemctl restart elasticsearch
   ```
   Then, run the following command to start Elasticsearch on boot up:
   ```shell
    sudo systemctl daemon-reload
    sudo systemctl enable elasticsearch
   ```
   Now that Elasticsearch is up and running, let's install Kibana.
   ##Install Kibana
   Update your `apt` package database and install Kibana.
   ```shell
   sudo apt-get update && sudo apt-get install kibana
   ```
   Kibana is now installed.

   Open the Kibana configuration file for editing:
   ```shell
   sudo nano /etc/kibana/config/kibana.yml
   ```
   In the Kibana configuration file, find the line that specifies server.host, and replace the IP address ("0.0.0.0" by default) with "localhost":
   ```YAML
   server.host: "localhost"
   ```
   Save and exit. This setting makes it so Kibana will only be accessible to the localhost. This is fine because we will use an Nginx reverse proxy to allow external access.

   Now enable the Kibana service, and start it:
   ```shell
   sudo systemctl daemon-reload
   sudo systemctl enable kibana
   sudo systemctl start kibana
   ```
   Before we can use the Kibana web interface, we have to set up a reverse proxy. Let's do that now, with Nginx.
   ##Install Nginx
   Because we configured Kibana to listen on `localhost`, we must set up a reverse proxy to allow external access to it. We will use Nginx for this purpose.
   >If you already have an Nginx instance that you want to use, feel free to use that instead. Just make sure to configure Kibana so it is reachable by your Nginx server (you probably want to change the host value, in /opt/kibana/config/kibana.yml, to your Kibana server's private IP address or hostname). Also, it is recommended that you enable SSL/TLS.
   
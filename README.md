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
| Kibana        | 5.1.0         |
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
   
   
   
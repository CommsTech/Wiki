---
title: ElasticSearch installation on Ubuntu
description: 
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# Install on Ubuntu
# How To Install and Configure Elasticsearch on Ubuntu 22.04

Published on April 25, 2022

- [Elasticsearch](https://www.digitalocean.com/community/tags/elasticsearch "Elasticsearch")
- [Ubuntu 22.04](https://www.digitalocean.com/community/tags/ubuntu-22-04 "Ubuntu 22.04")
- [Ubuntu](https://www.digitalocean.com/community/tags/ubuntu "Ubuntu")

![Default avatar](https://www.digitalocean.com/_next/static/media/default-avatar.14b0d31d.jpeg)

By [Alex Garnett](https://www.digitalocean.com/community/users/agarnett)

Senior DevOps Technical Writer

![How To Install and Configure Elasticsearch on Ubuntu 22.04](https://www.digitalocean.com/_next/static/media/intro-to-cloud.d49bc5f7.jpeg "How To Install and Configure Elasticsearch on Ubuntu 22.04")

## Not using Ubuntu 22.04?Choose a different version or distribution

Ubuntu 22.04

_A previous version of this article was written by_ [_Toli_](https://www.digitalocean.com/community/users/tollodim).

# [Introduction](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-22-04#introduction)[](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-22-04#introduction)

[Elasticsearch](http://www.elasticsearch.org/) is a platform for distributed search and analysis of data in real time. It is a popular choice due to its usability, powerful features, and scalability.

This article will guide you through installing Elasticsearch, configuring it for your use case, securing your installation, and beginning to work with your Elasticsearch server.

# [Prerequisites](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-22-04#prerequisites)[](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-22-04#prerequisites)

Before following this tutorial, you will need:

- An Ubuntu 22.04 server with 2GB RAM and 2 CPUs set up with a non-root sudo user. You can achieve this by following the [Initial Server Setup with Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-22-04)

For this tutorial, we will work with the minimum amount of CPU and RAM required to run Elasticsearch. Note that the amount of CPU, RAM, and storage that your Elasticsearch server will require depends on the volume of logs that you expect.

# [Step 1 — Installing and Configuring Elasticsearch](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-22-04#step-1-installing-and-configuring-elasticsearch)[](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-22-04#step-1-installing-and-configuring-elasticsearch)

The Elasticsearch components are not available in Ubuntu’s default package repositories. They can, however, be installed with APT after adding Elastic’s package source list.

All of the packages are signed with the Elasticsearch signing key in order to protect your system from package spoofing. Packages which have been authenticated using the key will be considered trusted by your package manager. In this step, you will import the Elasticsearch public GPG key and add the Elastic package source list in order to install Elasticsearch.

To begin, use cURL, the command line tool for transferring data with URLs, to import the Elasticsearch public GPG key into APT. Note that we are using the arguments -fsSL to silence all progress and possible errors (except for a server failure) and to allow cURL to make a request on a new location if redirected. Pipe the output to the `gpg --dearmor` command, which converts the key into a format that apt can use to verify downloaded packages.

```
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg
```

Copy

Next, add the Elastic source list to the `sources.list.d` directory, where `apt` will search for new sources:

```
echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
```

Copy

The `[signed-by=/usr/share/keyrings/elastic.gpg]` portion of the file instructs apt to use the key that you downloaded to verify repository and file information for Elasticsearch packages.

Next, update your package lists so APT will read the new Elastic source:

```
sudo apt update
```

Copy

Then install Elasticsearch with this command:

```
sudo apt install elasticsearch
```

Copy

Press `Y` when prompted to confirm installation. If you are prompted to restart any services, press `ENTER` to accept the defaults and continue. Elasticsearch is now installed and ready to be configured.

# [Step 2 — Configuring Elasticsearch](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-22-04#step-2-configuring-elasticsearch)[](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-22-04#step-2-configuring-elasticsearch)

To configure Elasticsearch, we will edit its main configuration file `elasticsearch.yml` where most of its configuration options are stored. This file is located in the `/etc/elasticsearch` directory.

Use your preferred text editor to edit Elasticsearch’s configuration file. Here, we’ll use `nano`:

```
sudo nano /etc/elasticsearch/elasticsearch.yml
```

Copy

**Note:** Elasticsearch’s configuration file is in YAML format, which means that we need to maintain the indentation format. Be sure that you do not add any extra spaces as you edit this file.

The `elasticsearch.yml` file provides configuration options for your cluster, node, paths, memory, network, discovery, and gateway. Most of these options are preconfigured in the file but you can change them according to your needs. For the purposes of our demonstration of a single-server configuration, we will only adjust the settings for the network host.

Elasticsearch listens for traffic from everywhere on port `9200`. You will want to restrict outside access to your Elasticsearch instance to prevent outsiders from reading your data or shutting down your Elasticsearch cluster through its [REST API] ([https://en.wikipedia.org/wiki/Representational_state_transfer](https://en.wikipedia.org/wiki/Representational_state_transfer)). To restrict access and therefore increase security, find the line that specifies `network.host`, uncomment it, and replace its value with `localhost` so it reads like this:

/etc/elasticsearch/elasticsearch.yml

```
. . .
# ---------------------------------- Network -----------------------------------
#
# Set the bind address to a specific IP (IPv4 or IPv6):
#
network.host: localhost
. . .
```

We have specified `localhost` so that Elasticsearch listens on all interfaces and bound IPs. If you want it to listen only on a specific interface, you can specify its IP in place of `localhost`. Save and close `elasticsearch.yml`. If you’re using `nano`, you can do so by pressing `CTRL+X`, followed by `Y` and then `ENTER` .

These are the minimum settings you can start with in order to use Elasticsearch. Now you can start Elasticsearch for the first time.

Start the Elasticsearch service with `systemctl`. Give Elasticsearch a few moments to start up. Otherwise, you may get errors about not being able to connect.

```
sudo systemctl start elasticsearch
```

Copy

Next, run the following command to enable Elasticsearch to start up every time your server boots:

```
sudo systemctl enable elasticsearch
```

Copy

With Elasticsearch enabled upon startup, let’s move on to the next step to discuss security.

# [Step 3 — Securing Elasticsearch](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-22-04#step-3-securing-elasticsearch)[](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-22-04#step-3-securing-elasticsearch)

By default, Elasticsearch can be controlled by anyone who can access the HTTP API. This is not always a security risk because Elasticsearch listens only on the loopback interface (that is, `127.0.0.1`), which can only be accessed locally. Thus, no public access is possible and as long as all server users are trusted, security may not be a major concern.

If you need to allow remote access to the HTTP API, you can limit the network exposure with Ubuntu’s default firewall, UFW. This firewall should already be enabled if you followed the steps in the prerequisite [Initial Server Setup with Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-22-04) tutorial.

We will now configure the firewall to allow access to the default Elasticsearch HTTP API port (TCP 9200) for the trusted remote host, generally the server you are using in a single-server setup, such as`==198.51.100.0==`. To allow access, type the following command:

```
sudo ufw allow from 198.51.100.0 to any port 9200
```

Copy

Once that is complete, you can enable UFW with the command:

```
sudo ufw enable
```

Copy

Finally, check the status of UFW with the following command:

```
sudo ufw status
```

Copy

If you have specified the rules correctly, you should receive output like this:

```
OutputStatus: active

To                         Action      From
--                         ------      ----
9200                       ALLOW      198.51.100.0
22                         ALLOW       Anywhere
22 (v6)                    ALLOW       Anywhere (v6)
```

The UFW should now be enabled and set up to protect Elasticsearch port 9200.

If you want to invest in additional protection, Elasticsearch offers the commercial [Shield plugin](https://www.elastic.co/downloads/shield) for purchase.

# [Step 4 — Testing Elasticsearch](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-22-04#step-4-testing-elasticsearch)[](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-22-04#step-4-testing-elasticsearch)

By now, Elasticsearch should be running on port 9200. You can test it with cURL and a GET request.

```
curl -X GET 'http://localhost:9200'
```

Copy

You should receive the following response:

```
Output{
  "name" : "elastic-22",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "DEKKt_95QL6HLaqS9OkPdQ",
  "version" : {
    "number" : "7.17.1",
    "build_flavor" : "default",
    "build_type" : "deb",
    "build_hash" : "e5acb99f822233d62d6444ce45a4543dc1c8059a",
    "build_date" : "2022-02-23T22:20:54.153567231Z",
    "build_snapshot" : false,
    "lucene_version" : "8.11.1",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}

```

If you receive a response similar to the one above, Elasticsearch is working properly. If not, make sure that you have followed the installation instructions correctly and you have allowed some time for Elasticsearch to fully start.

To perform a more thorough check of Elasticsearch execute the following command:

```
curl -X GET 'http://localhost:9200/_nodes?pretty'
```

Copy

In the output from the above command you can verify all the current settings for the node, cluster, application paths, modules, and more.

# [Step 5 — Using Elasticsearch](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-22-04#step-5-using-elasticsearch)[](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-22-04#step-5-using-elasticsearch)

To start using Elasticsearch, let’s first add some data. Elasticsearch uses a RESTful API, which responds to the usual CRUD commands: **c**reate, **r**ead, **u**pdate, and **d**elete. To work with it, we’ll use the cURL command again.

You can add your first entry like so:

```
curl -XPOST -H "Content-Type: application/json" 'http://localhost:9200/tutorial/helloworld/1' -d '{ "message": "Hello World!" }'
```

Copy

You should receive the following response:

```
Output{"_index":"tutorial","_type":"helloworld","_id":"1","_version":2,"result":"updated","_shards":{"total":2,"successful":1,"failed":0},"_seq_no":1,"_primary_term":1}
```

With cURL, we have sent an HTTP POST request to the Elasticsearch server. The URI of the request was `/tutorial/helloworld/1` with several parameters:

- `tutorial` is the index of the data in Elasticsearch.
- `helloworld` is the type.
- `1` is the ID of our entry under the above index and type.

You can retrieve this first entry with an HTTP GET request.

```
curl -X GET -H "Content-Type: application/json" 'http://localhost:9200/tutorial/helloworld/1'
```

Copy

This should be the resulting output:

```
Output{"_index":"tutorial","_type":"helloworld","_id":"1","_version":1,"found":true,"_source":{ "message": "Hello, World!" }}
```

To modify an existing entry, you can use an HTTP PUT request.

```
curl -X PUT -H "Content-Type: application/json"  'localhost:9200/tutorial/helloworld/1?pretty' -d '
{
  "message": "Hello, People!"
}'
```

Copy

Elasticsearch should acknowledge successful modification like this:

```
Output{
  "_index" : "tutorial",
  "_type" : "helloworld",
  "_id" : "1",
  "_version" : 2,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 1,
  "_primary_term" : 1
}

```

In the above example we have modified the `message` of the first entry to “Hello, People!”. With that, the version number has been automatically increased to `2`.

You may have noticed the extra argument `pretty` in the above request. It enables human-readable format so that you can write each data field on a new row. You can also “prettify” your results when retrieving data to get a more readable output by entering the following command:

```
curl -X GET -H "Content-Type: application/json" 'http://localhost:9200/tutorial/helloworld/1?pretty'
```

Copy

Now the response will be formatted for a human to parse:

```
Output{
  "_index" : "tutorial",
  "_type" : "helloworld",
  "_id" : "1",
  "_version" : 2,
  "_seq_no" : 1,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "message" : "Hello, People!"
  }
}

}
```

We have now added and queried data in Elasticsearch. To learn about the other operations please check [the API documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs.html).

# [Conclusion](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-22-04#conclusion)[](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-22-04#conclusion)

You have now installed, configured, and begun to use Elasticsearch. To further explore Elasticsearch’s functionality, please refer to the official [Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/index.html).
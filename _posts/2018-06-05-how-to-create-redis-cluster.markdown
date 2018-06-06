---
layout: post
title:  "How To Create Redis Cluster"
date:   2018-06-05 12:20:32 -0500
categories: redis
---

### How To Create Redis Cluster

#### The Easy Way
1. Go to redis folder and go into folder <span style="color:Brown">/utils/create-cluster/</span>
2. Run <code>./create-cluster start</code>
3. Run  <code>./create-cluster create</code>
4. Type in <span style="color:Brown">Yes</span> for the suggest cluster configuration.
5. When you see <span style="color:Brown">[OK] All 16384 slots covered</span>, it means your cluster created successfully.
5.  Run <code>./create-cluster stop</code> to stop the cluster

#### The Hard Way
> Which will help you understand the detailed operation

To create a cluster, the first thing we need is to have a few empty Redis instances running in cluster mode. This basically means that clusters are not created using normal Redis instances as a special mode needs to be configured so that the Redis instance will enable the Cluster specific features and commands.

The following is a minimal Redis cluster configuration file:

{% highlight yml %}
port 7000
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
appendonly yes
{% endhighlight %}

As you can see what enables the cluster mode is simply the <span style="color:Brown">cluster-enabled</span> directive. Every instance also contains the path of a file where the configuration for this node is stored, which by default is nodes.conf. This file is never touched by humans; it is simply generated at startup by the Redis Cluster instances, and updated every time it is needed.

Note that the minimal cluster that works as expected requires to contain at least three master nodes. For your first tests it is strongly suggested to start a six nodes cluster with three masters and three slaves.

To do so, enter a new directory, and create the following directories named after the port number of the instance we'll run inside any given directory.

{% highlight yml %}
mkdir cluster-test
cd cluster-test
mkdir 7000 7001 7002 7003 7004 7005
{% endhighlight %}

Create a redis.conf file inside each of the directories, from 7000 to 7005. As a template for your configuration file just use the small example above, but make sure to replace the port number 7000 with the right port number according to the directory name.

Now install redis in your <span style="color:Brown">cluster-test</span> folder by executing below scripts:

{% highlight yml %}
cd cluster-test
wget http://download.redis.io/redis-stable.tar.gz
tar xvzf redis-stable.tar.gz
cd redis-stable
make
{% endhighlight %}

Start each node like this:
{% highlight yml %}
cd 7000
../src/redis-server redis.conf
{% endhighlight %}

As you can see from the logs of every instance, since no nodes.conf file existed, every node assigns itself a new ID.

<span style="color:Brown">[82462] 26 Nov 11:56:55.329 * No cluster configuration found, I'm 97a3a64667477371c4479320d683e4c8db5858b1</span>

This ID will be used forever by this specific instance in order for the instance to have a unique name in the context of the cluster. Every node remembers every other node using this IDs, and not by IP or port. IP addresses and ports may change, but the unique node identifier will never change for all the life of the node. We call this identifier simply Node ID.

**Finally**, we are going to start our cluster !! Now that we have a number of instances running, we need to create our cluster by writing some meaningful configuration to the nodes.

This is very easy to accomplish as we are helped by the Redis Cluster command line utility called <span style="color:Brown">redis-trib</span>, a Ruby program executing special commands on instances in order to create new clusters, check or reshard an existing cluster, and so forth.

The <span style="color:Brown">redis-trib</span> utility is in the src directory of the Redis source code distribution. You need to install redis gem to be able to run <span style="color:Brown">redis-trib</span>.

{% highlight yml %}
gem install redis
{% endhighlight %}

To create your cluster simply type:

{% highlight yml %}
./src/redis-trib.rb create --replicas 1 127.0.0.1:7000 127.0.0.1:7001 \
127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005
{% endhighlight %}

The command used here is create, since we want to create a new cluster. The option <span style="color:Brown">--replicas 1</span> means that we want a slave for every master created. The other arguments are the list of addresses of the instances I want to use to create the new cluster.

Obviously the only setup with our requirements is to create a cluster with 3 masters and 3 slaves.

Redis-trib will propose you a configuration. Accept the proposed configuration by typing yes. The cluster will be configured and joined, which means, instances will be bootstrapped into talking with each other. Finally, if everything went well, you'll see a message like that:

<span style="color:Brown">[OK] All 16384 slots covered</span>

This means that there is at least a master instance serving each of the 16384 slots available.

### Play with your cluster
- **Ping your Cluster**
{% highlight yml %}
redis-cli -c -p 7000 ping
{% endhighlight %}
> You will get a "PONG" if cluster is working fine.

- **Insert Data**
{% highlight yml %}
redis-cli -c -p 7000 set test "test us"
{% endhighlight %}
> OK

- **Get Data**
{% highlight yml %}
redis-cli -c -p 7000 get test
{% endhighlight %}
> "Test US"

- **Insert A Bunch of Data with Bash Loop**
{% highlight yml %}
for i in `seq 1 150`; 
do 
redis-cli -c -p 7000 set "Test$i" "Test US";
done
{% endhighlight %}
> Many OKs
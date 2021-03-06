hello-samza
===========

Hello Samza is a starter project for Samza jobs.

Use Vagrant to get up and running.

1) Install Vagrant [http://www.vagrantup.com/](http://www.vagrantup.com/)  
2) Install Virtual Box [https://www.virtualbox.org/](https://www.virtualbox.org/)  

Then once that is done (or if done already) clone this repository and boot the virtual machine up.
 
    cd hello-samza
    vagrant up  

This will take ~ 10-15 minutes to install Kafka, Hadoop/YARN, Samza, configure everything together and launch the jobs.

Once the VM is launched and you are back at a command prompt go into the virtual machine and see whats running.

    vagrant ssh
    cd /vagrant

The wikipedia-feed Samza job that is running is consuming a feed of real-time edits from Wikipedia, and producing them to a Kafka topic called "wikipedia-raw".  You can view this in real-time by by using the Kafka console consumer to view the topic.

    deploy/kafka/bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic wikipedia-raw

The wikipedia-parser Samza job is then parsing the messages in wikipedia-raw, and extracting information about the size of the edit, who made the change, etc. It outputs these counts to the wikipedia-edits topic.

    deploy/kafka/bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic wikipedia-edits

The wikipedia-stats Samza job reads messages from the wikipedia-edits topic, and calculates counts, every ten seconds, for all edits that were made during that window. It outputs these counts to the wikipedia-stats topic.

    deploy/kafka/bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic wikipedia-stats

You can view the Samza jobs running in the YARN UI http://192.168.80.20:8088/cluster/apps too.

To see how this was setup and works look at `vagrant/bootstrap.sh` and [Hello Samza](http://samza.incubator.apache.org/).
# HiPerf Kafkit

A high performance and useful toolkit for Kafka users, including features as followed:
* Delete topics for Kafka version under 0.8.2.0
* Send messages to brokers efficiently with Kafka Clients API
* Generate data specified by configuration

## Notes
All features above only tested in TDH(<a href="http://www.transwarp.io">Transwarp Data Hub</a>)

## Download
You can download prebuilt binaries from either links below:
![Baidu Yun Cloud Storage](https://pan.baidu.com/s/1sk8A6CH)
![GitHub](https://github.com/bash-horatio/kafkit-binaries)


## Usage
Download prebuilt binaries and extract it somewhere, like `/root`
```sh
tar -zxvf kafkit.tar.gz -C /root
cd /root
```

### Delete topics
If the topic used for StreamSQL, get **consumer group** from WebUI `http://<Stream Server>:4044` first, like kafkit-kafkit.veh_info
```sh
bin/kafkit -c /etc/kafka1/conf/server.properties -a del -t veh_info -g kafkit-kafkit.veh_info
```

Delete in batch when topics and groups lists separated by comma are passed as parameters
```sh
bin/kafkit -c /etc/kafka1/conf/server.properties -a del -t veh_info,perf -g kafkit-kafkit.veh_info,tdh-streamsql.perf
```

Without `-g` given, only topics log files and ZNode will be deleted
```sh
bin/kafkit -c /etc/kafka1/conf/server.properties -a del -t perf
```

Topics log files among hosts are deleted by root through SSH and login passphrases are highly recommended. You can specify other user in `server.properties` by add a line below:
```sh
username=<OTHER USER>
```

Generally Kafka configuration `/etc/kafka1/conf/server.properties` is used to delete topic, you should **copy** it to somewhere, modify and pass it to `bin/kafkit.sh` through `-c`

**IMPORTANT and DANGEROUS**
All topics log files as well as `/brokers/topics` and `/consumers` ZNodes will be deleted if both `-t` and `-g` not given, which removes all data of Kafka
bin/kafkit -c /etc/kafka1/conf/server.properties -a del

### High Performance Producer
Change values of some items in `conf/producer.properties`
* bootstrap.server
* topic
Hostnames are highly recommended for `bootstrap.server`, especially when Kerberos enabled. If you want to use HiPerf Kafkit in your PC, you should add all broker hostnames in `/etc/hosts` or `C:\\System32\drivers\etc\hosts`

By default `files` is commented, and random-generated messages will be sent to borkers. Comment it out and specify it to the files you want to send. Comma-separated file lists are supported. After modification, just start sending by typing
```sh
bin/kafkit -c conf/producer.properties -a pro
```

### Data Generator
Only **vehicle infomation(blacklist)** supported now. Adjust values in `conf/generator.properties` accordingly, and execute
```sh
bin/kafkit -c conf/generator.properties -a gen
````


## Compilation
### Prerequisites:
* JDK 1.7+
* Scala 2.10+
* Apache Maven 3+
* Internet access

### Build
```sh
git clone http://gitlab-ci-token:mMjxpesFnv6vqGSAg85V@172.16.1.41:10080/horatio/hiperf-kafkit.git
cd hiperf-kafkit
bin/build.sh [-Pshade]
```


## Contribution
- Hiperf Kafkit is a free non-profitable hobbie project. Please don't expect immediate reaction on issues.
- If you have any questions, suggestions, ideas and so on, just create an issue here.
- Pull requests are welcome.
- Visit <a href="https://docs.transwarp.cn/4.7/StreamSQLManual-chunked.html?iframe=true">Transwarp Transpedia Website</a> for more information about Transwarp Stream and Kafka.
- Thanks for using HiPerf Kafkit!

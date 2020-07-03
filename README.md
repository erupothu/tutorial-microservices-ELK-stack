# tutorial-microservices-ELK-stack

<b>Install in ubuntu</b><br>

wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add - <br>
sudo apt-get install apt-transport-https <br>
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list <br>
sudo apt-get update && sudo apt-get install elasticsearch <br>
sudo apt-get update && sudo apt-get install kibana <br>
sudo apt-get update && sudo apt-get install logstash <br>

cd /etc/logstash/conf.d

nano logstash.conf (for a given log file) or
nano logstash-system.conf (for spring boot microservices direct connect to logstash @tcp://127.0.0.1:25826)

sudo service elasticsearch start <br>
sudo service kibana start <br>
sudo service logstash start <br>

http://localhost:5601/ <br>
http://localhost:9200 <br>
stack management

> stack management add index pattern -> create index pattern -> logstash-* -> save <br>

Discover <br>
change index pattern to logstash-* <br>

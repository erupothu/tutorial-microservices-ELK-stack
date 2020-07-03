# tutorial-microservices-ELK-stack

<b>Install in ubuntu</b><br>
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add - <br>
sudo apt-get install apt-transport-https <br>
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list <br>
sudo apt-get update && sudo apt-get install elasticsearch <br>
sudo apt-get update && sudo apt-get install kibana <br>
sudo apt-get update && sudo apt-get install logstash <br>
cd /etc/logstash/conf.d
nano logstash-config.conf
>
input {
  file {
    type => "java"
    path => "/home/user/log/book-log2.log"
    codec => multiline {
      pattern => "^%{YEAR}-%{MONTHNUM}-%{MONTHDAY} %{TIME}.*"
      negate => "true"
      what => "previous"
    }
  }
}

filter {
  if [message] =~ "\tat" {
    grok {
      match => ["message", "^(\tat)"]
      add_tag => ["stacktrace"]
    }
  }
 grok {
    match => [ "message",
               "(?<timestamp>%{YEAR}-%{MONTHNUM}-%{MONTHDAY} %{TIME})  %{LOGLEVEL:level} %{NUMBER:pid} --- \[(?<thread>[A-Za-z0-9-]+)\] [A-Za-z0-9.]*\.(?<class>[A-Za-z0-9#_]+)\s*:\s+(?<logmessage>.*)",
               "message",
               "(?<timestamp>%{YEAR}-%{MONTHNUM}-%{MONTHDAY} %{TIME})  %{LOGLEVEL:level} %{NUMBER:pid} --- .+? :\s+(?<logmessage>.*)"
             ]
  }
  date {
    match => [ "timestamp" , "yyyy-MM-dd HH:mm:ss.SSS" ]
  }
}

output {

  stdout {
    codec => rubydebug
  }
  elasticsearch {
    hosts => ["localhost:9200"]
  }
}

sudo service elasticsearch start
sudo service kibana start
sudo service logstash start

http://localhost:5601/ <br>
stack management

> stack management add index pattern -> create index pattern -> logstash-* -> save <br>

Discover <br>
change index pattern to logstash-* <br>

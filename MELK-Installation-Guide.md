# MELK Installation Guide

## Install Golang

Metricbeat cannot be installed directly, as Elastic currently doesnt natively support ARM architectures. This means compiling Metricbeat from source, which requires Golang 1.8.

```bash
wget https://storage.googleapis.com/golang/go1.8.linux-armv6l.tar.gz 
tar -C /usr/local -xzf go1.8.linux-armv6l.tar.gz 
export PATH=$PATH:/usr/local/go/bin 
apt install -t golang 
pip install virtualenv 
sudo nano /etc/dphys-swapfile 
edit the following line: 'CONF_SWAPSIZE=100' to be 'CONF_SWAPSIZE=1024'
sudo dphys-swapfile setup
sudo dphys-swapfile swapon
```
## Metricbeat on PIs

### Install Metricbeat

```bash
go get github.com/elastic/beats (yes, go is a command now. golang will complain, ignore this)
cd ~/go/src/github.com/elastic/beats/metricbeat/
git checkout 6.0
GOPATH=~/go make (this will take a while)
```

### Configure Metricbeat

```bash
nano ~/go/src/github.com/elastic/beats/metricbeat/metricbeat.yml
    - under section 'Elasticsearch output', comment 'output.elasticsearch:' and 'hosts: ["localhost:9200"]'
    - under section 'Logstash output', uncomment 'output.logstash:' and 'hosts: ["localhost:5044"]'
    - under section 'Logstash output', set 'hosts:' to '[localhost:9200]'
```

---

## Logstash on x68 Host

### Install Logstash

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list
sudo apt-get update && sudo apt-get install logstash
```

### Configure Logstash

```bash
touch /etc/logstash/conf.d/pipeline.conf
nano /etc/logstash/conf.d/pipeline.conf
    - add the following code to the file:
    input {
        beats {
            port => 9200
            }
        }

    output {
        elasticsearch {
            hosts => "localhost:9202"
            manage_template => false
            index => "%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"
        }
    file {
        path => "/var/log/logstash/%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"
        }
    }
```

---

## Elasticsearch on x68 Host

### Install Elasticsearch

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list
apt update && apt install elasticsearch
```

### Configure Elasticsearch

```
nano /etc/elasticsearch/elasticsearch.yml
    - uncomment 'network.host: “<some_ip_address>”' and 'http.port:9200'
    - set 'network.host:' to '0.0.0.0' and 'http.port:' to '9202'
``` 

---

## Kibana on x68 Host

### Install Kibana

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add - 
echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list 
apt update && apt install kibana
```

### Configure Kibana

```bash
nano /etc/kibana/kibana.yml
    - uncomment 'server.port: 5601' and 'elasticsearch.url: “http://localhost:9200”'
    - set 'server.port:' to '9203' and  'elasticsearch.url:' to '“http://localhost:9202”'
```

### Add Kibana Template

This only needs to be done once, regardless of the amount of nodes present in the setup

```bash
cd ~/go/src/github.com/elastic/beats/metricbeat/
./metricbeat setup -e -E output.logstash.enabled=false -E output.elasticsearch.hosts=['localhost:9999'] -E setup.dashboards.enabled:=true -E setup.dashboards.directory=/root/go/src/github.com/elastic/beats/metricbeat/module/system/_meta/kibana -E setup.kibana.host=localhost:9998
```

### Dashboard Configuration

Visualizing data in Kibana can be done in a variety of different ways, but the dashboard is what allows users to create an overview of multiple data sets as a way to compare the datasets. Creating visualizations in Kibana can be as easy or advanced as the user wants.

Below is a step-by-step guide on how to recreate the relevant graphs used in the O2-Balancer experiments.


#### System Load

1. Go to 'Visualize'
2. Click the '+' button to create a new visualization
3. Under 'Time Series', select 'Visual Builder'
4. Go to the tab 'Data' and subtab 'Metrics'
5. Select Aggregation type 'Average'
6. in 'Field', search for 'system.load.1' and select it
7. Select Group by 'Terms'
8. Assuming the metrics concern multiple devices, in the 'By' field, select 'host.keyword'
9. Select Order by 'Terms'
10. Go to subtab 'Options' 
11. Make sure the variables correspond to the following:
    - Data Formatter: Custom
    - Format String: '0%'
    - Chart Type: Line
    - Stacked: Stacked
    - Fill: 0.5
    - Line Width: 1
    - Point Size: 1
    - Split Color Theme: Rainbow
12. In the top toolbar, click 'Save' and name the visualization 'System Load'

#### Total network inbound in KB/s

1. Go to 'Visualize'
2. Search and select 'System Load'
3. Go to tab 'Data' and subtab 'Metrics'
4. In 'Field', search for 'system.network.in.bytes' and select it
5. Set 'Group by' to 'Everything'
6. Go to subtab 'Options'
7. Set 'Format String' to '0.00 b'
8. Set 'Template' to {{value}}/s
9. In the top toolbar, click 'Save' and select 'Save as new Visualization'
10. Rename to 'Total network inbound'

#### Total network outbound in KB/s

1. Go to 'Visualize'
2. Search and select 'Total network inbound'
3. Go to tab 'Data' and subtab 'Metrics'
4. In 'Field', search for 'system.network.out.bytes' and select it
5. In the top toolbar, click 'Save' and select 'Save as new Visualization'
6. Rename to 'Total network outbound'

#### Per PI network inbound in KB/s

1. Go to 'Visualize'
2. Search and select 'Total network inbound'
3. Go to tab 'Data' and subtab 'Metrics'
4. Select Group by 'Terms'
5. In the 'By' field, select 'host.keyword'
6. In the top toolbar, click 'Save' and select 'Save as new Visualization'
7. Rename to 'Per PI network inbound'

#### Per PI network outbound in KB/s

1. Go to 'Visualize'
2. Search and select 'Total network outbound'
3. Go to tab 'Data' and subtab 'Metrics'
5. Select Group by 'Terms'
6. In the 'By' field, select 'host.keyword'
7. In the top toolbar, click 'Save' and select 'Save as new Visualization'
8. Rename to 'Per PI network inbound'

#### Add to Dashboard

After all required visualizations have been created, they can be added to the dashboard as follows:
1. Go to the desired dashboard
2. In the top toolbar, click 'Edit'
3. In the top toolbar, click 'Add'
4. Search for the desired visualizations and only click once to add to the dashboard

---

## Prepare devices for runs

### Start ICN

```bash
systemctl start logstash.service
service elasticsearch start
service kibana start
```

### Start PIs

```bash
cd ~/go/src/github.com/elastic/beats/metricbeat/
./metricbeat -e
ps aux | grep metric
sudo renice -n 19 -p PROCESS_ID
```

### Access Kibana on local machine

```bash
ssh -L 9999:localhost:9203 pi@pi.dantalion.nl -p 6625
IN BROWSER: http://localhost:9999
Clear data
curl -XDELETE 'http://localhost:9202/*'
rm /var/log/logstash/metricbeat-*
```
#Elastic Search

* Ubuntu installation (vm in virtual box)
* Elastic search runs on java, so java installation is needed
* ports
  * elasticsearch 9200
  * kibana  5601
  * ssh 22

* elasticsearch set up
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list
sudo apt-get update && sudo apt-get install elasticsearch
* elasticsearch settings
sudo vi /etc/elasticsearch/elasticsearch.yml
* change the network host settings
*
### oic-job-scheduler

This post describes oic-project[1] cron used to schedule jobs to run periodically at fixed times. Here are the list of jobs:
- Start hosts
- Stop hosts
- Start HUE
- Start Jupyter
- Start Ambari services


> Start cluster hosts at 9:01 `_Monitoring host [instance-1]_`  

```sh
# Configure crontab
export VISUAL=nano
crontab -e 

# Add the following content, save and quit
1 7 * * * gcloud compute instances start hadoop-m --zone europe-west1-b
1 7 * * * gcloud compute instances start hadoop-w-0 --zone europe-west1-b
1 7 * * * gcloud compute instances start hadoop-w-1 --zone europe-west1-b
1 7 * * * gcloud compute instances start hadoop-w-2 --zone europe-west1-b
1 7 * * * gcloud compute instances start hadoop-w-3 --zone europe-west1-b

``` 

> Stop cluster hosts after 20:00:00 `_Monitoring host [instance-1]_`  

```sh
# Configure crontab
export VISUAL=nano
crontab -e 

# Add the following content, save and quit
1 18 * * * gcloud compute instances stop hadoop-m --zone europe-west1-b
1 18 * * * gcloud compute instances stop hadoop-w-0 --zone europe-west1-b
1 18 * * * gcloud compute instances stop hadoop-w-1 --zone europe-west1-b
1 18 * * * gcloud compute instances stop hadoop-w-2 --zone europe-west1-b
1 18 * * * gcloud compute instances stop hadoop-w-3 --zone europe-west1-b

``` 

> Start HUE on startup `_HDP Client Node [hadoop-w-3]_`  

```sh
# Configure crontab
export VISUAL=nano
crontab -e 

# Add the following content, save and quit
@reboot sudo /etc/init.d/hue start

``` 

> Start Jupyter on startup `_HDP Client Node [hadoop-w-3]_`  

```sh
# Configure crontab
export VISUAL=nano
crontab -e 

# Add the following content, save and quit
@reboot nohup sudo /usr/bin/python3.4 -m jupyterhub -f /home/agambo/jupyterhub_config.py &

``` 

> Start Ambari services on cluster startup `_HDP Master Node [hadoop-m]_`

```sh
# Create start_ambari_services.sh
cat <<EOF | tee /home/agambo/start_ambari_services.sh
# Run some ambari service on start up
curl -u admin:admin -i -H 'X-Requested-By: ambari' -X PUT -d '{"RequestInfo": {"context" :"Start HDFS via REST"}, "Body": {"ServiceInfo":{"state": "STARTED"}}}' http://localhost:8080/api/v1/clusters/hadoop/services/HDFS 
curl -u admin:admin -i -H 'X-Requested-By: ambari' -X PUT -d '{"RequestInfo": {"context" :"Start YARN via REST"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' http://localhost:8080/api/v1/clusters/hadoop/services/YARN
curl -u admin:admin -i -H 'X-Requested-By: ambari' -X PUT -d '{"RequestInfo": {"context" :"Start MAPREDUCE2 via REST"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' http://localhost:8080/api/v1/clusters/hadoop/services/MAPREDUCE2
curl -u admin:admin -i -H 'X-Requested-By: ambari' -X PUT -d '{"RequestInfo": {"context" :"Start HIVE via REST"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' http://localhost:8080/api/v1/clusters/hadoop/services/HIVE
curl -u admin:admin -i -H 'X-Requested-By: ambari' -X PUT -d '{"RequestInfo": {"context" :"Start TEZ via REST"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' http://localhost:8080/api/v1/clusters/hadoop/services/TEZ
curl -u admin:admin -i -H 'X-Requested-By: ambari' -X PUT -d '{"RequestInfo": {"context" :"Start ZOOKEEPER via REST"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' http://localhost:8080/api/v1/clusters/hadoop/services/ZOOKEEPER
curl -u admin:admin -i -H 'X-Requested-By: ambari' -X PUT -d '{"RequestInfo": {"context" :"Start OOZIE via REST"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' http://localhost:8080/api/v1/clusters/hadoop/services/OOZIE
curl -u admin:admin -i -H 'X-Requested-By: ambari' -X PUT -d '{"RequestInfo": {"context" :"Start SPARK via REST"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' http://localhost:8080/api/v1/clusters/hadoop/services/SPARK
EOF

# Configure crontab
export VISUAL=nano
crontab -e 

# Add the following content, save and quit
@reboot sleep 120; /home/agambo/start_ambari_services.sh

``` 




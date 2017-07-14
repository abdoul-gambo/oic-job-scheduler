### oic-job-scheduler

This post describes oic-project[1] cron used to schedule jobs to run periodically at fixed times. Here are the list of jobs:
- Start hosts
- Shutdown hosts
- Start Ambari services
- 


> Start Ambari services on cluster startup `_HDP Master Node [hadoop-m]_`

```sh
# Create start_ambari_services.sh
cat <<EOF | tee /home/agambo/start_ambari_services.sh
# Run some ambari service on start up
curl -u admin:admin -i -H 'X-Requested-By: ambari' -X PUT -d '{"RequestInfo": {"context" :"Start HDFS via REST"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' http
://localhost:8080/api/v1/clusters/hadoop/services/HDFS 
curl -u admin:admin -i -H 'X-Requested-By: ambari' -X PUT -d '{"RequestInfo": {"context" :"Start YARN via REST"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' http
://localhost:8080/api/v1/clusters/hadoop/services/YARN
curl -u admin:admin -i -H 'X-Requested-By: ambari' -X PUT -d '{"RequestInfo": {"context" :"Start MAPREDUCE2 via REST"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}
' http://localhost:8080/api/v1/clusters/hadoop/services/MAPREDUCE2
curl -u admin:admin -i -H 'X-Requested-By: ambari' -X PUT -d '{"RequestInfo": {"context" :"Start HIVE via REST"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' http
://localhost:8080/api/v1/clusters/hadoop/services/HIVE
curl -u admin:admin -i -H 'X-Requested-By: ambari' -X PUT -d '{"RequestInfo": {"context" :"Start TEZ via REST"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' http:
//localhost:8080/api/v1/clusters/hadoop/services/TEZ
curl -u admin:admin -i -H 'X-Requested-By: ambari' -X PUT -d '{"RequestInfo": {"context" :"Start ZOOKEEPER via REST"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}'
 http://localhost:8080/api/v1/clusters/hadoop/services/ZOOKEEPER
curl -u admin:admin -i -H 'X-Requested-By: ambari' -X PUT -d '{"RequestInfo": {"context" :"Start OOZIE via REST"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' htt
p://localhost:8080/api/v1/clusters/hadoop/services/OOZIE
curl -u admin:admin -i -H 'X-Requested-By: ambari' -X PUT -d '{"RequestInfo": {"context" :"Start SPARK via REST"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' htt
p://localhost:8080/api/v1/clusters/hadoop/services/SPARK
EOF

# Configure crontab
export VISUAL=nano
crontab -e 

# Add the following content, save and quit
@reboot sleep 120; /home/agambo/start_ambari_services.sh

``` 

> dd  



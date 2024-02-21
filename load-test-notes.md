
# Compliance
## Load Test Plan
https://docs.google.com/spreadsheets/d/135BPSYdMWjkjTlcH7h_oVDhjQ3jHZ6EHPMlyr6iKtdM/edit#gid=0

```
eval $(minikube docker-env)

docker build --no-cache -t apiwiz/compliance-api-checker:latest .

kubectl port-forward compliance-api-checker-588d8b9475-pdp29 9010:9010

```
Check OS Name
```
cat /etc/os-release
```

top -H -p <processId>
ps -T -p <processId>
vmstat
cat /proc/<processId>/task/<threadId>/status

get the number of available processor
```
nproc
```

## Identified Issues
- org.sqlite.SQLiteException: [SQLITE_BUSY]  The database file is locked
- Don't calculate the pod IP everytime the detect endpoint is called
- Make the Mongo DB updates async and provide the service call out response immediately
- Spot Bug bug items
- Reports Pagination is done at app instead of DB
- Incorrect usage of zgc and use vm flags in Dev
- returning Object instead of actual type
- null getDrilledDownDashboardReport dead empty null code
- Move all static Queries to constant  
- Too many loggers
- String requestId remove dead code
- CheckRunner move Content-Type String to static
- Use Cursor Pagination
- MongoDB primary read
- OpenAPI parsing the string multiple times
- Decrease Fixed Rate from 5000 to 500

## Compliance Code Business Logic Flow

### Encryption/Decryption
- get the compliance request from MongoDB


- Check if encryption is enabled
- Get Encryption Key
- Decrypt the encrypted key and generate key - Why is it required?
- decrypt the request body
- decrypt the response body
Shadow Spec
- **Find all custom masking from MongoDB**
- *Use for loop and add all the Masking Key*
- *Using for loop to unmask request header*
- *Using for loop to unmask response header*
- **Get Workspace Integration for appName header from MongoDB**
- Security Helper to get security report
- *trigger portfolio incident*
- *trigger seccruity failure notification*
Matched Spec
- **Find all custom masking from MongoDB**
- *Use for loop and add all the Masking Key*
- *Using for loop to unmask request header*
- *Using for loop to unmask response header*
- **Get Workspace Integration for appName header from MongoDB**
- Security Helper to get security report
- *trigger portfolio incident*
- *trigger security failure notification*


### Find all the matching specs
- Identify Shadow spec

## TODO
* Identify the compatible EC2 instance type
    Going ahead with General Purpose
    
* HZ and other concurrent distributed queue analysis
* Relation with threads, processors and CPU
    * How many number of threads created and allowed?
    https://stackoverflow.com/questions/7726871/maximum-number-of-threads-in-a-jvm

    Calculation,
    Get the thread stack size,
    java -XX:+PrintFlagsFinal -version | grep -iE 'ThreadStackSize'
    Let's say stack size is 1024bytes ie 1MB, then for a memory allocated say 1000MB, max 1000 threads can be created
    The number of process limit mentioned in the OS also is another factor.
    ```
    ulimit -a
    cat /proc/sys/kernel/pid_max
    cat /proc/sys/kernel/threads-max
    ```
    * How to see the total number of threads and it's current status?
        ```
        top -H -p <processId>    
        ls /proc/<processId>/task/
        cat /proc/<processId>/task/<threadId>/status
        ```

* Java Container way of Calculating threads to assign
    * https://mucahit.io/2020/01/27/finding-ideal-jvm-thread-pool-size-with-kubernetes-and-docker/
    * https://pretius.com/blog/jvm-kubernetes/
    * https://piotrminkowski.com/2023/02/13/best-practices-for-java-apps-on-kubernetes/
* Spring Boot Start up time
    * https://github.com/spring-projects/spring-boot/issues/11942
    * https://github.com/lwaddicor/spring-startup-analysis
    * https://www.baeldung.com/java-profilers
    * https://medium.com/kayvan-kaseb/tips-and-tricks-for-profiling-your-spring-boot-app-using-visualvm-de627cae02d1
    * https://phauer.com/2019/no-fat-jar-in-docker-image/
    * https://www.amitph.com/spring-boot-startup-monitoring/
    * https://www.reddit.com/r/java/comments/10ithz3/how_to_know_why_my_spring_boot_app_is_slower_on/

    * https://medium.com/springboot-chronicles/how-we-managed-to-bring-down-memory-footprint-of-our-spring-boot-micro-services-a-case-study-b41dd94e6c27
    * https://spring.io/blog/2018/12/12/how-fast-is-spring
    * https://bharathappali.medium.com/boost-your-java-application-startup-with-eclipse-openj9-scc-12f7f545a2d2
    * https://heidloff.net/article/openj9-jvm-for-quarkus-applications/
    * https://mirakl.tech/boosting-spring-boot-application-startup-time-a-practical-guide-e7d777450e1f
    * https://medium.com/@jean_sossmeier/spring-boot-jvm-1eea422be930


* Code Review Checker


## References
minikube service compliance-checker-service --url

https://mucahit.io/2020/01/27/finding-ideal-jvm-thread-pool-size-with-kubernetes-and-docker/


## Challenges & Learnings
Use Labelling wherever it's required
Primary for read and secondary for write
Overall Count is slower in mongoDB
Using UnionWith to make join on 
Cursor Pagination
        // Page 1
    db.students.find().limit(10)

    // Page 2
    last_id = ...  # logic to get last_id
    db.students.find({'_id': {'$gt': last_id}}).limit(10)

    // Page 3
    last_id = ... # logic to get last_id
    db.students.find({'_id': {'$gt': last_id}}).limit(10)
Avoid Overfetching of data in the rest API
Scheduled Thread



## Compliance Load Test Clean Up Scripts

sudo docker build -t gcr.io/apiwiz-nonprod/checker-openj9:v4 . --no-cache
sudo docker push gcr.io/apiwiz-nonprod/checker-openj9:v4

kubectl -n apiwiz-system edit deployment api-checker


kubectl -n apiwiz-system get pods | grep api-checker

use acme-team-test-performance
db.API.Compliance.App.Resource.Dashboard.drop()
db.API.Compliance.DashboardV2.drop()
db.API.Compliance.Partner.Dashboard.drop()
db.API.Compliance.Report.drop()
db.API.Compliance.Request.drop()
db.API.Compliance.Resource.Dashboard.drop()


apk add sqlite
sqlite3 ./apiwiz-compliance-checker.db
delete from compliance_detect_executor;

cd /data
sqlite3 ./apiwiz-compliance-checker.db
delete from compliance_detect_executor;

>api-checker.log
>api-checker-error.log
>api-checker-console.log

select count(1) from compliance_detect_executor;

watch 'kubectl -n apiwiz-system top po  | grep checker

grep 'end:updateStatus :' api-checker.log
grep 'begin:run Check :' api-checker.log

db.API.Compliance.Report.find().count()
db.API.Compliance.Request.find({'_id':ObjectId('646a59e6f9021b219d0a88d2')})



rm -r api-checker*.2023*.log


cp -r logs logs1500
 
 
select count(*) from compliance_detect_executor where status!='Completed';
select * from compliance_detect_executor where status!='Completed';

646a59e6f9021b219d0a88d2


scp target/api-checker-performance-test.jar divakarv@34.125.35.90:/home/divakarv/



db.API.Compliance.Request.find().count()




grep 'end:updateStatus :'   api-checker.log | tail -1700
grep 'begin:run Check :'   api-checker.log | tail -1500

grep 'end:updateStatus :'   api-checker.log | tail -1700

grep 'begin:run Check :'   api-checker.2023-05-21.8.log | tail -2000
grep 'end:updateStatus :'   api-checker.2023-05-21.7.log | tail -1900
grep 'end:updateStatus exception' api-checker.2023-05-21.7.log | tail -1900

grep 'end:updateStatus :' api-checker.2023-05-21.6.log | tail -1700

grep 'end:updateStatus :' api-checker-console.2023-05-21.9.log | tail -2000
grep 'end:updateStatus :'   api-checker.log | tail -2000

grep 'end:updateStatus  exception:'  api-checker.log | tail -2000

grep 'end:updateStatus ' api-checker.2023-05-21.10.log | tail -2500
grep 'end:updateStatus  exception:' api-checker.2023-05-21.0.log | tail -2000


nohup taskset -c 0  java -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.local.only=false -Dcom.sun.management.jmxremote.port=9010 -Dcom.sun.management.jmxremote.rmi.port=9010 -Djava.rmi.server.hostname=34.125.35.90  -Dconfig.properties=config.properties -Xmx1024m -Xms128m -XX:MaxRAM=1024m  -jar ./api-checker-performance-test.jar > nohup.log &
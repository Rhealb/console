# Getting Started
## Prerequisites
- Java 8
- MySQL

## Run it locally
- Install MySQL
- Setup MySQL
    - username: root
    - password: root
    - create database "cc"
- Run application without proxy:
```
./gradlew bootRun -x test
```
- Run application with proxy:
```
./gradlew -Dhttp.proxyHost=web-proxy.atl.hpecorp.net -Dhttp.proxyPort=8080 -Dhttps.proxyHost=web-proxy.atl.hpecorp.net -Dhttps.proxyPort=8080 bootRun -x test
```


## Build Docker Image
```
git clone ssh://git@218.92.191.5:31100/cc/cc-backend.git
cd cc-backend
sudo docker login
sudo ./gradlew build -x test buildDocker
```

## Run with Docker
```
sudo docker run -e spring_profiles_active=prod \
-e MYSQL_HOST=10.19.132.199 \
-e MYSQL_PORT=3306 \
-e MYSQL_USERNAME=root \
-e MYSQL_PASSWORD=CHANGE_ME \
-e CC_AUTH_URL=http://10.19.132.199:32201/api/v1 \
-e K8S_MASTER_URL=https://10.19.140.200:6443 \
-e K8S_ADMIN_USERNAME=admin \
-e K8S_ADMIN_PASSWORD=CHANGE_ME \
-e K8S_FILTERED_NAMESPACES=default,kube-system \
-e RABBITMQ_HOST=localhost \
-e RABBITMQ_PORT=5672 \
-e RABBITMQ_USERNAME=admin \
-e RABBITMQ_PASSWORD=CHANGE_ME \
-e OPENTSDB_URL=http://10.19.248.12:30142/api \
-p 8800:8800 \
-t 10.19.140.200:30100/console/cc-backend:latest
```

## Access the Actuator APIs
```
curl http://localhost:8080/system/health
```

## Access Backend Application APIs
```
curl http://localhost:8080/api/v1/health
```


## Env
```
  - name: JVM_MAX_HEAP
    value: {{ jvm_max_heap }}
  - name: JVM_XMS
    value: {{ jvm_xms }}
  - name: "MYSQL_HOST"
    value: "cc-mysql"
  - name: "MYSQL_PORT"
    value: "3306"
  - name: "MYSQL_USERNAME"
    value: "root"
  - name: "MYSQL_PASSWORD"
    value: "root"
  - name: "CC_AUTH_URL"
    value: "http://cc-auth:8801/api/v1"
  - name: "K8S_MASTER_URL"
    value: {{ master_addr }}
  - name: "K8S_ADMIN_USERNAME"
    value: {{ admin_username }}
  - name: "K8S_ADMIN_PASSWORD"
    value: {{ admin_password }}
  - name: "K8S_FILTERED_NAMESPACES"
    value: "default,kube-system"
  - name: "RABBITMQ_HOST"
    value: {{ rabbitmq_host }}
  - name: "RABBITMQ_PORT"
    value: "{{ rabbitmq_port }}"
  - name: "RABBITMQ_USERNAME"
    value: {{ rabbitmq_username }}
  - name: "RABBITMQ_PASSWORD"
    value: {{ rabbitmq_password }}
  - name: "OPENTSDB_URL"
    value: "http://10.19.140.200:30142/api"
  - name: "CEPH_MONITORS"
    value: {{ monitor_urls }}
  - name: "CEPH_MON"
    value: {{ monitor_urls }}
  - name: "KEYSTONE_URL"
    value: {{ keystone_url }}
  - name: "KEYSTONE_ADMIN_USERNAME"
    value: {{ keystone_admin_username }}
  - name: "KEYSTONE_ADMIN_PASSWORD"
    value: {{ keystone_admin_password}}
  - name: "CEPH_ENABLED"
    value: "false"
  - name: "NFS_ENABLED"
    value: "true"
  - name: "EBS_ENABLED"
    value: "true"
  - name: "EBS_REGION"
    value: {{ ebs_region }}
  - name: "EBS_ZONES"
    value: {{ ebs_zone }}
  - name: "NFS_ADDRESS"
    value: {{ nfs_url }}
  - name: "K8S_VIP_HOST"
    value: "{{ ca_vip }}"
  - name: "spring_profiles_active"
    value: "prod"
    
```
- MYSQL_HOST: mysql host, please use default
- MYSQL_PORT: mysql port, please use default
- MYSQL_USERNAME: mysql username, please use default
- MYSQL_PASSWORD: mysql password, please use default
- RABBITMQ_HOST: rabbitmq host, please use default
- RABBITMQ_PORT: rabbitmq port, please use default
- RABBITMQ_USERNAME: rabbitmq user name, please use default
- RABBITMQ_PASSWORD: rabbitmq password, please use default
- RABBITMQ_LISTENER_RETRY_ENABLED: whether rabbitmq retry enabled, default true
- RABBITMQ_LISTENER_RETRY_MAX_ATTEMPTS: rabbitmq retry max attempt times, default 5
- RABBITMQ_LISTENER_RETRY_INITIAL_INTERVAL: rabbitmq retry initial interval, default 5000
- RABBITMQ_LISTENER_RETRY_MULTIPLIER: rabbitmq retry multiplier, default 5
- RABBITMQ_LISTENER_RETRY_MAX_INTERVAL: rabbitmq retry max interval, default 60000
- REDIS_HOST: redis host, please use default
- REDIS_PORT: redis port, please use default
- REDIS_PASSWORD: redis password, please use default

- K8S_MASTER_URL: the k8s master url
- K8S_ADMIN_USERNAME: the k8s admin username
- K8S_ADMIN_PASSWORD: the k8s admin password
- K8S_VIP_HOST: the vip host
- K8S_FILTERED_NAMESPACES: namespace should be filtered for system using, please use default
- K8S_LOGIN_WITH_TOKEN: is login with bearer token, default false
- K8S_BEARER_TOKEN: the k8s login token (working on K8S_LOGIN_WITH_TOKEN is true)


- ALI_NODE_AUTO_SCALE_URL: ali yun node auto scale url
- ALI_NODE_AUTO_SCALE_USERNAME: ali yun node auto scale username
- ALI_NODE_AUTO_SCALE_PASSWORD: ali yun node auto scale password

- KEYSTONE_URL: keystone url 
- KEYSTONE_ADMIN_USERNAME: keystone admin user name
- KEYSTONE_ADMIN_PASSWORD: keystone admin password
- KEYSTONE_CACHE_ENABLED: whether keystone cache enabled, default true
- KEYSTONE_ADMIN_PROJECT: keystone admin project for filter
- KEYSTONE_TOKEN_EXPIRE_MINUTES: keystone token expire minutes, default 50

- AUTH_ENABLED: whether auth enabled
- NAMESPACE_STATUS_CHECK_PERIOD_MILLIS: namespace status check period millis for namespace event sourcing, default 60000

- CEPH_ENABLED: whether ceph enabled, default true
- CEPH_DIR_PREFIX: namespace ceph dir prefix, please use default
- CEPH_MON: ceph monitors host list, divided by comma

- NFS_ENABLED: whether nfs enabled, default false
- NFS_ADDRESS: nfs server address
- NFS_SERVER_DIR: nfs server shared dir
- NFS_DIR_PREFIX: namespace nfs dir prefix, please use default

- EBS_ENABLED: whether aws enabled, default false
- EBS_TOTAL_BYTES: aws ebs total bytes
- EBS_ACCESS_KEY: aws access key
- EBS_SECRET_KEY: aws secret key
- EBS_REGION: ebs create region
- EBS_ZONES: ebs create zone

- HTTP_CONNECT_TIMEOUT_MILLIS: http request connect timeout, default 60000
- HTTP_READ_TIMEOUT_MILLIS: http request read timeout, default 60000
- HTTP_WRITE_TIMEOUT_MILLIS: http request write timeout, default 60000

- OPENTSDB_URL: the opentsdb url 
- OPENTSDB_CLUSTER_NAME: the opentsdb cluster name
- OPENTSDB_INTERVAL_SECONDS: the opentsdb delay seconds, default 20

- SCHEDULER_HOST: the k8s node and namespace label scheduler host
- SCHEDULER_PORT: the k8s node and namespace label scheduler port
- SCHEDULER_USERNAME: the k8s node and namespace label scheduler username
- SCHEDULER_PASSWORD: the k8s node and namespace label scheduler password

- CC_TEMPLATE: the cc template url
- DEPENDENCY_CHECKER: the dependency checker url


- ENN_SECURITY_ENABLE: whether enable the security for gateway and default true   
- ENN_SECURITY_RECEIVE: security receive type, please use default ACCOUNT
- ENN_SECURITY_CLIENT_CONNECT_TIMEOUT_MILLIS: security http request connect timeout, default 60000
- ENN_SECURITY_CLIENT_READ_TIMEOUT_MILLIS: security http request read timeout, default 60000
- ENN_SECURITY_CLIENT_WRITE_TIMEOUT_MILLIS: security http request write timeout, default 60000

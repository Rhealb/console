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
git clone ssh://git@218.92.191.5:31100/cc/cc-terminal.git
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
-t 10.19.140.200:30100/console/cc-terminal:latest
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
  - name: "K8S_MASTER_HOST"
    value: "{{ca_vip}}"
  - name: "K8S_MASTER_PORT"
    value: "6443"
  - name: "spring_profiles_active"
    value: "prod"
  - name: JVM_MAX_HEAP
    value: 4096m
  - name: K8S_ADMIN_PASSWORD
    value: {{ admin_password }}
  - name: K8S_ADMIN_USERNAME
    value: {{ admin_username }}
  - name: KEYSTONE_ADMIN_PASSWORD
    value: {{ keystone_admin_password }}
  - name: KEYSTONE_ADMIN_USERNAME
    value: {{ keystone_admin_username }}
  - name: KEYSTONE_URL
    value: {{ keystone_url }}
    
```
- K8S_MASTER_HOST: the k8s master host
- K8S_MASTER_PORT: the k8s master port
- K8S_ADMIN_USERNAME: the k8s admin username
- K8S_ADMIN_PASSWORD: the k8s admin password
- K8S_MASTER_IS_SERCURE: whether use https, default true
- K8S_FILTERED_NAMESPACES: namespace should be filtered for system using, please use default

- KEYSTONE_URL: keystone url 
- KEYSTONE_ADMIN_USERNAME: keystone admin user name
- KEYSTONE_ADMIN_PASSWORD: keystone admin password
- KEYSTONE_CACHE_ENABLED: whether keystone cache enabled, default true

- AUTH_ENABLED: whether auth enabled, default true
- HTTP_CONNECT_TIMEOUT_MILLIS: http request connect timeout, default 30000
- HTTP_READ_TIMEOUT_MILLIS: http request read timeout, default 30000
- HTTP_WRITE_TIMEOUT_MILLIS: http request write timeout, default 30000
- PING_INTERVAL_MILLIS: websocket ping internal millis, default 30000

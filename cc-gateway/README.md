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
cd cc-gateway
sudo docker login
sudo ./gradlew build -x test buildDocker
```

## Run with Docker
```
sudo docker run -e spring_profiles_active=prod \
-e MYSQL_HOST=localhost \
-e MYSQL_PORT=3306 \
-e MYSQL_USERNAME=root \
-e MYSQL_PASSWORD=CHANGE_ME \
-e K8S_MASTER_URL=https://10.19.140.200:6443 \
-e K8S_ADMIN_USERNAME=admin \
-e K8S_ADMIN_PASSWORD=CHANGE_ME \
-e K8S_FILTERED_NAMESPACES=default,kube-system \
-e RABBITMQ_HOST=localhost \
-e RABBITMQ_PORT=5672 \
-e RABBITMQ_USERNAME=admin \
-e RABBITMQ_PASSWORD=CHANGE_ME \
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
        - name: MYSQL_HOST
          value: cc-mysql.cc-dev
        - name: MYSQL_PORT
          value: "3306"
        - name: MYSQL_USERNAME
          value: root
        - name: MYSQL_PASSWORD
          value: root
        - name: RABBITMQ_HOST
          value: cc-rabbitmq.cc-dev
        - name: RABBITMQ_PORT
          value: "5672"
        - name: RABBITMQ_USERNAME
          value: admin
        - name: RABBITMQ_PASSWORD
          value: enncloud
        - name: REDIS_HOST
          value: cc-redis.cc-dev
        - name: REDIS_PORT
          value: "6379"
        - name: REDIS_PASSWORD
          value: enn123456
        - name: KEYSTONE_URL
          value: http://10.19.248.200:30007/v3
        - name: KEYSTONE_ADMIN_USERNAME
          value: console
        - name: KEYSTONE_ADMIN_PASSWORD
          value: d2FuZzE5OTMwNDEx
        - name: K8S_MASTER_URL
          value: https://10.19.248.200:6443
        - name: K8S_ADMIN_USERNAME
          value: eWFuY2hlbmctcGFzc3dvcmQ=
        - name: K8S_ADMIN_PASSWORD
          value: eWFuY2hlbmctdXNlcm5hbWU=
        - name: BACKEND_URL
          value: http://cc-backend.cc-dev:8800
        - name: ORIGIN_URL
          value: http://cc-kubeorigin.cc-dev:8808
        - name: NOTIFICATION_URL
          value: http://cc-notification.cc-dev:8811
        - name: HARBOR_URL
          value: http://cc-harbor.cc-dev:8819
        - name: INITIALIZER_URL
          value: http://cc-initializer.cc-dev:8820
        - name: ACCOUNT_URL
          value: http://cc-account.cc-dev:8816
    
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

- KEYSTONE_URL: keystone url 
- KEYSTONE_ADMIN_USERNAME: keystone admin user name
- KEYSTONE_ADMIN_PASSWORD: keystone admin password
- KEYSTONE_CACHE_ENABLED: whether keystone cache enabled, default true
- KEYSTONE_ADMIN_PROJECT: keystone admin project for filter
- KEYSTONE_TOKEN_EXPIRE_MINUTES: keystone token expire minutes, default 50

- AUTH_ENABLED: whether auth enabled
- NAMESPACE_STATUS_CHECK_PERIOD_MILLIS: namespace status check period millis for namespace event sourcing, default 60000

- HTTP_CONNECT_TIMEOUT_MILLIS: http request connect timeout, default 60000
- HTTP_READ_TIMEOUT_MILLIS: http request read timeout, default 60000
- HTTP_WRITE_TIMEOUT_MILLIS: http request write timeout, default 60000


- CC_TEMPLATE: the cc template url
- DEPENDENCY_CHECKER: the dependency checker url

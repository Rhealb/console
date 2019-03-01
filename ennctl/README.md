# Getting Started
### Clone the source code to your `GOPATH`
1. Setup your Go environment and go path, make sure that you have Go 1.8+.
2. Clone project.

```bash
mkdir -p $GOPATH/src/cc && cd $GOPATH/src/cc && git clone ssh://git@gitlab.cloud.enndata.cn:10885/cc/ennctl.git
```

3. Install glide for package management.
```bash
make glide
```

4. Install package dependencies and install ennctl.
```bash
make install
```

### Configuration
1. Make sure you have installed kubectl correctly.
   - In config file `$HOME/.kube/config`, you should use userId/password confidential instead of K8S token.
   - In k8s context you used, make sure to setup default namespace by add key `namespace`. Or ennctl will use namespace `default`.
1. Setup ennctl by creating config file `$HOME/.ennctl/config` with following two lines.

```bash
# Adrress for gateway
gatewayURL: http://10.19.137.140:32210
# Address of auth
loginURL: http://10.19.137.140:32201/api/v1/login
```

# Usage
```bash
xhdeMacBook-Air-2:ennctl xh$ go install
xhdeMacBook-Air-2:ennctl xh$ ennctl get apps -n cc-demo
NAME			NAMESPACE		TYPE			STATUS			CREATOR			AGE
parse			cc-demo			custom			normal			zhtangsh		58d			
test			cc-demo			custom			normal			zhtangsh		49m			
test1			cc-demo			custom			normal			zhtangsh		3d
```

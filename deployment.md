# Gitlab CI 部署

https://docs.gitlab.com/operator/installation.html?tab=Manifest

## TLS certificates
https://cert-manager.io/docs/installation/
https://cert-manager.io/docs/installation/kubectl/

```
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.16.2/cert-manager.yaml
```

## metrics-server

kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml


## Install the GitLab Operator

kubectl create namespace gitlab-system

kubectl apply -f gitlab-operator-kubernetes-1.9.0.yaml

kubectl -n gitlab-system get deployment gitlab-controller-manager


## Install the Gitlab

https://gitlab.com/gitlab-org/cloud-native/gitlab-operator/-/blob/1.9.0/CHART_VERSIONS

```
apiVersion: apps.gitlab.com/v1beta1
kind: GitLab
metadata:
  name: gitlab
spec:
  chart:
    version: "8.8.0" # https://gitlab.com/gitlab-org/cloud-native/gitlab-operator/-/blob/<OPERATOR_VERSION>/CHART_VERSIONS
    values:
      global:
        hosts:
          domain: test.com # use a real domain here
        ingress:
          configureCertmanager: true
      certmanager-issuer:
        email: jerry@test.com # use your real email address here
```

## 镜像问题

docker.io/bitnami/redis-exporter:1.46.0-debian-11-r8
=>
docker pull docker.1ms.run/bitnami/redis-exporter:1.46.0-debian-11-r8

docker.io/bitnami/redis:6.2.16-debian-12-r1
=>
docker pull docker.1ms.run/bitnami/redis:6.2.16-debian-12-r1


docker tag docker.1ms.run/bitnami/redis:6.2.16-debian-12-r1 docker.io/bitnami/redis:6.2.16-debian-12-r1
docker tag docker.1ms.run/bitnami/redis-exporter:1.46.0-debian-11-r8 docker.io/bitnami/redis-exporter:1.46.0-debian-11-r8


docker.io/bitnami/postgres-exporter:0.12.0-debian-11-r86
=>  
docker pull docker.1ms.run/bitnami/postgres-exporter:0.12.0-debian-11-r86
docker tag docker.1ms.run/bitnami/postgres-exporter:0.12.0-debian-11-r86 docker.io/bitnami/postgres-exporter:0.12.0-debian-11-r86

docker.io/bitnami/postgresql:14.8.0
=>
docker pull docker.1ms.run/bitnami/postgresql:14.8.0
docker tag docker.1ms.run/bitnami/postgresql:14.8.0 docker.io/bitnami/postgresql:14.8.0


docker.io/minio/mc:RELEASE.2018-07-13T00-53-22Z
=>
docker pull docker.1ms.run/minio/mc:RELEASE.2018-07-13T00-53-22Z
docker tag docker.1ms.run/minio/mc:RELEASE.2018-07-13T00-53-22Z docker.io/minio/mc:RELEASE.2018-07-13T00-53-22Z


docker.io/minio/minio:RELEASE.2017-12-28T01-21-00Z
=>
docker pull docker.1ms.run/minio/minio:RELEASE.2017-12-28T01-21-00Z
docker tag docker.1ms.run/minio/minio:RELEASE.2017-12-28T01-21-00Z docker.io/minio/minio:RELEASE.2017-12-28T01-21-00Z


docker.io/minio/mc:RELEASE.2018-07-13T00-53-22Z
=>
docker pull docker.1ms.run/minio/mc:RELEASE.2018-07-13T00-53-22Z
docker tag docker.1ms.run/minio/mc:RELEASE.2018-07-13T00-53-22Z docker.io/minio/mc:RELEASE.2018-07-13T00-53-22Z

------

## CentOS 7(valid in usage)

https://segmentfault.com/a/1190000040220475

### install dependencies

```
yum install -y curl policycoreutils-python openssh-server

systemctl enable sshd
systemctl start sshd
```

### install postfix
```
yum install -y postfix

systemctl enable postfix
systemctl start postfix
```


### install gitlab
https://packages.gitlab.com/gitlab/gitlab-ce


wget https://packages.gitlab.com/gitlab/gitlab-ce/packages/el/7/gitlab-ce-17.7.2-ce.0.el7.x86_64.rpm


rpm -ivh gitlab-ce-17.7.2-ce.0.el7.x86_64.rpm
```
[root@cent7 packages]# rpm -ivh gitlab-ce-17.7.2-ce.0.el7.x86_64.rpm 
warning: gitlab-ce-17.7.2-ce.0.el7.x86_64.rpm: Header V4 RSA/SHA1 Signature, key ID f27eab47: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:gitlab-ce-17.7.2-ce.0.el7        ################################# [100%]
It looks like GitLab has not been configured yet; skipping the upgrade script.

       *.                  *.
      ***                 ***
     *****               *****
    .******             *******
    ********            ********
   ,,,,,,,,,***********,,,,,,,,,
  ,,,,,,,,,,,*********,,,,,,,,,,,
  .,,,,,,,,,,,*******,,,,,,,,,,,,
      ,,,,,,,,,*****,,,,,,,,,.
         ,,,,,,,****,,,,,,
            .,,,***,,,,
                ,*,.
  


     _______ __  __          __
    / ____(_) /_/ /   ____ _/ /_
   / / __/ / __/ /   / __ `/ __ \
  / /_/ / / /_/ /___/ /_/ / /_/ /
  \____/_/\__/_____/\__,_/_.___/
  

Thank you for installing GitLab!
GitLab was unable to detect a valid hostname for your instance.
Please configure a URL for your GitLab instance by setting `external_url`
configuration in /etc/gitlab/gitlab.rb file.
Then, you can start your GitLab instance by running the following command:
  sudo gitlab-ctl reconfigure

For a comprehensive list of configuration options please see the Omnibus GitLab readme
https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/README.md

Help us improve the installation experience, let us know how we did with a 1 minute survey:
https://gitlab.fra1.qualtrics.com/jfe/form/SV_6kVqZANThUQ1bZb?installation=omnibus&release=17-7

```

### start gitlab
```
# gitlab-ctl reconfigure
...
Deprecations:
Your OS, centos-7.9.2009, will be deprecated soon.
Starting with GitLab 17.8, packages will not be built for it.
Switch or upgrade to a supported OS, see https://docs.gitlab.com/ee/administration/package_information/supported_os.html for more information.

Update the configuration in your gitlab.rb file or GITLAB_OMNIBUS_CONFIG environment.


Notes:
Default admin account has been configured with following details:
Username: root
Password: You didn't opt-in to print initial root password to STDOUT.
Password stored to /etc/gitlab/initial_root_password. This file will be cleaned up in first reconfigure run after 24 hours.

NOTE: Because these credentials might be present in your log files in plain text, it is highly recommended to reset the password following https://docs.gitlab.com/ee/security/reset_user_password.html#reset-your-root-password.

gitlab Reconfigured!
You have new mail in /var/spool/mail/root

[root@cent7 packages]# cat /etc/gitlab/initial_root_password
# WARNING: This value is valid only in the following conditions
#          1. If provided manually (either via `GITLAB_ROOT_PASSWORD` environment variable or via `gitlab_rails['initial_root_password']` setting in `gitlab.rb`, it was provided before database was seeded for the first time (usually, the first reconfigure run).
#          2. Password hasn't been changed manually, either via UI or via command line.
#
#          If the password shown here doesn't work, you must reset the admin password following https://docs.gitlab.com/ee/security/reset_user_password.html#reset-your-root-password.

Password: Nw0LeCjGhW2IJwRlBFI3W949Rr0olCjJq2FBIVFnecI=

# NOTE: This file will be automatically deleted in the first reconfigure run after 24 hours.
You have new mail in /var/spool/mail/root
```

##  Update the configuration
* set the external_url
* set the initial_root_password
```
# grep -E "^external_url '" /etc/gitlab/gitlab.rb 
external_url 'http://192.168.50.130:9000'
# grep -E "^gitlab_rails\['initial_root_password'\]" /etc/gitlab/gitlab.rb 
gitlab_rails['initial_root_password'] = "Nw0LeCjGhW2IJwRlBFI3W949Rr0olCjJq2FBIVFnecI="
```

## configure the firewall
```
firewall-cmd --permanent --add-service=http
firewall-cmd --permanent --add-service=https
firewall-cmd --permanent --add-port=9000/tcp
firewall-cmd --reload
```

## 常用管理命令
```
gitlab-ctl reconfigure
gitlab-ctl start
gitlab-ctl stop
gitlab-ctl status
gitlab-ctl tail
```
# Gitlab

### 安装

#### 环境

* CentOS release 6.8 (Final)
* Gitlab 9.2

添加[浙大源](http://mirrors.lifetoy.org/) (或[清华](https://mirror.tuna.tsinghua.edu.cn/help/gitlab-ce/)) `vi /etc/yum.repos.d/gitlab-ce.repo`

```shell
[gitlab-ce]
name=Gitlab CE Repository
baseurl=http://mirrors.lifetoy.org/gitlab-ce/yum/el$releasever/
gpgcheck=0
enabled=1
```

```
sudo yum makecache
sudo yum install gitlab-ce
```

- 安装路径 `/opt/gitlab`
- 运行路径 `/var/opt/gitlab`
- 辅助工具 `/opt/gitlab/bin/gitlab-ctl`

### 配置

`vi /etc/gitlab/gitlab.rb`

```ruby
external_url 'http://git.jamboy.com'
gitlab_rails['lfs_enabled'] = true
gitlab_rails['lfs_storage_path'] = "/opt/gitlab/lfs-objects"

gitlab_rails['gravatar_enabled'] = true
gitlab_rails['gravatar_plain_url'] = "http://cn.gravatar.com/avatar/%{hash}?s=%{size}&d=identicon"
gitlab_rails['backup_keep_time'] = 604800
```

avatar 使用了[国内源](https://bbs.gitlab.com.cn/topic/4/gitlab%E4%BF%AE%E6%94%B9gravatar%E9%93%BE%E6%8E%A5%E4%B8%BA%E5%9B%BD%E5%86%85%E9%95%9C%E5%83%8F)

如果有自己的 nginx 服务，配置 `nginx['enable'] = false`，再 include `/var/opt/gitlab/nginx/conf` 中的配置。

#### Active Directory 整合

vi `/etc/gitlab/gitlab.rb`

```ruby
gitlab_rails['ldap_enabled'] = true
gitlab_rails['ldap_servers'] = YAML.load <<-EOS
main:
  enable: true
  label: 'LDAP'
  host: 'AD服务器地址'
  port: 389
  method: 'plain'
  allow_username_or_email_login: false
  bind_dn: 'AD用户名'
  password: 'AD密码'
  active_directory: true
  base: 'CN=Users,DC=company,DC=com'
  uid: 'sAMAccountName'
  attributes:
    name:       'cn'
    first_name: 'givenName'
    last_name:  'sn'
EOS
```

ldap测试

使用 [ADExplorer](https://technet.microsoft.com/en-us/sysinternals/adexplorer.aspx) 看看AD服务器的情况(base & uid & attributes)，再用 centos 自带的 命令 `ldapsearch`测试：

```shell
ldapsearch -v -h 192.168.2.2 -p 389 -b "CN=Users,DC=corp,DC=jamboy,DC=com" -D "someuser" -W
# -b base
# -D bind_dn
# -W 密码
```

能输出用户列表后，再用 gitlab 自带的命令测试：

```
gitlab-ctl reconfigure
gitlab-rake gitlab:ldap:check
```

### 备份

https://github.com/sund/auto-gitlab-backup


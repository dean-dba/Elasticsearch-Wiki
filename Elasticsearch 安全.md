# Elasticsearch 安全

#### Elasticsearch 本身提供的安全能力，主要基于X-pack实现，在7.1版本之后X-pack基础功能已全部免费，X-pack安全机制主要包括：认证授权、HTTPS传输、节点传输

#### 认证授权方式：LDAP、AD、账户密码等

#### 认证授权实现方式：用户、角色、空间，基于RBAC(role-based access control)基于角色访问控制实现

#### RBAC创建用户并授权流程：1.创建角色并授权空间    2.创建用户并授权角色

#### 生产环境中要谨慎使用脚本：脚本类型：inline(内联)：在语句中直接书写并使用、stored(存储)：提前写好，并保存起来，使用时引用

#### 生产环境中配置用户、角色、空间，可以通过Kibana轻松实现

#### ES 安全原则
- **内网访问，禁止暴露外网端口**
- **设置账户密码**
- **生产环境创建业务账户，并授权**
- **设置访问白名单，禁止未授权访问**

设置ES超级管理员密码
```
[elk@es-1 bin]$ elasticsearch-reset-password --username elastic -i
This tool will reset the password of the [elastic] user.
You will be prompted to enter the password.
Please confirm that you would like to continue [y/N]y


Enter password for [elastic]: 
Re-enter password for [elastic]: 
Password for the [elastic] user successfully reset.
```

##### 至此，Elasticsearch 安全就介绍完成了，请开始你的表演吧！

&nbsp;

**有兴趣的小伙伴，可加联系方式：Telegram：@dean_code**  

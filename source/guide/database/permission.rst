权限控制
==========

数据库的权限分为小程序端和管理端，管理端包括云函数端和控制台。
小程序端运行在小程序中，读写数据库受权限控制限制，管理端运行在云函数上，拥有所有读写数据库的权限。
云控制台的权限同管理端，拥有所有权限。
小程序端操作数据库应有严格的安全规则限制。


初期我们对操作数据库开放以下几种权限配置，每个集合可以拥有一种权限配置，权限配置的规则是作用在集合的每个记录上的。
出于易用性和安全性的考虑，云开发为云数据库做了小程序深度整合，在小程序中创建的每个数据库记录都会带有该记录创建者（即小程序用户）的信息，
以 _openid 字段保存用户的 openid 在每个相应用户创建的记录中。
因此，权限控制也相应围绕着一个用户是否应该拥有权限操作其他用户创建的数据展开。


以下按照权限级别从宽到紧排列如下：

- 仅创建者可写，所有人可读：数据只有创建者可写、所有人可读；比如文章。
- 仅创建者可读写：数据只有创建者可读写，其他用户不可读写；比如用私密相册。
- 仅管理端可写，所有人可读：该数据只有管理端可写，所有人可读；如商品信息。
- 仅管理端可读写：该数据只有管理端可读写；如后台用的不暴露的数据。


简而言之，管理端始终拥有读写所有数据的权限，小程序端始终不能写他人创建的数据，
小程序端的记录的读写权限其实分为了 “所有人可读，只有创建者可写“、”仅创建者可读写“、”所有人可读，仅管理端可写“、”所有人不可读，仅管理端可读写“。


对一个用户来说，不同模式在小程序端和管理端的权限表现如下：

小程序端读/写 自己/他人/任意 创建的数据

+----------------------------------------+--------+--------+--------+--------+----------+
|                  模式                  | 读自己 | 写自己 | 读他人 | 写他人 | 读写任意 |
+========================================+========+========+========+========+==========+
| 仅创建者可写，所有人可读               | √      | √      | √      | ×      | √        |
+----------------------------------------+--------+--------+--------+--------+----------+
| 仅创建者可读写                         | √      | √      | ×      | ×      | √        |
+----------------------------------------+--------+--------+--------+--------+----------+
| 仅管理端可写，所有人可读               | √      | ×      | √      | ×      | √        |
+----------------------------------------+--------+--------+--------+--------+----------+
| 仅管理端可读写：该数据只有管理端可读写 | ×      | ×      | ×      | ×      | √        |
+----------------------------------------+--------+--------+--------+--------+----------+


在设置集合权限时应谨慎设置，防止出现越权操作。


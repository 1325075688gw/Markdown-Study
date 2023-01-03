## Git

[TOC]

### 一、远端和本地如何建立联系

#### 1.本地生成秘钥

##### 1.ssh-keygen

```
ssh-keygen -t rsa -C "1325075688@qq.com"

然后一路点Enter
```

### 二、第一次创建仓库

#### 1.第一次push到远程仓库

##### 1.添加远程仓库地址

```
git remote add origin https://github.com/1325075688gw 不行


必须是具体的仓库地址
git remote add origin git@github.com:1325075688gw/Markdown-Study.git
```



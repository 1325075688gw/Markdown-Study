## Git

[TOC]

### 一、远端和本地如何建立联系

#### 1.本地生成秘钥

##### 1.ssh-keygen

```
ssh-keygen -t rsa -C "1325075688@qq.com"

然后一路点Enter

(/Users/gongwei/.ssh/id_rsa)
```

### 二、第一次创建仓库

#### 1.第一次push到远程仓库

##### 1.添加远程仓库地址

```
git remote add origin https://github.com/1325075688gw 不行


必须是具体的仓库地址
git remote add origin git@github.com:1325075688gw/Markdown-Study.git
```



```
.
├── Dockerfile
├── README.md
├── core_conf # core_conf模块
│   ├── __init__.py
│   ├── task #task文件夹
│   │   ├── __init__.py
│   │   └── cron_task # cron_task文件夹
│   │   ├── __init__.py
│   │   ├── cron_task.py # 用于定义该模块的定时任务
│   │   └── register_cron_task.py # 用于注册定时任务
│   └── util.py
├── lib # 存放lib通用工具库
│   ├── __init__.py
│   └── task # 存放task工具库
│   ├── __init__.py
│   └── cron_task # 存放cron_task工具库
│   ├── __init__.py
│   │   ├── cron_task_scheduler.py   # scheduler接口定义
│   ├── const # 常量定义、枚举定义
│   │   ├── __init__.py
│   │   ├── cron_task_const.py # cron_task枚举定义
│   │   └── trigger_const.py # trigger配置
│   ├── helper # 辅助函数文件夹
│   │   ├── __init__.py
│   │   ├── redis_helper.py
│   │   └── trigger_helper.py
│   ├── management # 脚本运行文件夹
│   │   ├── __init__.py
│   │   └── commands
│   │   ├── __init__.py
│   │   └── start_cron_task.py # 脚本运行文件
│   ├── manager # manager层
│   │   ├── __init__.py
│   │   ├── cron_task_manager.py
│   │   └── trigger_manager.py
│   ├── models # models层
│   │   ├── __init__.py
│   │   └── cron_task_models.py
│   ├── service # service层
│   │   ├── __init__.py
│   │   ├── cron_task_service.py
│   │   └── trigger_service.py
│   └── test # 测试文件夹
│   ├── __init__.py
│   ├── test_cron_task_manager.py
│   └── test_trigger.py
├── manage.py
├── requirements.txt
├── rpc
├── scripts
├── settings
│   ├── __init__.py
│   └── settings.py
└── pyvenv.cfg
```



git 设置用户名和邮箱，单独项目设置，全局项目设置，优先级

https://blog.csdn.net/bandaoyu/article/details/112968504


[toc]

### 一、MySQL函数

1. 插入数据时候添加当前时间

   ```mysql
   -- Mysql中写法：
   -- 直接调用NOW()函数就能获取当前时间，然后直接执行插入即可
   -- 例： 2022-07-31 12:45:34	
   insert into table (id,create_time) values(1,NOW());
   
   --  或者使用CURDATE()，获取的是当前"年-月-日"时间
   -- 例：  2022-07-31
   insert into table (id,create_time) values(2,CURDATE());
   
   --  或者使用CURTIME()，获取的是当前"时:分:秒"时间
   -- 例： 12:45:34
   insert into table (id,create_time) values(3,CURTIME());
   
   -- Oracle中写法:
   -- 使用SYSDATE,获取的是当前的“年-月-日 时:分:秒”时间
   --例： 2022-7-30 12:11:11
   insert into table (id,create_time) values(4,SYSDATE);
   
   
   ```

   
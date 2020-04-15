事务

# 事务

##  分布式事务
```
分布式事务的场景有很多，在rpc接口调用的时候，所有场景都会涉及分布式事务，所引发的问题也很多
```
#### 1. 回滚的场景
        1. 主事务A调用了B和C，B事务成功，C事务失败
        2. 主事务A调用了B和C，B成功，C成功，但A接受到的缺失C失败（主要场景是超时）
        3. A事务调用B和C，B，C成功后，A失败
#### 2. 回滚的处理
        1. 第一种场景其实和第三种场景大同小异。
        2. 第二种场景比较特殊，但处理办法基本相同
        3. 解决办法：所有rpc接口（这个工作量就直接翻倍）在开发的时候都需要有逆向过程，工作复杂度和工作量至少一倍。同时在数据库设计的时候还需要保证（如果有逻辑删除）逻辑删除的逆向性。
#### 3. 特殊场景
        1. 一次成功，不需要回滚
          * 诸如订单下单->支付->完成的这个过程，下单成功后，支付可以失败。但是下次创建订单时候直接检测到订单已经创建就跳过创建步骤。
          * 类似场景：每次打饭都先刷卡，再领饭，再吃饭。刷卡后，领饭途中盘子掉在了地上，再打饭的时候不需要刷卡；
          * 场景试用：前一个事务成功后不会进行修改和是删除。 

## 特例

#### 不要求强一致性的可以使用dataBus。可以保证只有在第一个事务成功。分布式事务回滚难度大。

#### 公司案例
1. 执行了select for update,由于表有索引，先把索引锁上，再去锁数据库表
2. 执行力update xxx by id set **,由于直接根据id，直接锁了数据库表，再去锁索引
3. 当两种情况同时出现的时候，就会出现死锁

id: 1f9dac45bf5641de8a1e70f9decccea6
parent_id: e8b77a1104c94488a5e6a80f8f5fa1cb
created_time: 2020-04-15T11:25:44.753Z
updated_time: 2020-04-15T11:11:00.557Z
is_conflict: 0
latitude: 0.00000000
longitude: 0.00000000
altitude: 0.0000
author: 
source_url: 
is_todo: 0
todo_due: 0
todo_completed: 0
source: joplin-desktop
source_application: net.cozic.joplin-desktop
application_data: 
order: 0
user_created_time: 2020-04-15T11:25:44.753Z
user_updated_time: 2020-04-15T11:11:00.557Z
encryption_cipher_text: 
encryption_applied: 0
markup_language: 1
is_shared: 0
type_: 1
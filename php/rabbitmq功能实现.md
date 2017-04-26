---
title: php代码块
date: 2017/1/14 20:46:25
categories: php
tags: 
- php
description:  货号单 0000001的生成方法&通过goole Api以经纬度获取城市名...
---

### RabbitMQ 功能实现
>引入php-amqplib类库,类库地址为[https://github.com/php-amqplib/php-amqplib](https://github.com/php-amqplib/php-amqplib)

###1.简单的示例代码实现

![](/assets/QQ截图20161212205751.png)

+ 生产者(发送消息者) 
```php
    public function producer(){
        //创建连接实例
        $connection = new AMQPStreamConnection('127.0.0.1', 5672, 'guest', 'guest');
        //创建一个连接通道
        $channel = $connection->channel();
        //声明队列,如果该队列不存在会创建
        $channel->queue_declare('hello', false, false, false, false);
        //创建消息实例
        $msg = new AMQPMessage('Hello World1!');
        //通过通道,推送消息到队列中
        $channel->basic_publish($msg, '', 'hello');
        //关闭通道
        $channel->close();
        //关闭连接
        $connection->close();
        echo " [x] Sent 'Hello World!'\n";
    }
```
+ 消费者(获取消息者)
```php
    public function consumer(){
        //创建连接实例
        $connection = new AMQPStreamConnection('127.0.0.1', 5672, 'guest', 'guest');
        //创建一个连接通道
        $channel = $connection->channel();
        //声明队列,如果该队列不存在会创建
        $channel->queue_declare('hello', false, false, false, false);
        //创建一个实例(这里用于回调)
        $callback_model=new Callback();
        //通过通道消费队列中的信息,并执行回调(这里为array($callback_model,'getQueueInfo'))
        $channel->basic_consume('hello', '', false, false, false, false,array($callback_model,'getQueueInfo'));
        //当存在回调时,这里将进入无限循环,每当队列中被推送新值,就会执行回调
        while(count($channel->callbacks)) {
            $channel->wait();
        }
        //关闭通道
        $channel->close();
        //关闭连接
        $connection->close();
    }
```
+ 回调方法
```php
class Callback extends Model{
    //$channel->basic_consume 执行回调方法时,会传入$msg对象
    public static function getQueueInfo($msg){
        //我这里将$msg中的主体(队列中的消息值) 和 当前进程号 存入表中
        $test_model=new Table();
        $test_model->content=$msg->body;
        $test_model->num=getmypid();
        $test_model->save();
        $msg->delivery_info['channel']->basic_ack($msg->delivery_info['delivery_tag']);
    }
}
```

###2.队列及消息的持久化设置

+ 首先设置队列的持久化

```php
//设置第三个参数durable 为true
//注意:如果这里hello队列已存在,RabbitMQ不允许重新定义现有队列,并且会返回错误,这里你可以声明一个新队列
$channel->queue_declare('hello', false, true, false, false);
```
+ 设置消息的持久化

```php
$msg = new AMQPMessage($data,
    array('delivery_mode' => 2) //使消息持久化
);

```

> 虽然设置了队列和消息的持久化,但RabbitMQ可能有时只是存入缓存不是磁盘中,如果需要更强力的保障,请使用[ publisher confirms](https://www.rabbitmq.com/confirms.html)

###3.合理调度实现

如果你想让工人处理并确认了当前任务后再接受新任务,需在**消耗信息**时设置
```php
//设置prefetch_count =1
$channel->basic_qos(null, 1, null); //参数为1 表示工人当前任务最多1个
$channel->basic_consume('hello', '', false, false, false, false);
```

###4.消息确认机制
```php
$channel->basic_consume('hello', '', false, false, false, false,array($callback_model,'getQueueInfo'));
//注意:$channel->basic_consume 的第四个参数为true时(即 no ack),则为关闭消息确认
//$channel->basic_consume 的第四个参数为false时,则为开启消息确认
//开启消息确认机制后,回调方法执行消息确认后,该信息才会被消耗. 当该工人或服务死后,未确认的信息会被再次放入到队列中
//回调方法中执行
$msg->delivery_info['channel']->basic_ack($msg->delivery_info['delivery_tag']);
```



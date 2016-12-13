### php + RabbitMQ 功能实现
引入php-amqplib类库,类库地址为[https://github.com/php-amqplib/php-amqplib](https://github.com/php-amqplib/php-amqplib)

**1.简单的示例代码实现**

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
        $callback_model=new Test();
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
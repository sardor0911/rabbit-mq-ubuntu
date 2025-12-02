# Install Rabbit-msq Server to Ubuntu

### Steps

##### 1. Install utilities
##### 2. Add repository for Erlang version 15.2.7.4 (27.3.4.6)
##### 3. Install Erlang version 15.2.7.4  (27.3.4.6)
##### 4. Add repository for Rabbitmq server version 4.2.1
##### 5. Install Rabbit mq server
##### 6. Download and add plugin for Rabbitmq server
##### 7. Enable plugins
##### 8. Add user for rabbitmq and set permisions


### 1. Install utilities
~~~
apt-get install curl gnupg apt-transport-https -y
~~~
### 2. Add repository for Erlang version 15.2.7.4 
~~~
sudo tee /etc/apt/sources.list.d/rabbitmq.list <<EOF
deb [arch=amd64 signed-by=/usr/share/keyrings/com.rabbitmq.team.gpg] https://deb1.rabbitmq.com/rabbitmq-erlang/ubuntu/noble noble main
deb [arch=amd64 signed-by=/usr/share/keyrings/com.rabbitmq.team.gpg] https://deb2.rabbitmq.com/rabbitmq-erlang/ubuntu/noble noble main
deb [arch=amd64 signed-by=/usr/share/keyrings/com.rabbitmq.team.gpg] https://deb1.rabbitmq.com/rabbitmq-server/ubuntu/noble noble main
deb [arch=amd64 signed-by=/usr/share/keyrings/com.rabbitmq.team.gpg] https://deb2.rabbitmq.com/rabbitmq-server/ubuntu/noble noble main
EOF
apt-get update -y
~~~
### 3. Install Erlang version 15.2.7.4
~~~
apt-get install -y erlang-base
~~~

### 4. Install Rabbit mq server
~~~
apt-get install rabbitmq-server -y --fix-missing
~~~
#### Start rabbitqm-server
~~~
systemctl start rabbitmq-server
~~~
#### Add autostart rabbitmq-server
~~~
systemctl enable rabbitmq-server
~~~
#### Check status rabbitmq-server
~~~
systemctl status rabbitmq-server
~~~

### 6. Download and add plugin for Rabbitmq server
#### Check list and status plugins
~~~
rabbitmq-plugins list
~~~
#### Download and add plugin for Rabbitmq server
~~~
wget https://github.com/rabbitmq/rabbitmq-delayed-message-exchange/releases/download/v4.2.0/rabbitmq_delayed_message_exchange-4.2.0.ez
~~~
#### Move plugins to folder Rabbitmq-server
~~~
mv rabbitmq_delayed_message_exchange-4.2.0.ez /usr/lib/rabbitmq/lib/rabbitmq_server-4.2.1/plugins/
~~~
### 7. Enable plugins
~~~
rabbitmq-plugins enable rabbitmq_management
rabbitmq-plugins enable rabbitmq_shovel
rabbitmq-plugins enable rabbitmq_shovel_management
rabbitmq-plugins enable rabbitmq_delayed_message_exchange
systemctl restart rabbitmq-server
systemctl status rabbitmq-server
~~~
#### Check status plugins
~~~
rabbitmq-plugins list
~~~
#### Result

<img width="320" height="600" alt="image" src="https://github.com/user-attachments/assets/56f0224f-1734-48cb-a2ce-3bca93abaab0" />

### 7. Add user for rabbitmq and set permisions
#### Add user and set password
~~~
rabbitmqctl add_user USER "PASSWORD"
~~~

#### Add permissions for user
~~~
rabbitmqctl set_permissions -p / USER ".*" ".*" ".*"
rabbitmqctl set_user_tags USER administrator
~~~
#### Enable Flags
~~~
rabbitmqctl list_feature_flags
rabbitmqctl enable_feature_flag all
rabbitmqctl enable_feature_flag all
~~~
## Access to Web UI for check
~~~
http://IP Address:15672
~~~

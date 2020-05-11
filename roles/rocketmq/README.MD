>0.创建集群前先执行以下命令检测可执行情况：
```
# ansible-playbook rocketmq.yaml -i ../inventory/hosts --syntax-check	//语法检查
# ansible-playbook rocketmq.yaml -i ../inventory/rocketmq_hosts --list-hosts	//任务执行和主机检查
```
>1.创建集群时，修改rocketmq_hosts文件。然后执行：
```
# ansible-playbook rocketmq.yaml -i ../inventory/rocketmq_hosts 命令创建。
```
>2.扩容集群时，修改rocketmq_rolling_update_hosts文件。然后执行：
```
# ansible-playbook rocketmq.yaml -i ../inventory/rocketmq_rolling_update_hosts 命令扩容。
```
>3.暂时不支持缩容，待更新。。。

>重要说明：hosts主机文件和vars变量文件的修改规则：
```
1）rocketmq_hosts和rocketmq_rolling_update_hosts文件：
	1.1) mqnamesrv主机组中ansible_host不能有相同的IP，因为一台主机只部署一个namesrv实例。
	1.2) mqbroker主机中主机名可以自定义唯一名称，ansible_host可以是同一IP，但是仅仅限于节点是slave的实例。即：同一台主机只能部署一个master实例，但可以部署多个slave实例。部署多个slave实例时，需要特别注意broker_id，listenPort和data_dir_flag三个变量的设置。broker_id变量需要在同名broker_name中唯一，listenPort和data_dir_flag需要在同ansible_host中唯一。也就是说，当broker_name相同时，broker_id一定不能相同（一主多从的场景）；ansible_host相同时，listenPort和data_dir_flag都一定不能相同。否则会因为冲突导致配置不正常实例无法启动。还有就是namesrv这个变量需要修改为指定的ip或者域名。
	1.3）扩容时修改rocketmq_rolling_update_hosts文件需要rocketmq_hosts文件对比，不能和rocketmq_hosts文件冲突，规则和以上两点一样。

2）vars/vars.yaml文件：
	通用变量在这里修改，若不改则使用默认值。
```

*执行前需要下载jdk8.zip压缩文件到commons/files/目录下，因为github不支持上次超过100M的文件。所以这里没有上传。*

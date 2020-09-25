# fluent-plugin-myscrub [![Gem Version](https://badge.fury.io/rb/fluent-plugin-myscrub.svg)](https://rubygems.org/gems/fluent-plugin-myscrub)

Fluent plugin for drop a event that include a invalid bytes in utf-8

fluentd的插件，可以过滤字段中含有utf-8非法字符的数据

## Installation 安装

Install it yourself as:

可以通过`gem`命令进行安装，如下：

    $ gem install fluent-plugin-myscrub

## Configuration 配置

配置很简单，指定type即可

```
<filter **>
  @type myscrub
</filter>
```

## Usage 使用

下面介绍一种使用方式，日志通过forward发送到fluentd采集端，然后再通过forward发送出去。

```
<source>
  @type forward
  port 24224
</source>

<filter **>
  @type myscrub
</filter>

<match **>
  @id forward
  @type forward
  expire_dns_cache 60
  ignore_network_errors_at_startup true
  slow_flush_log_threshold 120.0
  heartbeat_type none
  compress gzip
  <server>
    host fluentd-forward
    port 24224
  </server>
  <buffer>
    @type file_single
    path /fluentd/log
    total_limit_size 2G
    chunk_limit_size 10M
	retry_type periodic
	retry_wait 60
	flush_mode interval
	flush_interval 15s
	flush_thread_count 1
	retry_forever true
	overflow_action block
  </buffer>
</match>

```



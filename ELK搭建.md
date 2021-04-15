> ### ELK安装路程



> ElasticSearch

1. 解压文件

   ```shell
   tar -zxvf /guoc/gz/elasticsearch-7.9.2
   ```

2. 修改配置文件elasticsearch.yml

   ```yaml
   # ======================== Elasticsearch Configuration =========================
   #
   # NOTE: Elasticsearch comes with reasonable defaults for most settings.
   #       Before you set out to tweak and tune the configuration, make sure you
   #       understand what are you trying to accomplish and the consequences.
   #
   # The primary way of configuring a node is via this file. This template lists
   # the most important settings you may want to configure for a production cluster.
   #
   # Please consult the documentation for further information on configuration options:
   # https://www.elastic.co/guide/en/elasticsearch/reference/index.html
   #
   # ---------------------------------- Cluster -----------------------------------
   #
   # Use a descriptive name for your cluster:
   #
   cluster.name: g-application
   #
   # ------------------------------------ Node ------------------------------------
   #
   # Use a descriptive name for the node:
   #
   node.name: gnode-1
   #
   # Add custom attributes to the node:
   #
   #node.attr.rack: r1
   #
   # ----------------------------------- Paths ------------------------------------
   #
   # Path to directory where to store the data (separate multiple locations by comma):
   #
   path.data: /guoc/elk/elasticsearch-7.9.2/data
   #
   # Path to log files:
   #
   path.logs: /guoc/elk/elasticsearch-7.9.2/logs
   #
   # ----------------------------------- Memory -----------------------------------
   #
   # Lock the memory on startup:
   #
   #bootstrap.memory_lock: true
   #
   # Make sure that the heap size is set to about half the memory available
   # on the system and that the owner of the process is allowed to use this
   # limit.
   #
   # Elasticsearch performs poorly when the system is swapping the memory.
   #
   # ---------------------------------- Network -----------------------------------
   #
   # Set the bind address to a specific IP (IPv4 or IPv6):
   #
   network.host: 0.0.0.0
   #
   # Set a custom port for HTTP:
   #
   http.port: 8082
   #
   # For more information, consult the network module documentation.
   #
   # --------------------------------- Discovery ----------------------------------
   #
   # Pass an initial list of hosts to perform discovery when this node is started:
   # The default list of hosts is ["127.0.0.1", "[::1]"]
   #
   #discovery.seed_hosts: ["host1", "host2"]
   #
   # Bootstrap the cluster using an initial set of master-eligible nodes:
   #
   #cluster.initial_master_nodes: ["node-1", "node-2"]
   cluster.initial_master_nodes: ["gnode-1"]
   #
   # For more information, consult the discovery and cluster formation module documentation.
   #
   # ---------------------------------- Gateway -----------------------------------
   #
   # Block initial recovery after a full cluster restart until N nodes are started:
   #
   #gateway.recover_after_nodes: 3
   #
   # For more information, consult the gateway module documentation.
   #
   # ---------------------------------- Various -----------------------------------
   #
   # Require explicit names when deleting indices:
   #
   #action.destructive_requires_name: true
   
   ```

3. 自定义启动脚本es-start.sh

   ```shell
   vim es-start.sh
   
   /guoc/elk/elasticsearch-7.9.2/bin/elasticsearch &
   could not find java in bundled jdk at /opt/elasticsearch/elasticsearch-7.9.1/jdk/bin/java报错
   ```

4. 修改脚本执行权限并启动脚本

   ```shell
   chmod a+x ./es-start.sh
   ./es-start.sh
   ```

   

5. 启动ElasticSearch问题 解决方案传送门https://www.cnblogs.com/hellxz/p/11057234.html

   1. could not find java in bundled jdk at /opt/elasticsearch/elasticsearch-7.9.1/jdk/bin/java报错

      ```shell
      # 修改es安装目录文件属主为当前启动es的用户
      chown -R tgsd ./elasticsearch
      ```

   2. 

   

> Kinaba

1. 解压文件

   ```shell
   tar -zxvf /guoc/gz/kibana-7.9.2-linux-x86_64.tar.gz
   ```

2. 修改配置文件

   ```yaml
   # Kibana is served by a back end server. This setting specifies the port to use.
   #server.port: 5601
   server.port: 8083
   
   # Specifies the address to which the Kibana server will bind. IP addresses and host names are both valid values.
   # The default is 'localhost', which usually means remote machines will not be able to connect.
   # To allow connections from remote users, set this parameter to a non-loopback address.
   #server.host: "localhost"
   server.host: 0.0.0.0
   
   # Enables you to specify a path to mount Kibana at if you are running behind a proxy.
   # Use the `server.rewriteBasePath` setting to tell Kibana if it should remove the basePath
   # from requests it receives, and to prevent a deprecation warning at startup.
   # This setting cannot end in a slash.
   #server.basePath: ""
   
   # Specifies whether Kibana should rewrite requests that are prefixed with
   # `server.basePath` or require that they are rewritten by your reverse proxy.
   # This setting was effectively always `false` before Kibana 6.3 and will
   # default to `true` starting in Kibana 7.0.
   #server.rewriteBasePath: false
   
   # The maximum payload size in bytes for incoming server requests.
   #server.maxPayloadBytes: 1048576
   
   # The Kibana server's name.  This is used for display purposes.
   #server.name: "your-hostname"
   server.name: "g-hostname"
   
   # The URLs of the Elasticsearch instances to use for all your queries.
   #elasticsearch.hosts: ["http://localhost:9200"]
   elasticsearch.hosts: ["http://192.168.59.8:8082"]
   
   # When this setting's value is true Kibana uses the hostname specified in the server.host
   # setting. When the value of this setting is false, Kibana uses the hostname of the host
   # that connects to this Kibana instance.
   #elasticsearch.preserveHost: true
   
   # Kibana uses an index in Elasticsearch to store saved searches, visualizations and
   # dashboards. Kibana creates a new index if the index doesn't already exist.
   #kibana.index: ".kibana"
   
   # The default application to load.
   #kibana.defaultAppId: "home"
   
   # If your Elasticsearch is protected with basic authentication, these settings provide
   # the username and password that the Kibana server uses to perform maintenance on the Kibana
   # index at startup. Your Kibana users still need to authenticate with Elasticsearch, which
   # is proxied through the Kibana server.
   #elasticsearch.username: "kibana_system"
   #elasticsearch.password: "pass"
   
   # Enables SSL and paths to the PEM-format SSL certificate and SSL key files, respectively.
   # These settings enable SSL for outgoing requests from the Kibana server to the browser.
   #server.ssl.enabled: false
   #server.ssl.certificate: /path/to/your/server.crt
   #server.ssl.key: /path/to/your/server.key
   
   # Optional settings that provide the paths to the PEM-format SSL certificate and key files.
   # These files are used to verify the identity of Kibana to Elasticsearch and are required when
   # xpack.security.http.ssl.client_authentication in Elasticsearch is set to required.
   #elasticsearch.ssl.certificate: /path/to/your/client.crt
   #elasticsearch.ssl.key: /path/to/your/client.key
   
   # Optional setting that enables you to specify a path to the PEM file for the certificate
   # authority for your Elasticsearch instance.
   #elasticsearch.ssl.certificateAuthorities: [ "/path/to/your/CA.pem" ]
   
   # To disregard the validity of SSL certificates, change this setting's value to 'none'.
   #elasticsearch.ssl.verificationMode: full
   
   # Time in milliseconds to wait for Elasticsearch to respond to pings. Defaults to the value of
   # the elasticsearch.requestTimeout setting.
   #elasticsearch.pingTimeout: 1500
   
   # Time in milliseconds to wait for responses from the back end or Elasticsearch. This value
   # must be a positive integer.
   #elasticsearch.requestTimeout: 30000
   
   # List of Kibana client-side headers to send to Elasticsearch. To send *no* client-side
   # headers, set this value to [] (an empty list).
   #elasticsearch.requestHeadersWhitelist: [ authorization ]
   
   # Header names and values that are sent to Elasticsearch. Any custom headers cannot be overwritten
   # by client-side headers, regardless of the elasticsearch.requestHeadersWhitelist configuration.
   #elasticsearch.customHeaders: {}
   
   # Time in milliseconds for Elasticsearch to wait for responses from shards. Set to 0 to disable.
   #elasticsearch.shardTimeout: 30000
   
   # Time in milliseconds to wait for Elasticsearch at Kibana startup before retrying.
   #elasticsearch.startupTimeout: 5000
   
   # Logs queries sent to Elasticsearch. Requires logging.verbose set to true.
   #elasticsearch.logQueries: false
   
   # Specifies the path where Kibana creates the process ID file.
   #pid.file: /var/run/kibana.pid
   
   # Enables you to specify a file where Kibana stores log output.
   #logging.dest: stdout
   
   # Set the value of this setting to true to suppress all logging output.
   #logging.silent: false
   
   # Set the value of this setting to true to suppress all logging output other than error messages.
   #logging.quiet: false
   
   # Set the value of this setting to true to log all events, including system usage information
   # and all requests.
   #logging.verbose: false
   
   # Set the interval in milliseconds to sample system and process performance
   # metrics. Minimum is 100ms. Defaults to 5000.
   #ops.interval: 5000
   
   # Specifies locale to be used for all localizable strings, dates and number formats.
   # Supported languages are the following: English - en , by default , Chinese - zh-CN .
   #i18n.locale: "en"
   ```

3. 自定义启动脚本k-start.sh

   ```shell
   vim k-start.sh
   
   /guoc/elk/kibana-7.9.2-linux-x86_64/bin/kibana &
   ```

4. 修改脚本执行权限并启动脚本

   ```shell
   chmod a+x ./k-start.sh
   ./k-start.sh
   ```

5. 启动kibana问题

   1. ```shell
      log   [06:13:00.537] [error][plugins][reporting][validations] The Reporting plugin encountered issues launching Chromium in a self-test. You may have trouble generating reports.
      log   [06:13:00.538] [error][plugins][reporting][validations] Error: Failed to launch chrome!
      /guoc/elk/kibana-7.9.2-linux-x86_64/x-pack/plugins/reporting/chromium/headless_shell-linux/headless_shell: error while loading shared libraries: libnss3.so: cannot open shared object file: No such file or directory
      TROUBLESHOOTING: https://github.com/GoogleChrome/puppeteer/blob/master/docs/troubleshooting.md
      
          at onClose (/guoc/elk/kibana-7.9.2-linux-x86_64/node_modules/puppeteer-core/lib/Launcher.js:349:14)
          at Interface.helper.addEventListener (/guoc/elk/kibana-7.9.2-linux-x86_64/node_modules/puppeteer-core/lib/Launcher.js:338:50)
          at Interface.emit (events.js:203:15)
          at Interface.close (readline.js:397:8)
          at Socket.onend (readline.js:173:10)
          at Socket.emit (events.js:203:15)
          at endReadableNT (_stream_readable.js:1145:12)
          at process._tickCallback (internal/process/next_tick.js:63:19)
        log   [06:13:00.543] [error][plugins][reporting][validations] Error: Could not close browser client handle!
          at browserFactory.test.then.browser (/guoc/elk/kibana-7.9.2-linux-x86_64/x-pack/plugins/reporting/server/lib/validate/validate_browser.js:26:15)
          at process._tickCallback (internal/process/next_tick.js:68:7)
        log   [06:13:00.550] [warning][plugins][reporting][validations] Reporting plugin self-check generated a warning: Error: Could not close browser client handle!
        log   [06:17:58.226] [warning][fetcher][plugins][telemetry] Error sending telemetry usage data: FetchError: request to https://telemetry.elastic.co/xpack/v2/send failed, reason: getaddrinfo EAI_AGAIN telemetry.elastic.co telemetry.elastic.co:443
        Error: Failed to launch chrome!
      ```

      提示 启动谷歌浏览器失败 可以无视（因为linux服务器上没有chrome）；



> Logstash

1. 解压logstash.tar.gz

   ```sh
   tar -zxvf /guoc/gz/logstash-7.9.2.tar.gz
   ```

2. 在config文件夹向下自定义配置文件logstash-custom.conf并增加执行权限

   ```shell
   vim logstash-custom.conf
   #input 输入配置
   input {
     tcp {
       mode => "server"
       host => "0.0.0.0"
       port => 8084
       codec => json_lines
     }
   }
   filter {
   
   }
   #output 输出配置
   output {
     elasticsearch {
       hosts => ["127.0.0.1:8082"]
       index => "log-dev-%{+yyyy.MM.dd}"
     }
   }
   
   
   
   
   chown a+x /guoc/elk/logstash-7.9.2/config/logstash-custom.conf
   ```

3. 自定义启动脚本

   ```shell
   vim l-start.sh
   
   /guoc/elk/logstash-7.9.2/bin/logstash -f /guoc/elk/logstash-7.9.2/config/logstash-custom.conf &
   ```

4. 修改脚本执行权限并启动脚本

   ```shell
   chmod a+x ./l-start.sh
   ./l-start.sh
   ```

5. logstash 需要jdk环境 传送门https://www.cnblogs.com/xuliangxing/p/7066913.html

6. 启动logstash问题

   1. ```shell
      could not find java; set JAVA_HOME or ensure java is in PATH
      ```

      解决方案传送门https://blog.csdn.net/m0_38056893/article/details/105666440

      在/logstash/bin/logstash.lib.sh文件中首行配置JAVA_HOME即可

      ![image-20210224152900660](C:\Users\guochuang\AppData\Roaming\Typora\typora-user-images\image-20210224152900660.png)




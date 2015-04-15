# Graylog Ansible Playbook - Batteries included

Installs all the necesary requirements for graylog-server and graylog-web to run.




## Info
 - This installs all the necesary packages and configures them properly in order for you to have a graylog server and web in about 15 minutes
 - It installs and configures elasticsearch, mongodb, nginx, graylog-server and graylog-web
 - It also adds a default listener to port 514. Because of graylog not being able to bind to port 514 directly, there's an iptables rule that redirects traffic from port 514 to 5555 udp(or any port you set graylog_input_port to). 
 - More redirects cand be added easily. The input that listens to port 5555 is also aded automatically, as a raw UDP Input, but it can be changed easily to something else.
 - The input types are:

```
org.graylog.inputs.amqp.AMQPInput
org.graylog.inputs.codecs.CodecsModule
org.graylog.inputs.gelf.http.GELFHttpInput
org.graylog.inputs.gelf.tcp.GELFTCPInput
org.graylog.inputs.gelf.udp.GELFUDPInput
org.graylog.inputs.kafka.KafkaInput
org.graylog.inputs.misc.jsonpath.JsonPathInput
org.graylog.inputs.misc.metrics.LocalMetricsInput
org.graylog.inputs.radio.RadioAMQPInput
org.graylog.inputs.radio.RadioKafkaInput
org.graylog.inputs.random.FakeHttpMessageInput
org.graylog.inputs.raw.tcp.RawTCPInput
org.graylog.inputs.raw.udp.RawUDPInput
org.graylog.inputs.syslog.tcp.SyslogTCPInput
org.graylog.inputs.syslog.udp.SyslogUDPInput
org.graylog.inputs.transports.TransportsModule
org.graylog.plugin.inject.GraylogModule
org.graylog.plugin.inputs.MessageInput
```
 - Input json format borrowed from hggh https://github.com/hggh/graylog-vagrant/blob/master/modules/repos/files/create_graylog_inputs_gelf

 - There is also a role that installs nginx and generates a self signed ceritifate. At the end you will be able to access the graylo2-web interface using https://IP.AD.DR or https://servername. This was solely done because I do not like java and java keystores and it was easier for me this way. You could also set use_self_signed_cert to no, and add your own cert and key in the format of {{ server_name }}.crt and {{ server_name }}.key

## Asumptions
 - You will use this on CentOS 6.X. Not tested on version 7 
 - You will have SElinux set to Permissive
 - You have SSH binding to port 22


## Available Variables in Ansible


```
server_name: "{{ ansible_fqdn }}"

graylog_version: 1.0

graylog_input_port: 5555
input_type: org.graylog.inputs.raw.udp.RawUDPInput

password_secret: thisisnotasecurepassword

# generated using echo -n yourpassword | shasum -a 256
root_password_sha2: e3c652f0ba0b4801205814f8b6bc49672c4c74e25b497770bb89b22cdeb4e951

root_password_unencrypted: yourpassword

mongodb_user: graylog_user
mongodb_password: 123456

use_self_signed_cert: "yes"
```

## Contact

Developed for Bigstep by Marius Boeru <marius.boeru at bigstep com>
 * https://twitter.com/mboeru/
 * https://github.com/mboeru/
 * https://github.com/bigstepinc/

# Graylog2 Ansible Playbook - Batteries included


## Info
 - This installs all the necesary packages and configures them properly in order for you to have a graylog2 server and web in about 15 minutes
 - It installs and configures elasticsearch, mongodb, nginx, graylog2-server and graylog-web
 - It also adds a default listener to port 514. Because of graylog2 not being able to bind to port 514 directly, there's an iptables rule that redirects traffic from port 514 to 5555 udp(or any port you set graylog2_input_port to). 
 - More redirects cand be added easily. The input that listens to port 5555 is also aded automatically, as a raw UDP Input, but it can be changed easily to something else.
 - The input types are:
```
org.graylog2.inputs.amqp.AMQPInput
org.graylog2.inputs.codecs.CodecsModule
org.graylog2.inputs.gelf.http.GELFHttpInput
org.graylog2.inputs.gelf.tcp.GELFTCPInput
org.graylog2.inputs.gelf.udp.GELFUDPInput
org.graylog2.inputs.kafka.KafkaInput
org.graylog2.inputs.misc.jsonpath.JsonPathInput
org.graylog2.inputs.misc.metrics.LocalMetricsInput
org.graylog2.inputs.radio.RadioAMQPInput
org.graylog2.inputs.radio.RadioKafkaInput
org.graylog2.inputs.random.FakeHttpMessageInput
org.graylog2.inputs.raw.tcp.RawTCPInput
org.graylog2.inputs.raw.udp.RawUDPInput
org.graylog2.inputs.syslog.tcp.SyslogTCPInput
org.graylog2.inputs.syslog.udp.SyslogUDPInput
org.graylog2.inputs.transports.TransportsModule
org.graylog2.plugin.inject.Graylog2Module
org.graylog2.plugin.inputs.MessageInput
```
 - There is also a role that installs nginx and generates a self signed ceritifate. This was solely done because I do not like java and java keystores and it was easier for me this way. You could also set use_self_signed_cert to no, and add your own cert and key in the format of {{ server_name }}.crt and {{ server_name }}.key

## Asumptions
 - You will use this on CentOS 6.X. Not tested on version 7 
 - You will have SElinux set to Permissive
 - You have SSH binding to port 22


## Available Variables in Ansible

```
server_name: "{{ ansible_fqdn }}"

graylog2_version: 0.91

graylog2_input_port: 5555
input_type: org.graylog2.inputs.raw.udp.RawUDPInput

password_secret: thisisnotasecurepassword

# generated using echo -n yourpassword | shasum -a 256
root_password_sha2: e3c652f0ba0b4801205814f8b6bc49672c4c74e25b497770bb89b22cdeb4e951

root_password_unencrypted: yourpassword

mongodb_user: graylog2_user
mongodb_password: 123456

use_self_signed_cert: "yes"
```

## Contact

Marius Boeru <marius.boeru@bigstep.com>
 * https://twitter.com/mboeru/
 * https://github.com/mboeru/
 * https://github.com/bigstepinc/

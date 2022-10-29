# NX-OS Telemetry

https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/93x/progammability/guide/b-cisco-nexus-9000-series-nx-os-programmability-guide-93x/b-cisco-nexus-9000-series-nx-os-programmability-guide-93x_chapter_011000.html

## prerequsite

- 사전 설치 기능
  | name | |
  |---|---|
  |feature bash-shell| |
  |feature scp-server| |
  |feature grpc| |
  |feature telemetry| |

- 사전 설치 RPM packages
  | name | |
  |---|---|
  | mtx-infra | |
  | mtx-device | |
  | mtx-netconf-agent/mtx-restconf-agent/mtx-grpc-agent | (at least one) |
  | mtx-openconfig-all | (alternatively, selected individual models) |

- 설치 패키지 확인
  ```
  show install packages | i mtx
  or 
  yum list installed | grep mtx
  ```

- 패키지 설치   
  ```
  run bash
  cd /bootflash 
  yum install mtx-infra-2.0.0.0-9.2.1.lib32_n9000.rpm mtx-device-2.0.0.0-9.2.1.lib32_n9000.rpm ...

  or 

  install mtx-infra-2.0.0.0-9.2.1.lib32_n9000.rpm activate

  ```




#

```
#scp mtx-openconfig-all-1.0.0.0-9.3.8.lib32_n9000.rpm admin@198.18.66.105:.
cd etc/telegraf/cert
scp gnmi.crt admin@198.18.66.248:.
scp telegraf.crt admin@198.18.66.105:.

```

```
crypto ca trustpoint gnmicert
crypto ca import gnmicert pkcs12 gnmi.pfx abcxyz12345 
grpc certificate gnmicert
```

```
configure
telemetry
  certificate mycert.pem cisco.com
  destination-group 1
    ip address 172.31.219.148 port 50001 protocol gRPC encoding GPB 
  sensor-group 1
    path sys/bgp/ depth unbounded
    path sys/epId-1 depth unbounded
    path sys/intf depth unbounded

  sensor-group 2
    data-source NX-API
    path "show environment power" depth 0   
    path "show vlan id 2-5 counters� depth 0  
    path "show processes cpu sort" depth 0

  subscription 1
    dst-grp 1
    snsr-grp 1 sample-interval 30000

  subscription 2
    dst-grp 1  
    snsr-grp 2 sample-interval 60000
```
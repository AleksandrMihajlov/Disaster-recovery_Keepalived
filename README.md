Disaster-recovery_Keepalived Aleksandr Mihajlov SYS34  
  
**Задание1**  
  
![alt text](https://github.com/AleksandrMihajlov/Disaster-recovery_Keepalived/blob/main/1.png)  
![alt text](https://github.com/AleksandrMihajlov/Disaster-recovery_Keepalived/blob/main/1.1.png)  
<https://github.com/AleksandrMihajlov/Disaster-recovery_Keepalived/blob/main/1.pkt>  
  
**Задание2**  
  
![alt text](https://github.com/AleksandrMihajlov/Disaster-recovery_Keepalived/blob/main/2.png)  
![alt text](https://github.com/AleksandrMihajlov/Disaster-recovery_Keepalived/blob/main/2.1.png)  
![alt text](https://github.com/AleksandrMihajlov/Disaster-recovery_Keepalived/blob/main/2.2.png)  
![alt text](https://github.com/AleksandrMihajlov/Disaster-recovery_Keepalived/blob/main/2.3.png)  
  
```bash
#!/bin/bash

nc -z localhost 80
result=$?

if [[ -f /var/www/html/index.nginx-debian.html ]]; then
    file_exists=0
else
    file_exists=1
fi

if [[ $result -ne 0 || $file_exists -ne 0 ]]; then
    exit 1
fi
exit 0

```  
  
```
global_defs {
    script_user root
    enable_script_security
}

vrrp_script check {
    script "/etc/keepalived/script.sh"
    interval 3
}

vrrp_instance VI_1 {
        state BACKUP
        interface ens160
        virtual_router_id 15
        priority 250
        advert_int 1

        virtual_ipaddress {
              192.168.67.250/24
        }
        track_script {
           check
        }
}
```
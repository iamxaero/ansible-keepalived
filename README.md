#For exemple host or group var for cross vrrp instances:
```
keepalived_enable: yes
keepalived_vrrp_scripts:
    chk_script:
        script: '/usr/local/bin/manage_keepalived.sh status'
        rise: 1
        interval: 60
        fall: 2
keepalived_vrrp_instances:
    VI_44:
        interface: eth0
        state: MASTER
        virtual_router_id: 10
        priority: 150
        advert_int: 1
        smtp_alert: true
        auth_type: PASS
        auth_pass: password
        virtual_ipaddresses:
            - *.*.*.*
        track_scripts:
            - chk_script
        notify_master: /usr/local/bin/manage_keepalived.sh master
        notify_backup: /usr/local/bin/manage_keepalived.sh backup
        notify_fault: /usr/local/bin/manage_keepalived.sh fault
    VI_45:$
        interface: eth0
        state: BACKUP
        virtual_router_id: 11
        priority: 100
        advert_int: 1
        smtp_alert: true
        auth_type: PASS
        auth_pass: password
        virtual_ipaddresses:
            - *.*.*.*
        track_scripts:
            - chk_script
        notify_master: /usr/local/bin/manage_keepalived.sh master
        notify_backup: /usr/local/bin/manage_keepalived.sh backup
        notify_fault: /usr/local/bin/manage_keepalived.sh fault
```
#For exemple host or group var for single vrrp instances:
```
 keepalived_enable: yes
 keepalived_vrrp_scripts:
     chk_script:
         script: '/sbin/pidof nameservices'
         rise: 1
         interval: 5
         fall: 2
 keepalived_vrrp_instances:
     VI_55:
         state: MASTER (On the second server BACKUP)
         priority: 150 (On the second server less priority)
         interface: eth0
         virtual_router_id: 22
         advert_int: 1
         nopreempt: true
         smtp_alert: true
         auth_type: PASS
         auth_pass: password
         virtual_ipaddresses:
             - 10.10.10.01
         track_scripts:
             - chk_script
         notify_fault: /usr/bin/systemctl restart nameservices
```

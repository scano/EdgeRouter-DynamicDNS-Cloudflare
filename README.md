# EdgeRouter-DynamicDNS-Cloudflare

Commands to automatically update a host dns record on Cloudflare.
The system detects the ip change an update the record.
Firmware: 2.0.8

Customize:
  - CLOUDFLARE-EMAIL-ACCOUNT
  - CLOUDFLARE-API-KEY-ACCOUNT
  - HOST-NAME
  - ZONE
  
In my case router is on `10.0.0.1` and the custom fields are:
  - CLOUDFLARE-EMAIL-ACCOUNT: admin@domain.com
  - CLOUDFLARE-API-KEY-ACCOUNT: dc5aaaaaa11111aaaa111aaa11aa1a1a1a1a1be0
  - HOST-NAME: office.domain.com
  - ZONE: domain.com

Connect to router via ssh `$ ssh admin@10.0.0.1`

```
configure
set service dns dynamic interface pppoe0 service custom-cloudflare host-name HOST-NAME
set service dns dynamic interface pppoe0 service custom-cloudflare login CLOUDFLARE-EMAIL-ACCOUNT
set service dns dynamic interface pppoe0 service custom-cloudflare password CLOUDFLARE-API-KEY-ACCOUNT
set service dns dynamic interface pppoe0 service custom-cloudflare protocol cloudflare
set service dns dynamic interface pppoe0 service custom-cloudflare options zone=ZONE
commit ; save
exit
```

You can verify the status and force an update of the Dynamic DNS service using the commands below:

```
show dns dynamic status
update dns dynamic interface pppoe0
```

*Extra*

Create an automated task to update Dynamic DNS if needed

```
set system task-scheduler task dyndns_update executable arguments 'update dns dynamic interface pppoe0'
set system task-scheduler task dyndns_update executable path /opt/vyatta/bin/vyatta-op-cmd-wrapper
set system task-scheduler task dyndns_update interval 7d
```

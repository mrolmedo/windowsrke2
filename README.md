 ### PRE-CHECK
 
- [:white_check_mark:] CNI: calico [https://docs.rke2.io/install/quickstart#windows-agent-worker-node-installation]

- [:white_check_mark:] WindowsOptionalFeature  Installed and Enable
```
Get-WindowsOptionalFeature -Online -FeatureName containers
Get-WindowsFeature -Name Containers
```

- [:white_check_mark:] HNS and Hyper-V Host Compute Service running
```
Get-Service hns,vmcompute
Status   Name               DisplayName
------   ----               -----------
Running  hns                Host Network Service
Running  vmcompute          Hyper-V Host Compute Service
```

### CHECKS SERVICE 
- [:white_check_mark:] RKE2 Service

| Linux | Windows| 
| --- | --- |  
| systemctl status rke2-server | Get-Service -Name rke2
| systemctl status rancher-system-agent | Get-Service -Name rancher-wins
| journalctl -fu rke2-server     | Get-EventLog -LogName Application -Source 'rke2'  -Newest 500 | format-table  -Property TimeGenerated, ReplacementStrings -Wrap
  journalctl -fu rancher-system-agent    | Get-EventLog -LogName Application -Source 'rancher-wins'  -Newest 500 | format-table  -Property TimeGenerated, ReplacementStrings -Wrap

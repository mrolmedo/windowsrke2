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
| systemctl status rke2-agent | Get-Service -Name rke2
| systemctl status rancher-system-agent | Get-Service -Name rancher-wins
| journalctl -fu rke2-server     | Get-EventLog -LogName Application -Source 'rke2'  -Newest 500  format-table  -Property TimeGenerated, ReplacementStrings -Wrap
| journalctl -fu rancher-system-agent    | Get-EventLog -LogName Application -Source 'rancher-wins'  -Newest 500  format-table  -Property TimeGenerated, ReplacementStrings -Wrap


### CHECKS-PROCESS

- [:white_check_mark:] Kubelet, kube-proxy, calico running, containerd, containerd-shim-runhcs-v1
```
 Get-Process -Name calico-node,kubelet,containerd*,kube-proxy,containerd-shim-runhcs-v1
```
```
Output
Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    256      16    27692      40384   1,713.33   2748   0 calico-node
    313      22    43180      52568   2,244.58   3440   0 containerd
    346      25    49032      67980   7,920.91   2116   0 kubelet
    197      17    27320      37516     181.41    440   0 kube-proxy
```

### ERRORS

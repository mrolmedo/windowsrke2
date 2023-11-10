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

PS C:\etc\rancher> Get-service hns -RequiredServices

Status   Name               DisplayName
------   ----               -----------
Running  vfpext             Microsoft Azure VFP Switch Extension
Running  RpcSs              Remote Procedure Call (RPC)
Running  nsi                Network Store Interface Service

```

### CHECKS SERVICE 
- [:white_check_mark:] RKE2 Service
- [:white_check_mark:] Rancher System Agent (rancher-wins)

| Linux | Windows| 
| --- | --- |  
| systemctl status rke2-agent | Get-Service -Name rke2
| systemctl status rancher-system-agent | Get-Service -Name rancher-wins
| journalctl -fu rke2-server     | Get-EventLog -LogName Application -Source 'rke2'  -Newest 500 \| format-table  -Property TimeGenerated, ReplacementStrings -Wrap
| journalctl -fu rancher-system-agent    | Get-EventLog -LogName Application -Source 'rancher-wins'  -Newest 500  \|format-table  -Property TimeGenerated, ReplacementStrings -Wrap

#### Additional info
```
PS C:\Windows> (Get-EventLog -LogName "System" -Source "Service Control Manager" -EntryType "Information" -Message "*hns service*running*" -Newest 4).TimeGenerated

Wednesday, October 18, 2023 4:20:39 PM
Wednesday, October 18, 2023 4:18:03 PM
Monday, October 16, 2023 1:41:21 PM
Monday, October 16, 2023 1:36:13 PM
```

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

### CRICTL COMMAND
```
cd C:\var\lib\rancher\rke2\bin
```

-  #### List images
```
PS C:\var\lib\rancher\rke2\bin> ./crictl.exe -r 'npipe:////./pipe/containerd-containerd' image ls
IMAGE                                        TAG                                       IMAGE ID            SIZE
docker.io/rancher/pause                      3.6                                       2a67292b6e8ba       119MB
docker.io/rancher/wins                       v0.4.11                                   fb25704df6c5c       2.37GB
mcr.microsoft.com/dotnet/framework/runtime   4.8-20230613-windowsservercore-ltsc2022   00fa7942c440d       1.48GB

```

### ERRORS

| Issue  | Error  | Solution| 
| -----  | ---    |    --- | 
 Service hns stopped or StopPending status| Status StopPending Name hns | Identify the PID  PS C:\etc\rancher> Get-CimInstance  -ClassName Win32_Service -Filter "name = 'hns'" and kill it (Stop-Proccess -Id "PID")


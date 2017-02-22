**[fcgid:warn] [pid 10766:tid 139785079355136] (32)Broken pipe: [client 127.0.0.1:44177] mod_fcgid: ap_pass_brigade failed in handle_request_ipc function, referer:**
[https://www.virtualmin.com/node/32108](https://www.virtualmin.com/node/32108)
[https://www.allsupported.com/php-broken-pipe-mod_fcgid-ap_pass_brigade-failed-in-handle_request_ipc-function/](https://www.allsupported.com/php-broken-pipe-mod_fcgid-ap_pass_brigade-failed-in-handle_request_ipc-function/)
[https://www.virtualmin.com/node/6199](https://www.virtualmin.com/node/6199)


```text
<IfModule fcgid_module>
    # Context - server config
    FcgidMaxProcesses 2000
    # Otherwise php output shall be buffered
    # FcgidOutputBufferSize 0
    FcgidInitialEnv PHP_FCGI_MAX_REQUESTS 2000
    FcgidMaxRequestsPerProcess 2000
    FcgidIOTimeout  1200
    FcgidIdleTimeout  1200
    MaxRequestLen 15728640

    FcgidBusyTimeout 300
    FcgidBusyScanInterval 120
    FcgidProcessLifeTime  3600
    FcgidZombieScanInterval 3
</IfModule>
```
**Note:** `FcgidMaxProcesses`, `FcgidInitialEnv`, `FcgidMaxRequestsPerProcess` 的值应该是一样的，否则会得到以下错误
1. **[fcgid:warn] [pid 10766:tid 139785079355136] (32)Broken pipe: [client 127.0.0.1:44177] mod_fcgid: ap_pass_brigade failed in handle_request_ipc function, referer:**
2. **[fcgid:warn] [pid 12232:tid 139785162512128] (104)Connection reset by peer: [client 127.0.0.1:44536] mod_fcgid: ap_pass_brigade failed in handle_request_ipc function, referer:** **
- solved by @lawence3713

기기의 파일시스템 중 숨겨진 문자열을 찾는 문제입니다.

기기에 Linux USB-A를 연결하면 쉘이 켜집니다.

```
ubuntu@live:/mnt/opt/scanworks$ cat config.yaml
# Emedra ScanWorks - runtime configuration
# Generated at commissioning. DO NOT edit by hand.
site_id: stdymphna-rmc-01
upstream: https://api.emedra.example/v2

# Upstream API key (service account). TODO: move to secrets store before GA.
api_key: deadface{h4rdc0d3d_k3ys_1n_th3_c0nf1g}

integrity:
  salt: EM-2019          # calibration signature salt

debug_console:
  enabled: true
  function: debug.dump
  tables: [patients, calibration, audit]
```
USB를 마운트 하고 해당 경로에서 플래그를 확인할 수 있습니다.

- FLAG: deadface{h4rdc0d3d_k3ys_1n_th3_c0nf1g}

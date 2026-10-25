- solved by @lawence3713

BLE-Sniffer USB-A에서 패킷을 캡쳐하면 다음과 같은 패킷들이 스니핑된다.
```
handle=0x2a seq= 69 len= 6  9fdb212e1da3
handle=0x2a seq= 42 len= 6  5eea486eeed3
handle=0x37 seq= 23 len= 4  66516f3d
handle=0x2a seq=  2 len= 6  7a69792526ec
handle=0x37 seq=  6 len=12  4948526c593267395a57316c
handle=0x2a seq=  7 len= 6  479608ada143
...
```
seq는 순서를 나타낼 것이고, handle=0x37인 패킷은 아스키 범위 내의 문자열인 것을 확인할 수 있다.

이들만 해석하는 파이썬 코드를 짜면 된다.
```
import json

with open("/content/ble_scan.json", "r") as f:
    data = json.load(f)

packets = sorted(
    [item for item in data if item["handle"] == "0x37"],
    key=lambda item: item["seq"]
)

for item in packets:
    payload = item["payload"]

    decoded = "".join(
        chr(int(payload[i:i+2], 16))
        for i in range(0, len(payload), 2)
    )

    print(f"{decoded}", end='')
```
실행하면 Base64처럼 보이는 문자열들이 출력된다.
```
-->

St.Dymphna CS-9000 maint log | site=stdymphna-rmc-01 | tech=emedra-svc
last_patient: Gideon Marsh (MRN 100413)
note: telemetry unencrypted per EM-2019 default; flag deadface{un3ncrypt3d_b4bbl3_fr0m_th3_sku11}
```
deadface{un3ncrypt3d_b4bbl3_fr0m_th3_sku11}

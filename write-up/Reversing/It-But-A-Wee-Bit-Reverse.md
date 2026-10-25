- solved by @lawence3713

```
  int32_t main(int32_t argc, char** argv, char** envp)
00401209  {
00401215      void* fsbase;
00401215      int64_t rax = *(fsbase + 0x28);
00401233      printf("Enter password: ");
00401253      char buf[0x48];
00401253      int32_t result;
00401253      
00401253      if (fgets(&buf, 0x40, __TMC_END__))
00401253      {
00401275          buf[strcspn(&buf, "\n")] = 0;
00401275          
00401292          if (0xa == strlen(&buf))
00401292          {
004012ad              int64_t var_78_1 = 0;
004012ad              
00401305              while (true)
00401305              {
00401305                  if (var_78_1 >= 0xa)
00401305                  {
00401311                      puts("Access granted!");
00401311                      
0040136a                      for (int64_t i = 0; i < 0x1f; i += 1)
00401358                          putchar(buf[COMBINE(0, i) % 0xa] ^ *(i + &flag_enc));
00401358                      
00401371                      putchar(0xa);
00401376                      result = 0;
00401376                      break;
00401305                  }
00401305                  
004012dd                  if ((buf[var_78_1] ^ 0x42) != pw_enc[var_78_1])
004012dd                  {
004012e9                      puts("Access denied.");
004012ee                      result = 1;
004012f3                      break;
004012dd                  }
004012dd                  
004012f8                  var_78_1 += 1;
00401305              }
00401292          }
00401292          else
00401292          {
0040129e              puts("Access denied.");
004012a3              result = 1;
00401292          }
00401253      }
00401253      else
00401255          result = 1;
00401255      
0040137f      *(fsbase + 0x28);
0040137f      
00401388      if (rax == *(fsbase + 0x28))
00401390          return result;
00401390      
0040138a      __stack_chk_fail();
0040138a      /* no return */
00401209  }
```

메인함수는 위와 같고, `flag_enc`와 `pw_enc`는 다음과 같다.
```
00404010  char pw_enc[0xb] = "17,1*+,\'vp", 0
0040401b                                   00 00 00 00 00             .....
00404020  flag_enc:
00404020  17 10 0f 17 0e 08 0d 00 4f 4a 43 07 31 42 1b 36
00404030  00 0a 40 6d 16 1b 0d 01 11 19 1a 0c 5b 5c 0e
```
메인 함수는 버퍼에 `pw_enc` 를 0x42 와 XOR 한 값을 담고, 이를 `flag_enc` 와 XOR 한다.
**solve.py**
```
flag_enc = [0x17, 0x10, 0x0f, 0x17, 0x0e, 0x08, 0x0d, 0x00, 0x4f, 0x4a, 0x43, 0x07, 0x31, 0x42, 0x1b, 0x36, 0x00, 0x0a, 0x40, 0x6d, 0x16, 0x1b, 0x0d, 0x01, 0x11, 0x19, 0x1a, 0x0c, 0x5b, 0x5c, 0x0e]
pw_enc_origin = "17,1*+,\'vp"
pw_enc = []
for i in pw_enc_origin:
  pw_enc.append(ord(i))

for i in range(len(pw_enc)):
  pw_enc[i] ^= 0x42

flag = ''
for i in range(len(flag_enc)):
  dec = pw_enc[i % 0xa] ^ flag_enc[i]
  flag += chr(dec)

print(flag)
```

- Flag: deadface{x0r_1s_not_encryption}

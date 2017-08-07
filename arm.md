## arm環境 (ubuntu)
```sh
$ sudo apt-get install gdb-arm-none-eabi
```
arm-none-eabi-objdump とかもこれで入る

|命令|機械語|
|:--:|:--:|
|BEQ|0A|
|BNE|1A|
|BLE(より小さいか等しい)|DA|
|BGT(より大きい)|CA|

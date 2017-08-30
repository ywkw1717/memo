# Install
[https://github.com/idapython/src](https://github.com/idapython/src)

First, My IDA Demo version is 6.95, but I encountered many errors. I can not install idapython with "make" command. (Windows10)

## Get the idapython binary
Hence, I used binary that is already complete. (It can get here [https://github.com/idapython/bin](https://github.com/idapython/bin))

I selected `idapython-6.9.0-python2.7-win.zip`. Because there is not 6.95 version.

Therefore, IDA Demo need 6.9.0 version.

## Get the IDA Demo 6.9.0
There is not link 6.95 version in [official site](https://www.hex-rays.com/products/ida/support/download_demo.shtml).

But URL of 6.95 version is [https://out7.hex-rays.com/files/idademo695_windows.exe](https://out7.hex-rays.com/files/idademo695_windows.exe).

So I accessed [https://out7.hex-rays.com/files/idademo69_windows.exe](https://out7.hex-rays.com/files/idademo69_windows.exe).

I can get 6.9.0 version :)

## Install idapython in IDA Demo
1. Copy the whole "python" directory to %IDADIR%

2. Copy the contents of the "plugins" directory to the %IDADIR%\plugins\

3. Copy "python.cfg" to %IDADIR%\cfg

But happend error `can not load file`.

I changed python version 2.7.13 -> 2.7

Install is succeed!!!

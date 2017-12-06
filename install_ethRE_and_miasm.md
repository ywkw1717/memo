```shell
$ git clone https://github.com/jbcayrou/miasm.git
$ cd miasm
$ python setup.py build
libtcc.h not found

$ git clone http://repo.or.cz/tinycc.git
$ cd tinycc
$ ./configure
$ make
makeinfo not found

$ sudo apt-get install texinfo
$ make
$ make test
$ sudo make install
$ tcc -v
tcc version 0.9.27 (x86_64 Linux)

$ cd miasm
$ python setup.py build
Warning: TCC is not properly installed,
Miasm will be installed without TCC Jitter
Etheir install TCC or use LLVM jitter

tccのversionがダメっぽい

$ wget http://repo.or.cz/tinycc.git/snapshot/d5e22108a0dc48899e44a158f91d5b3215eb7fe6.tar.gz
$ tar -xvzf d5e22108a0dc48899e44a158f91d5b3215eb7fe6.tar.gz
$ cd tinycc-d5e2210
$ ./configure
$ make
texi2html not found

$ sudo apt-get install texi2html
$ make
$ make test // なんか失敗しているが続ける
$ make install
$ tcc -v
tcc version 0.9.26 (x86-64 Linux)

$ cd miasm
$ git checkout evm
$ python setup.py build
$ sudo python setup.py install

$ git clone https://github.com/jbcayrou/ethRE.git
$ cd ethRE
$ python ethre/ethre.py --account 0xde0b295669a9fd93d5f28d9ec85e40f4cb697bae -o ./mygraph.dot
ImportError: No module named elfesteem

$ git clone https://github.com/serpilliere/elfesteem.git
$ cd eflsteem
$ python setup.py build
$ sudo python setup.py install

% cd ethRE
$ python ethre/ethre.py --account 0xde0b295669a9fd93d5f28d9ec85e40f4cb697bae -o ./mygraph.dot
```

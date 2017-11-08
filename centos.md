Error:
```
/usr/include/gnu/stubs.h:7:27: error: gnu/stubs-32.h: No such file or directory
```

Solution:
```
sudo yum -y install glibc-devel.i686 glibc-devel
```

Make a 32bit applicatoin
```
sudo yum -y install glibc-devel.i686 glibc-devel libstdc++-devel.i686
```

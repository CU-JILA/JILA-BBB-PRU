# Cross-compiling pru-gcc



## Build and Installation Steps
```
git clone https://github.com/crosstool-ng/crosstool-ng
apt-get update
apt-get install apt-utils
```

## Download dependencies

To check required dependencies
Example:
```
cd crosstool-ng/testing/docker/ubuntu19.10/
```
```
apt-get install gcc g++ gperf bison flex texinfo help2man make libncurses5-dev python3-dev autoconf automake libtool libtool-bin gawk wget bzip2 xz-utils unzip patch libstdc++6 rsync git
```

## Build continued

```
cd crosstool-ng/
./bootstrp
./configure --enable-local
make
```
```
./ct-ng list-samples | grep pru
./ct-ng pru
``` 

If can't able to build as root:

Reference: "https://https://stackoverflow.com/questions/17466017/how-to-solve-you-must-not-be-root-to-run-crosstool-ng-when-using-ct-ng

```
./ct-ng menuconfig

  	-> Paths and misc options
       	-> Try features marked as EXPERIMENTAL
 			-> Allow building as root user (READ HELP!)
				-> Are you sure?
```
```
./ct-ng source
./ct-ng build
PATH=$HOME/x-tools/pru-elf/bin:$PATH
```


## Source and references: 

1.https://github.com/dinuxbg/gnupru#building-using-crosstool-ng

2.https://crosstool-ng.github.io/docs/install/#hackers-way
 

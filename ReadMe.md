# Concurrency Memory Bug Prediction Benchmarks

## Source Code Download
Please use the following commands to download the source code.
```
cd program_srcs
./get_src.sh
```

## Compiling

In the `CVE-Benchmark`, use the following command to compile the programs:
```
/path/to/clang++ -DSLEEP_FOR_RACE /path/to/input -o /path/to/output 
```

In `httrack-3.43.9`, see the file `INSTALL` for details.

In `lbzip2`, use the following commands to compile:
```
./build-aux/autogen.sh
configure --prefix=/path/to/install
make -j10 install
```

In `libvpx` and `x264`, use the following commands to compile:
```
configure --prefix=/path/to/install
make -j10 install
```
In `libwebp` and `pixz`, use the following commands to compile:
```
./autogen.sh
configure --prefix=/path/to/install
make -j10 install
```

In `mysql-5.7.36`, use the following commands to compile:
```
mkdir bld
cd bld
cmake ../ -DDOWNLOAD_BOOST=1 -DWITH_BOOST=../boost -DWITH_TREC=1 -DWITH_DEBUG=1 -DCMAKE_INSTALL_PREFIX=/path/to/install 
make -j10 install
cd ..
```

In `pbzip2-0.9.4` and `pigz`, modify the `Makefile` by replacing the `CC` with the path to your clang compiler, and the `PREFIX` with the path to your installation directory. Then run `make -j10 install` to compile and install the program.

In `x265`, use the following commands to compile:
```
cd build
cmake ../source
make -j10 
cd ..
```

## Program Running
Here we provide the commands used to run the programs. `${filename}` denotes the path to the input file. 

`CVE-benchmark`: 
```
/binary/file/path
```

`httrack`: see the `INSTALL` file in its directory for details.

`lbzip2`: 
```
lbzip2 -k -t -9 -z -f -n4 ${filename}
``` 

`libvpx`: 
```
vpxdec -t 4 -o ${filename%.*}.y4m ${filename}
```

`libwebp`: 
```
cwebp -mt $filename -o ${filename%".png"}.webp
```

`pbzip`: 
```
pbzip -k -f -p3 $filename
```

`pigz`: 
```
pigz -p 4 -b 32 -k ${filename} 
```

`pixz`: 
```
pixz -9 -k -p 4  ${filename}
```

`x264`: 
```
x264 --threads=4 -o ${filename%.*}".out" $filename
```

`x265`: 
```
x265 --input $filename  --pools 4 --frame-threads 2 -o ${filename%.*}".out" 
```

`mysql`:
```
cd /mysql/install/directory
cd mysql-test
./mysql-test-run --record 1st.test
./mysql-test-run --record almost_full.test
```

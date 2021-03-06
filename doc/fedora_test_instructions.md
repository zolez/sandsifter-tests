# Instructions for running Sandsifter on a Fedora Machine


## Preparing your system
```
sudo dnf install gcc glibc-static -y # Install gcc compiler and glibc-static
sudo pip install capstone # Install capstone for Python
```

## Download Capstone

```
wget https://github.com/aquynh/capstone/archive/master.zip # Download Capstone
unzip master.zip # Unpack to capstone-master
rm master.zip # Remove archive
```

## Download Sandsifter

```
wget https://github.com/rigred/sandsifter/archive/master.zip # Download Sandsifter
unzip master.zip # Unpack to sandsifter-master
rm master.zip # Remove archive
```

## Compile Capstone

```
cd capstone-master
sed -i -e 's/CAPSTONE_ARCHS ?= arm aarch64 mips powerpc sparc systemz x86 xcore/CAPSTONE_ARCHS ?= x86/g' config.mk # Change config file for only x86 compilation
./make.sh # Compile Capstone
sudo ./make.sh install # Install Capstone
cd ..
```

## Compile Sandsifter

```
cd sandsifter-master
make # Compile Sandsifter
cd ..
```

## Remove Capstone (This step is optional)
```
cd capstone-master
sudo ./make.sh uninstall # Uninstall Capstone
cd ..
rm -rf capstone-master # Remove Capstone directory. It was only needed to be able to compile Sandsifter
```

## Run Sandsifter
```
cd sandsifter-master
```

### Change terminal to be able to show Sandsifter correctly
```
export TERM='xterm-256color'
```

### If SELinux installed, you have to grant access to Sandsifter
```
sudo semodule -X 300 -i my-injector.pp 
sudo setsebool -P mmap_low_allowed 1 # If SELinux installed, you have to grant access to Sandsifter
sudo setsebool -P mmap_low_allowed 1 # If SELinux installed, you have to grant access to Sandsifter
```

### Finally run sandsifter
```
sudo ./sifter.py --unk --dis --len --sync --tick -- -P1 -t # Run Sandsifter
```

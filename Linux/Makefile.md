# Makefile

You can do a set of commands using Makefile.

I needed to assemble a new `.deb` package by using `make dpkg`. Then, I created a Makefile just like this:

```makefile
dpkg: binary dpkg-structure add-binary arm-debian

arm-debian:
	dpkg-deb --build --root-owner-group build/spcu_client_0.0.1-3_armhf

binary:
	env GOOS=linux GOARCH=arm go build -o build/spcu-client .

dpkg-structure:
	mkdir -p build/spcu_client_0.0.1-3_armhf
	mkdir -p build/spcu_client_0.0.1-3_armhf/opt/spcu/
	mkdir -p build/spcu_client_0.0.1-3_armhf/var/log/spcu/ 
	
	mkdir build/spcu_client_0.0.1-3_armhf/DEBIAN
	
	touch build/spcu_client_0.0.1-3_armhf/DEBIAN/control
	echo "Package: SPCU-Client" >> build/spcu_client_0.0.1-3_armhf/DEBIAN/control
	echo "Version: 0.0.1" >> build/spcu_client_0.0.1-3_armhf/DEBIAN/control
	echo "Architecture: armhf" >> build/spcu_client_0.0.1-3_armhf/DEBIAN/control
	echo "Maintainer: SPCUINC <Admin@spcuinc.com>" >> build/spcu_client_0.0.1-3_armhf/DEBIAN/control
	echo "Description: A program managing IoT devices." >> build/spcu_client_0.0.1-3_armhf/DEBIAN/control

	touch build/spcu_client_0.0.1-3_armhf/DEBIAN/postinst
	echo "#!/bin/sh" >> build/spcu_client_0.0.1-3_armhf/DEBIAN/postinst
	echo "echo 'Adding to the path...'" >> build/spcu_client_0.0.1-3_armhf/DEBIAN/postinst
	echo "export PATH=$PATH:/opt/spcu" >> build/spcu_client_0.0.1-3_armhf/DEBIAN/postinst
	chmod 0555 build/spcu_client_0.0.1-3_armhf/DEBIAN/postinst

add-binary:
	cp build/spcu-client build/spcu_client_0.0.1-3_armhf/opt/spcu/

clean:
	rm -r ./build/*
```

It is important to use `nano` over `vim` for editing. Specifically, Makefile has a huge issue with tabs. You should make sure to use tabs in the target blocks.

To always execute the makefile: 

```bash
make -B all
# Or
make --always-make all
```
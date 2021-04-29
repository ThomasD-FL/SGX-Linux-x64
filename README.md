# SGX-Linux-x64
Intel I7  Core Prozessor unterstuetzt nicht die SGX Technologie
Hardware Intel I7 3770 Family 5
Bios : no Intel Advanced menu (no update) :(
Anforderung : Intel SGX SDK
Lösung out-of-tree driver for Linux SGX Software stack
Software: VM - Box unter Windows 10 Pro
Ubuntu mate 20.04
Kernel schon gestript :)
instalieren von Docker und Docker Compose

> sudo apt-get remove docker docker-engine docker.io containerd runc

Weg mit den Altlasten

> sudo apt-get update
> curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
> lsb_release -cs
> echo   "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

> sudo apt-get update
> sudo apt-get updatesudo apt-get install docker-ce docker-ce-cli containerd.io
> sudo apt-get install docker-ce docker-ce-cli containerd.io
> apt-cache madison docker-ce

> sudo apt-get install docker-ce=5:20.10.6~3-0~ubuntu-bionic docker-ce-cli=5:20.10.6~3-0~ubuntu-bionic containerd.io

Achtung auf die Version achten !! ( 5:20.10.6~3-0~ubuntu-bionic)

> sudo docker run hello-world

wie immer :)

Installieren von dkms (Dynamic Kernel Module Support) und paar Linux Header für den Kernel

> sudo apt-get install dkms
> sudo find / -name dkms.conf
> less /etc/modprobe.d/dkms.conf

Wenn es der erste Kernel ist, einmal kontrollieren ob die Konf - Datei leer ist

> sudo dkms add -m sgx -v 

sgx dazu 

> dpkg-query -s linux-headers-$(uname -r)
> sudo apt-get install linux-headers-$(uname -r)

Header - Dateien und los !

> make

> sudo mkdir -p "/lib/modules/"`uname -r`"/kernel/drivers/intel/sgx" 
> sudo cp isgx.ko "/lib/modules/"`uname -r`"/kernel/drivers/intel/sgx"
 
 rüber copiert 

>sudo sh -c "cat /etc/modules | grep -Fxq isgx || echo isgx >> /etc/modules"
>sudo /sbin/depmod
>sudo /sbin/modprobe isgx
 
 Kernel Module analysiert 

>make

kann länger dauern :)

Leider gehört meine CPU nicht zur SGX -Gemeinde, kann bei Euch anders sein Good Luck! 
Also weiter:

Treiber suchen :

https://download.01.org/intel-sgx/sgx-linux/2.13/distro/ubuntu20.04-server/

> cd Downloads/
> ll
> sudo chmod +x sgx_linux_x64_driver_1.41.bin 

zweiter Versuch,  Intel sagt kein support und ?
> sudo ./sgx_linux_x64_driver_1.41.bin 


> copy /usr/src/sgx-1.41/ o 1.41
> sudo copy /usr/src/sgx-1.41/ o 1.41
> sudo copy /usr/src/sgx-1.41/  1.41

copieren 


> sudo dkms add -m sgx -v 1.41
> sudo /sbin/modprobe intel_sgx
> sudo dkms build -m sgx -v 1.41
> sudo groupadd sgx_prv
> sudo udevadm trigger
> ./build_compose_run.sh
> sudo make preparation
 
 >./build_and_run_aesm_docker.sh
> sudo apt-get install build-essential ocaml ocamlbuild automake autoconf libtool wget python-is-python3 libssl-dev git cmake perl
> sudo apt-get install libssl-dev libcurl4-openssl-dev protobuf-compiler libprotobuf-dev debhelper cmake reprepro unzip

Diese Dateien noch kopiert
cp external/toolset/{current_distr}/{as,ld,ld.gold,objdump} /usr/local/bin


jetzt noch sdk :

This repository supports to build the Intel(R) SGX SDK with below three combinations:

    USE_OPT_LIBS=0 --- build SDK using SGXSSL and open sourced String/Math
    USE_OPT_LIBS=1 --- build SDK using optimized IPP crypto and open sourced String/Math
    USE_OPT_LIBS=2 --- build SDK with no mitigation using SGXSSL and optimized String/Math
    USE_OPT_LIBS=3 --- build SDK with no mitigation using IPP crypto and optimized String/Math
    The default build uses USE_OPT_LIBS=1, if you directly type $ make sdk as above. You can switch to the other build combinations instead by entering the following command:


> sudo make sdk USE_OPT_LIBS=1    ich habe hier 1 genommen
> sudo make clean
> sudo make sdk_install_pkg

jetzt noch python und die Umgebung :

> sudo apt-get install build-essential python
> source ${sgx-sdk-install-path}/environmen    Ich hoffe Ihr habt der path

und jetzt der Test :

> cd sgxsdk/SampleCode/LocalAttestation/
> make SGX_MODE=SIM
> cd bin
> ./app
> 
succeed to load enclaves.
succeed to establish secure channel.
Succeed to exchange secure message...
Succeed to close Session...

geht :))







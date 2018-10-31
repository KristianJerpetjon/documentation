Ubuntu 18.04
====
Installing prerequisites
~~~~
Enable the ubuntu universe repository
**NOTE:** If you already have the universe repo enabled this is not neccesary
::
	$ sudo add-apt-repository universe
Install the following packages
::
	$ sudo apt-get install clang-6.0 git cmake nasm qemu-system-x86 python-jsonschema python-psutil python-pystache

Install the antlr4-python2-runtime package
**NOTE:** make sure you install the antlr4 dependency as a user and not using sudo
::
	$ sudo apt-get install python-pip
	$ pip install antlr4-python2-runtime

Setting up environment
~~~~
IncludeOS requires that you have the :code:`INCLUDEOS_PREFIX` defined in your environmet.
To utilize the provided tools like the :code:`boot` command you also need to add :code:`${INCLUDEOS_PREFIX}` to your path.
**NOTE:** Setting the CC and CXX variables to clang-5.0 or clang-6.0 is for now the best option however you can try at your own risk using gcc-7.3.0 or gcc-8.2.0 instead
::

    export CC=clang-6.0
    export CXX=clang++-6.0
    export INCLUDEOS_PREFIX=${HOME}/includeos
    export PATH=$PATH:$INCLUDEOS_PREFIX/bin

Building includeos
~~~~
The following commands will build an out of tree includeos and install it to wherever $INCLUDEOS_PREFIX is located
**NOTE:** change paths to your own liking
:: 
    mkdir ${HOME}/git
    cd ${HOME}/git
    git clone https://github.com/hioa-cs/IncludeOS.git
    mkdir ${HOME}/build/includeos-clang
    cd ${HOME}/build/includeos-clang
    cmake ${HOME}/git/IncludeOS
    make -j
    make install

Testing the installation
~~~~
You can manually compile a service for includeos using cmake or use the :code:`boot . -c command` for both building and running your desired service
**NOTE:** The --create-bridge is only required if you need a network bridge on your service and do not have one running
::
   cd ${HOME}/git/IncludeOS/examples/http-server
   boot --create-bridge . -c

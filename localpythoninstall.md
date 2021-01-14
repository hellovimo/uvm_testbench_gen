# How to install Python in local directory without root user permission

1. Down the required python package from the [Python Download](https://www.python.org/downloads/) page.
2. Untar the package and place them in the required directory path.
3. cd into the untar-ed directory.
4. Since we are going to install in the local directory as you might not be having root permission to install, execute the below command
   - `Syntax: ./configure --prefix=<path where you wanted to install the libraries>`
   - `Eg:     ./configure --prefix=~/installed`
5. Next run the below command to execuit the initial process.
   - `make`
6. Here you need to watch carefully to be sure the make is clean or not. Because sometimes the make might miss installing certain packages for example:
   - ctypes : This package is required for the installation of packages using the pip mode.
   - tkinter : This package is required for the tcl/tk i.e. the GUI applications especially for the tool.
7. Lets take the worst case scenario where both these packages are missing. Follow the below steps before continuing.
   - 7.1 The ctypes lib is built into the libffi package and you can download from the [libffi download](https://sourceware.org/libffi/) page.
   - 7.2 Follow the README steps to install the package locally. For example the below steps:
     - `./configure --prefix=~/installed`
     - `make`
     - `make install`
   - 7.3 Next download the tcl and tk package from the [TCL/TK download](https://www.tcl.tk/software/tcltk/download.html) page. You need to install both tcl and tk package separately and follow the README steps to install locally. Steps for installation will be similar as provided in step 7.2 
8. After following the steps in 7 to install ctypes and tcl/tk packages, these libraries are stored in the local/prefix path which you have provided.
   - `~/installed/lib and ~/installed/lib64`
9. Next we need to setup some environment variables before proceeding with the Python package installation. We need to setup LD_LIBRARY_PATH and LD_RUN_PATH as shown below.
   - ```
     export LD_LIBRARY_PATH=~/installed/lib:~/installed/lib64`
     export LD_RUN_PATH=~/installed/lib64`
     ```
10. Next we need to repeat the Step 4 and Step 5 with a change in configure command as given below. Note, the path for the CPPFLAGS is the downloaded libffi packages. And LDFLAGS is the path where you have the libffi installed libs files.
    ```
    ./configure --prefix=~/installed --with-tcltk-libs='-L~/installed/lib' --with-tcltk-includes='-I~/installed/include' LDFLAGS="-L~/installed/lib64" CPPFLAGS="-I~/packages/libffi/include"
    ```
11. Next you need to execute the below command to complete the installation process.
    - `make`
    - `make altinstall`
12. Now the ctypes/libffi lib is installed, this will help us in installing other necessary packages using pip3. 
13. But before that what is Pip? Pip is the standard package manager for Python. It allows you to install and manage additional packages that are not part of the Python standard library. 
14. The Steps to install pip locally is shown below. 
    - 14.1 Download the pip package by executing the below command.
      - `wget https://bootstrap.pypa.io/get-pip.py`
    - 14.2 Next execute the below command to install pip. This will install pip to your local directory `.local/bin`.
      - `python3 get-pip.py --user`
15. Next we will install the Openpyxl package which is used for opening/writing/reading spreadsheets inside the Python scripts. 
    - 15.1 To install the openpyxl package using pip, execute the below command. 
      - `pip3 install openpyxl`
    - 15.2 To install the openpyxl package locally follow the below steps.
       - 15.2.1 Download the package from the [Openpyxl Download](https://pypi.org/project/openpyxl/) page.
       - 15.2.2 Unpack the tar file, cd into the directory and execute the below command.<br/>`python3 setup.py install`
       - 15.2.3 It will mostly install the files inside the lib directory which you used for installing the Python. For eg `~/installed/lib/python3.*/site-packages/openpyxl`.
      
16. Now you have all the required packages installed i.e. Python, Tkinter(TCL/TK), Openpyxl to run the uvm_testbench_gen tool, you need to setup following below environment variables in general to run the tool or any python scripts.
      - ```
        Environment Variables: 
        export PATH=$PATH:~/.local/bin
        export PYTHONPATH=$PYTHONPATH:~/installed/lib64:~/installed
        export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:~/installed/lib:~/installed/lib64
        
        Aliases:
        alias python3="~/installed/bin/python3.*"
        ```

Hope the above steps would have been helpful for you to locally install Python and required packages without root permission so that you execute and experience the [UVM Testbench Generator tool](https://github.com/hellovimo/uvm_testbench_gen) !!

[Back To The Home Page](./)

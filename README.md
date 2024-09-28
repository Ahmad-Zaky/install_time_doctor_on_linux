# Install Time Doctor On Linux (Ubuntu)

Here we will focus on how to install Time Doctor tool on Linux, and the common issues you may face during the installation.

## Installation

### Install the desktop app

1. open the time doctor downloads site and download the file. [click here](https://2.timedoctor.com/downloads)
    
2. go to downloads directory and move the downloaded script file `td2-ubuntu-interactive-v3.14.54.sh` to home.
    

`cp td2-ubuntu-interactive-v3.14.54.sh ~`

3\. change file permission.

`chmod +x td2-ubuntu-interactive-v3.14.54.sh`

4\. run the script using this command.

`bash td2-ubuntu-interactive-v3.14.54.sh`

### Install the extension (chrome)

1. install the extension on chrome/firefox browsers.
    
2. open the desktop applicatoin first and then start configure the extension.
    
3. by right clicking on the extension icon you will find a drop menu from which you will select `Options` .
    
4. a new tab will be openned with a form which has two inputs, a text input which will have the host as a value ex. `tfs.aas.com.sa` and the second drop menu input which will have the integration value ex. `Visual Studio Online (TFS)` .
    

### Install the extension (firefox)

1. once you filled the two input fields you will click the `Add` button to submit the form.
    
2. in firefox you will have differnt path to get to the configuration tab.
    
3. right click on the extension icon and pick `Manage Extension`.
    
4. in the oponned extension management tab you will on the top right side a three horizontal dotted button.
    
5. left click the button, a drop menu will appear, pick the `Preferences` option which will opens the host configuration tab.
    
6. Next steps will be same as chrome browser.
    

## Common Issues

Most issues that developers encounter are related to the newer versions of Ubuntu. The Time Doctor tool is somewhat outdated and has compatibility problems with recently released Ubuntu versions. It is primarily compatible with Ubuntu 18.04 (Bionic) and 20.04 (Focal). When attempting to install it on Ubuntu versions beyond these, compatibility issues begin to arise.

### Issue #1 - running script in a directory other than home

Certain Ubuntu versions have an issue where the script will only work if it is run from the home directory, rather than the Downloads directory where the file is located after being downloaded, or also any other directories.

### Issue #2 - Time doctor screenshots are blank

Time doctor is only compatable with Xorg display server protocol, and new ubuntu releases has wayland which is a new display server protocol.

To fix it you will disable wayland to switch back to org by following these commands.

1. open config file `/etc/gdm3/custom.conf`
    

`nanon /etc/gdm3/custom.conf`

2\. uncomment this option by removing the `#` from the beginning of the line.

`#WaylandEnable=false`

3\. save the file and reboot the OS.

### Issue #3 - Time Doctor Extension asks for desktop to be installed first and refuse to start working

- Some brawsers does not work with Time doctor extension like `brave`.
    
- The issue may relate to insufficient permissions that these brawser does not have.
    
- Unfortunately right now, you will only be able to start the extension by installing the extension on chrome, or firefox.
    

### Issue #4 - Submit exension host configuration does not work

- Sometimes if everything works fine and you are trying to submit configuring the host, when you try to submit the form it simply does not work.
    
- The fix we found, which worked with this issue is simply to upgrade the browser version like upgrade chrome and/or firefox.
    

### Issue #5 - the desktop app does not open

- Sometimes you the installation works fine, but the app does not open, and crashes with this output error.
    

`[Main] ERROR sfproc.webview - The render process was terminated "satusCode = 159 " "status = \"Crashed\" " "url = QUrl(\"\") "`

- to fix the issue, export this env var `QTWEBENGINE_DISABLE_SANDBOX`with value `1`
    

`export QTWEBENGINE_DISABLE_SANDBOX=1`

- Prefer to append that export env var into `.bashrc` in your home directory to work on each terminal startup.
    
- rerun time doctor, it should work just fine.
    

### Issue #6 - Installing time doctor script fails

- some how the installation fails due to not installed dependencies.
    
- try to install these dependencies first and reinstall the time doctor script.
    

``` bash
sudo apt install libxcb-xinerama0  
sudo apt install libxcb-xinput0

 ```

### Issue #7 - Unable to locate libffi7 error while installing time doctor script

- if you have new ubuntu releases above 22.04 (jammy), the installation may fail with this error `Unable to locate libffi7` .
    
- the issue happens because new ubuntu releases have only the new `libffi8` version, and time doctor depends on `libffi7` version.
    
- a way to fix this issue is by installing `libffi7` manually on linux by following these steps:
    

1. firstly you will download the source package files from [here](https://launchpad.net/ubuntu/+source/libffi/3.3-4) and move them to new directory called `libffi7` or any name you prefer.
    

``` bash
mkdir -p libffi7
mv libffi_3.3-4.dsc libffi_3.3.orig.tar.gz libffi_3.3-4.debian.tar.xz libffi7/

 ```

2\. change your directory to Downloads `cd ~/Downloads`

3\. install these prerequisite packages as follows.

``` bash
sudo apt-get install dpkg-dev
sudo apt-get install build-essential
sudo apt install debhelper dh-autoreconf libltdl-dev dejagnu texinfo

 ```

4\. unpack the source package

`dpkg-source -x libffi_3.3-4.dsc`

5\. build the source package

`dpkg-buildpackage -us -uc`

6\. install the built package from the parent directory

`sudo dpkg -i ../libffi7_3.3-4_amd64.deb`

7\. reinstall the time doctor script, the app should run just fine.

PipeDroid
=========

Create NANDroid backups directly to the computer and restore them on the fly.

This utility is designed to work on GNU/Linux. It *should* work on Mac OS X (I'll test it at some point and eventually put a guide) and on Windows (using Cygwin or similar), however I haven't tested it. I definitely am not going to port it to Windows/batch, however pull requests are well accepted if you want to do that.


**Note:** tar backups have not been implemented yet.

Dependencies
------------

* **EasyBashGUI** and at least one of it's supported dialogs (for PipeDroid's GUI), get it from https://github.com/BashGui/easybashgui
* **xterm**, also needed for the GUI
* **pv**, needed to display backup progress
* **gzip**, used for compression
* **tar**, used to create tarball backups
* **adb**, used to transfer the data

Also:

* The Android device needs to be **rooted**!
* **BusyBox** needs to be installed on the Android device!
* **ADB** needs to be enabled!

Installation on Ubuntu/Debian-based distros
--------------
Run the following commands line by line in a terminal window:

    sudo apt-get install git pv android-tools-adb make bash
    cd
    git clone https://github.com/BashGui/easybashgui
    cd easybashgui
    sudo make install  ## Follow directions!
    cd
    git clone https://github.com/Davideddu/pipedroid
    cd pipedroid
    
Done. Run "./pipedroid" without quotes to start the graphical user interface.

Usage (graphical interface)
---------------------------

Simply run *pipedroid* and follow the directions. Make sure you have only one device connected. All network ADB devices will be disconnected when starting the program. The program will tell you when to connect them again if you don't have a USB cord.

If you don't know what to choose as compression method, read the dedicated section below.

If you read it and still don't know what to choose, here's a simplified version of the section below:

* ***Compress remotely, decompress and recompress locally*** - **Recommended** - Lower disk space usage, higher CPU usage, faster transfer.
* ***Compress remotely, decompress locally*** - Higher disk space, standard CPU usage, faster transfter.
* ***Compress remotely, keep compressed*** - Lower disk usage, low CPU usage, faster transfer, can't estimate ETA or transfter progress.
* ***No compression at all*** - No remote compression (use this if your device has problems with gzip), higher disk usage, slow transfer, low CPU usage on both PC and Android. device

If this doesn't help, choose the third option!

You need to add an extension to the files. For raw images use .img, for tarballs use .tar (doh!).


Usage (command line)
--------------------

Run either *pipedbackup_img* or *pipedbackup_tar* (work in progress) to create a backup manually.

*pipedbackup_img* and *pipedbackup_tar* accept the following arguments:

    [remote source] [local destination] [decompress|keep_compressed|no_compression]

* *remote source* is either the partition (for \*_img) or the directory (for \*_tar) that you want to backup.
* *local destination* is the output file (you need to check the extensions because PipeDroid won't)
* The last argument is a compression method, described in the next section. If not specified, *recompress* will be assumed.

### Compression methods
* ***decompress*** *(Compress remotely, decompress locally)*: the backup is compressed (using gzip) remotely, then decompressed back locally. This allows much faster transfers (usually around 20 MB/s vs 5 MB/s using no compression), and it is possible to check the progress of the operation. Note that the transfer speed is calculated ***after*** decompressing: this means that it will be much higher than the actual (compressed) transfer rate, and that it will seem unstable (it actually isn't).
* ***keep_compressed*** *(Compress remotely, keep compressed)*: the backup is compressed remotely but not decompressed. gz will be automatically added at the end of the local destination to avoid troubles with gunzip. As the compressed size cannot be calculated in advance, it is impossible to monitor the progress of the transfer or provide an ETA.
* ***recompress*** *(Compress remotely, decompress and recompress locally)*: it's a mix of *decompress* and *keep_compressed*. The backup is compressed remotely, decompressed locally, the progress and the decompressed transfer speed are calculated and the backup gets compressed again. With this method you'll be able to monitor the progress, estimate the remaining time and the backup will take a less disk space. However it will use more CPU and the transfer speed will not be the actual transfer speed. This is the recommended method.
* ***no_compression*** *(No compression at all )*: the backup will not be compressed at any time. You will be able to monitor the progress, but the transfer will be slower.

Reporting bugs
----------------

####Known bugs:

* Incorrect values when double-clicking on menu items when Zenity is used as back-end (EasyBashGUI bug)

**Make sure you're running the latest version of PipeDroid and EasyBashGUI before reporting bugs!**

If the script doesn't behave as expected, run it again. If it still doesn't work after running it twice, report a bug. Bugs about backup/restore not working on a particular device will be taken care of only if the log is provided (it's located at ~/.pipedroid/pipedroid.log) and device information is provided. This is how a device-specific bug report should look like:

    Title: Can't backup raw images from CyanogenMod 11 20140828, Franco kernel v.###
    -----------------------------------------------
    Device make and model: Samsung Galaxy Express (AT&T)
    Device name: expressatt
    ROM: CyanogenMod 11 20140828, Franco kernel v.###
        or
    Recovery: TWRP 5.0   # when running the script in recovery
    Log: http://paste.ubuntu.com/#######      # post the log in a pastebin and provide the link
    Running on Ubuntu 14.04 LTS 64 bit

Bug reports about not supported platforms (Windows, Mac OS X) are welcome, however GNU/Linux bugs have a higher priority. If you can, test your feature on a GNU/Linux virtual machine so that you can tell me if the bug is platform-specific or a global bug.

Pull requests, feature suggestions and contributions of any kind are well accepted!

Updated Thursday, 28. August 2014 06:54PM (UTC+1, Europe/Rome)
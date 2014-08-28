PipeDroid
=========

Create NANDroid backups directly to the computer, on the fly.

**Note:** restore and tar backups have not been implemented yet.

Dependencies
------------

* **EasyBashGUI** and at least one of it's supported dialogs (for PipeDroid's GUI), get it from https://github.com/BashGui/easybashgui
* **xterm**, also needed for the GUI
* **pv**, needed to display backup progress
* **gzip**, used for compression
* **tar**, used to create tarball backups
* **adb**, used to transfer the data

Also:

* The Android device needs to be rooted
* **BusyBox** needs to be installed on the Android device
* **ADB** needs to be enabled

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

Updated Thursday, 28. August 2014 06:54PM (UTC+1, Europe/Rome)
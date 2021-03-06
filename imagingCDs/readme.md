## There are many ways to create disk images.

#### Short version here (longer, more accurate version below):  

One of the simplest methods is to run the program ``dd`` from the command line from a computer running a Linux operating system.  Check out documentation for ``dd`` [here](http://www.gnu.org/software/coreutils/manual/html_node/dd-invocation.html).

* *Note :  This method will need little modification to run on a Mac.  If you are using a Windows operating system, [Cygwin](https://cygwin.com/index.html) provides a "Unix-like" environment and a large collection of open-source command line tools useful for this process.* 

##### 1. open a terminal window.

##### 2. find the path to the cd drive. 

```
mount|grep ^'/dev'
```

one of the lines of output will look like something like this:

``` 
/dev/sr# on /media/username/VERY_LIKELY_A_CD-ROM_NAME type iso9660
```

The specifics will vary, but you get the idea.  The important part is that you'll look at the list of output, and you'll recognize that one of the /dev s is your optical drive.  

The value for "sr#" in that line of output is your optical media drive.  Plug that value into the ``dd`` command in step 2.  

##### 3. unmount the CD

```
umount /dev/sr#
```

##### 4. run ``dd`` to generate an .iso file of the CD-ROM

```
dd if=/dev/sr# of= pathto/filenameforCD-ROMimage.iso
```

you will see an output like this:

```
998000+0 records in
998000+0 records out
633044992 bytes (633 MB) copied, 255,874 s, 2,5 MB/s
```



and your .iso file is ready to go wherever you saved it.  

----

#### Longer, more accurate version:  

dd is not a perfect solution.  There are problems, for example you'll run into difficulty if the media is damaged, and dd does not do error-checking, and the above recommended method has not specified block size. There is much to debate about the merits of various approaches to producing disk images.

In the meantime, here are some particularly valuable resources for simple and practical tools to try, as well as in-depth explanations and concise-but-detailed descriptions of a range of approaches.

##### 1. The [Guymager](http://guymager.sourceforge.net/) program is an [open-source forensic imaging program](http://www.forensicswiki.org/wiki/Guymager), focused on ease-of-use and speed.  Guymager is included as part of the Bitcurator digital forensics suite.  

* Bitcurator provides this excellent and clear tutorial for creating disk images via Guymager: 
["Creating a disk image using Guymager"](http://wiki.bitcurator.net/index.php?title=Creating_a_Disk_Image_Using_Guymager) 

* The [Bitcurator wiki](http://wiki.bitcurator.net/index.php?title=Main_Page) and [project](http://www.bitcurator.net/) is an excellent resource to check out for many data curation questions and tools, and they are very responsive and helpful with questions.     

##### 2. Here is an excellent and very through discussion of creating images for optical media, provided by the Koninklijke Bibliotheek, the National Library of the Netherlands: 
* ["Preserving Optical Media from the Command Line"](http://blog.kbresearch.nl/2015/11/13/preserving-optical-media-from-the-command-line/)

* (Thank you Koninklijke Bibliotheek for sharing your research, compiling this excellent resource, and sharing it with the digital preservation community, and thank you to NDS Resident Dinah Handel [@DinahHandel](https://twitter.com/DinahHandel)for alerting me to this page!)

##### 3. One of the sources cited by the KB optical media discussion is this analytic report about producing disc images and addressing errors, produced by George Blood Audio Video Film for the Library of Congress in April 2014:  

* [Report on Producing Disk Images](http://www.digitizationguidelines.gov/audio-visual/documents/Preserve_DVDs_BloodReport_20140901.pdf?loclr=blogsig) 


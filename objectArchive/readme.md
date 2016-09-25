# Introduction
The EaaS object archive (or technically the EaaS object archive facade) is one key 
component to support seamless integration of institutional object-repositories with 
EaaS and to make EaaS a cost-effective and scalable access solution for born-digital 
content.   

By design, EaaS separates an emulated computer system (emulation environment), 
i.e. an emulator configuration in combination with a bootable disk image containing 
an installed and configured operating system with software applications etc, and user 
objects, such as CD-ROMs or individual files to be rendered. 
Objects and emulation environment are loaded and "bound" to an emulation environment only 
if needed, e.g. if a user requests a certain object to be rendered using a suitable 
emulation environment. 

The main rationale for this design were costs and preservation planning complexity:
as the number of objects are in thousands or millions, the number of necessary 
emulation environments is rather small, typically about 10-20 "base" environments 
representing typical computer systems of technical epochs. By separating objects from 
their rendering environments, emulation-based preservation planning strategies can 
focus on a small set of emulated "computer systems" (emulation environments) to be 
kept alive.      

# Example - Setting up a Poor Man's File-based Object Archive

create a folder `objects/`
The folder should contain the following directory/files structure:

```
objects/ 
 |- object1id/ 
 |   \ iso/ 
 |      |- disk1.iso 
 |      |- disk2.iso 
 |- object2id/ 
 |   \ floppy/ 
 |      |- disk1.img
```


# Interfaces
The object archive interface defines currently three methods: 

```
public List<String> getObjectList(); 
```
This method returns a list of available object IDs. The implementation of this method is optional. Object lists 
are currently only used for UI purposes.   

```	
public FileCollection getObjectReference(String objectId);
```
This Method returns a `FileCollection` object for a given object ID. See below. 	

```	
String getName(); 
```
This method returns an identification string for the current object archive instance. 

# Data Structures

```XML
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="FileCollection">
    <xs:complexType>
      <xs:sequence>
        <!-- object id -->
        <xs:element type="xs:string" name="id"/>
        
        <!-- For each media of a digital object is represented by a FileCollectionEntry -->
        <xs:element name="FileCollectionEntry">
          <xs:complexType>
            <xs:sequence>
              <!-- media type. allowed values: iso, cdrom, floppy -->
              <xs:element type="xs:string" name="type"/>
              
              <!-- accessible reference to the image file --> 
              <xs:element type="xs:string" name="url"/>
            
              <!-- in some cases the filename of the image file matters. E.g. if we cannot guess the 
              filename from the URL or an emulator requires a certain suffix etc. -->
              <!-- optional -->
              <xs:element type="xs:string" name="localAlias"/>
              
              <!-- optional: provide file size information. in cases when the file size is not block aligned 
              (i.e. not a multiple of 512) the file will be padded to be used with block-layer tools. If the 
              file size is set, the file will be trimmed to that size if accessed by the emulator. -->
              <xs:element type="xs:string" name="filesize"/>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
</xs:schema>
```
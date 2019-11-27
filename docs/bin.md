#Command line tool

The tool is available to download from <https://github.com/jandvorak-uwb/pmc/releases/download/1.0/pmc.zip>

##System requirements

The application was tested on 64-bit versions of Windows 10 (version 1909) and Debian 10. Other Linux distributions may also work, however the installation proces of the dependencies may differ.

###.NET Core 2.1 / Mono

####Windows 10
[Download here](https://dotnet.microsoft.com/download/dotnet-core/thank-you/runtime-2.1.14-windows-x64-installer)

####Debian 10
[Installation instructions](https://www.mono-project.com/download/stable/#download-lin-debian)

###NetCDF (Optional)

####Windows
<http://www.unidata.ucar.edu/downloads/netcdf/ftp/netCDF4.3.3.1-NC4-64.exe>
When you install this library, select the option to add its location to your system PATH.

####Debian 10

!!! caution
    The NetCDF format is currently not supported on the Linux platform due to unresolved bug causing segmentation fault.

Run as superuser:
```
apt-get install libnetcdf-dev
ln -s /usr/lib/x86_64-linux-gnu/libnetcdf.so /usr/lib/libnetcdf.so.7
```

##Usage

###Encoding
```
pmc encode --src input_file --bonds input_bonds_file --dst output_file --qr 0.01 --qa 0.1 --qc 0.001
```

* ```--src input_file``` Input trajectory file in supported format
* ```--bonds input_bonds_file``` Input bonds file in supported format
* ```--dst output_file``` Path to newly created compressed file
* ```--qr <value>``` Quantization of residues - controls the distortion of data (Optional, default value: 0.01)
* ```--qa <value>``` Quantization of angles - controls only the data rate (Optional, default value: 0.1)
* ```--qc <value>``` Quantization of canonical molecule - controls only the data rate (Optional, default value: 0.001)
* ```-v``` Enable verbose mode

###Decoding
```
pmc decode --src input_file --bonds input_bonds_file --dst output_file
```

* ```--src input_file``` Input compressed trajectory file
* ```--bonds input_bonds_file``` Input bonds file in supported format
* ```--dst output_file``` Path to newly created file in supported format (determined by file extension)
* ```-v``` Enable verbose mode

##Supported bond formats

###Text files (.txt)

Each line of the file contains list of integers separated by blank space and represents all the bonds incident with corresponding atom. For example, content of the bond file for single molecule of water could be as follows:

```
1 2
0
0
```

###Gromacs format (.gro)

Currently, this format can be used only for protein structures.

##Supported trajectory data formats

###Text files (.txt)
Each line of the file contains list of single precision floating point numbers separated by blank space and represents positions of corresponding atom in each frame. For example, trajectory consisting of two atoms (A and B) in three frames should be as follows:

```
xA1 yA1 zA1 xA2 yA2 zA2 xA3 yA3 zA3
xB1 yB1 zB1 xB2 yB2 zB2 xB3 yB3 zB3
```


###Binary files

Such file consists of single 32-bit signed integer value containing number of coordinates per atom in file, i.e. __3 * number of frames__, folowed by __3 * number of atoms * number of frames__ 32-bit floating point values. For the above example text file, the data would look followingly:
```
<9><xA1><yA1><zA1><xA2><yA2><zA2><xA3><yA3><zA3><xB1><yB1><zB1><xB2><yB2><zB2><xB3><yB3><zB3>
```

###Amber NetCDF format (.nc)

!!! caution
    The NetCDF format is currently not supported on the Linux platform due to unresolved bug causing segmentation fault.
 

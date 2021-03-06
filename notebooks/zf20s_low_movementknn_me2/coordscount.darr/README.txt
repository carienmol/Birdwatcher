This directory contains a numeric array. The array can be read in Python using the Darr library
(https://pypi.org/project/darr/), but if that is not available it should be straightforward to
read the data in other environments using the information below.

Description of data format
==========================

The file 'arrayvalues.bin' contains a numeric array in the following format:

  Numeric type: 64‐bit signed integer (-9223372036854775808 to 9223372036854775807)
  Byte order: little (most-significant byte last)
  Array length: 497
  Array order layout:  C (Row-major; last dimension varies most rapidly with memory address)

The file only contains the raw binary values, without header information.

Format details are also stored in json format in the separate UTF-8 text file,
'arraydescription.json' to facilitate automatic reading by a program.

If present, the file 'metadata.json' contains metadata in json UTF-8 text format.

Example code for reading the data
=================================

Python with Numpy:
------------------
import numpy as np
a = np.fromfile('arrayvalues.bin', dtype='<i8')

Python with Numpy (memmap):
---------------------------
import numpy as np
a = np.memmap('arrayvalues.bin', dtype='<i8', shape=(497,), order='C')

R:
--
fileid = file("arrayvalues.bin", "rb")
a = readBin(con=fileid, what=integer(), n=497, size=8, signed=TRUE, endian="little")
close(fileid)

Matlab/Octave:
--------------
fileid = fopen('arrayvalues.bin');
a = fread(fileid, 497, '*int64', 'ieee-le');
fclose(fileid);

Julia (version < 1.0):
----------------------
fileid = open("arrayvalues.bin","r");
a = map(ltoh, read(fileid, Int64, (497,)));
close(fileid);

Julia (version >= 1.0):
-----------------------
fileid = open("arrayvalues.bin","r");
a = map(ltoh, read!(fileid, Array{Int64}(undef, 497)));
close(fileid);

IDL/GDL:
--------
a = read_binary("arrayvalues.bin", data_type=14, data_dims=[497], endian="little")

Mathematica:
------------
a = BinaryReadList["arrayvalues.bin", "Integer64", ByteOrdering -> -1];
a = ArrayReshape[a, {497}];

Maple:
------
a := FileTools[Binary][Read]("arrayvalues.bin", integer[8], byteorder=little, output=Array);


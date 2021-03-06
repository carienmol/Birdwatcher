Disk-based storage of a ragged array
====================================

This directory is a data store for a numeric ragged array. This is a sequence of subarrays that
all have the same shape except for one dimension. On disk, these subarrays are concatenated along
their variable dimension. The data can be read in Python using the Darr library, but if that is
not available, they can also be read in other environments with a little more effort.

There are two subdirectories, each containing an array stored in a self-explanatory format. See
the READMEs in the corresponding directories to find out in detail out how. However, example code
is provided below for a number of analysis environments, which in many cases is suffcient.

The subdirectory "values" holds the numerical data itself, where subarrays are simply appended
along their variable length dimension (first axis). So the number of dimensions of the values
array is one less than that of the ragged array. A particular subarray can be be retrieved using
the appropriate start and end index along the first axis of the values array. These indices
(counting from 0) are stored in a different 2-dimensional array in the subdirectory "indices". The
first axis of the index array represents the sequence number of the subarray and the second axis
(length 2) represents start and (non-inclusive) end indices to be used on the values array. To
read the n-th subarray, read the nt-h start and end indices from the indices array and use these
to read the array data from the values array.

This ragged array has 497 subarrays.

Example code for reading the data
=================================

Python with Numpy (memmap):
---------------------------
import numpy as np
i = np.memmap('indices/arrayvalues.bin', dtype='<i8', shape=(497, 2), order='C')
v = np.memmap('values/arrayvalues.bin', dtype='<u2', shape=(418397, 2), order='C')
def get_subarray(seqno):
    starti, endi = i[seqno]
    return v[starti:endi]
a = get_subarray(2)  # example to read third subarray

R:
--
fileid = file("indices/arrayvalues.bin", "rb")
i = readBin(con=fileid, what=integer(), n=994, size=8, signed=TRUE, endian="little")
i = array(data=i, dim=c(2, 497), dimnames=NULL)
close(fileid)
fileid = file("values/arrayvalues.bin", "rb")
v = readBin(con=fileid, what=integer(), n=836794, size=2, signed=FALSE, endian="little")
v = array(data=v, dim=c(2, 418397), dimnames=NULL)
close(fileid)
get_subarray <- function(seqno){
    starti = i[seqno,1] + 1  # R starts counting from 1
    endi = i[seqno,2]  # R has inclusive end index
    return (v[starti:endi])
}
a = get_subarray(3)  # example to read third subarray


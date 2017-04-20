# libgumath
C-library for registering kernels specialized to types defined by libndtypes and extracting the specialized kernel with a "query" or "resolution" function.

These kernels are limited to kernels whose resultant ndtype (low-level datashape) can be predicted based on input ndtypes (low-level datashapes) and does not depend on values.

# Overview
NumPy's ufunc object provides a high-level Python object for registering low-level kernels that do math.  Numba provides a mechanism to produce these low-level kernels usng a subset of Python syntax.   However, because NumPy's ufunc object is not built on a C-library, several aspects of the NumPy ufunc have to be re-implemented in Numba.  

The purpose of this library is to be a C-library that implements as much of the (generalized) ufunc capability as it makes sense to do in a way that can be re-used by any high-level language (Python, R, Scala, Node, etc.).

It is desired that corresponding libraries for each language that use this C-library to implement the equivalent of NumPy's umath module in each language will be built.  

#  need a simple mechanism which allows registration of kernels with the library (either single element or N-elements --- where an element can be any datashape spec).

We also need a "query" or "resolution" capability so that a particular kernel can be recovered based on argument ndtypes.   This is related to multiple-dispatch discussions you can read about, but I'm not so interested in that unless we can relate the data-shape system to those mechanisms.   I think the concept of "query" might be better for this purpose.   

Perhaps interfaces could be used to extend beyond the simple registration-retrieval system and allow the return of compatible kernels for non-exact matches of ndtype.  This is why I referenced the book above.   Feel free to buy it and charge it to Continuum if you would like it. 

We need something that uses ndtypes to specify the types of the elements and 

  1) respects its extensibility
  2) recognizes the availability of JIT compilers to build the spec that is needed on-the-fly and thus can reduce the need for resolution to another kernel when the spec you are calling with is not initially available.   

Also, we need a system that is composable.   In other words, one should be able to relatively easily build a "composite" kernel that implements a directed graph of kernels.   All we need to do is implement single composability (i.e. we don't need to build any graph code).  


# libgumath
C-library for registering kernels specialized to types defined by libndtypes and extracting the specialized kernel with a "query" or "resolution" function.

These kernels are limited to kernels whose resultant ndtype (low-level datashape) can be predicted based on input ndtypes (low-level datashapes) and does not depend on values.

# Overview
NumPy's ufunc object provides a high-level Python object for registering low-level kernels.  Numba provides a mechanism to produce these low-level kernels usng a subset of Python syntax.   However, because NumPy's ufunc object is not built on a C-library, several aspects of the NumPy ufunc have to be re-implemented in Numba.    

The purpose of this library is to be a C-library that implements as much of the (generalized) ufunc capability as it makes sense to do in a way that can be re-used by any high-level language (Python, R, Scala, Node, etc.).

It is desired that corresponding libraries for each language that use this C-library to implement the equivalent of NumPy's umath module in each language will be built.  

# Requirements

1) Need a simple mechanism which allows registration of kernels with the library (either single element kernels or N-element kernels).  An element can be any spec from libndtype.

2) Need a "query" or "resolution" capability so that a particular kernel can be recovered based on argument ndtypes.   This is related to multiple-dispatch, but the concept of specialized-routine "resolution" might be better suited.   Whatever mechanism makes sense should be used.

3) Elements must use libndtypes to specify the types of the elements and respect the exstensibility of libndtypes and recognize the availability of JIT compilers to build function pointers that are needed on the fly --- perhaps after calling a function from this library or from the libndtypes library to determine the ndtype of the output(s). 

4) The ability to compose kernels.  In other words if we have a functions f(a,b), g(c,d), and h(e,f) we should be able to build kernels representing any of the functions f(g({1},{2}),h({3},{4})) where the arguments are any of the 16 choices (with repeats) of a,b,c,d.  We could also leave this general kind of composing to the higher-level JIT.   TBD  

5) A specific modification of number 4 are reduction functions (reduceat, reduceby, reduce, etc.) built from appropriate kernels (i.e. 2-argument kernels). 

6) Familiarity with the ufuncobject.c code from NumPy is essential to understand this library (and in particular how generalized ufuncs work at their core). 

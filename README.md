# NIF file C++ code generator

Generates C++ structs and classes to represent data found in .nif files used in Fallout New Vegas. Files used in other NetImmerse/Gamebryo games have not been considered yet.

Definitions of types are taken from a nif.xml file, which has been slightly edited for compatibility. The original repository can be found [here](https://github.com/niftools/nifxml).

This generator was made as part of a currently private project, and is a **work in progress**. The generated code is not ready for compilation.


## How to run

Run generator.py.

## Output

The program will output a set of .hpp and .cpp files for each .nif file version which is shown to appear in Fallout New Vegas, as specified in the nif.xml.

### Files

* typedefs.hpp -- Contains typedefs necessary in all other files. Currently only used to enforce the size of boolean values to be 1 byte.
* enums.hpp -- Contains definitions of enumerations. Enumerations are placed into a NiEnums namespace.
* bitfields.hpp --- Contains definitions of bitfield types. Bitfield types are represented as struct with overloaded bitwise and (&) and assignment operators.
* structs.hpp and structs.cpp -- Contain definitions of compound types.
* NiMain etc. -- A single folder contains definitions of objects found in a single module, as defined in the nif.xml.
  * There is a pair of .hpp and .cpp files for every object type in the module.
* factory.hpp factory.cpp -- A global constant map of type names to factory functions for every object type.

Every object has:
* a static factory function which calls the constructor
* a constructor which reads the object data
* a destructor which frees the memory allocated in the constructor
* a function for resolving pointers from indices to actual addresses

Objects retain the class hierarchy given in the nif.xml

Every compound has:
* a constructor
* a destructor
* a function for resolving pointers from indices to actual addresses

#### Considerations

The generated files require the existence of:

* a NifReader type, which encapsulates file access from the concrete types
* a NifData type, which contains information about all object read from a single file, and can be used to resolve pointers in object from indices to actual memory addresses
* NifPointer and NifPointerList types, which transparently enable the indices and actual addresses to be placed in the same memory location

These types are not generated by the generator.

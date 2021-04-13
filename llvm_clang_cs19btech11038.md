# LLVM/CLANG
The LLVM compiler infrastructure project is a set of compiler and toolchain technologies,which can be used to develop a front end for any programming language and a back end for any instruction set architecture.
The Clang project provides a language front-end and tooling infrastructure for languages in the C language family
##  C++11  AND C ++14 FEAUTRES USED 
1) decltype(auto)

2) member intializers

3) aggreates

4) Variable templates

## c++11

clang uses all standard c++11 features which include initialization of class objects by rvalues, Atomic operations, and others
1) initialization of class objects by rvalues
2) variadic templates- template with **parameter packs** list

## CLASS HEIRACY:
There is lot of heiracy i have added some of it for more reference visit llvm source code
					
```
* Type class
* use class
* stringref
* metadata
*patterns for opcode
* Module class
*bf_iterator_storage
*detail
	*DenseMapPair
	*Analysis pass concept
	* BCField
* Value class
	* user class
		*instruction class	
		*constant class
			* globalvalue  class
				* the function class
				* the global variable class
	* basicblock class
	* argument class
* remarks
	*argument class
	*bitstreamremarkParserHelper
		*argument
*object

```


  

  

##  OOPS Design Decisions

  

### Polymorphism:

* 2 functions in a class can have same name but different parameters

	#### overriding:

	* Make these protected so only (final) subclasses can be copied around

### inheriantance

* one class can inherite other class

* mutliple classes can inherite one class

###  encapsulation

* datahiding using private,public,protected

* classes can have static method private,public ,final

* objects cannot access private variables

* only friend classes can access protected part

### classes and objects

* classes are the methods used in oops

### Abstraction
Abstraction means displaying only essential information and hiding the details. Data abstraction refers to providing only essential information about the data to the outside world, hiding the background details or implementation.
  
### Message passing
 Objects communicate with one another by sending and receiving information to each other
  

  

  

## DESIGN PATTERN:

there are lot of design patterns

### builder design pattern:

* cflgraphbuilder

* code gen pass builder etc....

### adaptor design pattern

* A range adaptor for a pair of iterators

### proxy design pattern

This proxy class models a common pattern where we delegate to either the top-level AAResults aggregation if one is registered, or to the current result if none are registered

### visitor design pattern

* CVSymbolvisitor

* DebugSubsectionVisitor

### iterator design pattern

* llvm::BitMaskClassIterator

### factory design pattern:

* imutAVLfactory

### observer design pattern
* GISelChangeObserver
  

  
## USAGE OF ITERATORS:

* llvm::BitMaskClassIterator

* DenseMapIterator

- directory_iterator: to yield member of a directory

  

  

  

## OWN DATASTRUCTURES USAGE:

Set-like Containers

* SmallSet:If you have a set-like data structure that is usually small and whose elements are reasonably small
* denseset
* FoldingSet

Map -like Containers

* DenseMap

Bit storage containers

* BitVector
* SparseBitVector

Sequential Containers

* std::vector
* std::list
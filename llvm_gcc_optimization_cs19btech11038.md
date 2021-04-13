# GCC
GCC is compiler infrastrure project used for several languages
# LLVM/CLANG
The LLVM compiler infrastructure project is a set of compiler and toolchain technologies,which can be used to develop a front end for any programming language and a back end for any instruction set architecture.
The Clang project provides a language front-end and tooling infrastructure for languages in the C language family

## Comparisions between gcc and llvm 

### Based on generating objective files:
#### gcc 
1)  read source code
2) preprocess the source file
3) Intermeriate representation
4) optimizing
5) generating assembly file
6) assembly file generates objective file
#### llvm
the compilation is based in integrate self implemented compilers at the backend
here it similar to gcc but the difference arise is 
* it omits the process of generating object files these are generated from IR

#### Intermediate representation:
* llvm IR’s data structures manare so consie such that they occupy less memory during compilation and faster traversal.
* gcc IR is not so consise compared with llvm .
#### Difference in languages support of gcc and llvm(front ends)
* past traditional languages like Ada, Fortran, and Go(GNAT) which are supported by gcc and c(gcc),c++(g++),objective c
* these languages front ends are GNAT,GCC,G++ FOR GCC
* new emerging langues uses llvm like Swift, Rust, Julia, and Ruby(rubinius). Llvm also support c,c++(clang),ada ,fortan(flang) and go(llgo).
* Languages front ends are rubinius,clang,flang,llgo.
#### Differences based on architecture(backends)
* gcc supports more less popular  architecutes
* gcc supports Alpha,ARM,AVR,Blackfin,Epiphany (GCC 4.8) etc.

* llvm supports ARM, Qualcomm Hexagon, MIPS, Nvidia etc.

#### Differnces based on language extenction
* gcc supports more language extenctions com assembly feature like Template instalation ,bounded member functions,multiversional functions etc...

* llvm only supports some of them like Extened integer types, selectively disabling optimization,loop hit optimization,attributes for thread security check(using locks )
#### Difference based on  error messages:
* llvm is more sofisticated based on error messages
much more features on diagonizing

## OPTIONS SUPPORTED
### VARIOUS OPTIONS IN GCC COMPILER:
#### Basic options are:
-c
*	Compile or assmemble code but not link .
	Output is object file without linking f.o

-S
*	stoping after the stage of compilation not doing assemble.the code is assemble code. f.s

-E
*	Stoping after preprocessing stage

-o file
*	output file to be placed in given file
 

### optimization options:



#### CODE SIZE OPTIMIZATIONS:
    -Os
    -fno-unroll-loops
* For unrolling loops so that compile time would decrease

```
-finline-limit=n(gcc)
```
* finline makes the function just copied into the other so that function calling  will not happen .so execution time decreases

```
-fno-jump-tables
```
#### Performance and remaining  Optimization
    -fselective-scheduling2 -fsel-sched-pipelining
* pipeling

```
-fthread-jumps
```  
* thread jumping

-ftree-loop-linear  -ftree-loop-optimize -ftree-loop-vectorize
**Program Instrumentation options,Assembler Options like -Wa,Linker Options,Directry Options:,Directory Options,Developer Options**

#### Warning options like Wmising braces- warn if braces are missed
    -Wunused
    -Wuninitialized
    -Walloc-larger-than=n
    -warray-bounds=n

### OPTIMIZATION:
-O<level> flags turns on some of the optimization flags
 
-O0
*	no optimization

-O1
*	the compiler tries to reduce code size and execution time, without performing any optimizations that take a great deal of compilation time

	
-O2
*	if no the space and speed tradoff are present  these optimization could be used this increses both compilation time and the performance of the generated code
	  -falign-functions  -falign-jumps -falign-loops  -falign-labels
etc

-O3
*	here it has all optimization flagsof -o2 and it adds
 -finline-functions, -funswitch-loops
etc

-Os
* Optimization of size .
Adds all -O2 flags expect tho dont increase size




# ***llvm options***:
### Basic options like:
	-E
		whsh runs the prepocessor stage
	-S
		runs preproccing and llvm generation and optimizations as well
	-c 
		all stemps of compilation plus assembling and generating .o file object file
### CODE SIZE OPTIMIZATIONS:
    -Os
    -Oz(llvm)
    -mllvm -inline-threshold=n(llvm)
#### Language selection options like
	-std=<language>
	-std=<library>
#### Target Selection Options
	-arch <architecture>
	-march=<cpu>
#### Code Generation Options:
	-O0
		-no optimizations
	-O1
	-O2
		-moderate optimization
	-O3
	-Os
		extra optimization on reducing code size


##  compilers to generate code for various architectures using its various backends
LETS TAKE A PROGRAM
``
int fact(int n,int a)
{
	if(n==1)
		return a;
	else
		return fact(n-1,n*a) ;
}
``

## ARM
```
	.arch armv5t
	.eabi_attribute 20, 1
	.eabi_attribute 21, 1
	.eabi_attribute 23, 3
	.eabi_attribute 24, 1
	.eabi_attribute 25, 1
	.eabi_attribute 26, 2
	.eabi_attribute 30, 6
	.eabi_attribute 34, 0
	.eabi_attribute 18, 4
	.file	"fac.c"
	.text
	.align	2
	.global	fact
	.syntax unified
	.arm
	.fpu softvfp
	.type	fact, %function
fact:
	@ args = 0, pretend = 0, frame = 8
	@ frame_needed = 1, uses_anonymous_args = 0
	push	{fp, lr}
	add	fp, sp, #4
	sub	sp, sp, #8
	str	r0, [fp, #-8]
	str	r1, [fp, #-12]
	ldr	r3, [fp, #-8]
	cmp	r3, #1
	bne	.L2
	ldr	r3, [fp, #-12]
	b	.L3
.L2:
	ldr	r3, [fp, #-8]
	sub	r0, r3, #1
	ldr	r3, [fp, #-8]
	ldr	r2, [fp, #-12]
	mul	r1, r2, r3
	bl	fact
	mov	r3, r0
.L3:
	mov	r0, r3
	sub	sp, fp, #4
	@ sp needed
	pop	{fp, pc}
	.size	fact, .-fact
	.ident	"GCC: (Ubuntu/Linaro 7.5.0-3ubuntu1~18.04) 7.5.0"
	.section	.note.GNU-stack,"",%progbits

```
# SIZE
```
  #  text	   data	    bss	    dec	    hex	filename
 # 415324	   4956	   2784	 423064	  67498	./helloworld-arm
```
## AARCH 64
```
    .arch armv8-a
	.file	"fac.c"
	.text
	.align	2
	.global	fact
	.type	fact, %function
fact:
	stp	x29, x30, [sp, -32]!
	add	x29, sp, 0
	str	w0, [x29, 28]
	str	w1, [x29, 24]
	ldr	w0, [x29, 28]
	cmp	w0, 1
	bne	.L2
	ldr	w0, [x29, 24]
	b	.L3
.L2:
	ldr	w0, [x29, 28]
	sub	w2, w0, #1
	ldr	w1, [x29, 28]
	ldr	w0, [x29, 24]
	mul	w0, w1, w0
	mov	w1, w0
	mov	w0, w2
	bl	fact
.L3:
	ldp	x29, x30, [sp], 32
	ret
	.size	fact, .-fact
	.ident	"GCC: (Ubuntu/Linaro 7.5.0-3ubuntu1~18.04) 7.5.0"
	.section	.note.GNU-stack,"",@progbits

```
# SIZE
```
 text	   data	    bss	    dec	    hex	filename
 1519	    544	      8	   2071	    817	./aarch64
```
### X86_64
```
    .file	"fac.c"
	.text
	.globl	fact
	.type	fact, @function
fact:
.LFB0:
	.cfi_startproc
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	subq	$16, %rsp
	movl	%edi, -4(%rbp)
	movl	%esi, -8(%rbp)
	cmpl	$1, -4(%rbp)
	jne	.L2
	movl	-8(%rbp), %eax
	jmp	.L3
.L2:
	movl	-4(%rbp), %eax
	imull	-8(%rbp), %eax
	movl	-4(%rbp), %edx
	subl	$1, %edx
	movl	%eax, %esi
	movl	%edx, %edi
	call	fact
.L3:
	leave
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE0:
	.size	fact, .-fact
	.ident	"GCC: (Ubuntu 7.5.0-3ubuntu1~18.04) 7.5.0"
	.section	.note.GNU-stack,"",@progbits
```
# SIZE
```
 text	   data	    bss	    dec	    hex	filename
  1519	    544	      8	   2071	    817	./x86_64
```
# CLANG:
## X86_64
```
fact(int, int):                              # @fact(int, int)
        push    rbp
        mov     rbp, rsp
        sub     rsp, 16
        mov     dword ptr [rbp - 8], edi
        mov     dword ptr [rbp - 12], esi
        cmp     dword ptr [rbp - 8], 1
        jne     .LBB0_2
        mov     eax, dword ptr [rbp - 12]
        mov     dword ptr [rbp - 4], eax
        jmp     .LBB0_3
.LBB0_2:
        mov     eax, dword ptr [rbp - 8]
        sub     eax, 1
        mov     ecx, dword ptr [rbp - 8]
        imul    ecx, dword ptr [rbp - 12]
        mov     edi, eax
        mov     esi, ecx
        call    fact(int, int)
        mov     dword ptr [rbp - 4], eax
.LBB0_3:
        mov     eax, dword ptr [rbp - 4]
        add     rsp, 16
        pop     rbp
        ret
```
## ARM
```
fact(int, int):
        push    {r11, lr}
        mov     r11, sp
        sub     sp, sp, #16
        str     r0, [sp, #8]
        str     r1, [sp, #4]
        ldr     r0, [sp, #8]
        cmp     r0, #1
        bne     .LBB0_2
        b       .LBB0_1
.LBB0_1:
        ldr     r0, [sp, #4]
        str     r0, [r11, #-4]
        b       .LBB0_3
.LBB0_2:
        ldr     r0, [sp, #8]
        sub     r1, r0, #1
        ldr     r2, [sp, #4]
        mul     r3, r0, r2
        mov     r0, r1
        mov     r1, r3
        bl      fact(int, int)
        str     r0, [r11, #-4]
        b       .LBB0_3
.LBB0_3:
        ldr     r0, [r11, #-4]
        mov     sp, r11
        pop     {r11, lr}
        bx      lr
```
# ARM AARCH64
```
fact(int, int):                              // @fact(int, int)
        sub     sp, sp, #32                     // =32
        stp     x29, x30, [sp, #16]             // 16-byte Folded Spill
        add     x29, sp, #16                    // =16
        str     w0, [sp, #8]
        str     w1, [sp, #4]
        ldr     w8, [sp, #8]
        cmp     w8, #1                          // =1
        b.ne    .LBB0_2
        ldr     w8, [sp, #4]
        stur    w8, [x29, #-4]
        b       .LBB0_3
.LBB0_2:
        ldr     w8, [sp, #8]
        subs    w0, w8, #1                      // =1
        ldr     w8, [sp, #8]
        ldr     w9, [sp, #4]
        mul     w1, w8, w9
        bl      fact(int, int)
        stur    w0, [x29, #-4]
.LBB0_3:
        ldur    w0, [x29, #-4]
        ldp     x29, x30, [sp, #16]             // 16-byte Folded Reload
        add     sp, sp, #32                     // =32
        ret
```
Findings in both clang and gcc:
* ARM IS USING armv5t architecture
* AARCH 64 IS USING armv8-a
* AARCH 64 IS  MORE OPTIMIZATION for some function inline and recursions IT TAKES LESS TIME .
*  Arm uses mov,sub were as Aarch 64 uses ldp
* COMPARED WITH ARM  as aarch 64 is 64 bit and arm is 32 bit aaarch 64 is more optimized in space
**AARCH 64 and X86_64 IS CODE IS MORE OPTIMIZED AS COMPARED WITH ARM based on number of bits**  
* On size text,data used
**AARCH64~=X86_64<ARM**
* ARM 64-bit is the densest code than ARM32 -bit code  AND slighly less than GCC
*  binaries for x86-64 tend to be often quite a bit larger
## OPTIMIZATIONS
#### Tail Optimization

```
//fact.c
int fact(int n,int a)
{
	if(n==1)
		return a;
	else
		return fact(n-1,n*a) ;
}

```


``Running ->gcc -O0 -S fact.c``

* fact.s is generated as a recurseive call in assemble file

``Running -> gcc -O3 -S fact.c``
* fact.s it created lot of tables without any recursion involved which 
optimized the recursion

# OPTIMIZATION
LETS TAKE A MORE NUMBER OF FOR LOOPS WHICH DOES NOT REQUIRE much SPACE
``
int main (void)
{
  for(int i=0;i<1000000000;i++)
  {
       if(1==0)
        {
    }
  }
  return 0;
}
``
### GCC:
```
O0:
	real	0m1.527s
	user	0m1.527s
	sys	0m0.000s
01:
	real	0m0.566s
	user	0m0.563s
	sys	0m0.004s
O2:
	real	0m0.003s
	user	0m0.001s
	sys	0m0.002s
O3:
	real	0m0.003s
	user	0m0.001s
	sys	0m0.002s
Os:
	real	0m0.003s
	user	0m0.002s
	sys	0m0.001s	

```
# Finding
``CLEARLY FOR THE O3 AND O2 HAVE GREATLY OPTIMIZED THE FOR LOOP WITH LOT OF ITERATIONS
Os has obtimized sys time and as it have all O2  flags so,it has less optimizations``
### LLVM:
```
O0:
	real	0m1.476s
	user	0m1.476s
	sys	0m0.000s
01:
	real	0m0.002s
	user	0m0.003s
	sys	0m0.000s
O2:
	real	0m0.003s
	user	0m0.001s
	sys	0m0.002s
O3:
	real	0m0.003s
	user	0m0.001s
	sys	0m0.002s
Os and Oz:
	real	0m0.003s
	user	0m0.002s
	sys	0m0.001s	

```
# Finding
``CLEARLY FOR THE O1,O3 AND O2 HAVE GREATLY OPTIMIZED THE FOR LOOP WITH LOT OF ITERATIONS
Os has obtimized sys time and as it have all O2  flags so,it has less optimizations``

## Example 2:High memory usage program
```
#include <stdio.h>

int fact(int n,int a)
{
  if(n==1)
    return a;
  else
    return fact(n-1,n*a) ;
}
int main (void)
{
 
  fact(100000000,1);
  return 0;
}
```
### GCC:
```
O0:	SEGMANT FAULT DUE TO HIGH DATA USAGE
01: SEGMANT FAULT DUE TO HIGH DATA USAGE
O2:
	real	0m0.002s
	user	0m0.003s
	sys	0m0.000s
 text	   data	    bss	    dec	    hex	filename
   1463	    544	      8	   2015	    7df	./k
O3:
	real	0m0.003s
	user	0m0.003s
	sys	0m0.000s
	size:
	 text	   data	    bss	    dec	    hex	filename
   1795	    544	      8	   2347	    92b	./k
Os:
	real	0m0.002s
	user	0m0.003s
	sys	0m0.000s
		size:
	text	   data	    bss	    dec	    hex	filename
   1447	    544	      8	   1999	    7cf	./k
   ```
  # Finding
``O2 and above obtimizations have really great execution times
Os has greatly optimized space``

### LLVM:
```
O0:	SEGMANT FAULT DUE TO HIGH DATA USAGE
01: 
	real	0m0.003s
	user	0m0.002s
	sys	0m0.001s
O2:
	real	0m0.002s
	user	0m0.003s
	sys	0m0.000s
  text	   data	    bss	    dec	    hex	filename
   1932	    472	      8	   2412	    96c	./k
O3:
	real	0m0.003s
	user	0m0.003s
	sys	0m0.000s
	size:
	  text	   data	    bss	    dec	    hex	filename
   1932	    472	      8	   2412	    96c	./k
Os and Oz:
	real	0m0.002s
	user	0m0.003s
	sys	0m0.000s
		size:
	text	   data	    bss	    dec	    hex	filename
    992	    472	      8	   1472	    5c0	./k
```
# Finding
``O1 and above obtimizations have really great execution times
Os has greatly optimized space.``



**LLVM IS HAVE MORE OPTIMIZING POWER THAN GCC.**

*Clang and LLVM improve the performance more  in comparison to GCC*
*GCC has almost same performance advantage over Clang and LLVM for most programs at the O2 and O3 levels.*
## Example 3
### Vector creation add adding to vector
```
typedef struct vector {
    void **items;
    int capacity;
    int total;
} vector;
void vector_add(vector *v, void *item) {
    if (v->capacity == v->total)
        vector_resize(v, v->capacity * 2);
    v->items[v->total++] = item;
}
```

### gcc:
```
O0:
	real	0m0.592s
	user	0m0.392s
	sys	0m0.200s
O1:
    real	0m0.507s
    user	0m0.275s
    sys	0m0.231s
O2:
    real	0m0.527s
    user	0m0.311s
    sys	0m0.215s
O3:
    real	0m0.341s
    user	0m0.129s
    sys	0m0.212s
Os:
    real	0m0.478s
    user	0m0.314s
    sys	0m0.165s
```
### LLVM:
```
O0:
    real	0m0.631s
    user	0m0.424s
    sys	0m0.208s
O1:
    real	0m0.523s
    user	0m0.262s
    sys	0m0.261s
O2:
    real	0m0.383s
    user	0m0.168s
    sys	0m0.215s
O3:
    real	0m0.362s
    user	0m0.150s
    sys	0m0.209s
Oz:
    real	0m0.514s
    user	0m0.320s
    sys	0m0.194s
```
# Finding
* Clang and LLVM optimize the vectors at the O2 level, while GCC optimizes the vectors at the O3 level
* Except for vectorized programs, GCC does not greatly improve the performance at the O3 level compared with that at the O2 level

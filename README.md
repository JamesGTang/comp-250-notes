**Winter 2016**
**COMP-250: Introduction to Computer Science**


Algorithms
==========
#####Informal Definition
An algorithm is the specification of a sequence of instructions to be carried out by a processor.

Algorithms can be run on a computer, but not necessarily:

* The Mayans had algorithms to predict solar eclipses centuries in advance.
* Egyptians had algorithms to build pyramids.
* Indians had algorithms for factorizing polynomials.
* Greeks had algorithms to build all kinds of geometric constructions using only a compass and straight-edge.

An example of a compass and straight-edge construction:

    e.g. Compass and straight-edge construction
    Problem: angle bisection
        Input: An angle defined by three points AOB
        Output: A point C such that AOC = BOC
    Algorithm:
        Draw circle centered at O to find A' and B'
        Draw circles centered at A' and B' of the same radius to find C
        AOC and BOC will then bisect AOB



We will mostly be concerned with algorithms that can be run by computers, since they are much faster than we are.

Computer Science
----------------
####Definition
Computer science is the study of algorithms for computing machines.

####Formal definition of an algorithm
A well-ordered collection of unambiguous effectively computable operations that when executed produces a result and halts in a finite amount of time.

####What distinguishes computer algorithms?

* Instructions are executed very quickly
* Algorithms must be fully specified before execution
* Algorithms must be unambiguous

Grade School Algorithms
-----------------------
* Unary addition: taking a set A and a set B, their sum C will be the union of A and B. You have to count everything out individually.
* Unary multiplication: taking a set A and a set B, their product B will be the substition of each element of set A by a set B'.

Unary representation is quite inefficient. It works well for addition of small sets because it's easy to visualize, but once sets get too large, your representation will contain way too many characters.

Numeral representations, such as the Roman numerals, came up with more efficient representations to describe larger numbers. However, since the largest character M only represents 1000, it is hard to represent numbers that are of orders of magnitude larger. Furthermore, the romans didn't even have a zero in their system, which means that addition was impossible. Adding 0 expanded how mathematics could work.

####Algorithm 1: Addition

Using a base-10 addition table:

![Addition Table](http://i.imgur.com/BigcVCd.png)

**Addition (base 10): Add two _N_ digit numbers _a_ and _b_ represented by an array of digits.**

	carry = 0
	for i = 0 to N-1 do 
		r[i]  <-- R[a[i],b[i],carry] // only keeps track of the right-most digit of the sum.
		carry <-- L[a[i],b[i],carry] // only keeps track of the left-most digit of the sum.

		/* 
			Note that:
			R[a[i],b[i],carry] = (a[i]+b[i]+carry) % 10
			L[a[i],b[i],carry] = (a[i]+b[i]+carry) / 10
		*/

	end for
	r[N] <-- carry

Example:

	2343+4519=?

	N = 4
	a[]:   [2 3 4 5]
	b[]:   [4 5 1 9]
	r[]: [0 0 0 0 0] 
	/*r[] has one more index because adding 2 numbers of equal magnitude 
	can only lead to a result that's 1 magnitude higher.*/

	The first iteration of the loop would look like this:
		r[0] = R[a[0],b[0],0] = (5 + 9 + 0) % 10 = 14 % 10 = 4
		carry = L[a[0],b[0],0] = 14 / 10 = 1

	So your r[] would be updated to look like so:        [0 0 0 0 4]
	Running your iteration would lead to a final r[] of: [0 6 8 6 4]


Generalization to any base B:
	
	R[a[i],b[i],carry] = (a[i]+b[i]+carry) % B
	L[a[i],b[i],carry] = (a[i]+b[i]+carry) / B

A base 8 example:
	
	 (1205)_8
	+ (736)_8
	---------
	 (2143)_8

	 Which is equal to 1123_X (X being the symbol for 10)

####Algorithm 2: Multiplication

Using a base-10 multiplication table:

![Multiplication table](http://i.imgur.com/RTyqZTI.png)

**Multiplication (base 10): Multiply two _N_ digit numbers _a_ and _b_ represented by an array of digits.**

	// Part 1: actually multiplying

	for j = 0 to N - 1 do 
		carry <-- 0
		for i = 0 to N - 1 do //Looking at all digits from 1st number multiplied by 1 digit of the 2nd number

			prod        <-- (a[i] * b[j] + carry)
			tmp[j][i+j] <-- prod % 10 //keeps least significant digit
			carry       <-- prod / 10 //drops least significant digit

		end for
		temp[j][N+j] <-- carry 
	end for	

	// Part 2: summing all the products together

	carry <-- 0 

	for i = 0 to 2 * N - 1 do
		sum <-- carry
		for j = 0 to N - 1 do
			sum <-- sum + tmp[j][i]
		end for
		r[i]  <-- sum % 10
		carry <-- sum / 10
	end for
	r[2*N] <-- carry

Generalizing for any base B:
	
	Replace all modulo and division operations by B instead of 10.

Analysis of Algorithms
----------------------
We analyze the space and time required to run an algorithm. The space used is less important than time.

We do not analyze the time required to run each possible combination of inputs. This would take into account too many cases and would include too many details, rendering our analysis useless.

In reality, we analyze time as a function of the size of the inputs (which is the parameter *N*). For all inputs of the same size, we would have a precise bound.

####Analysis of Addition
			
	carry = 0  //CONSTANT TIME

	for i = 0 to N-1 do //LOOP = LINEAR TIME
		
		r[i]  <-- (a[i]+b[i]+carry) % 10 //CONSTANT TIME
		carry <-- (a[i]+b[i]+carry) / 10 //CONSTANT TIME

	end for

	r[N] <-- carry //CONSTANT TIME


	Given c, c', c''

	c    >= T("carry <-- 0")
	c''  >= T("r[N]<--carry")
	c'   >= T("r[i]..., carry"")
	c'*N >= T("for...")



	Time(N) <= c + c'N + c'' <= c_1 + c_2 * N
	Time(N) is O(N).

The constants c1 and c2 depend on the hardware that you run the algorithm on. The important thing to take home is that this algorithm has linear time, meaning that doubling the inputs will double time. This statement becomes more true for larger input sets.

####Analysis of Multiplication

	// Part 1: actually multiplying

	for j = 0 to N - 1 do 						  //LINEAR TIME
		carry <-- 0 							  //CONSTANT TIME
		for i = 0 to N - 1 do 					  //LINEAR TIME

			prod        <-- (a[i] * b[j] + carry) //CONSTANT TIME
			tmp[j][i+j] <-- prod % 10 			  //CONSTANT TIME
			carry       <-- prod / 10 			  //CONSTANT TIME

		end for
		temp[j][N+j]  				     		  //CONSTANT TIME
	end for	

	// Part 2: summing all the products together

	carry <-- 0 

	for i = 0 to 2 * N - 1 do 					  //LINEAR TIME
		sum <-- carry 							  //CONSTANT TIME
		for j = 0 to N - 1 do 					  //LINEAR TIME
			sum <-- sum + tmp[j][i] 			  //CONSTANT TIME
		end for
		r[i]  <-- sum % 10 					      //CONSTANT TIME
		carry <-- sum / 10 						  //CONSTANT TIME
	end for
	r[2*N] <-- carry 							  //CONSTANT TIME

	Time(N) = c_1 + c_2*N + c_3*N^2
	Time(N) is O(N^2)

Since both parts of the algorithm have two nested for loops with linear time, they will be of quadratic time (since N x N = N<sup>2</sup>).

####Big O Notation

This notation drops all lower exponents and constants in Time(N).
For example, addition is O(N) and multiplication is O(N<sup>2</sup>).

Today, the best multiplication algorithm has a running time of O (N x 2<sup>log* N</sup>). This is very close to linear time, as the iterated logarithm (log*) function increases incredibly slowly. Of course, this makes for a much more sophistic algorithm.

Bases and Binary Representation
===============================

####Base 8 to Base 2

(2143)<sub>8</sub> = 2 x 8<sup>3</sup> + 1 x 8<sup>2</sup> + 4 x 8<sup>1</sup> + 3 x 8<sup>0</sup>

Since 8 = 2<sup>3</sup>, you can simply convert the digit in base 8 to its binary representation. Each digit of 8 is equivalent to three binary digits. Thus, the conversion can be done by only local operations.

(2143)<sub>8</sub> = (010 001 100 011)<sub>2</sub> = (010001100011)<sub>2</sub>

An equivalent process is done to convert binary numbers to the much more readable hexadecimal base unit.

####Base 2 to Base 10

(10010010101010101)<sub>2</sub> = (75093)<sub>10</sub>

We take each power of 2 in base 10, multiply them by their binary digit, and sum the digits up.

	Powers of 2 in base 10:

	2 ** 0  = 1     |   2 ** 11 = 2'048 
	2 ** 1  = 2     |   2 ** 12 = 4'096
	2 ** 2  = 4     |   2 ** 13 = 8'192
	2 ** 3  = 8     |   2 ** 14 = 16'384
	2 ** 4  = 16    |   2 ** 15 = 32'768 
	2 ** 5  = 32    |   2 ** 16 = 65'536
	2 ** 6  = 64    |   2 ** 17 = 131'072
	2 ** 7  = 128   |   2 ** 18 = 262'144
	2 ** 8  = 256   |   2 ** 19 = 524'288
	2 ** 9  = 512  	|   2 ** 20 = 1'048'576
	2 ** 10 = 1'024 |   2 ** 21 = 2'097'152

This process is special to base 2 because we only have two options for each digit: included or excluded from the sum. Doing the same process in the other direction much more complicated operations, since include/exclude aren't the only operations. You can have from 0 to 9 times of each digit.

####Algorithm 3: Convert integer to binary

**Input: a number _n_**

**Output: the number _m_ expressed in base 2 using a bit array _b[]_**

	i <-- 0
	while m > 0 do
		b[i] <-- m%2
		m    <-- m/2
		i    <-- i+1
	end while

Example:

	75093 = 0b10101010111001000

	/2    %2  | /2    %2
	----- --  | ----- --
	37546  1  | 73     1
	18773  0  |	36     1
	9386   1  | 18     0
	4693   0  | 9      0
	2346   1  | 4      1
	1173   0  | 2      0
	586    1  | 1      0
	293    0  | 0      0
	146    1  |

Why does it work? 

	Note how any number can be represented as:
	m = b * (m/b) + m%b

	Since integer division of a number drops the remainder. When you add the remainder back, you get the original number again.

Generalizing for Base B:

	Replace 2 by B.

####Fractional numbers

Example:

26.375 = (11010.011)<sub>2</sub>

The integer portion of the number can be calculated separately from the fractional part. The latter can be found by multiplying negative powers of 2.

	0.375*8/8
	= 3/8
	= 11/1000
	= 1.1/100
	= 0.11/10
	= 0.011/1
	= 0.011

Certain fractions yield infinite decimal numbers. You can separate the fraction into a non-repeating part and a repeating part.

####Binary representation in computers

Computer memory does not expand or retract depending on the size of the data, so we always work with a fixed size of memory. The smallest unit of memory is a _byte_. A byte contains 8 bit registers for memory. 

Data representation on a byte is circular because adding to the maximum value of (11111111)<sub>2</sub> resets the number to 00000000 (which is equivalent to all operations being modulo 256).

* Unsigned representation: Each byte contains a positive value from 0 to 255 (2<sup>8</sup>-1).
* Signed representation: Each byte contains a value from -128(-2<sup>7</sup>) to 127(2<sup>7</sup>-1). The greatest bit is used to tell the sign of the number.

Here is a table of binary numbers:

	Binary     Signed   Unsigned

	00000000   0		0
	00000001   1		1
	. 		   .	    .
	.          .		.
	01111111   127	    127
	10000000   128	   -128
	10000001   129	   -127
	.		   .        .
	.		   .        .
	11111111   255     -1

On modern computers, we use more than one byte to represent our numbers. Thus, the bounds of our numbers would change depending on the amount of bits allocated to a number.

Interesting example of the rollover:

``` java

//This loop runs to infinity!
for (short s = 32767;s<32768;s++){
	System.out.println(s);
}
	/*Output:

	 32767
	-32768
	-32767
	   ...
	     0
	     1
	   ...
	 32766
	 32767
	   ...
	*/
```
In our computer memory, we store the location of our bytes in addresses. Address size varies depending on the number of bits of your computer.

####How this works in Java

In Java, we use different amounts of bytes for different primitive types.

	Boolean 00000000 								(1 byte)

	Byte    00000000 								(1 byte)

	Char    00000000 								(1 byte)

	Short   00000000 00000000 						(2 bytes)

	Int     00000000 00000000 00000000 000000000	(4 bytes)

	Long    00000000 00000000 00000000 000000000	(8 bytes)
	     	00000000 00000000 00000000 000000000

	Float   00000000 00000000 00000000 000000000	(4 bytes)

	Double  00000000 00000000 00000000 000000000	(8 bytes)
            00000000 00000000 00000000 000000000

The address keeps track of the first byte, since the bytes used are adjacent to each other. For instance, the address of an integer will only track the first byte and will automatically know that the other three bytes are adjacent.

For reference types in Java, we can use addresses themselves.

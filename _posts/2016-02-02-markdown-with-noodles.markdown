---
title: "Bit Models"
layout: post
date: 2016-02-02 14:40
tag: bits,model,bit,models,binary,magnitude,calculation,computing
#star: true
---

# Bit Models

A bit, short for binary digit, is a binary valued variable. The two possible values are typically written as 1 and 0, or true and false. On a computing chip (processor, memory chip, etc.), they are represented by high and low voltages

Everything in computing is based upon combinations of bits
bits can be either 1 or 0. Since a single bit can represent only two values, we must group bits together to represent a wider range of numbers.

                        0 0 0 1 0 0 1 1

When bits are grouped, there are a variety of methods for interpreting them collectively. Each method of interpretation is called a bit model.

## Magnitude-only Bit Model

The simplest bit model is for nonnegative whole numbers. In this case, each bit represents a nonnegative integer power of 2. The place values of the bits are as follows:

    example bit value		 0	 0	 0	 1	 0	 0	 1	 1
    place value             27   26	 25	 24	 23	 22	 21	 20
    place value (base 10) 	128	 64	 32	 16	 8	 4	 2	 1

The total value of the number represented is found by adding up the place values of all the bits. In the example above, the value represented in the 8 bits (1 byte) is 19:

    0 + 0 + 0 + 16 + 0 + 0 + 2 + 1= 19

Given 8 bits, it is possible to store whole numbers in value up to

    7
    ∑2i = 28 − 1= 255
    i=0

or in the range 0 to 255.

The significance of bits can be thought of as which digits most change the number. It is common practice to list bits from highest to lowest, left to right, following the same convention used to write base 10 numbers.

In general, storing numbers only within the range 0–255 is not terribly useful. Some things do use this range, such as graphical display pixel values, but obviously a wider range is needed for most computations. This is accomplished by grouping more bits together. For example, by grouping 4 bytes (32 bits) together, the magnitude-only bit model can represent whole numbers in value up to


    31
    ∑ 2i = 232 − 1= 4,294,967,295
    i=0

or in the range 0–4,294,967,295.

In C, several data types use the magnitude-only bit model. An unsigned char is a 1-byte (8 bits) variable with a range of 0 to 255. On a 32-bit system, an unsigned int is a 4-byte (32 bits) variable with a range of 0 to 4,294,967,295, and an unsigned short int is a 2-byte (16 bits) variable with a range of 0 to 65,535. Technically, the C language does not define the size of an int.

## Sign-Magnitude Bit Model

In the case where signed whole numbers are desired, a common practice is to allocate the highest order bit to be the sign bit. This is called the sign-magnitude model:

    example bit value            1      0 	0 	1 	0 	0 	1 	1
    place value			        sign 	26	25	24	23	22	21	20 
    place value (base 10) 	   + or − 	64	32 	16 	8 	4	2	1

By common convention, a value of 0 in the sign bit indicates a positive number, while a value of 1 in the sign bit indicates a negative number.

The sign-magnitude model suffers from two drawbacks. First, notice that there are two possible bit values for zero: 00000000 can be interpreted as “positive zero,” while 10000000 is interpreted as “negative zero.” This does not make much sense. Even more important, using this bit model makes binary addition somewhat complicated. If we have zero or two negative numbers, we can perform addition exactly as outlined for the magnitude-only bit model. However, if we have one negative number and one positive number, we must instead perform a subtraction.While subtraction is not a terribly difficult task, it would be nice if we could use the same method for binary addition regardless of the signs of the two numbers. Because of these two drawbacks, no data types in C use the sign-magnitude bit model.

## Two’s Complement Bit Model

Using the two’s complement bit model, positive integers (and zero) are represented exactly the same as they are in the magnitude-only bit model. Negative numbers are represented by applying the following sequence of steps: 
1. Write the bits for the positive version of the number. 
2. Invert (flip) all the bits. 
3. Add 1.


For example, to represent −7, we proceed through the following steps:

    positive value(+7)		0 	0	0	0	0 	1 	1 	1
    invert all bits 		1	1 	1 	1 	1	0	0	0
    add 1                   1 	1 	1	1	1 	0 	0	1

The process of adding 1 is carried out exactly as described in the previous sections. Based on our example, we find that the two’s complement bit representation for −7, using 8 bits, is 11111001. When a two’s complement number has a 1 in the highest bit, it indicates that the number is negative. To find the value, we perform the same steps:

    unknown value (+?)		1 	1	1	1	1 	0 	0 	1
    invert all bits 		0	0 	0 	0 	0	1	1	0
    add 1                   0 	0 	0	0	0 	1 	1	1

After performing these steps, the value provides the magnitude of the negative number. In this example, we get a magnitude of 7, so the original bit pattern 11111001 is known to be −7.
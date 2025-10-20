<!--
Title: Subtraction Through Addition: The Power of Complements
Description: Explore how subtraction through addition works using complements — from Pascal’s machine to modern binary systems.
Author: George Manoj
Date: 2025-10-19
Tags: complement arithmetic, binary, pascaline, 2's complement, subtraction
-->

# Subtraction Through Addition: The Power of Complements

## Introduction
I studied computer applications at the postgraduate level, and like many students, I learned about 2’s complement from textbooks and lectures. At the time, I accepted it as just another technique someone had invented — useful, but not something I questioned deeply.

Fast forward 30 years, and I happened to stumble upon *Hacking: The Art of Exploitation*, a book on C programming. It inspired me to start from scratch and revisit the fundamentals. This time, I didn’t take things at face value. I wanted to understand the origins of 2’s complement — whether it was a clever trick or rooted in deeper mathematical principles. Why does flipping the bits and adding one work so elegantly? Was it discovered by accident, or is there a logical foundation behind it?

## Pascal’s Insight
While researching the origins of 2’s complement, I came across a fascinating historical parallel: Blaise Pascal’s Pascaline machine. Built in the 1600s, it addressed subtraction in base-10 using the concept of 9’s complement plus one — essentially the decimal equivalent of binary 2’s complement. This clever approach allowed the machine to perform subtraction through addition, completely sidestepping the complexity of borrowing logic.

📺 [Watch this video on Pascaline](https://www.youtube.com/watch?v=3h71HAJWnVU)

## Borrowing vs. Complements

In base-10 arithmetic, let’s look at a simple subtraction problem: **100 – 28**.

Using the traditional *borrowing* method, you’d need to subtract 8 from 0 — which isn’t directly possible, so you borrow from the next digit. This process repeats across digits and quickly becomes cumbersome, especially when designing digital circuits that must handle these operations automatically.

There are **two challenges** here:
1. The *borrowing problem*  
2. The *subtraction problem*  

Both add complexity when building arithmetic circuits.

Let’s first tackle the **borrowing problem** using a neat trick.  
If we rewrite the subtraction problem as:

> **100 – 28 = 1 + (99 – 28)**

We can subtract each digit *without borrowing*.  
This method uses a concept called **complements**. Specifically, if we find the **9’s complement** of 28 and then add 1, we can replace subtraction with simple addition.

### What Is the 9’s Complement?

For any digit, its 9’s complement is the number you must add to it to make 9.  
For example, the 9’s complement of 0 is 9, the 9’s complement of 1 is 8, and so on.

| Digit          | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|----------------|---|---|---|---|---|---|---|---|---|---|
| 9’s Complement | 9 | 8 | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |

So, using the above table, the 9’s complement of 28 is **71**.

Now, going back to our rewritten problem:

> **100 – 28 = 1 + (99 – 28)**  
> (note: 99 – 28 is nothing but the 9’s complement of 28)  
> **= 1 + 71**  (adding one to get the 10’s complement)  
> **= 72**

This matches the actual answer to 100 – 28, but we did it without borrowing!  

Rearranging **100 as (99 + 1)** helps illustrate an important rule:  
> The **10’s complement** of a number is simply its **9’s complement plus one**.  
> The n's complement of a number is **(n-1)'s complement plus one**.

---

### Tackling the Subtraction Problem

Now let’s address the **subtraction problem** using another neat trick.

As we saw in the Pascaline video earlier, Pascal cleverly designed his machine to handle subtraction by internally using a form of complementation — effectively *hardcoding* the logic (the above table) into its mechanism.  
In a similar way, when we perform subtraction using the **9’s complement method**, we can refer to the same complement table shown above.  

By converting the subtrahend to its 9’s complement, we can **turn subtraction into simple addition**, completely avoiding direct subtraction or borrowing logic.

---

## Building the Intuition

Let’s explore this idea further with another base-10 example: **583 – 298**.

Our earlier example (**100 – 28**) was simple because it conveniently used 100, a clean power of ten.  
This time, we don’t have that luxury. But what if we *create* one?

Since both numbers are three digits, the next power of ten is **10³ = 1000**.  
If we add and subtract 1000, the overall value doesn’t change — but it helps us work with complements.

We can rewrite the expression as:

> **583 – 298 = (583 + 1000 – 298) – 1000**  
> which is  
> **583 – 298 = (583 + 10’s complement of 298) – 1000**  
> which is the same as  
> **(583 + 1 + (999 – 298)) – 1000**  
> **(583 + (9’s complement of 298) + 1) – 1000**

From the table above, the 9’s complement of 298 is **701**.

Now let’s compute:

> **583 + 701 + 1 – 1000**  
> = **583 + 702 – 1000**  
> = **1285 – 1000**

We have two options here:

1. **The mathematical way:**  
   1285 – 1000 = **285**, which is the correct answer — but we technically performed a subtraction, the one thing we wanted to avoid.

2. **The complement way (used in circuits):**  
   Notice that **1285** exceeds our base (1000). In complement arithmetic, this is called an **overflow**.  
   We can safely **ignore the overflow digit** (the extra 1 in the thousand’s place) and take the remaining digits — **285** — as our result.

---

## Why Complements Simplify Machine Arithmetic

Humans can easily borrow across digits, but machines prefer consistency.  
So instead, they compute the **10’s complement** as the **9’s complement + 1**.

In other words, a machine can follow a simple rule:  
> If the result exceeds the base (a power of ten), drop the overflow digit.

That’s how the subtraction and borrowing problem is turned into addition using complement arithmetic.  
In this system, **overflow is expected — a feature, not a bug.**

Let’s flip our earlier problem: **298 – 583**

We can rewrite it using complement logic:  
> **(298 + 9’s complement of 583 + 1) – 1000**  
> = **(298 + 416 + 1) – 1000**  
> = **715 – 1000**

Ignoring the –1000 for a moment, the result, **715**, is *less than 1000* — meaning there’s **no overflow**.

We have two ways to interpret this:

1. **The mathematical way:**  
   715 – 1000 = **–285**, which is the correct answer — but again, it involves a subtraction step, something we’re trying to avoid in circuits.

2. **The complement way (used in machines):**  
   Since **715** didn’t exceed the base (1000), we call this an **underflow**.  
   In complement arithmetic, we can find the answer by taking the **10’s complement of 715** and marking it as **negative**.

To find the magnitude:
> **1000 – 715 = 285**  
> Final answer: **–285** (The machine will add a negative sign in this case)

> In practice, computers don’t actually subtract 1000.  
> They simply check whether the result exceeded the fixed number of digits:  
> - If it **did exceed**, the result is **positive**.  
> - If it **did not exceed**, the result is **negative**, and we take the complement to find its magnitude and add a negative sign.

---

## Why Complements Matter

Complement-based subtraction works beautifully because:

- ✅ **Overflow** → result is positive (discard extra digit)  
- ❌ **Underflow** → result is negative (take the complement again)

This elegant logic allows machines to handle **both positive and negative results using the same addition circuitry** — no need for separate subtraction hardware.

That’s the real power of complements:  
They merge addition and subtraction into a **single, unified process**, making hardware **simpler, faster, and more reliable**.

---

## How 10’s Complement Leads to 2’s Complement in Binary Systems

Binary systems use **2’s complement** to represent negative numbers:

1. Flip all bits → 1’s complement  
2. Add 1 → making it 2’s complement

**Example: 00101101 (45 in decimal)**  
- Flip bits: 11010010 → this is the **1’s complement**  
- Add 1: 11010011 → this is the **2’s complement**, which represents **–45**  

> Note: If you interpret the raw binary as an unsigned number, it equals 211. In signed arithmetic, the first bit is considered the **sign bit**.  
> In a 16-bit signed system: 1111111111010011 → magnitude is still 45, negative sign is indicated by the Most Significant Bit (MSB).

Mathematically:  
- **11111111 – 00101101 = 11010010** ← this is the 1’s complement (same as flipping the bits)  
  - In binary this allows us to design a circuit that **flips the bits** instead of subtracting.  
- **+1 → 11010011** ← this is the 2’s complement.

Now, when performing arithmetic with 2’s complement:

- If the result **overflows** the fixed bit width → the answer is **positive** (discard the extra bit)  
- If there is **no overflow** → the answer is **negative** (take the 2’s complement again to find its magnitude)

---

## Conclusion
What began as a textbook technique has revealed itself to be a profound architectural choice — one that simplifies hardware, unifies logic, and echoes centuries of mathematical insight. From Pascal’s mechanical ingenuity to modern binary systems, complements offer a consistent, elegant way to handle subtraction through addition.

For me, revisiting 2’s complement wasn’t just about technical clarity. It was about reclaiming curiosity, honoring intuition, and finding beauty in the fundamentals. Thirty years later, I’m not just relearning — I’m re-seeing.

---


## References

- *Hacking: The Art of Exploitation* by Jon Erickson  
- [YouTube: Pascaline Machine](https://www.youtube.com/watch?v=3h71HAJWnVU)  
- [Wikipedia: Two’s Complement](https://en.wikipedia.org/wiki/Two%27s_complement)  
- [RIT Math Handout on Two’s Complement](https://www.rit.edu/academicsuccesscenter/sites/rit.edu.academicsuccesscenter/files/documents/math-handouts/DM3_TwosComplement_BP_9_22_14.pdf)  
- Microsoft Copilot for grammar, proofreading, article flow, and concept clarification

 
*Footnote: Language polished with AI assistance. All technical explanations of 2’s complement are my own.*

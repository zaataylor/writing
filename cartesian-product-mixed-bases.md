# Computing Cartesian Products Using Mixed-Base Numbers

The other week, a work-related problem got me thinking about the [Cartesian Product](https://en.wikipedia.org/wiki/Cartesian_product). Specifically, I asked myself, "What's an intuitive way to compute the Cartesian product?" Frankly, this isn't something I've ever really thought much about before, but after considering it for a bit, I realized one way to think of this is in terms of incrementing a mixed-based number with a certain number of digits, starting from $0$.

## Example
Consider a practical, if contrived, example: suppose I want to see how many outfits I can make out of the shoes, pants, and shirts I have. In real life, I'd _never_ do this with my wardrobe, because I don't actually care how many possible outfits I have 😄. Anyways, let's say I have a set of $3$ t-shirts, $2$ pairs of pants, and $2$ pairs of shoes. Let's represent these different items as sets:
- Call the set of t-shirts $T$. Then we have $T = \set{\mathsf{red_{shirt}}, \mathsf{green_{shirt}}, \mathsf{black_{shirt}} }$
- Call the set of pants $P$. Then we have $P = \set{\mathsf{denim_{pants}}, \mathsf{corduroy_{pants}} }$
- Call the set of shoes $S$. Then we have $S = \set{\mathsf{running_{shoes}}, \mathsf{dress_{shoes}} }$

The set of all possible outfits is the Cartesian Product, denoted as $T \times P \times S$.

Now, how would you normally compute how many possible outfits there are? For me, the most intuitive approach would be to fix elements in two out of three of the sets, then vary the elements in the last set; this would produce $n$ elements of the Cartesian product, where $n$ is the size/cardinality of the last set. 

For instance, if we were to fix one element in $T$ and one in $P$, this would mean that we'd only vary the elements we choose from $S$, producing a total of $2$ outfits since the size of $S$, denoted $|S|$, is $2$. After varying elements in the last set, I'd keep the element in the first set fixed, choose and fix an element in the second set, and then repeat the algorithm described above until I got through all possible combinations for the second and third sets. Then, I'd unfix the element in the first set, choosing a new element from that set to fix, then restart the algorithm described at the beginning of this paragraph. Once all combinations of all elements in the three set have been fixed, the full Cartesian product will have been computed.

Programmers will likely recognize this as the nested for-loop approach, for which the psuedocode might look something like:

```python
outfits = {} # a set
for shirt in T
	for pants in P
		for shoes in S
			outfits.add({shirt, pants, shoes})
```

This approach works fine when the number of sets is small and thus, easy to explicitly write out for-loops for (see what I did there? 😉). But, it's not as pretty/elegant/flexible as other approaches.

Nonetheless, executing this algorithm yields:

{
$( \mathsf{red_{shirt}},\space \mathsf{denim_{pants}},\space \mathsf{running_{shoes}} ),$
$( \mathsf{red_{shirt}},\space \mathsf{denim_{pants}},\space \mathsf{dress_{shoes}} ),$
$( \mathsf{red_{shirt}},\space \mathsf{corduroy_{pants}},\space \mathsf{running_{shoes}} ),$
$( \mathsf{red_{shirt}},\space \mathsf{corduroy_{pants}},\space \mathsf{dress_{shoes}} ),$

$( \mathsf{green_{shirt}},\space \mathsf{denim_{pants}},\space \mathsf{running_{shoes}} ),$
$( \mathsf{green_{shirt}},\space \mathsf{denim_{pants}},\space \mathsf{dress_{shoes}} ),$
$( \mathsf{green_{shirt}},\space \mathsf{corduroy_{pants}},\space \mathsf{running_{shoes}} ),$
$( \mathsf{green_{shirt}},\space \mathsf{corduroy_{pants}},\space \mathsf{dress_{shoes}} ),$

$( \mathsf{black_{shirt}},\space \mathsf{denim_{pants}},\space \mathsf{running_{shoes}} ),$
$( \mathsf{black_{shirt}},\space \mathsf{denim_{pants}},\space \mathsf{dress_{shoes}} ),$
$( \mathsf{black_{shirt}},\space \mathsf{corduroy_{pants}},\space \mathsf{running_{shoes}} ),$
$( \mathsf{black_{shirt}},\space \mathsf{corduroy_{pants}},\space \mathsf{dress_{shoes}} )$
}

## Another Approach: Using Mixed-Base Numbers
A [mixed-radix number](https://en.wikipedia.org/wiki/Mixed_radix), also sometimes called a mixed-base number, is a number in which the values at each position can vary in terms of their bases. Let's compare mixed-based numbers with decimal numbers.

Take the decimal value $19$. Call this number $D$. Then, $D$ has two digits, so $D = d1|d2 = 1_{10}|9_{10}$, where both $d1$ and $d2$ are in base-10. We use $|$ to make each position in our number visually distinguishable. Now, let's compute $D + 1$ by hand, paying close attention to what happens to the digits; this will given us a little intuition for the counting process when working with mixed-base numbers. 

$D + 1 = 19 + 1$. To compute this, we first add $1$ to the least significant digit of $D$, $d2 = 9_{10}$. Since we're in base-10, adding $1$ to $9$ results in $0_{10}$, with a carry of $1$. We add this carried value to the next least significant digit, $d1 = 1$, with a result of $2_{10}$. Since there is nothing left to carry in this case, we know the addition is complete, with result $20_{10}$. So, $D + 1 = 2_{10}|0_{10}$.	

Now, consider a mixed-based number where the first digit is in base-3, and the second digit is in base-10. Let's call our number $K$, and let $K$ take value $2_{3}|9_{10}$, so $K = d1|d2 = 2_{3}|9_{10}$. Now, let's compute $K + 1$, as we did before with $D$.

$K + 1 = 2_{3}|9_{10} + 1$. Let's add $1$ to the least significant digit of $K$, $d2 = 9_{10}$. As before, this digit is in base-10, so adding $1$ to $9_{10}$ results in $0_{10}$, with a carry of $1$. We add this carried value to the next least significant digit, $d2 = 2_{3}$. Adding $1$ to this results in $0_{3}$ since we're in base-3, with a carry of $1$. This means that $K + 1 = 1_{3}|0_{3}|0_{10}$. As in the former case, we're done with the addition now that we've carried all possible values.

### So What?

How does this apply to computing the Cartesian product? The connection may feel tenuous at first, but let's go back to our original example and make it plain.

We know that:
- $|T| = 3$
- $|P| = 2$
- $|S| = 2$
- The number of sets involved in the product is $3$. Let $j = 3$.

With these 4 pieces of information, we know enough to compute the Cartesian product.

Let's create a mixed-based number, $N$, consisting of $j$ digits. Then, $N = d1|d2|d3$. Now, let $d1$'s base correspond to the cardinality of $T$, so that $d1$ is a base-3 number. Then, do the same with $d2$ and $P$, so $d2$ is base-2. Finally, do the same with $d3$ and $S$, so $d3$ is in base-2. Initialize $N$ to the "zero" value, so that $N = 0_{3}|0_{2}|0_{2}$.

Given the previous correspondence of $d1$ to $T$, $d2$ to $P$, and $d3$ to $S$, maybe the values of these digits also have some relation to their corresponding sets? In this case, let's treat them as indices into arrays backing each set. So, given $S = \set{ \mathsf{running_{shoes}}, \mathsf{dress_{shoes}} }$, we can let $S' = [ \mathsf{running_{shoes}}, \mathsf{dress_{shoes}} ]$ be the underlying array representing the set. Then, $d3 = 0$ corresponds to a reference to the $0$-index element of $S'$ (i.e. $S'[0]$).

Extending these correspondences to all digits, for $N = 0_{3}|0_{2}|0_{2}$, we have:
- $d1 = 0 \rightarrow T'[0] = \mathsf{red_{shirt}}$
- $d2 = 0 \rightarrow P'[0] = \mathsf{denim_{pants}}$
- $d3 = 0 \rightarrow S'[0] = \mathsf{running_{shoes}}$

Notice that the set comprised of these items, $\set{ T'[0], P'[0], S'[0] } = \set{ \mathsf{red_{shirt}}, \mathsf{denim_{pants}}, \mathsf{running_{shoes}} }$, is also the first element of the Cartesian product we computed above!

### Counting

With this first example, you may begin to see where this is going: we can increment $N$ by $1$, and each digit of the sum provides an index into the array backing that digit's corresponding set! Requiring $N$ be a mixed-base number enables us to account for the varying cardinalities of each set. Adding $1$ to $N$ -- and carrying as required for each respective base -- effectively enables us to automatically fix and vary different elements of each set like we did in the initial example. If we take a sum and add each of its digits to a tuple, creating a new tuple for each sum, we'll find that the results of adding $1$ to $N$ each time create a lexicographic ordering of the tuples. In other words:

$$
\begin{equation}
	\begin{aligned}
	& N = 0_{3}|0_{2}|0_{2} \rightarrow (0, 0, 0) \\
	& N + 1 = 0_{3}|0_{2}|1_{2} \rightarrow (0, 0, 1) \\
	& N + 2 = 0_{3}|1_{2}|0_{2} \rightarrow (0, 1, 0) \\
	& N + 3 = 0_{3}|1_{2}|1_{2} \rightarrow (0, 1, 1) \\
	& N + 4 = 1_{3}|0_{2}|0_{2} \rightarrow (1, 0, 0) \\
	& N + 5 = 1_{3}|0_{2}|1_{2} \rightarrow (1, 0, 1) \\
	& N+ 6 = 1_{3}|1_{2}|0_{2} \rightarrow (1, 1, 0) \\
	& N + 7 = 1_{3}|1_{2}|1_{2} \rightarrow (1, 1, 1) \\
	& N + 8 = 2_{3}|0_{2}|0_{2} \rightarrow (2, 0, 0) \\
	& N + 9 = 2_{3}|0_{2}|1_{2} \rightarrow (2, 0, 1) \\
	& N + 10 = 2_{3}|1_{2}|0_{2} \rightarrow (2, 1, 0) \\
	& N + 11 = 2_{3}|1_{2}|1_{2} \rightarrow (2, 1, 1) \\
	\end{aligned}
\end{equation}
$$

You may notice that we stopped the iteration at $N + 11 = 2_{3}|1_{2}|1_{2} \rightarrow (2, 1, 1)$. Why did we do this? Well, if we add one more to this value, the result overflows such that we now have a new digit. In other words, $(N + 11) + 1 = 1_{3}|0_{3}|0_{2}|0_{2} \rightarrow (1, 0, 0, 0)$. Intuitively, this makes sense because the cardinality of the Cartesian product is equal to the product of the cardinalities of each constituent set. Said another way, $|T \times S \times P| = |T| * |S| * |P| = 3 * 2 * 2 = 12$. So, as we only expect the Cartesian product to contain twelve elements, the first overflow we see is a indication that we've generated all members of the product already. We can also just keep track of how many elements we've already computed, since this can easily be derived using the size of each set. This is a useful termination condition when programming.

## Representation in Programming
I [implemented](https://github.com/zaataylor/cartesian/blob/3b3244d82727e79782eaa90b0b1e2cf0652cb81b/cartesian/cartesian.go#L52) the counting-based algorithm I described above in Go because: (1) that's the language I'm working with most recently at work and (2) I'm trying to become better at using it.

The core logic of the algorithm looks similar to this:
```golang
for count < max {
	if count == 0 {
		count += 1
		// First element: [0, 0, ..., 0]
		tmp := make([]int, data.length)
		indices = append(indices, tmp)
	}
	if count < max {
		// increment by 1, then take modulus
		sum := (data[data.length-shiftIndex] + 1) % moduli[data.length-shiftIndex]
		data[data.length-shiftIndex] = sum
		// carry the 1 if the sum is 0
		if sum == 0 {
			for sum == 0 && data.length-shiftIndex > 0 {
				// shift down 1 (i.e. one digit to the left)
				shiftIndex += 1
				// increment by "1", then take modulus
				sum = (data[data.length-shiftIndex] + 1) % moduli[data.length-shiftIndex]
				data[data.length-shiftIndex] = sum
			}
		}
		tmp := make([]int, data.length)
		copy(tmp, data)
		c.indices = append(c.indices, tmp)
		// We've computed one more element
		count += 1
		// Reset for next addition
		shiftIndex = 1
	}
}
```

To represent the mixed-based number $N$ from our earlier example, I created an `int`-type slice, `data`, whose length corresponds to the number of sets involved in the Cartesian product. So, from the earlier example, `data` would be initialized as `data := make([]int, 3)`. To accommodate the respective bases of each position in our number, I created another `int`-type slice, `moduli`, of the same size. For each index `i` in `data`, we treat `data[i]` as an index into the $i$-th set in the product. Similarly, we interpret the value at `moduli[i]` as the cardinality of the $i$-th set in the product. I use `shiftIndex` to maintain my position in `data`, equivalent to keeping track of what digit we'd be adding or carrying $1$ to when we add by hand. I also use `count` to maintain the total number of elements computed so far, and `max` as the total number of possible elements in the product; this is for the termination condition I described previously.

To mimic the process of counting and carrying, we do the following:
1. If you have _just_ started counting (i.e. not added $1$ to anything yet), just return `data` and exit. As it's initialized to $0$, the values in it uniquely specify the first member of the product. Otherwise, go to Step 2.
2. Set `shiftIndex = 1`.
3. Add $1$ to the current significant position in $N$ by adding $1$ to `data[data.length - shiftIndex]`, then taking the appropriate modulus for that position's base using the value in `moduli[data.length - shiftIndex]`. We assign the result to the digit at the same position, `data[data.length - shiftIndex]`, then examine the sum.
4. If the sum was _not_ $0$, then we go to Step 5. The current values in `data` correspond to indices into the arrays backing the sets we're taking the product of. If the sum _was_ $0$, that means we need to carry a $1$ to add to the next digit in $N$. We increment `shiftIndex` by $1$, add $1$ to `data[data.length - shiftIndex]`, then take the appropriate modulus for that position's base using the value in `moduli[data.length - shiftIndex]`. We assign the result to the digit at the same position, `data[data.length - shiftIndex]`, then examine what the sum was.
5. We repeat step 4 until either there are no more digits we can carry to without "overflowing" beyond the length of `data` OR the most recent result is non-zero.
6. Increment `count` by $1$.
7. Go back to Step 1.

We perform these steps as long as `count < max`, and when this condition fails, we'll have computed the entire product, albeit somewhat indirectly since we computed _indices_ into each respective set-backing array, rather than the values in those arrays themselves. But, the next part, accessing the values from the indices, is easy™️.

### P.S.

You'll notice that throughout this post, we essentially treat the value $1$ we're adding as $1_{x}$, where $x$'s value takes on the value of the base of whatever number $1$ is being added to. In this way, $1$ is a _little_ special :).
# OpenMP parallelization
**We will see Openmp parallelization examples with main ideas here.** We will use one machine/node with possibly many cores and exploit the many cores so that the parts of the program which can be run parallely (not depending on one another), is run parallely. 


## Small notes

First of all, one should add `-fopenmp` (or `-qopenmp`) while compiling the c/cpp program. for example like: `g++ test.cpp -o test -fopenmp`.

Then as usual, you have to do `./test` to run the program (with all the cores).  

If you want to restrict number of cores, one way to do that is to issue `export OMP_NUM_THREADS=2` before running the program. 


---
## For loop

Suppose you have a simple C program of printing something 6 times. Your code looks like:
```C
#include <stdio.h>

int main() {
	for (int i = 0; i < 6; ++i) {
		printf("Hello, world, %d.\n", i);
	}
	return 0;
}
```
Now you want to parallelize it. One way of doing this would be to add `pragma directive` as follows:
```C
#include <stdio.h>

int main() {
	#pragma omp parallel {							// Defines a parallel region, which is code that will be executed by multiple threads in parallel.
		#pragma omp for								// Causes the work done in a for loop inside a parallel region to be divided among threads.
		for (int i = 0; i < 6; ++i) {
			printf("Hello, world, %d.\n", i);
		}
	}
	return 0;
}
```

Basically a shorthand for this same program is:
```C
#include <stdio.h>

int main() {
	#pragma omp parallel for
	for (int i = 0; i < 6; ++i) {
		printf("Hello, world, %d.\n", i);
	}
	return 0;
}
```

Now suppose, you want to restrict the number of threads, you can use `#pragma omp parallel num_threads(4)` for the non-shorthand case (and `#pragma omp parallel for num_threads(4)` for the simplified case.)


---
## Data sharing
Now look at the program, it takes a an array and puts numbers in it in the first part and ijn the second part, it sums a function of those.  
```C
#include <stdio.h>

#define SIZE 10

int main() {
	// Input array
	int a[SIZE];
	for (int i = 0; i < SIZE; i++) {
		a[i] = 2 * i * i;
	}
	
	// summing part
	double sum = 0.0;
	for(int i = 0; i < SIZE; i++){
		sum = sum + a[i]/sqrt(i);
	}
	
	
	printf("sum is %f\n", sum);
	return 0;
}
```

The first part can be Embarrassingly parallelized, i.e., parallelized with little effort. Just add `#pragma omp parallel for` before the first for loop, and you are done. (Some common Embarrassingly parallel problems are Monte Carlo, Numerical integration, Discrete Fourier Transformation etc. )

For the second for loop, look a little more carefully. Now let's say there are two parallel threads there. Both variables are trying to add something to the variable `sum` at the same time. So it is **shared** between all the threads at the same time. On the contrary, you can have **private** variables (and sometimes you would prefer such variables) of which each thread have a copy of their own. If you don't give any such declaration to the variables, by **default**, everything is **shared**. 









Look at this example from https://www.cs.utah.edu/~hari/teaching/paralg/simone_archer.pdf
```C
#pragma omp parallel for
for (i = 1; i < N; i++) {
	a[i] = 2.0 * i * (i - 1);
	b[i] = a[i] - a[i - 1];
} 
```
Here, `b[i]` needs both `a[i]` and  `a[i-1]` to be properly computed first. However, `a[i-1]` is possibly being computed in some other thread. Now in that specific thread, it is possible that that is still not computed. So the value of `b[i]` will not be correct at all. 


Look at another example, 
```C
double x;
#pragma omp parallel for
for (i = 1; i < N; i++) {
	x = sqrt(b[i]) - 1;
	a[i] = x * x + 2 * x + 1;
}
```
Clearly, the variable `x` is written by more than one threads, possibly at the same time. So `a[i]` would be in problem, which `x` to take. 
Simple solution is to have an **private** `x` for all the threads:
```C
double x;
#pragma omp parallel for private(x)
for (i = 1; i < N; i++) {
	x = sqrt(b[i]) - 1;
	a[i] = x * x + 2 * x + 1;
}
```
Another solution would be to declare x inside the loop so that in each loop, x is defined seperately.

```C
#pragma omp parallel for
for (i = 1; i < N; i++) {
	double x = sqrt(b[i]) - 1;
	a[i] = x * x + 2 * x + 1;
}
```














Good practises:


If you're just reading data through a shared data/array/pointer, then there shouldn't be a problem. But if you're also writing to it, then you need to make sure you don't have a race condition.

The definition of data racing is: 
'If multiple threads write without synchronization to the same memory unit, including cases due to atomicity considerations as described above, then a data race occurs. Similarly, if at least one thread reads from a memory unit and at least one thread writes without synchronization to that same memory unit, including cases due to atomicity considerations as described above, then a data race occurs. If a data race occurs then the result of the program is unspecified.'



To get some practical ideas about data racing, look at:
https://stackoverflow.com/questions/7865555/openmp-shared-vs-firstprivate-performancewise
https://stackoverflow.com/questions/26998183/how-do-i-deal-with-a-data-race-in-openmp
https://stackoverflow.com/questions/3952174/avoid-race-in-openmp-in-a-parallel-for-loop
https://pages.tacc.utexas.edu/~eijkhout/pcse/html/omp-data.html
https://forum.openmp.org/viewtopic.php?t=2108
https://www3.nd.edu/~zxu2/acms60212-40212-S12/Lec-11-02.pdf
https://www.cs.utah.edu/~hari/teaching/paralg/simone_archer.pdf				(See this)
http://jakascorner.com/blog/2016/06/omp-data-sharing-attributes.html		(and this)


---
## Barrier
It may happen that in a parallel region, 
 - there is a for loop first, 
 - then you have to make sure that calculations from each thread is done
 - then you do something (possibly sequential).
 - Then you do something else in parallel (possibly). 
What would you do?

You would just add a `#pragma omp barrier` before the just 2nd loop (after the first loop) to make sure that the first loop is completed for all the threads.





Other notable constructs are
`sections`



Some important sources
https://www.openmp.org/resources/
http://jakascorner.com/blog/
http://jakascorner.com/blog/2016/04/omp-introduction.html
http://jakascorner.com/blog/2016/06/omp-data-sharing-attributes.html
https://docs.microsoft.com/en-us/cpp/parallel/openmp/reference/openmp-directives?view=msvc-160
https://cims.nyu.edu/~stadler/hpc17/material/ompLec.pdf



## Tricks

When you use the openmp compiler option, a c variable `_OPENMP` will be defined. Thus, you can have conditional compilation by writing
```
#ifdef _OPENMP
	...
#else
	...
#endif
```

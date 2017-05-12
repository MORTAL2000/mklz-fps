# Fast Parallel Sort
By: Matthew Lee (mkl1) and Kevin Zhang (klz1)

## Summary

We submitted a fast parallel sample sorting algorithm in Go into [15-210's Sorting Competition](http://www.cs.cmu.edu/~15210/sort.html) by Professor Guy Blelloch. We optimized our implementation for performance on a 72-core machine called Aware and also tested on a variety of other machines such as the 20 core unix machines and the gates machines. After the competition, we analyzed and compared our performance with the provided code from the competition as well as parallel sorting implementations in other languages.

## Background

Sorting is a classical problem in computer science. The challenge we had was to correctly optimize the performance of different sorting algorithms on large parallel and distributed machines. The key input and output data structure was an array that needed to be sorted with key value pairs that were both doubles. Sorting is fundamentally not a computationally expensive problem and more of a memory expensive problem. The benefit of parallelization in sorting comes from the ability to sort partitions of the array independently at the same time.

The dependencies in the program depend on the sorting algorithm. For example, with merge sort, the dependencies are like a tree where each branch needs to wait for its adjacent branch to complete before it can merge together and move up to the next merge. The parallelism with merge sort is that we can use fork join parallelism to compute different sections of the tree independently with a work stealing queue.

With sample sort, there are a few initial dependencies where the program needs to first sample elements from the array, sort them, and then determine the splitters between buckets. Then the processors can fill up each bucket in parallel by working on a segment of the array. Finally each bucket can be sorted in parallel.

## Approach

### Matthew


### Kevin


## Results

### Matthew


### Kevin


## References

Axtmann, M., & Sanders, P. (2017). Robust Massively Parallel Sorting. In 2017 Proceedings of the Ninteenth Workshop on Algorithm Engineering and Experiments (ALENEX) (pp. 83-97). Society for Industrial and Applied Mathematics.

## List of Work by Each Student

Matthew wrote all of the Go code while Kevin worked on the Java and CUDA sorting programs. In addition, Matthew made the presentation while Kevin worked on the final report.

# Checkpoint

## Summary

We are planning on competing in [15-210's Sorting Competition](http://www.cs.cmu.edu/~15210/sort.html) by Professor Guy Blelloch. In order to compete effectively, we are going to implement different parallel comparison-based sorts in a few languages and optimize for performance on the provided 72-core machine as well as run tests on a variety of other machine such as the unix machines, gates machines, latedays cluster, and Xeon Phis.

## Background

Sorting is a classical problem in computer science. The challenge with sorting these days is correctly optimizing the performance of different sorting algorithms on increasingly parallel and distributed machines. 

This parallel sorting competition was staged by Professor Guy Blelloch and the rest of the 210 team to incentivize students to help work on this real-world problem by writing efficient parallel sorting algorithms in a variety of languages, and especially garbage-collected languages. In addition, they would like to test how their SML work-stealing scheduler holds up against highly optimized solutions from Carnegie Mellon students.

## The Challenge

The main source of the challenge will be managing memory access and bandwidth requirements and creating a way for the sorting algorithm to adapt quickly and easily to each machine's configuration. Sorting is a problem with low arithmetic intensity; thus, making intelligent design choices based on the architecture of the target machine (e.g. caches, hyperthreading, etc.) will be crucial to getting good performance. 

We will also be coding in higher-level languages which, unlike C, will probably not expose as much low-level control over the machine to us. We will need to make use of our 418 knowledge to coax these high-level languages into giving us good performance.

## Resources

We will do initial testing on the 20 core unix machines and the Gates machines. 

Our “starter code” consists of the two reference implementations that Blelloch’s team provided. We will need to modify the given C++ code or install gcc 4.9 in order to get it to run on the GHC machines though. 

There is also substantial existing literature on parallel sorting algorithms. We will refer to those such as this paper on [Robust Massively Parallel Sorting](http://epubs.siam.org/doi/pdf/10.1137/1.9781611974768.7).

We will need access to the Xeon Phi machines in addition to the four days’ worth of Aware access that Blelloch’s team will give us.

## Goals and Deliverables

### Plan to achieve
* Analyse the performance of the C++ solution that they provided - why does it perform the way it does? Has it approached theoretical peak performance, and if not, what is slowing it down?
* Implement a semi-optimized parallel sorting algorithms in at least 2 garbage-collected languages (Java and Go) and analyse their performance. This will give us a rough idea of how the various languages compare in terms of:
  * Raw performance
  * How much control the languages provide the programmer over the low-level performance-determining details.
* Implement a highly optimized sorting algorithm on one chosen language. This would constitute our main submission to the sorting contest. (Go)
* The concrete goal is to make it run faster than their SML and C++ solutions.
* Implement a parallel sort in CUDA and/or ISPC.

### Hope to achieve
* Highly optimize for more than one language (Java if time is on our side).
* Implement an automated way of searching the parameter space (autotuning). This will allow us to make good use of the 4 days we have on the Aware machine.

### Planned Demo
* Performance comparison graphs between our optimized code and the baseline on different machines.

## Platform Choice

The platform has been already been determined for us by the rules of the contest, but we have chosen the programming languages that we will use, which are Go, Java, and C++ because they provide us with the most flexibility for parallelism.

We will be creating a C++ version with CUDA and/or ISPC as stated in our stretch goals because sorting is a bandwidth-intensive problem, and GPUs have much higher bandwidth currently compared to CPUs.

----

## Project Checkpoint 

### Progress

So far, Matt has implemented a parallel Sample Sort algorithm in Go that sorts 100 million pairs in 1.9 seconds. Kevin is still working on the parallel sorting algorithm in Java and has tested the performance of the built in parallel sort algorithm. We have analyzed the performance of their C++ code on the unix machines and have cemented our focus on Go as our main submission into the parallel sorting competition. We will be submitting our Go implementation soon to Guy Blelloch in order to unlock access to the Aware machine and begin testing both implementations on the Aware machines. We have also created a sample graph below with the Go implementation compared to the C++ implementation on unix5 to demonstrate our current performance.

### Potential Issues

We don't foresee any issues with completing the project. The only problem might be submitting 2 languages to the sorting competition because there are many projects due on the week of May 4th. However, we anticipate being able to complete all of our planned tasks by the class competition on May 12th.

## Schedule

| Week            | Tasks         | Progress     |
| --------------- | ------------- | ------------ |
| Apr 10 - Apr 16 | Review literature on parallel sorting algorithms <br/> Get the provided C++ code to compile and run on GHC machines and then analyse the performance of the provided C++ code <br/> Choose at least 2 garbage-collected languages and get them working on the GHC machines (set up the environment so we are able to compile and run parallel code) | Completed 4/14 <br/> Completed 4/25 <br/> Completed 4/14 |
| Apr 17 - Apr 23 | Implement first attempt at parallel sorting in the garbage-collected languages of our choice <br/> Analyse initial results from the first attempt and choose a language to focus on for our main contest submission | Completed 4/23 <br/> Completed 4/25 |
| Apr 24 - Apr 30 | Do write-up for project checkpoint (due Apr 25) <br/> Focus on optimizing the parallel sort in the language of our choice (Matthew - Go, Kevin - Java) <br/> Submit our progress so far to Guy Blelloch’s team in order to request access to Aware machine | Completed 4/25 <br/> In Progress (Both) <br/> Planned (Both) |
| Apr 31 - May 5  | Test and optimize on the Aware machine <br/> Submit to the 210 sorting competition (due May 4) | Planned (Both) <br/> Planned (Matthew)|
| May 6 - May 12  | Write C++ sorting code with CUDA and/or ISPC (Not part of 210 competition) <br/> Prepare for final presentation | Planned (Kevin) <br/> Planned (Both)|

----

# Proposal

## Summary

We are planning on competing in [15-210's Sorting Competition](http://www.cs.cmu.edu/~15210/sort.html) by Professor Guy Blelloch. In order to compete effectively, we are going to implement different parallel comparison-based sorts in a few languages and optimize for performance on the provided 72-core machine as well as run tests on a variety of other machine such as the unix machines, gates machines, latedays cluster, and Xeon Phis.

## Background

Sorting is a classical problem in computer science. The challenge with sorting these days is correctly optimizing the performance of different sorting algorithms on increasingly parallel and distributed machines. 

This parallel sorting competition was staged by Professor Guy Blelloch and the rest of the 210 team to incentivize students to help work on this real-world problem by writing efficient parallel sorting algorithms in a variety of languages, and especially garbage-collected languages. In addition, they would like to test how their SML work-stealing scheduler holds up against highly optimized solutions from Carnegie Mellon students.

## The Challenge

The main source of the challenge will be managing memory access and bandwidth requirements and creating a way for the sorting algorithm to adapt quickly and easily to each machine's configuration. Sorting is a problem with low arithmetic intensity; thus, making intelligent design choices based on the architecture of the target machine (e.g. caches, hyperthreading, etc.) will be crucial to getting good performance. 

We will also be coding in higher-level languages which, unlike C, will probably not expose as much low-level control over the machine to us. We will need to make use of our 418 knowledge to coax these high-level languages into giving us good performance.

## Resources

We will do initial testing on the 20 core unix machines and the Gates machines. 

Our “starter code” consists of the two reference implementations that Blelloch’s team provided. We will need to modify the given C++ code or install gcc 4.9 in order to get it to run on the GHC machines though. 

There is also substantial existing literature on parallel sorting algorithms. We will refer to those such as this paper on [Robust Massively Parallel Sorting](http://epubs.siam.org/doi/pdf/10.1137/1.9781611974768.7).

We will need access to the Xeon Phi machines in addition to the four days’ worth of Aware access that Blelloch’s team will give us.

## Goals and Deliverables

### Plan to achieve
* Analyse the performance of the C++ solution that they provided - why does it perform the way it does? Has it approached theoretical peak performance, and if not, what is slowing it down?
* Implement a semi-optimized parallel sorting algorithms in at least 2 garbage-collected languages (currently thinking Java, Haskell and Go) and analyse their performance. This will give us a rough idea of how the various languages compare in terms of:
  * Raw performance
  * How much control the languages provide the programmer over the low-level performance-determining details.
* Implement a highly optimized sorting algorithm on one chosen language. This would constitute our main submission to the sorting contest.
* The concrete goal is to make it run faster than their SML solution.

### Hope to achieve
* Implement an automated way of searching the parameter space (autotuning). This will allow us to make good use of the 4 days we have on the Aware machine.
* Highly optimize for more than one language.
* Implement a parallel sort in CUDA too.

### Planned Demo
* Performance comparison graphs between our optimized code and the baseline on different machines.

## Platform Choice

The platform has been already been determined for us by the rules of the contest, but we will choose a couple of the programming languages that we will use such as C++, Go, Java, Haskell, and possibly others depending on how well they are able to support SIMD instructions on Intel's chips.

We may also try getting a version to work on CUDA as stated in our stretch goals because sorting is a bandwidth-intensive problem, and GPUs have much higher bandwidth currently compared to CPUs.

## Schedule

| Week            | Tasks         |
| --------------- | ------------- |
| Apr 10 - Apr 16 | Review literature on parallel sorting algorithms <br/> Get the provided C++ code to compile and run on GHC machines <br/> Analyse the performance of the provided C++ code <br/> Choose at least 2 garbage-collected languages and get them working on the GHC machines (set up the environment so we are able to compile and run parallel code) |
| Apr 17 - Apr 23 | Implement first attempt at parallel sorting in the garbage-collected languages of our choice <br/> Analyse initial results from the first attempt and choose a language to focus on for our main contest submission <br/> Instrument code, use profiling tools, etc. to obtain deeper insight into the performance of the code we wrote in the previous week <br/> Submit our progress so far to Guy Blelloch’s team in order to request access to Aware machine |
| Apr 24 - Apr 30 | Do write-up for project checkpoint (due Apr 25) <br/> Focus on optimizing the parallel sort in the language of our choice <br/> Test and optimize on the Aware machine | 
| Apr 31 - May 5  | Optimize more <br/> Submit to the 210 sorting competition (due May 4) |
| May 6 - May 12  | Prepare for final presentation |

# Lecture 1 Introduction

What is a distributed system?

* multiple cooperating computers
* storage for big websites, bigdata computation such as MapReduce, p2p file sharing
* a lot of critical infrastructure out there is built out of distributed systems, infrastructure that requires more than one computer to get its job done or it's sort of inherently needs to be spread out physically

But

* you should try everything else before you try building distributed systems because they're not simpler

Why distributed system?

* some sort of parallelism
* fault tolerance
* physical reasons
* security | isolation

Challenges

* concurrent, many concurrent parts
* partial failures, unexpected partial failures over networks
* performance, tricky to speed up thousand servers

Why take this course?

* academic curiosity to driven by giant websites
* very important computing infrastructure
* lab sequence, focus on performance and fault tolerance

Course structure

* teacher, Robert Morris
* couple of components, lectures, papers, two exams, four labs, project (optional)
* papers one read per work

Today paper to read: [MapReduce](mapreduce.pdf)

Learn from papers

* ideas
* implementation details
* evaluations

Labs

* Lab1 MapReduce
* Lab2 Raft for fault tolerance
* Lab3 K/V Server
* Lab4 Sharded K/V Servers
* Or without lab4, you could do project

there are tests, if all tests passed, you would got four score.

Questions

* Lab is the single important

Infrastructure

* Storage
* Communication
* Computation

Goal

* Discover abstractions where use of simplifying the interface to these storage and computation infra

Topics

* Implementation, RPC, Threads, Concurrency Control
* Performance, Scalability (2x computers, 2x throughput)
* Fault-Tolerance
    * Availability, built with failure to design
    * Recoverability, could continue to work and keep correctness after back
    * Tools
        * Non-Volatile Storage, expensive to update, should try to avoid writes
        * Replication, replicas may drift out-of-sync
* Consistency
    * Put(k,v)
    * Get(k) -> v
    * may have different versions across replicas
    * Strong system guarantee the consistency, Weak allows stale read
    * strong consistency is expensive
    * bad idea to put replicas into same rack, same machines
    * structure weak guarantee is useful in some applications with high performance

MapReduce

* Huge computation, for indexing web content or analyzing the link structure of the entire web to identify most important pages
* Off computation, and get data back
* To have a framework, run giant distributed computation
* Write a simple map function, and a reduce function, without know the framework details
* Input 1, Input 2, Input 3 -> Map function (parallelism), output and collect items to reduce -> Reduce function
* Whole computation is a Job, invocation for mapreduce called a Task
* For example, key:filename and value:filecontent, the map function could split the content into words, and reduce function could be called with all reduced keys, and emit the result

Questions

* In real world, a MapReduce job output could be another job's input
* Shortcoming is it requires to be function pure
* Master will organize the workers and assign tasks to them
* Reduce worker will fetch all the results, and emit the output to a cluster service
* The input is from GFS, it split up big files across servers with each 64k chunk
* in 2004, the most constraining bottleneck in their MapReduce system was network throughput, for example, the central switch, 50mbit/sec per machine
* MapReduce will cleverly send task to Map worker which already stores the input data in local
* The row and the column, moving the piece of data, fetch them into a single machine to do reduce, is most expensive job in MapReduce
* Output is also stored in GFS
* in 2020, the root switch is NOT ONLY a single machine anymore, modern data center has far more network throughput

Lab 1 [MapReduce](6.824 Lab 1_ MapReduce.pdf).

---
title: "CMU 15-445 Database Systems (Fall 2021)"
categories: Database
toc: true
toc_label: ""
toc_sticky: true
---

# Introduction

This is a write-up for projects in the Database Systems course of CMU. In this course, we implemented several features of a database management system (DBMS), such as Buffer Pool Manager, Index, Query Execution,... According to the policy on academic integrity of this course, I will keep the course repository private.

# Project 0: C++ Primer

This project is meant to show how to set up the working environment (use Git, CMake, GoogleTest framework, etc. ) and how to write modern C++ (C++11/14/17). The project is quite simple, but it requires a good understanding of object-oriented programming concepts in C++, including base class and derived class, virtual functions, and inheritance,...

# Project 1: Buffer Pool

In this course, we will implement some features for the BusTub database management system (an educational DBMS created by CMU). This is a disk-oriented database, so in this project, we will implement a buffer pool manager for it instead of using `mmap` (a function in C), which gives all the control to the operating system. The buffer pool is a storage manager that is responsible for moving physical pages back and forth from the main memory to the disk. In addition, the buffer pool manager must support multiple threads accessing it concurrently by using latches to protect critical sections of internal data structures.

## Task 1: LRU Replacement Policy

There are two common data structures for implementing LRU policy: an array with a global counter (keep track of the time an object is accessed) and a queue based on a linked list.

I chose to use a queue because I don't want to introduce a global variable, which requires using a mutex to protect access to shared global variables. Using a queue can take advantage of its First In First Out (FIFO) property, which is the same as the LRU policy. Recently accessed objects will be at the head of the queue, whereas objects that have not been accessed for a long time will be at the end of the queue. Hence, we can delete those elements in constant time when we want to evict them to make room for other objects.

## Task 2: Buffer Pool Manager Instance

Write later

## Task 3: Parallel Buffer Pool Manager

Write later
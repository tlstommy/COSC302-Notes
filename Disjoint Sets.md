
# Disjoint Sets
lecture notes link: http://web.eecs.utk.edu/~jplank/plank/classes/cs302/Notes/Disjoint/

When a node has a NULL link, we call it the "root" of a set. If you call Find() on a node with a NULL link, it will return the node's item number, and that is the set id of the node. Therefore, when you first start, every node is the root of its own set, and when you call Find(i), it will return i.

When you call Union(i, j), remember that i and j must be set id's. Therefore, they must be nodes with NULL links. What you do is have one of those nodes set its link to the other node.


Union is pretty simple, too, but you have some choices about how to determine which node sets its link field to the other. We use three methods to do this:

- Union by size: Make the node whose set size is smaller point to the node whose set size is bigger. Break ties arbitrarily.

- Union by height: The height of a set is the longest number of links that it takes to get from one of its items to the root. With union by height, you make the node whose height is smaller point to the node whose height is bigger.

- Union by rank with path compression: This works exactly like union by height, except you don't really know the height of the set. Instead, you use the set's rank, which is what its height would be, were you using union by height. The reason that you don't know the height is that when you call Find(i) on an element, you perform "path compression." What this does is set the link of all non-root nodes from the path from i to the root, to the root.

---
# Running Times

We assume the number of items in the instance of disjoint sets is n:
- Initializing an instance of disjoint sets: O(n)
- Performing Union() in any of the implementations, size height or Rank with path compression: O(1)

- Performing Find() in Union by size or height: O(log(n)) N is number of elements
- Performing Find() in Union by rank with path compression: O(α(n)), where α(n) is the inverse of Ackerman's function. 

While you cannot say that Find() is constant time, for all practical purposes, it is.

Ackerman function:
```
A = ackerman function 
A(0) = 1
A(1) = 1
A(2) = 2 ^ 2 = 4
A(3) = 3 ^ 3 ^ 3  = 729

```

---

# C++ Implementation

The API for disjoint sets is pretty minimal. In C++, I use an Interface to define the class, because I am going to have three separate implementations. Each one of those is in its own subclass.
- A constructor takes the number of elements in the set. Each element starts off in its own set, whose set id equals the element.

- The Find(e) operator returns the set id for element e. This can be any integer. However, if two elements are in the same set, than calling Find() on them will yield the same result.

- The Union(s1, s2) operator takes two set ids, and merges all elements in the two sets so that they are in one set. It returns the set id of the new set. Calling Find() on any element in the newly merged set will return this set id.

- Print(): This shows the internal state of the data structure.

```
/* disjoint.hpp
   Interface and subclass specification for disjoint sets.
   James S. Plank
   Tue Sep 25 15:48:18 EDT 2018
 */

#pragma once
#include <vector>

/* The Disjoint Set API is implemented as a c++ interface, 
   because I am implementing it three ways.  Each subclass
   implementation is in its own cpp file. */

class DisjointSet {
  public:
    virtual ~DisjointSet() {};
    virtual int Union(int s1, int s2) = 0;
    virtual int Find(int element) = 0;  
    virtual void Print() const = 0;
};

/* The first subclass implements Union-by-Size. */

class DisjointSetBySize : public DisjointSet {
  public:
    DisjointSetBySize(int nelements);
    int Union(int s1, int s2);
    int Find(int element); 
    void Print() const ;

  protected:
    std::vector <int> links;
    std::vector <int> sizes;
};
/* The second subclass implements Union-by-Height. */

class DisjointSetByHeight : public DisjointSet {
  public:
    DisjointSetByHeight(int nelements);
    int Union(int s1, int s2);
    int Find(int element); 
    void Print() const;

  protected:
    std::vector <int> links;
    std::vector <int> heights;
};

/* The third subclass implements 
  Union-by-Rank with path compression. */

class DisjointSetByRankWPC : public DisjointSet {
  public:
    DisjointSetByRankWPC(int nelements);
    int Union(int s1, int s2);
    int Find(int element); 
    void Print() const;

  protected:
    std::vector <int> links;
    std::vector <int> ranks;
};
```
Find(e)

Keeps running untile the link is -1
```
int Disjoint::Find(int element)
{
    while (links[element] != -1) element = links[element];
    return element;
}
```

disjointSetsBySize

```
DisjointSetBySize::DisjointSetBySize(int nelements)
{
    //each element is in its own set, so all links are -1 and all sizes are 1
    links.resize(nelements, -1);
    sizes.resize(nelements, 1);
}
```

Union(s1,s2)

first checks to make sure that the set id's are valid, and then chooses a parent and a child from s1 and s2. The parent will be the one with the bigger of the two sets. It changes the link field of the child to point to the parent, and then it updates the size of the parent in the sizes vector:

```
int Disjoint::Union(int s1, int s2)
{
    int p, c;

    if (links[s1] != -1 || links[s2] != -1) {
        cerr << "Must call union on a set, and not just an element.\n";
        exit(1);
    }
    if (sizes[s1] > sizes[s2]) {
        p = s1;
        c = s2;
    } else {
        p = s2;
        c = s1;
    }
    links[c] = p;
    sizes[p] += sizes[c];    /* HERE */
    return p;
}
```

---
# more notes

- union by size vs union by height is basically the same but you switch height with size and vice versa

- When calling union, the one with the larger height becomes the parent!

- When you call Union(i, j), remember that i and j must be set id's. Therefore, they must be nodes with NULL links. What you do is have one of those nodes set its link to the other node.






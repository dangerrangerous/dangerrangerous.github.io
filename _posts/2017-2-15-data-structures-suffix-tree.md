---
layout: post
comments: true
title: Data Structures&#58 Ukkonen's Suffix Tree
---

### How to find a repeating pattern in a string
First, a brief history of how this blog post came to be. I was recently tasked
with finding repeating patterns in a string. I began to attack the problem
with sheer brute force. <!--excerpt.start-->It was easy enough to find repeating characters, but
what about patterns like 1234512345 where 12345 is repeated? Suddenly brute force became a much less
attractive option, and I really didn't want to bother with Regex. Having done
some work with Microsoft's System.Speech voice recognition as well as
researching Pocket Sphinx, I knew that trees were used to build vocabularies for things like auto-completion.
I thought, "maybe a tree could hold all of the different possible patterns..."
a quick search and voila, turns out that this problem is pretty close to what
is referred to as The Longest Repeated Substring problem, and we can use a
Suffix Tree to solve this problem. Not only that, a dude named Ukkonen came
up with an algorithm to make this tree 'online' and run in linear time. Rad. Next thing I know I'm in way too deep over my head but I had [gone too far](https://www.youtube.com/watch?v=la_k_r61ZlA) to turn back!

If it wasn't for [Anurag Singh's](http://www.geeksforgeeks.org/ukkonens-suffix-tree-construction-part-1/) article and source code I would not have been able to implement
this, so thanks! For a step-by-step walk through with images, check out [Ukkonen's suffix tree in plain english](http://stackoverflow.com/questions/9452701/ukkonens-suffix-tree-algorithm-in-plain-english) on stack overflow.

### Get Pumped
If you are familiar with trees as a data structure, this should be business as
usual. If this is your first tree, you may want to check out the book, [Data
Structures and Algorithm Analysis in C++](http://www.uoitc.edu.iq/images/documents/informatics-institute/Competitive_exam/DataStructures.pdf) by Mark Allen Weiss. Fear not my friends,
only through battles with the unfamiliar do we achieve growth. [Fear is the mind-killer](https://www.youtube.com/watch?v=kJsYKhEV6o0).

### Basics of Constructing the Suffix Tree
Our suffix tree T for an n-character string will have n leaves numbered 1 to n.
For our example we will start with a string S with a length 6 (n=6). S = abc123
I know there's no pattern yet, but our second string will be abc123abc123.
Some Rules:
* We will label each edge with a nonempty substring of S.
* No two edges coming out of the same node can have the same edge label.
* To avoid implicit suffixes - suffixes that don't end at a leaf but should -
we will append a termination character $.

The naive approach of building the suffix tree will take O(n<sup>2</sup>) time which is
why we are using Ukkonnen's algorithm. If you don't know what that means, you're
in way over your head, awesome! Basically a two character string will take time of 4 to compute,
3 will take 9 time units, and 4 will take 16 time units etc. When working with long strings this
can be a problem.

### Basics of Ukkonen's algorithm
The algorithm constructs implicit suffix trees for each character. T1 for n1, T2 for n2 etc.
Each suffix tree is built on top of the previous, T2 on top of T1. Once we reach the last
character and have implicit tree Tn, we build the true suffix tree from Tn by appending $.
Ukonnen's algorithm builds so long as it is fed characters, so it is considered online.
It accomplishes this in O(n) time. I imagine this would be very useful for streams.

Ukkonen's algorithm works in phases. One phase for each character in the string with length n.
Each phase is further divided into extensions. So each phase i+1 has extensions j+1, one extension
for each of the i+1 suffixes of our string S[1...i+1]. In phase i+1, tree Ti+1 is built from
tree [Ti](https://www.youtube.com/watch?v=ujqQ6x2hASg). Take a deep breath. We are essentially walking one step at a time building from our
previous phase. For string abc the results will be first edge = abc, second edge = bc, third edge = c. Starting with a in our first edge. Adding b to our first edge to have ab, and adding a second edge to contain b. Can you can guess the next step?

There are 3 rules for extensions:
* Rule 1: If the path from the root labelled S[j..i] end at a leaf edge, add character S[i+1] to the end of the label
on that leaf edge.
* Rule 2: If the path from the root ends at a non-leaf edge, i.e. there are more characters on the path, and the next character is not S[i+1], then create a new leaf edge with label s[i+1] and number j is created
from character S[i+1]. A new internal node will also be created if we end inside a non-leaf edge
* Rule 3: If the path from the root ends at a non-leaf edge and the next character is s[i+1] which
is already in the tree, do nothing.

Note: There will not be more than one edge going out of any node starting with the same character.

In short, we have a Suffix Tree of string S with length n, there are n phases and for a phase
j(1 <= j <= n), we add the j-th character via j extensions following one of the 3 rules. Our goal
is to use Ukkonnen's algorithm to get this to build in O(n) time.

### Suffix links
![fig15](/public/Fig15.jpg){:class="img-responsive"}
Notice the red line moving from all paths labeled 'c'. A pointer from a substring node with path
label 'c' to another node with path label c is called a suffix link. If 'c' is an empty string, the suffix
link from the internal node will go to root.

Suffix links provide a shortcut to find the end of the path. For a more in-depth explanation, see [Ukonnen's suffix tree part 2](http://www.geeksforgeeks.org/ukkonens-suffix-tree-construction-part-2/)

### Trick 1: Skip/Count
If the number of characters on the edge is less than the number of characters we need to travel to get from node to leaf, we can directly skip to the next node. If the number of characters is more than the number of characters we need to travel, we can skip directly to the character on that edge.
This has the effect of doing the walk down proportional to the number of nodes rather than characters.
This allows us to build in O(n<sup>2</sup>).

### Edge-label compression
Because the path labels are represented as characters in a string, the suffix tree will take O(n<sup>2</sup>) space to store the path labels. We can replace these substrings with two (start,end) indices pairs on each edge, reducing space requirements to O(n).

### Observations and Tricks
* Trick 2: As soon as rule 3 applies in any extension of a phase no more work needs to be done. If a new internal node v gets created in an extension j and rule 3 applies in the next extension j+1, then we need to add a suffix link from node v to current node (if we are on an internal node) or root node. We will create something called ActiveNode to help us set suffix links.
* Trick 3: Create a global index e to replace the end position i of all leaf edges and set e to equal the phase number. Now in any phase we can just increment e and the extension on all leaf edges will be done.

Using tricks 1-3, a suffix tree can be built in linear time! If you want to walk through the algorithm step by step, I will once again defer to [Anurag Singh's](http://www.geeksforgeeks.org/ukkonens-suffix-tree-construction-part-4/) article.

We will work through the code in Part 2 of this Data Structure series on Ukkonnen Suffix Trees. Until then, take it easy.

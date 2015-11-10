# Hashing <a name="hashing"></a> [Instructor Notes](https://usu.instructure.com/courses/388377/files/58790856/download?wrap=1)

|Hashing|Concepts|
|---|---|
|[Hash Function](#hash-function)|Hash Function|
|[Collisions](#collisions)|Linear Probing|
||Clustering|
||Quadratic Probing|



## Hash Function <a name="hash-function"></a>


> This function is called a hash function because it “makes hash” of its inputs

A hash function is a function that:  
- When applied to an Object, returns a number- When applied to equal Objects, returns the same number for each- When applied to unequal Objects, is very unlikely to return the same number for each
- This function has no other purposeHash function with sliding bits and exclusive or

~~~cpp
unsigned int  hash(string key){unsigned int hashVal=0;   for (i=0; i < key.length(); i++)      hashVal =  (hashVal << 5) ^ key[i]  ^ hashVal;   return hashVal %TABLESIZE;}~~~

__Properties of a good hash function:__  - Minimize collisions  - Fast to compute  - Scatter data evenly through hash table. Data may have patterns to them.  - Uses all bits of the key – generally helps in scattering non-random data  - If mod is used, base should be prime  

## Collisions <a name="hash-collisions"></a>

>When two values hash to the same array location, this is called a collision- Solution #1: Search from desired spot for an empty location  (with wrap)- Solution #2: Try again with a second hash function...and a third, and a fourth, and a fifth, ... to find a better location.- Solution #3: Create a linked list of values that hash to this location

### Linear Probing

> Use a vacant spot in table following HASH(key) – “open” addressing means that we find an unused address and use it. Evaluation - Longer search times - Clustering gets worse and worse (snowballs).  Primary clustering is when two keys that hash onto different values compete for same locations in successive hashes. - Better if successive probe locations are picked in “scrambled” order to lessen clustering.
- One problem with the “linear probing” is the tendency to form “clusters”

### Clustering
> A cluster is a group of items not containing any open slots- The bigger a cluster gets, the more likely it is that new values will hash into the cluster, and make it ever bigger- Clusters cause efficiency to degrade

- Primary Clustering
> is the tendency for certain open-addressing hash tables collision resolution schemes to create long sequences of filled slots.

- Secondary Clustering
> when different keys that hash to same locations compete for successive hash locations.

### Quadratic Probing

- Quadratic probing eliminates primary clustering, but not secondary clustering. __Note:__ For both linear and quadratic probing, the sequences checked are key independent.Two different keys which hash to same location, keep competing.### Double Hashing> Have two functions, Step gives the increment for Hash. - Define: Step(key) = 1 + key % (TABLESIZE - 2) – gives personalized increment. - Notice, the location of Step is never used directly as the hash value. It is only an auxiliary function. - If TABLESIZE and TABLESIZE–2 are primes, it works better. - Hash(key) = key % TABLESIZE - If Hash(key) is full, successively add increment as specified by Step(key). - __example:__ for key = 38 and TABLESIZE = 13
- - Each of the probe sequences visits all the table locations if the size of the table and the size of the increment are relatively prime with respect to each other. ### Bucket <a name="collisions-bucket"></a>
>Another way to handle collisions is to have each entry in the hash table hold more than one (B) item. - Each entry is called a bucket. - This doesn’t really solve the problem. As soon as the bucket fills up, you still have to deal with collisions. - I think it would be better to just have the original table B times as large. ### Separate Chaining <a name="collisions-separate-chaining"></a>
>Each location in the hash table is a pointer to a linked list of things that hashed to that location. __Advantages:__  
- Saves space if records are long  - Collisions are no problem   - Overflow is solved as space is dynamic   - Deletion is easy  
__Disadvantages:__  
- Links require space   - Following linked list is more time consuming

>Works well, but we have the overhead of pointers. ## Rehash <a name="hash-rehash"></a>
>Rehash – create a larger  (or smaller) array.  Rehash all old elements into new array.## Efficiency <a name="hash-efficiency"></a>
> Hash tables are actually surprisingly efficient- Until the table is about 70% full, the number of probes (places looked at in the table) is typically only 2 or 3- Sophisticated mathematical analysis is required to prove that the expected cost of inserting into a hash table, or looking something up in the hash table, is O(1)- Even if the table is nearly full (leading to occasional long searches), efficiency is usually still quite high- load factor = number-of-elements / TABLESIZE - Should not exceed 2/3. - With chaining, the concern is the average size of a chain. ## Deletions <a name="hash-deletions"></a>
 > __solution:__ Use lazy deletions  
 
## Cuckoo Hashing <a name="hash-cuckoo"></a>
>Cuckoo hashing  worst-case constant lookup time. >The name derives from the behavior of some species of cuckoo, where the cuckoo chick pushes the other eggs or young out of the nest when it hatches; analogously, inserting a new key into a cuckoo hashing table may push an older key to a different location in the table.- There are two hash functions, one for each table.- If a key can’t go in either location, it tries to move the existing key to its alternative spot in the other table.- When a new key is inserted, the new key is inserted in one of its two possible locations, "kicking out", that is, displacing, any key that might already reside in this location. - This displaced key is then inserted in its alternative location, again kicking out any key that might reside there, until a vacant position is found, or the procedure enters an __infinite loop__. In the latter case, the __hash table__ is rebuilt __in-place__ using new __hash functions__:
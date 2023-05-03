Download Link: https://assignmentchef.com/product/solved-cs-1410-project-6-dynamic-arrays
<br>
11.14 Programming Project #6:Dynamic ArraysBackgroundEarlier in the semester, you studied the concept of a vector. As you recall, a vectoracts very much like an array, except that a vector automatically grows to hold moredata as needed. In this project, you will create a class that works like a vector ofintegers.An important element in the design of a vector is the use of a private, dynamicallyallocated array, via a pointer that keeps track of where the array is in memory. As youcontinue your training as a programmer, you will need to master the use of dynamicallyallocated storage and pointers.Important note: Just to be clear, you may not use std::vector to do any of this —you are creating your own vector class! You will write code that constitutes thebeginning of the real vector class in the C++ library (except that DynArray will not begeneric—it will only hold integers). We will use the original name used by the C++Standards Committee before they changed it to vector: DynArray (for “dynamic array”,pronounced DINE-array).ObjectiveTo gain confidence in the use of pointers and dynamically allocated storage.The DynArray ClassFor efficiency of operation, DynArray has two notions of size:• the current capacity, which is the size of the allocated array in the heap• the actual size, which is the number of elements in useIf these values are the same, then the array is full and needs to be made larger beforeit can add more items. This is done by:• allocating a new, larger array (typically 1.5-to-2 times the current capacity)• copying the currently used data to the first “size” locations of the new array• deleting the old array, thus returning its memory to the heap• having your internal pointer point to the new heap arrayIn order to operate like a true vector, your class must contain at least the followingfunctions:1. A default constructor that creates a DynArray with a default capacity of 2.Its size will initially be zero. Remember that size refers to the number ofelements currently stored by the user in the DynArray.2. A parameterized constructor receiving an integer, n, that creates a DynArray ofcapacity n. Its size will initially be zero.3. A destructor that deletes any dynamically allocated storage. This prevents amemory leak.4. A function size() that returns the current size of your DynArray instance. Thesize will change as integer values are added or removed.5. A function capacity() that returns the current allocated capacity of theDynArray. The capacity is defined as the number of integer values that can bestored in the DynArray instance. The capacity changes when the underlyingarray grows.6. A function clear() that returns the dynamic memory to the heap, and resets itssize to zero and capacity to the default of two, allocating fresh heap memory.7. A function push_back(int n) that adds the integer value n to the end of theDynArray. If it is not large enough to hold this additional value, you mustincrease the size of the backing array, as explained above.8. A function pop_back(), that decrements the size of the DynArray by 1. Nochange is made to the allocation.9. A function at(int n) that returns the value of the integer stored at index n ofthe DynArray. If the index is outside the range of the vector (no element at thatindex), this function should throw a std::runtime_error with an appropriatemessage.Vector InternalsIn order to make a DynArray instance grow, you will need to use dynamically allocatedstorage. Vectors actually store their data in a array allocated on the heap. You will storea pointer to this array in an instance of the DynArray class.You will also need to track the size of the allocated array (its capacity, which is thenumber of ints it can hold altogether), and the size, which is the number of elements inuse by the user of the DynArray object (element in positions 0 through size – 1). Eachtime that you add a new element to the vector, its size increases by one.The figure below shows a DynArray object and its associated dynamically allocatedarray of integers.

<img decoding="async" data-recalc-dims="1" data-src="https://i0.wp.com/www.ankitcodinghub.com/wp-content/uploads/2020/03/912.png?w=980&amp;ssl=1" class="lazyload" src="data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==">

 <noscript>

  <img decoding="async" src="https://i0.wp.com/www.ankitcodinghub.com/wp-content/uploads/2020/03/912.png?w=980&amp;ssl=1" data-recalc-dims="1">

 </noscript>Values are inserted into the vector using the push_back function. Since we alwaysknow the size of a vector, it is easy to calculate where in the array we store the valuethat is being pushed-back (it will be in position size). Consider, for example, a vectorwhose capacity is currently 5, and whose size is 3. This means that we have an arraythat can hold 5 integer values, but we have only stored 3 values in the array so far.These values are stored in the array at index 0, index 1, and index 2. There are actuallyvalues stored at the other indices of the array, but this is garbage. We show this with adash. This is illustrated in the figure below.

<img decoding="async" data-recalc-dims="1" data-src="https://i0.wp.com/www.ankitcodinghub.com/wp-content/uploads/2020/03/131.png?w=980&amp;ssl=1" class="lazyload" src="data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==">

 <noscript>

  <img decoding="async" src="https://i0.wp.com/www.ankitcodinghub.com/wp-content/uploads/2020/03/131.png?w=980&amp;ssl=1" data-recalc-dims="1">

 </noscript>If we now do a push_back(91), we just store the value 91 in the next available slot inthe array, which is at index 3. Now we increment the index. The capacity does notchange.Growing the Vector takes more work. What do we do when the vector is full? Here isthe process:1. Dynamically allocate a new array of integers that is somewhat larger than theexisting array. An algorithm that is often used is to double the size of the array.2. Copy all of the numbers from the first array into the second, in sequence.3. Add the new value at the next open slot in the new array (size).4. Change capacity to be the capacity of the new array.5. Increment size6. Delete the original array.7. Change the data pointer to now point to the new array.This is illustrated in the following figure:

<img decoding="async" data-recalc-dims="1" data-src="https://i0.wp.com/www.ankitcodinghub.com/wp-content/uploads/2020/03/967.png?w=980&amp;ssl=1" class="lazyload" src="data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==">

 <noscript>

  <img decoding="async" src="https://i0.wp.com/www.ankitcodinghub.com/wp-content/uploads/2020/03/967.png?w=980&amp;ssl=1" data-recalc-dims="1">

 </noscript>DestructorsDestructors are use to gracefully destroy class instances. If a class instance has apointer that refers to heap memory, then the dsestructor should return that memory tothe heap by calling delete on the pointer. Destructors are called automatically whenan object goes out of scope (like when execution exits a {}-block or returns from afunction). Failing to do this makes the memory unavailable and is called a memoryleak. It is good practice to design classes to be self-managing: constructors place newobjects in a valid state, and destructors free any dynamic resources. If a class uses nodynamic resources (such as memory, network connections, etc.), then a destructorprobably isn’t needed.The driver for this project is provided for you and does the following:1. Create an empty vector2. Add three values to the vector in this order: 21,32,41 using push_back3. Test at() to verify that it catches an attempt to access a position out of bounds4. Clear the vector5. Add 12 values to the vector from 0 to 11 using push_back6. Output the size of the vector7. Output the capacity of the vector8. Display the values in the vectorHere is the expected output using the main.cpp provided for you:capacity = 2Pushing three values into samThe values in sam are: 21 31 41 invalid index————–sam has been cleared.Sam’s size is now 0Sam’s capacity is now 2—————Push 12 values into sam.Sam’s size is now 12Sam’s capacity is now 16—————Test to see if contents are correct…0 1 2 3 4 5 6 7 8 9 10 11————–Test pop_back…0 1 2 3 4 5 6 7 8 9 10
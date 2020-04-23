# NumaArray

Implementation of three arrays which are transparently distributed onto multiple NUMA nodes using [Silo](https://github.com/stanford-mast/Silo), i.e., the i'th stripe of the array is allocated on NUMA node i. The arrays adpots and refines the [NUMA array](https://gist.github.com/lorenzhs/0a2a67b669779ab7a60e34fa0c566227) from Lorenz Hübschle-Schneider.

1. An array, stored in a unique pointer.
2. An array wrapped in a simple array class.
3. An aligned array.

Additionally, transparent huge pages and a fallback for non-NUMA systems are used. The arrays require libnuma, Silo and [Topo](https://github.com/stanford-mast/Topo).

## Usage

Add and link the NUMA array library.
```Cmake
# Add the repository to your subdirectories
add_subdirectory(your-path/numa_array/include)
# link library
target_link_libraries(your-application PRIVATE numa_array)
```

Use the array in our code.
```C++
// Include the library
#include <numa_array.hpp>

// Create and use the array
size_t size = 1000000;
// Alignment of 16 bytes
size_t alignment = 16;

Numa::AlignedArray<T> aligned_arr(size, alignment);

Numa::Array<T> arr(size);

auto uptr_arr = Numa::make_numa_arr(size);
```

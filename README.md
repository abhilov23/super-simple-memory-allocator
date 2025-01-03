
# Super Simple Memory Allocator

This repository contains a basic memory allocator implemented in C, providing custom `malloc` and `free` functions for dynamic memory management. The allocator uses a simple free list to manage memory blocks, demonstrating fundamental concepts of memory allocation in C.

## Features

- **Custom `malloc` (`ss_malloc`)**: Allocates memory blocks of specified sizes during runtime.
- **Custom `free` (`ss_free`)**: Deallocates previously allocated memory blocks to prevent memory leaks.
- **Simple Free List Management**: Maintains a free list to keep track of available memory blocks.

## Implementation Details

The allocator is implemented in `super-simple-handler.c` and provides two main functions: `ss_malloc` and `ss_free`.

### `ss_malloc(size_t size)`

This function allocates a memory block of at least `size` bytes. It searches the free list for a suitable block using a first-fit strategy. If a suitable block is found, it is split if necessary, and the remaining part is added back to the free list. If no suitable block is found, the function extends the heap using `sbrk`.

**Key Steps**:

1. **Alignment**: The requested size is aligned to the nearest multiple of `ALIGNMENT` to ensure proper memory alignment.
2. **Search Free List**: The free list is traversed to find the first block that is large enough to satisfy the request.
3. **Split Block**: If the found block is larger than needed, it is split into two blocks: one to satisfy the allocation and the other to remain in the free list.
4. **Extend Heap**: If no suitable block is found, the heap is extended by calling `sbrk`.

### `ss_free(void *ptr)`

This function deallocates a previously allocated memory block pointed to by `ptr`. The block is added back to the free list and adjacent free blocks are coalesced to form larger blocks, reducing fragmentation.

**Key Steps**:

1. **Add to Free List**: The block is added to the beginning of the free list.
2. **Coalesce Adjacent Blocks**: The free list is traversed to merge adjacent free blocks into larger blocks.

## Usage

To use the custom memory allocator in your project:

1. **Include the Header**: Include the header file in your source code.

   ```c
   #include "super-simple-handler.h"
   ```

2. **Compile the Source**: Compile `super-simple-handler.c` along with your project files.

   ```bash
   gcc -o my_program my_program.c super-simple-handler.c
   ```

3. **Allocate and Free Memory**: Use `ss_malloc` and `ss_free` for dynamic memory management.

   ```c
   int *array = (int *)ss_malloc(10 * sizeof(int));
   // Use the array
   ss_free(array);
   ```

## Limitations

- **Thread Safety**: The allocator is not thread-safe and should be used in single-threaded environments.
- **Error Handling**: Minimal error handling is implemented; for example, `ss_malloc` returns `NULL` if allocation fails.
- **Performance**: The simple first-fit strategy and lack of advanced optimizations may lead to fragmentation and reduced performance for certain workloads.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Acknowledgments

This project is inspired by educational resources on memory allocation in C, such as [Memory Allocators 101 - Write a simple memory allocator](https://arjunsreedharan.org/post/148675821737/memory-allocators-101-write-a-simple-memory).


#define M61_DISABLE 1
#include "m61.h"
#include <stdlib.h>
#include <string.h>
#include <stdio.h>
#include <inttypes.h>
#include <assert.h>


// variables feed struct
int num_malloc = 0;
int num_free = 0;
int fail_al = 0;
int t_al_sz = 0;
char* max = NULL; 

int total_size = 0;
int active_size = 0;
int fail_size = 0;

struct node* head = NULL;

#define MAX_ADDRS 0  



/// m61_malloc(sz, file, line)
///    Return a pointer to `sz` bytes of newly-allocated dynamic memory.
///    The memory is not initialized. If `sz == 0`, then m61_malloc may
///    either return NULL or a unique, newly-allocated pointer value.
///    The allocation request was at location `file`:`line`.

void* m61_malloc(size_t sz, const char* file, int line) {
    (void) file, (void) line;   // avoid uninitialized variable warnings

    // Check for active allocation. if sz==0 doesnt count as an allocation.  
    
    if(sz==0 || (int)sz<0) 
    {
    	fail_al++;
	fail_size+= sz;
    }
    num_malloc ++; 
    t_al_sz+= sz;
    total_size+=sz;
    active_size+=sz;
    push(&head, 1, sz);
   
    return base_malloc(sz);
}


/// m61_free(ptr, file, line)
///    Free the memory space pointed to by `ptr`, which must have been
///    returned by a previous call to m61_malloc and friends. If
///    `ptr == NULL`, does nothing. The free was called at location
///    `file`:`line`.

void m61_free(void *ptr, const char *file, int line) {
    (void) file, (void) line;   // avoid uninitialized variable warnings
    num_free++;
    base_free(ptr);
}


/// m61_realloc(ptr, sz, file, line)
///    Reallocate the dynamic memory pointed to by `ptr` to hold at least
///    `sz` bytes, returning a pointer to the new block. If `ptr` is NULL,
///    behaves like `m61_malloc(sz, file, line)`. If `sz` is 0, behaves
///    like `m61_free(ptr, file, line)`. The allocation request was at
///    location `file`:`line`.

void* m61_realloc(void* ptr, size_t sz, const char* file, int line) {
    void* new_ptr = NULL;
    if (sz)
        new_ptr = m61_malloc(sz, file, line);
    if (ptr && new_ptr) {
        // Copy the data from `ptr` into `new_ptr`.
        // To do that, I must figure out the size of allocation `ptr`.
    }
    m61_free(ptr, file, line);
    return new_ptr;
}


/// m61_calloc(nmemb, sz, file, line)
///    Return a pointer to newly-allocated dynamic memory big enough to
///    hold an array of `nmemb` elements of `sz` bytes each. The memory
///    is initialized to zero. If `sz == 0`, then m61_malloc may
///    either return NULL or a unique, newly-allocated pointer value.
///    The allocation request was at location `file`:`line`.

void* m61_calloc(size_t nmemb, size_t sz, const char* file, int line) {
    // Your code here (to fix test014).
    void* ptr = m61_malloc(nmemb * sz, file, line);
    if (ptr)
        memset(ptr, 0, nmemb * sz);
    return ptr;
}


/// m61_getstatistics(stats)
///    Store the current memory statistics in `*stats`.

void m61_getstatistics(struct m61_statistics* stats) {
    // Stub: set all statistics to enormous numbers
    memset(stats, 255, sizeof(struct m61_statistics));

    int length = listSize(head);
    for(int i = 0;i<=num_free;i++){
	int position = length - i;
	deleteNode(&head, position);
   }	 

    
    stats->active_size = active_size;       // # bytes in active allocations
    stats->ntotal = num_malloc - fail_al;    // # total allocations
    stats->total_size = total_size;            // # bytes in total allocations
    stats->nfail = fail_al;                 // # failed allocation attempts
    stats->fail_size = fail_size;             // # bytes in failed alloc attempts
    stats->heap_min = 0;              // smallest allocated addr
    stats->heap_max = 0;              // largest allocated addr
    stats->nactive = stats->ntotal - num_free;     // # active allocations
}

void push(struct node** header, int allocated, size_t sz)
{
    // allocate new node
    struct node* new_node = (struct node*) malloc(sizeof(struct node));
  
    // fill with size and if allocated - allocated is boolean 1 or 0
    new_node->allocated  = allocated;
    new_node->al_size  = sz;
  
    //make head and point to new
    new_node->next = (*header);
    (*header)    = new_node;
}

void deleteNode(struct node **header, int position)
{
   if (*header == NULL)
      return;
 
   struct node* temp = *header;
 

    if (position == 0)
    {
        *header = temp->next;   // Change head
        free(temp);               // free old head
        return;
    }
 
    for (int i=0; temp!=NULL && i<position-1; i++)
         temp = temp->next;
 
    if (temp == NULL || temp->next == NULL)
         return;
 
    struct node *next = temp->next->next;

    active_size = active_size - temp->next->al_size;
    free(temp->next);
 
    temp->next = next;
}

int listSize(struct node *node)
{
  int count = 0;
  while (node != NULL)
  {
     count++;
     node = node->next;
  }
   return count;
}

void printList(struct node *node)
{
  while (node != NULL)
  {
     printf(" %d -- %zd\n", node->allocated,node->al_size);
     node = node->next;
  }
}


/// m61_printstatistics()
///    Print the current memory statistics.

void m61_printstatistics(void) {
    struct m61_statistics stats;
    m61_getstatistics(&stats);
	
    printf("malloc count: active %10llu   total %10llu   fail %10llu\n",
           stats.nactive, stats.ntotal, stats.nfail);
    printf("malloc size:  active %10llu   total %10llu   fail %10llu\n",
           stats.active_size, stats.total_size, stats.fail_size);
}


/// m61_printleakreport()
///    Print a report of all currently-active allocated blocks of dynamic
///    memory.

void m61_printleakreport(void) {
    
}

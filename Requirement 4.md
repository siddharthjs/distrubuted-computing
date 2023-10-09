# Requirement 4 Breakdown

## Detailed Explanation

**Trial Division**:

Trial division is one of the most straightforward methods for integer factorization. The principle is to test each integer (or more efficiently, just the primes) to see if it divides the number `N` without leaving a remainder.

Steps for Trial Division:
1. Start with the smallest prime number, which is `2`.
2. Check if `2` divides the number `N` without leaving a remainder.
3. If it does, then `2` is a factor of `N`, and you can divide `N` by `2`.
4. If not, try the next number.
5. Continue this process until you've found all the factors or until you reach a number that is greater than the square root of `N` because beyond that point, you won't find any new factors.

## Pseudo-code:

```plaintext
Function trial_division(N):
    Initialize a list 'factors'
    For i from 2 to square root of N:
        While N is divisible by i:
            Add i to the 'factors' list
            Divide N by i
    If N is greater than 1:
        Add N to the 'factors' list
    Return 'factors'
```

## C Implementation:

```c
#include <stdio.h>
#include <math.h>
#include <stdlib.h>

// Define a dynamic array structure to hold factors
typedef struct {
    unsigned long* items;
    int count;
    int capacity;
} DynamicArray;

// Initialize dynamic array
DynamicArray init_array() {
    DynamicArray array;
    array.count = 0;
    array.capacity = 10;
    array.items = malloc(array.capacity * sizeof(unsigned long));
    return array;
}

// Add item to dynamic array
void add_item(DynamicArray* array, unsigned long item) {
    if (array->count == array->capacity) {
        array->capacity *= 2;
        array->items = realloc(array->items, array->capacity * sizeof(unsigned long));
    }
    array->items[array->count++] = item;
}

// Factorize the number N using trial division
DynamicArray trial_division(unsigned long N) {
    DynamicArray factors = init_array();

    for (unsigned long i = 2; i <= sqrt(N); i++) {
        while (N % i == 0) {
            add_item(&factors, i);
            N /= i;
        }
    }
    if (N > 1) {
        add_item(&factors, N);
    }

    return factors;
}

int main() {
    unsigned long N = 56;  // Example number
    DynamicArray factors = trial_division(N);

    printf("Factors of %lu are: ", N);
    for (int i = 0; i < factors.count; i++) {
        printf("%lu ", factors.items[i]);
    }
    printf("\n");

    // Cleanup
    free(factors.items);

    return 0;
}
```

> **Note**: In the C implementation provided, a dynamic array structure is used to store the factors of the number. This ensures that the array can grow as needed to accommodate all the factors without having to specify a fixed size upfront. This solution finds the prime factorization of the number `N`. If `N` itself is a prime number, the only factor in the array will be `N` itself.

# Requirement 5 Breakdown

## Detailed Explanation

**Multi-threading**:

Handling multiple tasks concurrently in the server context often implies multi-threading. Each incoming request can be processed in its own thread, ensuring concurrent processing.

**Non-blocking Handling**:

The server must remain receptive to new incoming requests even while processing the current ones, up to a cap of 10 simultaneous requests. 

**Queue or List Structure**:

To manage and store multiple incoming requests, we should employ a suitable data structure, such as a queue or list. As new requests come in, they are added to this list and then processed. If the list's size goes beyond 10, additional requests can either be queued for later or rejected, depending on the desired behavior.

## Pseudo-code:

```plaintext
Initialize an empty list 'requestQueue'
While server is running:
    If there's a new request AND size of 'requestQueue' < 10:
        Add request to 'requestQueue'
        Spawn a new thread to handle this request
    Else if size of 'requestQueue' >= 10:
        Either reject new request or queue it for later based on desired behavior
```

## C Implementation:

```c
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>

// Assuming a request structure which contains necessary info about the request
typedef struct {
    // ... Some request related fields ...
} Request;

// ... Shared structures and other preliminary definitions ...

Request* requestQueue[10];
int currentRequests = 0;

void* handle_request(void* arg) {
    Request* req = (Request*)arg;
    // Handle the request
    
    // ... Request handling logic ...
    
    free(req); // Free the request memory after handling
    return NULL;
}

int main() {
    // Initialization of server and other resources
    
    while (1) { // Continuously check for new requests
        if (/*There's a new request*/ && currentRequests < 10) {
            Request* newRequest = /* Fetch the new request */;
            
            requestQueue[currentRequests++] = newRequest; // Add to the request queue
            
            pthread_t tid;
            pthread_create(&tid, NULL, handle_request, newRequest);
            pthread_detach(tid); // If you don't intend to pthread_join later
        } else if (currentRequests >= 10) {
            // Handle the scenario when there are too many requests, 
            // either reject or queue for later based on desired behavior
        }
    }

    // Cleanup and server shutdown logic
    return 0;
}
```

> **Note**: This C implementation provides a fundamental view of the requirement. Handling requests in a real-world server would involve much more complexities such as robust error handling, sophisticated mechanisms for fetching requests, advanced data structures for queue management, and so on.

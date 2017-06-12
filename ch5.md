# chapter5 Pthread

## 1 Process and pthread

## 2 Pthread
a thread maintains its own:  
* stack pointer
* register
* scheduling properties
* set of signals
* thread special data


### static in thread
C11 provides a keyword **_Thread_local** that splits off a static varible so that each thread has its version.
```c
#include <stdio.h>
#include <pthread.h>


#define N 2

void *thread(void *vargp);
char **ptr;


int main()
{
    int i;
    pthread_t tid;

    char *msgs[N] = {"hello from foo", "hello from bar"};
    ptr = msgs;

    for(i=0;i<N;i++)
        pthread_create(&tid, NULL, thread, (void *)i);

    pthread_exit(NULL);

}


void *thread(void *vargp)
{
    int myid = (int)vargp;
    static _Thread_local int cnt = 0;

    printf("[%d]:%s cnt = %d \n", myid, ptr[myid], ++cnt);

    return NULL;
}

```
### 2.1 pthread management
### 2.2 pthread mutex
### 2.3 pthread conditional variable

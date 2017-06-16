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

Mutexes are used to prevent data inconsistencies due to race condition.
```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>


void *thread_func();
pthread_mutex_t mutex1 = PTHREAD_MUTEX_INITIALIZER;
int count = 0;

int main()
{
    int rc1, rc2;
    pthread_t thread1, thread2;

    if(rc1 = pthread_create(&thread1, NULL, thread_func, NULL))
    {
        printf("thread1 create failed:%d.\n", rc1);
    }

    if(rc2 = pthread_create(&thread2, NULL, thread_func, NULL))
    {
        printf("thread2 create failed:%d.\n", rc1);
    }

    pthread_join(thread1, NULL);
    pthread_join(thread2, NULL);

}


void *thread_func()
{
    pthread_mutex_lock(&mutex1);
    count++;
    printf("counter value: %d.\n", count);
    pthread_mutex_unlock(&mutex1);
}

```
### 2.3 pthread conditional variable
Conditional variable is used by one thread to signal another thread an event/value.

```c
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

#define NUM_THREADS 3
#define TCOUNT 10
#define COUNT_LIMIT 12

int count = 0;
int thread_ids[3] = {0,1,2};
pthread_mutex_t count_mutex;
pthread_cond_t count_threshold_cv;

void *inc_count(void *t)
{
    int i;
    long my_id = (long)t;

    for(i=0; i<TCOUNT; i++)
    {
        pthread_mutex_lock(&count_mutex);

        count++;

        if(count == COUNT_LIMIT)
        {
            pthread_cond_signal(&count_threshold_cv);
            printf("inc_count(): thread %ld, count %d, threshold reached\n",
                    my_id, count);
        
        }
    
        printf("inc_count(): thread %ld, count %d, unlocking mutex\n",
                my_id, count);
    
        pthread_mutex_unlock(&count_mutex);
    
        sleep(1);
    
    }

    
    pthread_exit(NULL);
}




void *watch_count(void *t)
{

    long my_id = (long)t;

    printf("starting watching count: thread %ld.\n", my_id);

    pthread_mutex_lock(&count_mutex);
    while(count < COUNT_LIMIT)
    {
        pthread_cond_wait(&count_threshold_cv, &count_mutex);
        printf("watch_count(): thread %ld Conditional signal received.\n", my_id);
    }

    count += 125;
    printf("watch_count(): thread %ld count now = %d.\n", my_id, count);

    pthread_mutex_unlock(&count_mutex);
}


int main()
{
    int i, rc;
    long t1 = 1, t2 = 2, t3 = 3;

    pthread_t threads[3];
    pthread_attr_t attr;

    pthread_mutex_init(&count_mutex, NULL);
    pthread_cond_init(&count_threshold_cv, NULL);

    pthread_create(&threads[0], NULL, watch_count, (void *)t1);
    pthread_create(&threads[1], NULL, inc_count, (void *)t2);
    pthread_create(&threads[2], NULL, inc_count, (void *)t3);

    // wait for all threads to complete
    for(i=0; i<NUM_THREADS; i++)
        pthread_join(threads[i], NULL);

    //clean up
    pthread_mutex_destroy(&count_mutex);
    pthread_cond_destroy(&count_threshold_cv);


}
```

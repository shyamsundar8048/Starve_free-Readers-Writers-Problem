# Starve_free-Readers-Writers-Problem
Consider a shared database where multiple users can access or modify data,where multiple readers can read the data but no two can modify/write at same time or no one can read while other writing.If both simultaneously happens race condition occur. We try to solve this problem without starvation.
<br>
# Initialising Semaphores
```C++
counting semaphore rd_cnt=0; //which represents the total no of consecutive readers,helps in allowing all at once;
<br>
                                                           ---/*binary semaphores*/---
<br>
semaphore valid=1; //whenever there is value 1 that implies to pop a element from the request queue and execute the request;
<br>
semaphore mutex=1; //To avoid race condition in changing the readcount value in readers section.
<br>
semaphore alter=1; // this alter semaphore allows only either of reader or writer to enter into critical section. 
```
# Readers Code

```C++
Read(){
wait(valid);   //executes a pop operation to the request queue and allows one after other
wait(mutex);
rd_cnt++;
if (rd_cnt == 1) wait(alter);
signal(mutex);
signal(valid); // to allow the next request from the queue
//enters the critical section
//reading is performed
wait(mutex);
rd_cnt--;
if (rd_cnt == 0) signal(alter);
signal(mutex)
}
```
# Writers Code
```C++

Write(){
wait(valid);
wait(alter); //if the previous process is a reader one and current is a writer this allows to excute after the signal
//enters the critical section
//writing is performed
signal(alter); //to allow other process into the critical section
signal(valid);// to allow the next request from the queue
}
```
#Solution

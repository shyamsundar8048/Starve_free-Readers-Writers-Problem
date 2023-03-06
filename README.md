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
we start the solution by creating a queue named `"REQUESTS"` and push all the requests with time.
<br>
we have used four semaphores to control the excution pattern.semaphore `"rd_cnt"` helps us to find out no of read requests have been made at that particular time.
<br>we won't allow the writer to enter the contol section until the completion of these rd_cnt no of read requests.this control is done using "alter", the semaphore used to alter the control section to either read or write according to the request.This helps to ensure that race condition is eliminated,which is the main problem of reader-writer problem.
<br>
For starve_free we try to implement it in a `"fifo"` way ie which pops the requests according to their order of entering into queue, This order is maintained using the semaphore `"valid"` which implies the current request is served and new request can be made to get executed. 
<br>
And in the case of multiple reads it allows all consecutive readers to get served. And whenever a new reader enters to control section while other read is getting served ,we allow this reader to change the value of the rd_cnt which is no of readers at that time in control section.
<br>
After a request got served we reduce the value of rd_cnt, if increment and decrement occurs same time `"Race Condition"` might occur for avoiding this we use semaphore `"mutex"`.By using all these we can make a starve free solution for our problem.

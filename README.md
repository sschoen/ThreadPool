# ThreadPool
Header only thread pool that is easy to use, returns futures to get return values, and accepts functions with arguments.
## Usage
Simply instantiate a pool like any other class


```c++
#include "ThreadPool.h"

int main () {
  
  /* ================== Example Functions ================== */
  auto job = [](){ std::cout << "inside job" << std::endl;};
  auto jobWithArgs = [](int i){std::cout << "inside jobWithArgs, arg passed in is: " << i << std::endl;};
  auto jobWithReturn = [](){ return std::string("returned from jobWithReturn");};
  
  
   /* ================== Creating a Pool ================== */
   
  //the pool will allow you to make any number of threads up to std::size_t
  ThreadPool pool(5); //creates an instance of the threadpool with 5 threads
  //or
  ThreadPool pool2; // Create a default pool with ONE thread
  
  /* ================== Adding Jobs to the Queue/Obtaining Return Values ================== */
  
  pool.enqueue(job); //this is how you pass it a job. Easy as that!
  pool.enqueue(jobWithArgs, 1); //this is how you pass it a job with arguments. Easy!
  
  //ThreadPool::push actually returns a future variable for the functions return type that you gave it.
  //To access the returned value just use "auto <varname> = pool.push()" or "future<type> <varname> = pool.push()".
  //Using auto allows you to not need to remember what the return type of the function is.
  auto future = pool.enqueue(jobWithReturn); 
  
  //Now with the future variable from the push function, just use "future.get()" to access the data!
  std::string returnedString = future.get(); //future.get() blocks until the value is ready to be obtained
  
  /* ================== Utility Functions ================== */
  
  //gets the current thread count of the threadpool
  std::size_t numthreads = pool.getThreadCount(); //returns a std::size_t
 
  
  }
```

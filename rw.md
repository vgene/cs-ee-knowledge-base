# Readers-Writers Problems

## Introduction

Readers-Writers Problems are a group of common computing problems in concurrency.

## Categories: {#categories}

1. First RW problem \(Readers-preference\)
2. Second RW problem \(Writers-preference\)
3. Third RW problem \(no starvation\)

## C++ Implementation {#implementation}

[Ref. Wikipedia - Readers–Writers problem](https://en.wikipedia.org/wiki/Readers–writers_problem "Implementations from Wikipedia")

**First RW Problem**

{%ace edit=false, lang='c_cpp', theme="monokai" %}
semaphore resource=1;
semaphore mutex=1;
readcount=0;

/* Please note that:
   resource.P() is equivalent to wait(resource)
   resource.V() is equivalent to signal(resource)
   mutex.P() is equivalent to wait(mutex)
   mutex.V() is equivalent to signal(mutex)
*/

writer() {
    resource.P();//Lock the shared file for a writer

    <CRITICAL Section>
    // Writing is done

    <EXIT Section>
    resource.V();//Release the shared file for use by other readers. Writers are allowed if there are no readers requesting it.
}

reader() {
    mutex.P();//Ensure that no other reader can execute the <Entry> section while you are in it
    <CRITICAL Section>
    readcount++;//Indicate that you are a reader trying to enter the Critical Section
    if (readcount == 1)//Checks if you are the first reader trying to enter CS
        resource.P();//If you are the first reader, lock the resource from writers. Resource stays reserved for subsequent readers
    <EXIT CRITICAL Section>
    mutex.V();//Release

    // Do the Reading

    mutex.P();//Ensure that no other reader can execute the <Exit> section while you are in it
    <CRITICAL Section>
    readcount--;//Indicate that you are no longer needing the shared resource. One less readers
    if (readcount == 0)//Checks if you are the last (only) reader who is reading the shared file
        resource.V();//If you are last reader, then you can unlock the resource. This makes it available to writers.
    <EXIT CRITICAL Section>
    mutex.V();//Release
}
{%endace%}


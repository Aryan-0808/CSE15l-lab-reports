# Lab Report 5  
## The Post  
### Bug in lab 7  
In the folder `lab7`, I ran `bash test.sh` to run the tests. When I ran it, it gave me an error message that one lab failed. The one which failed was testMerge2().  

There was an error because of the method `merge(List<String> list1, List<String> list2)`. I thought there was in infinite loop but I couldnt find out where there was an infinite loop. I also couldnt figure out where it was occuring.
## TA's reply  
You got where the error is, in one of the `while` loops. I think you should check out `ListExamples.java`, and look into the last `while` loop, there may be some errors in that.

## Information given to me by the TA  
I went into `ListExamples.java`, and looked at the various while loops and there seemed to be a similarity in the second and third `while` loop. I tried to change `index1` in the last(third) `while` 
loop to `index2`. There was an error because the iterator being used for the second list was `index2` but instead it was incrementing `index1`. Below is the tests running successfully.

## Information to understand the files 
Repository structure
- lab7
  - ListExamples.java
  - ListExamplesTests.java
  - test.sh
  - lib
     - 

===============
Problem example
===============

In this section we present more complicated example with full details to give you 
better overall look at abilities of our services. It is still an elementary example 
but it will tell you much more about the specific of online judging.

.. important::
	In the following example we created test cases according to the advice in the :ref:`good test cases design <appendix-good-test-cases-design>` appendix.

The problem is to count the sum of numbers from *1* to given *n* i.e. *1* + *2* + *3* + ... + *n*, 
we call it the *Initital sum* problem. We are going to preper it to handle with multiple 
input instances in a single test case by proper input / output specification design. 
Look at the following problem description:

.. admonition:: Example - The Initial Sum
  :class: note

    For a positive integer *n* calculate the value of the sum of all positive integers that are not greater than *n* i.e. *1* + *2* + *3* + ... + *n*. For example when *n* = *5* then the correct answer is *15*.

    **Input**
      In the first line there will be the number *1* |le| *t* |le| *10000000* which is the number of instances for your problem. In each of the next *t* lines there will be one number *n* for which you should calculate the described initial sum.

    **Output**
      For each *n* print the calculated initial sum. Separate answers with new line character.

    **Example 1**
      | **Input**
      | 4
      | 1
      | 2
      | 3
      | 4
      | **Output**
      | 1
      | 3
      | 6
      | 10
            
    **Example 2**
      | **Input**
      | 2
      | 10
      | 11
      | **Output**
      | 55
      | 66

.. note::
  Input specification allows us to construct rich test cases which are able to distinguis between faster and slower solutions.

Referring to the solution to the *Initial sum* problem take into account that the possible input data can be too big for the standard *int* type thus we will use the *long long* type. Before we set test cases let us present two distinct solutions to the problem:

.. code-block:: cpp

   long long initsum(long long n)
   {
     int i;
     long long sum = 0;
     for (i=1; i <= n; i++)
     {
       sum += i;
     }
     return sum;
   }
   
   int main()
   {
     int t;
     long long n;
     scanf("%d", &t);
     while (t > 0)
     {
       scanf("%lld", &n);
       printf("%lld\n", initsum(n));
       t--;
     }
     return 0;
   }

.. admonition:: Complexity note
  :class: note

  The first solution refers directly to the definition of the problem i.e. the function *initsum* iterates from *1* to *n* to calculate desired value. The calculation requires *n* operations of addition to obtain the result.

It is basic school knowledge that there exists the compact formula for that problem and we use it in the second implementation:

.. code-block:: cpp
   
   long long initsum(long long n)
   {
     return n*(n+1)/2;
   }
   
   int main()
   {
     int t;
     long long n;
     scanf("%d", &t);
     while (t > 0)
     {
       scanf("%lld", &n);
       printf("%lld\n", initsum(n));
       t--;
     }
     return 0;
   }   

Both programs are correct answers to the problem but if we want to distinguish the algorithms we can design test cases that only the second solution can pass. 

.. note::
  It highly depends on the computational power of the machine that runs test cases. We present test cases that are valid for the computer of this text's author. 

Our suggestion is to design one test case which is easy to pass for both algorithms to give information that the solution is correct and the second test case that is possible to pass only for the second algorithm. It can give an information to the user, that his solution is correct but too slow. The user submitting solution similar to the first one will get information about test cases and will be able to see that his program passes first test case and exceed time limit in the second test case.

We cannot put all input and output data here because of its size thus we write it in shortened manner:

.. admonition:: Example
  :class: note

    **Test case 1**
      | **Input file**
      | 1000
      | 1
      | 2
      | ...
      | 1000
      | 1000000
      | **Output file**
      | 1
      | 3
      | ...
      | 500500
      | 500000500000
            
    **Test case 2**
      | **Input file**
      | 999000
      | 1001
      | 1002
      | ...
      | 1000000
      | **Output file**
      | 501501
      | 502503
      | ...
      | 500000500000

Computational power of current machines is enough to finish first test case instantly. Both presented algorithms finished computations with time below *0.01s*. However it is a good test case for a corectness verification only. 

First *1000* positive integers give us the assurance that solution is mathematically correct. We have also added single test with big number i.e. *n = 1000000* to make sure that user's solution bases on *long long* type. On the other hand the second test case is rich enough to make the first algorithm to exceed even *5s* time limit. 

The second algorithm works fast enough to pass that test case in time below *0.1s*. We have huge gap between *0.1s* and *5s* thus we can easily choose safe value as our time limit, for example *1s*.

We still haven't chosen judges for test cases and master judge for the problem. We don't have floating point numbers in our output file specification thus we rather decide to choose *Ignoring extra whitespaces* judge for both test cases. It leaves users with possiblity of small formating errors without risk of unwanted rejections of theirs solutions. For example it is possible to replace new line characters with spaces in output formatting and still pass the test case.

We assume that we want to accept every correct solution but distinguish the better ones and give them a better score. The *Score is % of correctly solved sets* master judge is perfect for that purpose. Submitting the first solution achieves the result of *50%* while the second solution passes both test cases and its result is *100%*.

.. note::
  Presented scoring method assumed both tests as equally worth *50%* each. To achieve different distribution of scores you need to modify the master judge and pick the scoring of test cases arbitrary. We present :ref:`the example <master-judges-weighted>` in the section :ref:`writing master judges <judges-advanced>`.

To sum up we present full problem specification:

.. admonition:: Complete *The Initial Sum* Example
  :class: note

  **Title:** The Initial Sum
   
  **Description** 
    **The task**
      For a positive integer *n* calculate the value of the sum of all positive integers that are not greater than *n* i.e. *1* + *2* + *3* + ... + *n*. For example when *n* = *5* then the correct answer is *15*.
     
    **Input / output specification**
      **Input** 
        In the first line there will be the number *1* |le| *t* |le| *10000000* which is the number of instances for your problem. In each of the next *t* lines there will be one number *n* for which you should calculate the described initial sum.
      **Output** 
        For each *n* print the calculated initial sum. Separate answers with new line character.

    **Examples**
      **Example 1**
        | **Input**
        | 4
        | 1
        | 2
        | 3
        | 4
        | **Output**
        | 1
        | 3
        | 6
        | 10
            
      **Example 2**
        | **Input**
        | 2
        | 10
        | 11
        | **Output**
        | 55
        | 66
 
  **Test cases**
    **Test case 1**
      | **Input file**
      | 1000
      | 1
      | 2
      | ...
      | 1000
      | 1000000
      | **Output file**
      | 1
      | 3
      | ...
      | 500500
      | 500000500000
      | 
      | **Judge:** Ignoring extra whitespaces
      | **Time limit:** 1s
            
    **Test case 2**
      | **Input file**
      | 999000
      | 1001
      | 1002
      | ...
      | 1000000
      | **Output file**
      | 501501
      | 502503
      | ...
      | 500000500000
      | 
      | **Judge:** Ignoring extra whitespaces
      | **Time limit:** 1s


  **Master judge:** Score is % of correctly solved sets


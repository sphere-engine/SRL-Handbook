
Problems
========


In this section we are going to give you insight into submission flow and online judge problem 
construction. From the previous section we know basics of the concept of the online judging. 

To better understand and make it possible to create own problems we will describe components of 
the problem:

 - **Description**
 
   - The task
   - Input / output specification
   - Input / output examples
   
 - **Test case(s)** (possibly many)
 
   - Input file, 
   - Output file
   - Time limit
   - Judge
   
 - **Master judge**


Before we begin the analysys of the individual components let us take a look at the complete 
diagram of submission flow to embed the listed components in overall view.

.. _submission-flow-digram:

.. image:: ../_static/full_diagram.png
   :alt: Full submission flow diagram
   :width: 700px
   :align: center


The diagram is the extension of the one from the previous section. The main difference is the 
appearance of the *master judge* component. Since we have introduced possibility of many test 
cases (input, output and judge) thus we need the summary component to combine the results from 
test case judges - we call that summary component *master judge*. 

Every submission needs to have precised *programming language* to choose proper compiler or interpreter. 

The problem description is the crucial part for users to make it possible to solve the problem.

Let us recall the *Integer Power* problem since we are going to use it as a demonstration 
example. The task described in the problem is to compute the value of *a*\ :sup:`b` \ for given 
integers *a* and *b*

.. _problem-description:

Description
~~~~~~~~~~~

User must be able to understand the problem to solve it successfully. Description contains 
complete information needed to solve the problem thus it is the most important feature from 
the user's point of view.

**The task**

The formal text that describes the specificity of the problem usually in mathematical manner. 

.. admonition:: Tips
  :class: tip

  To achieve good quality of the description it is a good habit to:
    * graphically illustrate the problem,
    * support a dry theory with simple examples.

Referring to the *Integer Power* problem we could write the taks as follows:

.. admonition:: Example (part 1)
  :class: note   
   
   Write the program which takes two numbers *a* and *b* and returns the value of *a*\ :sup:`b`\. 
   For example for *a = 3* and *b = 4* the result is *3*\ :sup:`4`\ = *81*   

**Input / output specification**

The previous part was formal from the mathematical perspective but not syntactically. Automatic 
judging is only possible when we can make an assumption on the problems's input and output behaviour. 
The interior of the user's program is a black box for us right now. We specify what is the input file 
structure to make it possible for users to implement proper input reading. 

.. tip::
  The specification of input file should be an information that user can rely on.

On the other hand we expect that user produces output file with a predictable structure. Formal 
specification of the output file is an information that we are going to rely on to verify the correctness 
of the submission.

Referring to the *Integer Power* problem let us recall :ref:`first solution <integer-power-human>` 
of the problem. We didn't specify any formal input or output syntax in the previous section. First attempt 
was the solution with the human readable interface and after that we modified the code to achieve a :ref:`raw version <integer-power-auto>`. We can use that source code as a base for following formal specification:

.. admonition:: Example (part 2)
  :class: note

   **Input** 
      In the only line of the input there will be two integer numbers *1* |le| *a* |le| *8* and *0* |le| *b* |le| *10* separated by a single space character.
   
   **Output**
      Program should write a single number which is a value of *a*\ :sup:`b`\.

.. important::
  Typically we create the input / output specification before or independently of implementation.


**Input / output examples**

.. The examples here are dedicated to ilustrate the input and output files structure. In the best case scenario they cover every distinct configuration of parameters (up to numbers, letters etc.) which is important for more complex problems.

Referring to the *Integer Power* problem we present how we could compose the examples:

.. admonition:: Example (part 3)
  :class: note

   **Example 1**
    | **Input**
    | 3  4
    | **Output**
    | 81
            
   **Example 2**
     | **Input**
     | 7  0
     | **Output**
     | 1
    
.. _test-case:

Test case(s)
~~~~~~~~~~~~

Just as the description was for users only so the test cases are for the machine checker. 
This is the essence of the automatic judging idea. The vast majority of the usages implements 
the following schema: there is a model input paired to a model output along with the program 
which can compare that model output with the user's output to decide whether user's answer is 
good or not.
 
**Input and output files**

Input file contains the problem instance and it must be consistent with the input specification. 
The output file should contain corresponding correct answers formatted in accordance to the 
output specification.

.. note::
  It is not necessery to write the solution to the program to create the model output file - it can be obtained in any manner, however, it is a good practice to have model solution.

Referring to the *Integer Power* problem we present how we could prepare following test cases:

.. admonition:: Example (part 4)
  :class: note

   **Test case 1**
     | **Input file**
     | 3 4
     | **Output file**
     | 81

   **Test case 2**
     | **Input file**
     | 7 0
     | **Output file**
     | 1

   **Test case 3**
     | **Input file**
     | 2 10
     | **Output file**
     | 1024


.. tip::
  It is recomended to construct the problems that are able to repeat the desired procedure as many times as we want to make it possible to test the user's submission with one test case. There are many reasons for that approach and for further information please visit :ref:`good test cases design<appendix-good-test-cases-design>` appendix.

**Time limit**

We have already pointed that one of the features of online judging is the possiblity of estimating 
the time complexity. To achieve that the author of the problem has to adjust the *time limit* for program 
execution. We cover this topic in :ref:`testing time complexity <appendix-testing-time-complexity>` appendix.

.. admonition:: Example (part 5)
  :class: note

  Our toy example problem is much too simple in assumptions to allow us to present example of time limits that distinguish different algorithms thus we put default time limit of *1s*. In the next section we present more complex example where we further discuss the time limit which can help to estimate the algorithm quality.

.. _judges-simple:

**Judge**

The judge is a program which process user's output file after execution. Its task is to establish if the 
submission passed the test case and potentially also returns *the score*. When the user's program pass the 
test case the returned status is *"accepted"*.

Usually the judge implementation is reduced to compare the model output file with the user's output file. 
We support problem setters with default judges:

  * **Ignoring extra whitespaces** - compares output files up to extra whitespaces
  * **Ignoring floating point errors up to a specific position** - allows the floating point numbers to be 
    inaccurate i.e. we can accept the errors up to for example *0.01*
  * **Exact judge** - requires output files to be identical 

More information about default judges you can find in the section :ref:`judges <judges-normal>`. You find there also information about *the score*.

.. tip::
  The *Ignoring extra whitespaces* judge is one of the most popular default choice. It is more liberal for output formating errors which in fact doesn't affect on the solution semantic correctness. 

.. Similarly *Ignoring floating point errors up to a specific position* judge is popular choice for problems where result numbers are not integers.

It is possible to create custom test case judges. The author can implement any kind of verification having full 
access to the input file, base input file, user's output file and even user's source code. For more information 
visit the section :ref:`writing test case judges <judges-advanced>`.

.. admonition:: Example (part 6)
  :class: note
  
  For the *Integer Power* problem we decide to use default *Ignoring extra whitespaces* judge for each test case thus we allow the user to generate extra whitespaces before and after the resulting number *a*\ :sup:`b`\. 

  For example when user's solution prints " |_| |_| |_| *81* |_| |_| |_| |_| " as a result for *"3 4"* problem instance it is still correct answer.

.. _judges-master:

Master judge
~~~~~~~~~~~~

We have discussed the individual test cases for the problem and established that each of them returns information, 
i.e. status and the score. The master judge is the component which combines all incoming results obtained from test 
cases to produce the final result which is the status and the score. You can look again at the 
:ref:`submission flow diagram <submission-flow-digram>` for better understanding.

There are predefined master judges proper for most situations:

  * **Generic masterjudge** - it gathers information from test case judges and requires each of them to achieve *"accepted"* as the result to establish final result as the *"accepted"*. 
  
  * **Score is % of correctly solved sets** - it is a more liberal masterjudge which allows to accept incomplete solution with the score which is the percentage of correctly solved test cases. 

You can learn more in :ref:`Master judges <master-judges-normal>`.

.. hint::
  When you need to use more complex master judge it is possible to create the new one or modify the existing ones. You have access to the source code of default master judges and they can be used as a base for your modifications. 

  Further information about designing master judges you can find in the section :ref:`writing master judges <judges-advanced>`.

.. admonition:: Example (part 7)
  :class: note

  The last missing part for the example we successively improve is the choice of master judge. We created two test cases and there is no need to implement the specific own master judge thus we select default one. 

  When we need to distinguish the solutions as better or worst (but both correct) we should rather choose *Score is % of correctly solved sets* but in our situation each test case is a pure verification of correctness (i.e. no performance aspects tested) thus we select *Generic masterjudge* to force the user's solution to pass all test cases.

      
Complete example
~~~~~~~~~~~~~~~~

We have discussed all components of the problem specification therefore we are able to present whole problem setting:


.. admonition:: Complete *The Integer Power* Example
  :class: note

  **Title:** The Integer Power
   
  **Description** 
    **The task**
      Write the program which takes two numbers *a* and *b* and returns the value of *a*\ :sup:`b`\. For example for *a* = *3* and *b* = *4* the result is *3*\ :sup:`4`\ = *81*
     
    **Input / output specification**
      **Input** 
        In the only line of the input there will be two integer numbers *1 |le| a |le| 8* and *0 |le| b |le| 10* separated by a single space character.
      **Output** 
        Program should write a single number which is a value of *a*\ :sup:`b`\.

    **Examples**
      **Example 1**
        | **Input**
        | 3  4
        | **Output**
        | 81
              
      **Example 2**
        | **Input**
        | 7  0
        | **Output**
        | 1
 
  **Test cases**
    **Test case 1**
      | **Input file**
      | 3 4
      | **Output file**
      | 81
      | 
      | **Judge:** Ignoring extra whitespaces
      | **Time limit:** 1s

    **Test case 2**
      | **Input file**
      | 7 0
      | **Output file**
      | 1
      | 
      | **Judge:** Ignoring extra whitespaces
      | **Time limit:** 1s


  **Master judge:** Generic master judge

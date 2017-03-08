.. _judges-normal:

Judges
======

We have mentioned that the problem setter can choose from default test cases judges and master judges. In practice it is usually enough.

Test case judges
----------------

Each judge returns piece of information about execution of the submission:
 - execution time
 - used memory
 - status (look :ref:`statuses <appendix-statuses>`)
 - score (optional)

.. important::
  Execution time and used memory are obtained automaticaly from system. The score depends on judge design and can take any value.

There are six default test case judges:
  
**Ignoring extra whitespaces** (id = 1)
  It compares output files up to extra whitespaces. It is the standard 
  judge for all problems with exactly one correct solution (in particular for :ref:`binary problems <dictionary-binary-problem>`).
  
  .. admonition:: Example
    :class: note

    The prime number problem (i.e. for a number *n* determine if *n* is a prime number and then return *1* as a result, otherwise result is *0*).

.. The problem of prime factorisation of the number (i.e. for a number *n* find the prime factors *p*\ :sub:`1`\, *p*\ :sub:`2`\, ..., *p*\ :sub:`k`\ that *n* = *p*\ :sub:`1`\ |sdot| *p*\ :sub:`2`\ |sdot| ... |sdot| *p*\ :sub:`k`\). It is known that the prime factorisation of the number is unique up to the order of prime factors so if we require in output specification to write sorted list of factors there is only one good answer to the problem.

**Ignoring floating point errors up to 10^-2** (id = 2)
  It allows the floating point numbers to be inaccurate i.e. we can accept the errors up 
  to *0.01*. It is good choice for problems when the result is an approximation which don't 
  require good precision.
  
  .. admonition:: Example
    :class: note

    The problem of the triangle area, i.e. for given integer side lenghts *a*, *b*, *c* calculate the area of the triangle. In general the result is not an integer and accuracy is not crucial.

**Ignoring floating point errors up to 10^-6** (id = 3)
  it allows the floating point numbers to be inaccurate i.e. we can accept the errors up 
  to *0.000001*. When you need a good precision it is obviously better choice than the previous judge.

  .. admonition:: Example
    :class: note

    The problem of the value of the *sine* function. The range of the sine function is [*-1,1*] thus the precision is important.

**Score is source length** (id = 4)
  it is the modification of the *Ignoring extra whitespaces* judge i.e. it verifies 
  the correctness of the solution in the same manner but in addition it returns the score which 
  is the length of the source code. The judge is designed for challange type problems where the 
  objective is to solve the problem with the shortest source code.

  .. admonition:: Example
    :class: note

    The problem of determining first *n* numbers in decimal expansion of the number |pi| for given *n*. The challange is to solve that problem with the shortest possible source code.

**Academy** (id = 9)
  It works exactly the same as *Ignoring extra whitespaces* judge but in addition it gives the access to difference between model output file and user's output. Read :ref:`submission information <appendix-submission-information>` appendix to learn more about the place where the difference between files is stored.

**Exact judge** (id = 10)
  It requires output files to be identical.


.. _master-judges-normal:

Master judges
-------------

Each master judge returns piece of information about the submission:
 - execution time
 - used memory
 - status (look :ref:`statuses <appendix-statuses>`)
 - score (optional)

The master judge can arbitrarily set each part of that information based on the information received from test case judges (look :ref:`diagram <submission-flow-digram>`). In our default master judges we assumed following natural convention for that values:

Execution time
  The overall execution time is the sum of times from test case judges.

Used memory
  The overall used memory is the maximum value of used memory from test case judges.

.. important::
  The final status and the score can be freely combined based on statuses and scores from test case judges.

We have two default master judges both mentioned in section :ref:`problems <judges-master>`:

**Generic masterjudge** (id = 1000)
  It gathers information from test case judges and requires each of them to achieve *"accepted"* status to establish final status as the **accepted**.

  Example accepted result:

  .. image:: ../_static/status-generic.png
    :width: 700px
    :align: center

  |

  When any test case ends with error the final answer is inherited from the first failed test case. For example when the problem has five test cases and the second and the fourth ones failed, the final result is inherited from the second test case.

  Example **time limit exceeded** and **wrong answer** results:
  
  .. image:: ../_static/status-tle.png
    :width: 700px
    :align: center  

  |

  .. image:: ../_static/status-wa.png
    :width: 700px
    :align: center  

  |
  
  Generic masterjudge combines the execution times of all testcases and yields the sum as the final score.
  
  .. tip::
    It is a proper choice when the problem setter requires that the solution fulfills all his requirements i.e. it is correct and sufficiently efficient.
  
**Score is % of correctly solved sets** (id = 1001)
  It is a more liberal masterjudge which allows to accept incomplete solution with the score which is the 
  percentage of correctly solved test cases. 

  Example result:
  
  .. image:: ../_static/status-percentage.png
   :width: 700px
   :align: center

  |
  
  For example when the problem has five test cases and only the second failed, the final score is equal to *80%*. The advantage is that the user gets more information about the correctness level of its solution.

  .. tip::
    It is a proper choice when the problem setter wants to distinguish user's solutions. It is possible to design test cases to be easier or more difficult to pass.

  .. admonition:: Example
    :class: note

    The problem of power function i.e. for (possibly big) integer numbers *a* and *b* calculate the value of *a*\ :sup:`b`\. 

     * The first test case can deliver only input instances for which the result is in the standard numeric type scope. 
     * Another test case can require from the solution to implement the big numbers model. 

    These two test cases give an information on the advancement of the solution. 

     * The third test case could also take into account the aspect of the performance and distinguish solutions implementing naive algorithms from the better ones which implement the fast power algorithm.

  The least advanced (but in some way correct) solutions will pass the first test case and achieve the result of *33%* while the more complex solutions (implementing big numbers) are able to pass the first and the second test and achieve the result of *66%*. To achieve the best result of *100%* the solution needs to implement both big numbers and fast power algorithms to pass all three test cases.
        

.. _master-judges-flags:        

Enabling master judge
~~~~~~~~~~~~~~~~~~~~~

The crucial part of setting the master judge is to decide how it should interpret the score. There are three options the author can choose:

  - binary (bin)
  - maximum (max)
  - minimum (min)

Setting any of these options determines what is the final submission's score and which score is better.

Binary
  The final success result is simply **accepted** status and the score is the execution time. The faster submissions are better by default. The *Generic masterjudge* usually works with that option.

Maximum
  The final score comes from test case judges and we establish that the greater values are the better ones. It is the proper choice for the *Score is % of correctly solved sets* masterjudge.

Minimum
  The final score comes from test case judges and we establish that the smaller values are the better ones. For example usage of *Score is source length* test case judge yields the score intended to be minimised.

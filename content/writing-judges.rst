==============
Writing judges
==============

In the previous section we have discussed default *test case judges* and *master judges* which are sufficient for most situations. 
However there are problems which require individual solutions due to nature of the problem. In this section we present examples of the problems along with descriptions and motivation.

.. _judges-advanced:
        
Writing test case judges
----------------

Test case judge has access to the following information:
 * model input
 * model output
 * user's output
 * user's source code

Impossible model output file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. admonition:: Task example
  :class: note

  For given function *f* find its root i.e. the argument *x*\ :sub:`0`\ that *f* ( *x*\ :sub:`0`\ ) = *0*.

In general there are many solutions to the problem, for example for polynomial *x*\ :sup:`2`\ + *x* - *2* the numbers *1* and *-2* are both correct answers. 

You can see that it is hard to prepare model output file in test case. There are possibly infinitely many solutions for the certain functions thus it is impossible to keep all of them in the output file. It forces us to use different approach.

.. admonition:: Judge description
  :class: note

  The test case judge should verify the condition from the problem task i.e. for the user's answer from the output file it should check if that answer is a root of the function.

  Test case judge uses his access to model input file to read the problem instance.


Ambiguous model output file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. admonition:: Task example
  :class: note

  For given graph *G* with *n* vertices *1*, *2*, ..., *n* determine if it has hamiltonian cycle (i.e. closed loop through a graph that visits each node exactly once). If the hamiltonian cycle exists print it as a sequence of vertices.

It is easy to see that *1-2-3-1* is the same cycle as *2-3-1-2*. We could add the requirement to start with the smallest vertex number. Unfortunately it is possible that there exists many different hamiltonian cycles which are not cyclic shifts. We could again use the previous approach and verify if user's answer is really hamiltonian cycle. Alternatively we can build model output file with all possible hamiltonian cycles:

.. admonition:: Judge description
  :class: note

  For user's answer the judge looks for that specific one on the list contained in model output file.

.. caution::
  It can be problematic to keep all answers due to possible huge number of good solutions.


.. _master-judges-advanced:

Writing master judges
-------------

Similarly to test case judges it is possible to create custom master judges. In certain situations the problem setter may want to extend functionality of existing master judge or even implement brand new one. In this section we present examples of the master judges along with a motivation.

Master judge has access to the following information:
 * results from test case judges
 * user's source code

.. _master-judges-weighted:

Weighted % of correctly solved sets
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Here we present the generalisation of *Score is % of correctly solved sets* master judge. It was a little disadventage that each test case is worth the same and to increase to influence of some submission's aspect you were forced to produce many test cases.

For example when your test cases verify three aspects *A*, *B* and *C* of the problem and you would like to put weights *20%*, *30%* and *50%* respectively, you were able to do that by creating *10* test cases. Two of them responsible for an aspect *A*, three of them responsible for an aspect *B* and five of them responsible for an aspect *C*. However it is inconvenient and you can consider following idea:

.. admonition:: Master judge description
  :class: note

  Master judge has the information about the number of test cases and weights which it should assign to each test case. The final score is the weighted sum of accepted test cases.

  For example for three test cases *a,b,c* and weights *20%, 30%, 50%* the submission gets one of the possible results depending on passed test cases:

   * **no test case passed** - *0%*
   * **a** - *20%*
   * **b** - *30%*
   * **a,b** - *50%*
   * **c** - *50%*
   * **a,c** - *70%*
   * **b,c** - *80%*
   * **a,b,c** - *100%*

.. _master-judges-forbidden-structures:

Forbidden structures in source code
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The problem setter may require that the solution cannot use some programming structures. For example he may want to allow to use language *C++* but with no access to STL library to force users to implement efficent data structures manually. Another example is to restrict source codes to not use loop structures to support only solutions based on recursion.

.. admonition:: Master judge description
  :class: note

  Master judge uses access to the user's source code to detect usages of forbidden keywords (for example loops: while, for, goto). When forbidden keyword is detected the final status is set to *wrong aswer* in other case the master judge performs classical verification (for example the same as Generic masterjudge).


.. _master-judges-memory-limits:

Used memory limitations
~~~~~~~~~~~~~~~~~~~~~~~

We cannot directly support memory limit due to the reasons explained in :ref:`testing the memory complexity of algorithms <appendix-testing-memory-complexity>` appendix. To make possible to bound the amount of available memory one can implement master judge for that purpose.

.. admonition:: Master judge description
  :class: note

  Master judge gathers the information of used memory from test case judges and takes the maximum value as the result (note that this is the behaviour of default master judges). We verify the memory limit with respect to the user's solution programming language to adjust the master judge for all programming languages we allow to use.

.. important:: 
  It's very imporant to adjust the memory limit to the programming language due to different memory needs of programs in different languages.


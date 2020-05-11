Builtin Functions
====================================
This pages describes all available builtin functions.

Cash flow functions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Add CashFlows 
""""""""""""""

addCashFlows( e\ :sub:`1`\ : Transfers , e\ :sub:`2`\ : Transfers) : Transfers

The addCashFlows() function returns the pointwise addition of e\ :sub:`1`\ and e\ :sub:`2`\. Note e\ :sub:`1`\ and e\ :sub:`2`\ should be ```Transfers`` of equal lenght.

Dicount 
""""""""

dicount( e\ :sub:`1`\ : Transfers , e\ :sub:`2`\ : Float , e\ :sub:`3`\ : Float) : Float

The discount() function returns the discounted transfers,  e\ :sub:`1`\, with interest rate e\ :sub:`2`\ at timepoint e\ :sub:`3`\.

Dicount sub tax
""""""""""""""""

discountSubTax( e\ :sub:`1`\ : Transfers , e\ :sub:`2`\ : Float , e\ :sub:`3`\ : Float, e\ :sub:`4`\ : Float) : Float

The discount() function returns the discounted transfers,  e\ :sub:`1`\, with interest rate e\ :sub:`2`\ at timepoint e\ :sub:`3`\ subtracted the tax of e\ :sub:`4`\.

Scale 
""""""""""""""""

scale( e\ :sub:`1`\ : Transfers , e\ :sub:`2`\ : Float) : Transfers

The scale() function returns transfers e\ :sub:`1`\ scaled by  e\ :sub:`21`\.

Sum cashflows 
""""""""""""""""

sumCashFlows( e\ :sub:`1`\ : List<Transfers>) : Transfers

The sumCashFlows() function returns the pointwise summed transfers of e\ :sub:`1`\.


IFunction functions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Create piecewise constant 
""""""""""""""""""""""""""

createPiecewiseConstant(e\ :sub:`1`\ : Transfers) : IFunction

The createPiecewiseConstant() function returns a piecewise IFunction specified by the transfers of e\ :sub:`1`\.

Create sum 
"""""""""""""

createSum(e\ :sub:`1`\ : List<IFunction> ) : IFunction


The createSum() function returns a IFunction resulting calling all IFunctions in the list e\ :sub:`1`\ and summing the result of these.


List functions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Average 
""""""""

avg( e\ :sub:`1`\ : List<Float> ) : Float

the avg() function returns the average of the list e\ :sub:`1`\.


Count
""""""

count( e\ :sub:`1`\ : List<_> ) : Float

the count() function returns the length of the list e\ :sub:`1`\.


Max of list
""""""""""""

maxOfList( e\ :sub:`1`\ : List<Float> ) : Float

the maxOfList() function returns the maximum value in the list e\ :sub:`1`\.


Min of list
""""""""""""

minOfList( e\ :sub:`1`\ : List<Float> ) : Float

the minOfList() function returns the minimum value in the list e\ :sub:`1`\.

Range
""""""

range( e\ :sub:`1`\ : Float , e\ :sub:`2`\ : Float ) : List<Float>

the range() function returns a list containing all integer values in [e\ :sub:`1`\ , e\ :sub:`2`\ ].

Single
"""""""

single( e\ :sub:`1`\ : List<T\ :sub:`1`\> ) : T\ :sub:`1`\

the single() function returns the single value contained in e\ :sub:`1`\. Note e\ :sub:`1`\ must contain exactly one value.

Skip
"""""""

skip( e\ :sub:`1`\ : Float , e\ :sub:`2`\ : List<T\ :sub:`1`\> ) : List<T\ :sub:`1`\>

the skip() function returns the list of e\ :sub:`2`\ having skippid the first e\ :sub:`1`\ elements.

Sum
"""""""

sum( e\ :sub:`1`\ : List<Float> ) : Float

the sum() function returns the summed values of e\ :sub:`1`\.


Unique 
""""""""""""""""

unique( e\ :sub:`1`\ : List<T\ :sub:`1`\>) : List<\ :sub:`1`\>

The unique() function returns a list containing the unique elements of e\ :sub:`1`\.


Map functions
^^^^^^^^^^^^^^

Create map 
"""""""""""

createMap( e\ :sub:`1`\ : List<T\ :sub:`1`\> , e\ :sub:`2`\ : List<T\ :sub:`2`\>) : Map<T\ :sub:`1`\,T\ :sub:`2`\>

The createMap() function returns a map from the values e\ :sub:`1`\ to the values of e\ :sub:`2`\. Note e\ :sub:`1`\ and e\ :sub:`2`\ must be lists of equal length.

Map to values
"""""""""""""""

mapToValues(e\ :sub:`1`\ : Map<T\ :sub:`1`\, T\ :sub:`2`\>) : List<T\ :sub:`2`\>

the mapToValues() function returns a list containing all the values of e\ :sub:`1`\.

Math functions
^^^^^^^^^^^^^^

Absolute 
"""""""""

abs( e\ :sub:`1`\ : Float ) : Float

The abs() function returns the absolute value of e\ :sub:`1`\. E.g. ``abs(-4)`` returns 4.

Bound
""""""

bound( e\ :sub:`1`\ : Float , e\ :sub:`2`\ : Float ,  e\ :sub:`3` \ : Float)  : Float


the bound() function returns e\ :sub:`1`\ bounded within e\ :sub:`2`\ and e\ :sub:`3`\. E.g. ``bound(-3 , -2 , 3)`` returns -3.



Max
""""""

max( e\ :sub:`1`\ : Float , e\ :sub:`2`\ )  : Float


the max() function returns the maximum of e\ :sub:`1`\ and e\ :sub:`2`\. E.g. ``max(-3 , -2 )`` returns -2.


Min
""""""

min( e\ :sub:`1`\ : Float , e\ :sub:`2`\ )  : Float


the min() function returns the minimum of e\ :sub:`1`\ and e\ :sub:`2`\. E.g. ``min(-3 , -2 )`` returns -3.


Power
"""""

pow( e\ :sub:`1`\ : Float , e\ :sub:`2`\ : Float ) : Float


The pow() function returns e\ :sub:`1`\ to the power of e\ :sub:`2`\. E.g. ``pow(4 , 3)`` returns 4 to the power of 3.


Sqr - Should be renamed to sqrt
"""""""""""""""""""""""""""""""""""

sqr( e\ :sub:`1`\ : Float ) : Float

the sqr() function returns the square root of e\ :sub:`1`\.



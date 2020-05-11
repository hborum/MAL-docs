Constructs
====================================

Declaration ``d``
-------------------


``init s₁ end``
^^^^^^^^^^^^^^^^^^^^^^^

The init declaration specifies how to initialize a portfolio with the statement s₁.

Example
"""""""""

.. code-block:: text
  :linenos:

    init
      update pol in Policies
      with
        pol.Reserve = 3
      end
    end


``manage s₁ end``
^^^^^^^^^^^^^^^^^^^^^^^

Example
"""""""""

.. code-block:: text
  :linenos:

  manage
         update pol in Policies
         with
           pol.Reserve = 3
         end
  end


The manage declaration specifies how to manage a portfolio with the statement s₁.


``fun f (x₁ : Type₁, ... , xₙ  : Typeₙ) = e₁``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Specifies the function f(x₁, ... , xₙ). 
The arguments of ``f`` is x₁ to xₙ. The body of the function is e₁.

Example
"""""""""

.. code-block:: text
  :linenos:

  fun square(x : Float) = x * x


``action a (x₁ : Type₁, ... , xₙ  : Typeₙ) = s₁``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Specifies the action f(x₁, ... , xₙ). 
The arguments of ``a`` is x₁ to xₙ. The body of the action is s₁.

Example
"""""""""

.. code-block:: text
  :linenos:

  action updateReserve(policy : Policy , newReserve : Float) = 
    policy.Reserve = newReserve


Statements ``s``
-----------------


``update x in e₁ [where e₂] with s₁ end``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Iterates over the list ``e₁`` and performs the update specified by ``s₁``. 
It is optional to specify ``where e₂`` as a filter

Example
"""""""""""

.. code-block:: text
  :linenos:

  update pol in Policies
  with
    pol.Reserve = 3
  end


.. code-block:: text
  :linenos:

  update pol in Policies
  where Reserve < -2
  with
    pol.Reserve = 3
  end

``let x = e₁``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Declares the variable ``x`` with the value of ``e₁``
Example
"""""""""""

.. code-block:: text
  :linenos:

  update pol in Policies
  with
    let newReserve = 3 * 3
    pol.Reserve = newReserve
  end


``do a(e₁, .., eₙ)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Performs the action ``a`` using the arguments ``e₁`` to ``eₙ``.

Example
"""""""""""

.. code-block:: text
  :linenos:

  update pol in Policies
  with
    do updateReserve(pol, 42)
  end


Expressions ``e``
------------------


``e₁:Tag``
^^^^^^^^^^^^^^^^^^^^^^^^

Filters the list ``e₁`` using ``Tag``.

.. code-block:: text
  :linenos:

  update group in Groups:Interest
  with
    group.Reserve = 33
  end

.. code-block:: text
  :linenos:

  update group in Groups:{Interest, Expense}
  with
    group.Reserve = 33
  end


``let x = e₁ in e₂ end``
^^^^^^^^^^^^^^^^^^^^^^^^^^

Binds the variable ``x`` to ``e₁`` within ``e₂``.

.. code-block:: text
  :linenos:

  update group in Groups:Interest
  with
    group.Reserve = 
      let newReserve = 33 
      in 
        newReserve * newReserve 
      end
  end

``if e₁ then e₂ else e₃``
^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns ``e₂`` when ``e₁`` is ``true`` and ``e₃`` when ``e₁`` is ``false``.

.. code-block:: text
  :linenos:

  fun absolute(v : Float) =
    if v < 0
    then v * 0 - 1
    else v


``match e₁ with | Tag xᵢ -> eᵢ end``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns the value ``eᵢ`` with a tag matching the value of ``e₁``.

.. code-block:: text
  :linenos:

  fun interestFee(group : Group) =
    match group with
    | Interest iGrp -> iGrp.Fee
    | Expense eGrp -> 0
    | Risk rGrp -> 0
    end


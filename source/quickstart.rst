Quick Start
====================================

In order to simulate a projection using an MAL-file, say ``demo.fmal``.
We will assume the following folder structure.

.. code-block:: text

	.
	+-- run-debug.bat
	+-- examples
	|   +-- demo.fmal
	|   +-- excelInput
	|   +-- ....
	+-- out
	|   +-- ...
	...

To run ``demo.fmal`` use the ``--run`` flag.

``run-debug.bat --run examples\demo.fmal result``

Which produces a projection output in ``out/result.xslt``

And generates the files:
  * ``out/FMAGen.cs``
  * ``out/FMAGen.dll`` 
  * ``out/FMAGen.pbd``.
  

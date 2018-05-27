Installation
============

Dependencies
------------

Python 2.7 or Python 3 with installed SAP HANA client.

SQLAlchemy-HANA relies on the existing database clients for the underlying
database connection. It can use the supported SAP HANA Python Driver **hdbcli**
(supported since SAP HANA SPS 2) or the open-source project **PyHDB**.

For installing a SAP HANA database client please consult the respective
documentation:

* `Install SAP HANA Python Client <https://help.sap.com/viewer/0eec0d68141541d1b07893a39944924e/2.0.02/en-US/39eca89d94ca464ca52385ad50fc7dea.html>`_
* `Install PyHDB <https://github.com/SAP/PyHDB#install>`_

Install SQLAlchemy-HANA
-----------------------

Install from Python Package Index:

.. code-block:: bash

    $ pip install sqlalchemy-hana

You can also install the latest development version directly from a cloned
git repository.

.. code-block:: bash

    $ git clone https://github.com/SAP/sqlalchemy-hana.git
    $ cd sqlalchemy-hana
    $ python setup.py install


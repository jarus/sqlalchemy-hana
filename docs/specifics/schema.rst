Schema Definition
=================

SQLAlchemy provides an expressive system for describing database schemas.
To allow usage of additional features like different table types,
SQLAlchemy-HANA provides additional interfaces.

Case sensitivity in object names
--------------------------------

In SAP HANA, all case insensitive identifiers e.g. table names are
represented using uppercase text.
In SQLAlchemy on the other hand all lower case identifier names are
considered to be case insensitive.
To settle differences between SAP HANA and other database dialects,
the SQLAlchemy-HANA dialect converts all case insensitive and case
sensitive identifiers to the right notation during operations.

Using a mixed case name on the SQLAlchemy side indicates a case sensitive
identifier, and SQLAlchemy-HANA will quote the name and SAP HANA creates
the object with the same notation.

If you don't need to work existing database objects it is recommended to keep
all identifier in lower case on Python/SQLAlchemy side.

If you have explicit name requirements then you should use the ``quote``
option of SQLAlchemy's schema classes to explicit configure quoting of
identifier names. See attribute ``quote`` of
:py:class:`~sqlalchemy.schema.Table`.


Table types
-----------

SAP HANA supports
`multiple table types <https://help.sap.com/viewer/4fe29514fd584807ac9f2a04f6754767/2.0.03/en-US/20d58a5f75191014b2fe92141b7df228.html>`_
and SQLAlchemy-HANA provides an interface to define the type for a
:py:class:`~sqlalchemy.schema.Table` object or
`Mapping <http://docs.sqlalchemy.org/en/latest/orm/mapping_styles.html#declarative-mapping>`_
class.

The interface is an additional table argument called ``hana_table_type`` which expects
the value ``COLUMN`` or ``ROW``. If the table type isn't specified the database default
will be applied.

Example: Table object
^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

    >>> Table('user_column', metadata, Column('id', Integer), hana_table_type='COLUMN')
    >>> Table('user_row', metadata, Column('id', Integer), hana_table_type='ROW')


Example: Declarative mapping
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

    Base = declarative_base()

    class User(Base):
        __tablename__ = 'user_column'
        __table_args__ = {'hana_table_type': 'COLUMN'}

        id = Column(Integer, primary_key=True)

    class User(Base):
        __tablename__ = 'user_row'
        __table_args__ = {'hana_table_type': 'ROW'}

        id = Column(Integer, primary_key=True)


Auto increment behavior
-----------------------

SQLAlchemy :py:class:`~sqlalchemy.schema.Table` objects which include integer
primary keys are usually assumed to have "auto incrementing" behavior, which
means that primary key values can be automatically generated upon INSERT.

SAP HANA provides support for ``IDENTITY`` columns but the support within
SQLAlchemy-HANA isn't complete yet and it's recommended to use sequences.

Sequences
^^^^^^^^^

To make use of a sequence as source for primary key, use the
:py:class:`~sqlalchemy.schema.Sequence` object and pass it to the
:py:class:`~sqlalchemy.schema.Column` definition.

.. code-block:: python

    >>> Table(
        'my_table', metadata,
        Column('id', Integer, Sequence('id_seq'), primary_key=True)
    )

Constraints
-----------

Foreign key constraint
^^^^^^^^^^^^^^^^^^^^^^

In SAP HANA the following ``ON UPDATE`` and ``ON DELETE`` foreign key referential
actions are available:

* ``SET RESTRICT``
* ``CASCADE``
* ``SET NULL``
* ``SET DEFAULT``

SQLAlchemy assumes by default the referential action ``NO ACTION`` but this
action doesn't exist in SAP HANA and the default is ``SET RESTRICT``.
Therefore any update or deletion in a referenced table are prohibited if
there are any matched records in the referencing table.

CHECK constraints
"""""""""""""""""

Table level check constraints can be created and reflected with the
SQLAlchemy-HANA dialect but column level check constraints are not available
in SAP HANA.

.. code-block:: python

    >>> Table(
        'my_table', metadata,
        Column('id', Integer),
        CheckConstraint('id>5', name='my_check_const')
    )

Column and Data types
---------------------

As with all SQLAlchemy dialects, all
`generic types <https://docs.sqlalchemy.org/en/latest/core/type_basics.html#generic-types>`_
of SQLAlchemy are supported in SAP HANA with SQLAlchemy-HANA.

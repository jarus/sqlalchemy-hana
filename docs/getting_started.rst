Getting started
===============

If you do not have already access to a SAP HANA instance, you can also use
the `SAP HANA Express edition <https://www.sap.com/developer/topics/sap-hana-express.html>`_
to gather first experiences and develop first applications.

After installation of SQLAlchemy-HANA, you can create an
:py:class:`~sqlalchemy.engine.Engine` via :py:func:`~sqlalchemy.create_engine`
which connects to a SAP HANA database.

::

    >>> from sqlalchemy import create_engine
    >>> engine = create_engine('hana://username:password@example.org:30015')

By default the ``hana://`` schema will use hdbcli (from the SAP HANA Client) as
underlying database driver. With the schema ``hana+pyhdb://`` it is also
possible to use PyHDB.

With the engine you can execute directly SQL on the database:

::

    >>> engine.execute('SELECT 1 FROM DUMMY').first()
    (1,)

But also utilize the extensive feature set of SQLAlchemy like describing a database
table with Python syntax and create it in the database.

::

    >>> from sqlalchemy import MetaData, Table, Column, Integer, String
    >>> metadata = MetaData()

    # Define table within Python
    >>> python_ideas = Table(
        'python_ideas', metadata,
        Column('id', Integer, primary_key=True),
        Column('idea', String(100)),
        hana_table_type='COLUMN'
    )

    # Create table in database
    >>> metadata.create_all(engine)

    # Insert first row
    >>> engine.execute(python_ideas.insert(), id=1, idea='Create Flask application with SAP HANA backend')
    >>> engine.execute(python_ideas.insert(), id=2, idea='Create Pyramid application with SAP HANA backend')

    # Fetch inserted rows
    >>> engine.execute(python_ideas.select()).fetchall()
    [
        (1, 'Create Flask application with SAP HANA backend'),
        (2, 'Create Pyramid application with SAP HANA backend')
    ]


For more details about SQLAlchemy features please see the detailed
`SQLAlchemy documentation <http://docs.sqlalchemy.org/en/latest/>`_.


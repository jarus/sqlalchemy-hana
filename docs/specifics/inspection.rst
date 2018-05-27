Runtime Inspection API
======================

SQLAlchemy provides with the
`inspector API <https://docs.sqlalchemy.org/en/latest/core/inspection.html>`_
a convenient way to retrieve SQLAlchemy objects like
:py:class:`~sqlalchemy.schema.Table` from existing database schemas.
SQLAlchemy-HANA provides the underlying function for SQLAlchemy so that the
same API is usable with SAP HANA.

Special reflection method
-------------------------

Beside the normal inspect function the dialect provides an additional method.

.. automethod:: sqlalchemy_hana.dialect.HANAInspector.get_table_oid

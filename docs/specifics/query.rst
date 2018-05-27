Query API
=========

LIMIT/OFFSET support
--------------------

SAP HANA supports ``SELECT`` statements with ``LIMIT`` and ``OFFSET``,
but it only allows ``OFFSET`` in conjunction with ``LIMIT``.

If your application make use of :py:func:`sqlalchemy.orm.Query.offset` or
:py:func:`sqlalchemy.sql.expression.Select.offset` then SQLAlchemy-HANA will
automatically append a ``LIMIT`` of 2147384648 to support applications with
multi database support. The limit value of 2147384648 was chosen, because it
is also the maximum size of a result set.

RETURNING support
-----------------

SAP HANA doesn't support the non-SQL standard ``RETURNING`` clause and
therefore applications can't use
:py:func:`~sqlalchemy.sql.expression.Insert.returning` function.

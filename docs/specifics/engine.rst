SQLAlchemy Engine
=================

Connect to tenant database
--------------------------

SAP HANA supports multiple dedicated and isolated databases within a
SAP HANA system. These databases are referred to as tenant databases.
For more informations see the
`SAP HANA documentation <https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.03/en-US/623afd167e6b48bf956ebb7f2142f058.html>`_.

With the hdbcli database driver you only need to know the tenant database
name for creating an :py:class:`~sqlalchemy.engine.Engine`.

.. code-block:: python

    from sqlalchemy import create_engine
    engine = create_engine('hana://user:pass@host/tenant_db_name')

.. warning::

    PyHDB doesn't support connecting via database name. Instead it is
    required to specify the database SQL port. The method to calculate
    the dynamic SQL port is part of the
    `SAP HANA documentation <https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.03/en-US/440f6efe693d4b82ade2d8b182eb1efb.html>`_.

Connect with credentials from hdbuserstore
------------------------------------------

Beside providing the connection details as database URI, you can refer to
an existing key in the
`SAP HANA Client Secure User Store <https://help.sap.com/viewer/b3ee5778bc2e4a089d3299b82ec762a7/2.0.03/en-US/dd95ac9dbb571014a7d7f0234d762fdb.html>`_.

::

    >>> from sqlalchemy import create_engine
    >>> engine = create_engine('hana://userkey=my_user_store_key')

Creation of an user store key is possible with the ``hdbuserstore`` command-line tool.

::

    hdbuserstore SET <KEY> <host:port> <USERNAME> <PASSWORD>
    hdbuserstore SET my_user_store_key example.org:30015 username password

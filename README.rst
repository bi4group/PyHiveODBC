======
PyHiveODBC
======

PyHiveODBC is based on PyHive to implement the Hive dialect for SQLAlchemy, on pyodbc as Python DB-API, on the HortonWorks Hive ODBC driver (compatible with Microsoft HDInsight). 

pre-requisites:
a working HortonWorks Hive ODBC driver register with unixodbc
(sudo apt-get install unixodbc on ubuntu).

Usage
=====

SQLAlchemy
----------
First install this package to register it with SQLAlchemy (see ``setup.py``).

.. code-block:: python

    from sqlalchemy import *
    from sqlalchemy.engine import create_engine
    from sqlalchemy.schema import *
    
    engine = create_engine('hive+pyodbc://your_hiveodbc_dsn')
    
    logs = Table('my_awesome_data', MetaData(bind=engine), autoload=True)
    print select([func.count('*')], from_obj=logs).scalar()

Note: query generation functionality is not exhaustive or fully tested, but there should be no
problem with raw SQL.

Passing session configuration
-----------------------------

.. code-block:: python

    # SQLAlchemy
    create_engine('presto://user@host:443/hive', connect_args={'protocol': 'https'})
    create_engine(
        'hive://user@host:10000/database',
        connect_args={'configuration': {'hive.exec.reducers.max': '123'}},
    )


Requirements
============

Install using

- ``pip install pyhive[hive]`` for the Hive interface and
- ``pip install pyhive[presto]`` for the Presto interface.

PyHive works with

- Python 2.7 / Python 3
Changelog
=========
See https://github.com/dropbox/PyHive/releases.

Contributing
============
- Please fill out the Dropbox Contributor License Agreement at https://opensource.dropbox.com/cla/ and note this in your pull request.
- Changes must come with tests, with the exception of trivial things like fixing comments. See .travis.yml for the test environment setup.
- Notes on project scope:

  - This project is intended to be a minimal Hive/Presto client that does that one thing and nothing else.
    Features that can be implemented on top of PyHive, such integration with your favorite data analysis library, are likely out of scope.
  - We prefer having a small number of generic features over a large number of specialized, inflexible features.
    For example, the Presto code takes an arbitrary ``requests_session`` argument for customizing HTTP calls, as opposed to having a separate parameter/branch for each ``requests`` option.


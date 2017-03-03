Configuring Dependencies
========================
.. include:: /.shared/work-in-progress.rst

Node.js
-------
If **Node.js** was installed via **nvm** then no further configuration of this package is required. Otherwise please make sure that `node` executable is on your `PATH`.

MongoDB
-------
.. include:: /.shared/russian.rst

First of all authorization should be enabled. It prevents unauthorized users from accessing records in the database. To do this uncomment ``security`` section in file ``/etc/mongod.conf`` and add the following line in it:

.. code-block:: ini

  authorization = enabled

Setting Up Administrators
~~~~~~~~~~~~~~~~~~~~~~~~~
An admin user which has rights to create new users is the first user that should be created. Adding users is performed initializing a JavaScript object on a `mongo shell` with a sufficient dataset and passing it in :func:`db.createUser` function.

.. code-block:: js

  use admin

  var user = {
    "user" : "admin",
    "pwd" : "password",
    roles : [
      {
        "role" : "userAdminAnyDatabase",
        "db" : "admin"
      }
    ]
  }

  db.createUser(user);

Теперь для осуществления действий с системой управления базами данных необходимо подключаться к ней следующим образом::

  $ mongo admin -u admin -p

Добавление пользователей
~~~~~~~~~~~~~~~~~~~~~~~~

Важную часть безопасности системы составляет разграничение полномочий, предоставляемых различным компонентам и приложениям, использующих базу данных. При добавлении полномочий для компонента или приложения необходимо ограничивать его сферу видимости как минимум на уровне базы данных, как максимум - на уровне коллекций, необходимых для работы компонента или приложения.

Добавление пользователя осуществляется аналогично добавлению администратора, за исключением указания рабочей базы данных вместо административной:

.. code-block:: js

  use briskhome

  var user = {
    "user" : "briskhome",
    "pwd" : "password",
    roles : [
      {
        "role" : "readWrite",
        "db" : "briskhome"
      }
    ]
  }

  db.createUser(user);

Предоставление пользователю доступа к нескольким базам данных осуществляется путём добавления объектов в массив ``roles``:

.. code-block:: js

  use briskhome

  var user = {
    "user" : "briskhome",
    "pwd" : "password",
    roles : [
      {
        "role" : "readWrite",
        "db" : "briskhome"
      },
      {
        "role" : "readWrite",
        "db" : "test"
      }
    ]
  }

  db.createUser(user);

Ограничение доступа на уровне коллекций осуществляется путём добавления *пользовательских ролей*. Более подробная информация содержится `в документации MongoDB <https://docs.mongodb.com/v3.0/core/collection-level-access-control/>`_, а также `в блоге Tugdual Grall <http://tgrall.github.io/blog/2015/02/04/introduction-to-mongodb-security/>`_.

OpenLDAP
--------
.. include:: /.shared/russian.rst

Создание схемы
~~~~~~~~~~~~~~

.. note:: **Справочные материалы**: официальная инструкция по созданию схемы от OpenLDAP, руководство по созданию схемы в LinuxGazette, руководство по типам данных в OpenLDAP.

Существуют определённые правила присвоения названий:

*	Каждый атрибут или объект, создаваемый для эксплуатации в рамках Briskhome должен относиться к зоне, официально зарегистрированной в IANA (1.3.6.1.4.1.46307).
*	Название каждого атрибута или объекта должно содержать префикс ``x-briskhome-`` в соответствии с RFC-4520.
*	В произвольной схеме сначала должны быть определены кастомные атрибуты, только затем объекты.

Определение атрибутов
^^^^^^^^^^^^^^^^^^^^^

Пример определения атрибутов ``x-briskhome-PersonFullName`` и ``x-briskhome-ExpirationDate``::

  attributeType ( 1.3.6.1.4.1.46307.3.1.1
    NAME 'x-briskhome-PersonFullName'
    DESC 'Briskhome Person full name'
    SUP name )

  attributeType ( 1.3.6.1.4.1.46307.3.1.2
    NAME 'x-briskhome-ExpirationDate'
    DESC 'Briskhome Guest account expiration date'
    EQUALITY generalizedTimeMatch
    ORDERING generalizedTimeOrderingMatch
    SYNTAX 1.3.6.1.4.1.1466.115.121.1.24
    SINGLE-VALUE )


Определение объектов
^^^^^^^^^^^^^^^^^^^^

Пример определения объектов ``x-briskhome-Person`` и ``x-briskhome-GuestPerson``::

  objectClass ( 1.3.6.1.4.1.46307.3.2.1
    NAME 'x-briskhome-Person'
    DESC 'Briskhome Personal Account'
    SUP person STRUCTURAL
    MUST (x-briskhome-PersonFullName $ userPassword $ uid) )

  objectClass ( 1.3.6.1.4.1.46307.3.2.1.1
    NAME 'x-briskhome-GuestPerson'
    DESC 'Briskhome Guest Account'
    SUP x-briskhome-Person STRUCTURAL
    MUST (x-briskhome-PersonFullName $ x-briskhome-ExpirationDate $ userPassword $ uid) )


Определение макросов
^^^^^^^^^^^^^^^^^^^^

Официальный макрос для ``briskhome.schema`` (не работает)::

  # Official Briskhome Schema v1.0
  # Egor Zaitsev <ezaitsev@briskhome.com>

  # Briskhome's IANA Assigned OID
  objectIdentifier  x-briskhome-OID             1.3.6.1.4.1.46307
  #
  objectIdentifier  x-briskhome-Directory       x-briskhome-OID:3
  #
  objectIdentifier  x-briskhome-AttributeType   x-briskhome-Directory:1
  objectIdentifier  x-briskhome-ObjectClass     x-briskhome-Directory:2
  #
  objectIdentifier  x-briskhome-PersonFullName  x-briskhome-AttributeType:1
  objectIdentifier  x-briskhome-ExpirationDate  x-briskhome-AttributeType:2
  #
  objectIdentifier  x-briskhome-Person          x-briskhome-ObjectClass:1
  #
  objectIdentifier  x-briskhome-GuestPerson     x-briskhome-Person:1
Структура зоны::

  # v1.0 Egor Zaitsev <ezaitsev@briskhome.com> 14.11.2015
  └── x-briskhome-OID (1.3.6.1.4.1.46307)
      ├── x-briskhome-Reserved (0)
      ├── x-briskhome-Reserved (1)
      ├── x-briskhome-Reserved (2)
      ├── x-briskhome-Directory (3)
      │   ├── x-briskhome-Reserved (0)
      │   ├── x-briskhome-AttributeType (1)
      │   │   ├── x-briskhome-Reserved (0)
      │   │   ├── x-briskhome-PersonFullName (1)
      │   │   └── x-briskhome-ExpirationDate (2)
      │   └── x-briskhome-ObjectClass (2)
      │       ├── x-briskhome-Reserved (0)
      │       └── x-briskhome-Person (1)
      │           ├── x-briskhome-Reserved (0)
      │           └── x-briskhome-GuestPerson (1)
      └── x-briskhome-CertificateAuthority (4)
          ├── x-briskhome-Root (0)
          ├── x-briskhome-Component (1)
          └── x-briskhome-User (2)
              └── x-briskhome-rootAuth (65918)


Применение схемы
^^^^^^^^^^^^^^^^
Создаем временный файл::

  $ cd /tmp
  $ nano ldap.conf

.. note:: В случае, если планируется использовать **RADIUS сервер**, необходимо также включить схемы, которые идут в комплекте с ним.

Во временный файл добавляем список всех схем, включая пользовательские::

  # Include OpenLDAP default schemas
  include /etc/ldap/schema/core.schema
  include /etc/ldap/schema/collective.schema
  include /etc/ldap/schema/corba.schema
  include /etc/ldap/schema/cosine.schema
  include /etc/ldap/schema/duaconf.schema
  include /etc/ldap/schema/dyngroup.schema
  include /etc/ldap/schema/inetorgperson.schema
  include /etc/ldap/schema/java.schema
  include /etc/ldap/schema/misc.schema
  include /etc/ldap/schema/nis.schema
  include /etc/ldap/schema/openldap.schema
  include /etc/ldap/schema/ppolicy.schema
  # Include custom Briskhome schema
  include /tmp/briskhome.schema

Создаем временную директорию для работы со схемой::

  $ mkdir ldif_output

Конвертируем пользовательскую схему в формат ldif, который  понимает OpenLDAP::

  $ slaptest -f schema_convert.conf -F ldif_output

Отредактировать файл - заменить в нем значения dn и cn, удалить метаданные::

  $ sudo nano ldif_output/cn=config/cn=schema/cn={12}briskhome.ldif
  ...
  dn: cn=briskhome,cn=schema,cn=config
  cn: briskhome
  ...

Удалить из файла следующие строки (значения могут отличаться)::

  ...
  structuralObjectClass: olcSchemaConfig
  entryUUID: 10dae0ea-0760-102d-80d3-f9366b7f7757
  creatorsName: cn=config
  createTimestamp: 20080826021140Z
  entryCSN: 20080826021140.791425Z#000000#000#000000
  modifiersName: cn=config
  modifyTimestamp: 20080826021140Z
  ...

Добавить схему::

  $ ldapadd -x -W -D cn=admin,cn=config -f ldif_output/cn\=config/cn\=schema/cn\=\{12\}briskhome.ldif

В случае, если не работает авторизация в OpenLDAP, выполняем следующие шаги:

Генерируем пароль админа, после чего останавливаем службу OpenLDAP::

  $ slappasswd -h {MD5}
  $ sudo service slapd stop

Глубоко в LDAP открываем файл и вставляем в него сгенерированный пароль::

  $ sudo nano /etc/ldap/slapd.d/cn\=config/olcDatabase={0}config.ldif
  ...
  dn: olcDatabase={0}config,cn=config
  changetype: modify
  add: olcRootPW
  olcRootPW: {MD5}PASSWORD
  ...

Запускаем службу OpenLDAP::

  $ sudo serice slapd start

Cоздание структуры
~~~~~~~~~~~~~~~~~~

ВНИМАНИЕ: данный раздел еще не прошел практическую проработку.

Изменение пароля
~~~~~~~~~~~~~~~~

..  danger:: Сброс пароля администратора базы требует административного доступа к серверу.

Процесс смены пароля администратора службы каталогов состоит из нескольких шагов. Прежде всего необходимо выяснить некоторые параметры работы службы:

.. code-block:: console

  # ldapsearch -LLL -Y EXTERNAL -H ldapi:/// -b  cn=config olcRootDN=cn=admin,dc=briskhome,dc=com dn olcRootDN olcRootPW

.. code-block:: text
  :emphasize-lines: 4

  SASL/EXTERNAL authentication started
  SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
  SASL SSF: 0
  dn: olcDatabase={1}mdb,cn=config
  olcRootDN: cn=admin,dc=briskhome,dc=com
  olcRootPW: {SHA}ksixAVfgRXavGCpkPefc6hRHL4X=

Таким образом, текущий пароль хеширован с использованием алгоритма **SHA** и хранится в записи ``dn: olcDatabase={1}mdb,cn=config``. Создание нового пароля осуществляется с помощью утилиты **slappasswd**:

.. code-block:: console

  # slappasswd -h {SHA}

Система попросит ввести новый пароль и подтвердить его, а затем выведет на экран хеш введённого пароля::

  New password:
  Re-enter new password:
  {SHA}W6ph5Mm5Pz8GgiULbPgzG37mj9g=

После получения хеша нового пароля необходимо внести его в базу данных службы каталогов:

.. code-block:: console

  # ldapmodify -Y EXTERNAL -H ldapi:///

.. code-block:: text

  SASL/EXTERNAL authentication started
  SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
  SASL SSF: 0

Необходимо ввести следующие команды и дважды нажать ``Enter``:

.. code-block:: text

  dn: olcDatabase={1}hdb,cn=config
  replace: olcRootPW
  olcRootPW: {SHA}W6ph5Mm5Pz8GgiULbPgzG37mj9g=

В случае успешного исполнения операции ответ службы каталогов должен быть следующим:

.. code-block:: text

  modifying entry "olcDatabase={1}hdb,cn=config"

По завершении смены пароля административной учётной записи службы каталогов необходимо перезапустить службу **slapd**:

.. code-block:: console

  # service slapd restart

OpenSSL
-------

Optional Dependencies
---------------------
DNS Server
~~~~~~~~~~

Freeradius
~~~~~~~~~~

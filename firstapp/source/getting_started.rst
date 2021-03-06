.. only:: shade

    ===================================================================
    How to launch and delete OpenStack instances using Python and Shade
    ===================================================================

.. only:: jclouds

    ===================================================================
    How to launch and delete OpenStack instances using Java and Jclouds
    ===================================================================

.. only:: fog

    ===============================================================
    How to launch and delete OpenStack instances using Ruby and Fog
    ===============================================================

.. only:: libcloud

    =============================================================================
    How to launch and delete OpenStack instances using Python and Apache Libcloud
    =============================================================================

=============
What you need
=============

This tutorial will guide you with step-by-step instructions on how to
get a basic authentication token on DreamCompute and launch a virtual
machine.

You will need to have your username, password and project ID
available. This information can be easily retrieved from
`DreamCompute's dashboard, in the openrc file
<https://dashboard.dreamcompute.com/project/access_and_security/api_access/openrc/>`_.

You must also need to have

.. only:: dotnet

      `OpenStack Cloud SDK for Microsoft .NET 1.4.0.1 or later installed
      <https://www.nuget.org/packages/openstack.net>`_.

      .. note::

         To install the OpenStack .NET SDK, use the NeGet Package Manager that
         is included with Visual Studio and Xamarin Studio. You simply add a
         package named 'openstack.net' and the NeGet Package Manager
         automatically installs the necessary dependencies.

      .. warning::

         This document has not yet been completed for the .NET SDK.

.. only:: fog

      `fog 1.19 or higher installed
      <http://www.fogproject.org/wiki/index.php?title=FOGUserGuide#Installing_FOG>`_
      and working with ruby gems 1.9.

.. only:: jclouds

    `jClouds 1.9 or higher installed <https://jclouds.apache.org/start/install>`_.

    Once jclouds and maven are installed, you can install dependencies with
    \`mvn dependency:copy-dependencies "-DoutputDirectory=./lib"\`, compile the
    program with \`javac -classpath ".:lib/\*" JCloudsNova.java\`, and run the
    program with \`java -classpath ".:lib/\*" JCloudsNova\`

.. only:: libcloud

    `libcloud 0.15.1 or higher installed
    <https://libcloud.apache.org/getting-started.html>`_.

.. only:: pkgcloud

      `pkgcloud 1.2 or higher installed
      <https://github.com/pkgcloud/pkgcloud#getting-started>`_.

     .. highlight:: javascript

.. only:: openstacksdk

     a recent version of `openstacksdk <http://python-openstacksdk.readthedocs.org/en/latest/users/index.html>`_
     installed.

.. only:: phpopencloud

    `a recent version of php-opencloud installed <http://docs.php-opencloud.com/en/latest/>`_.

    .. warning::

       This document has not yet been completed for the php-opencloud SDK.

.. only:: shade

     `a recent version of shade library installed <https://pypi.python.org/pypi/shade/0.11.0>`_.

     .. note:: Before proceeding, install the latest version of shade.

Authenticate
~~~~~~~~~~~~

Add the following code to a file or copy and paste them in an
interactive shell, when available. Make sure to use your username,
password and project ID.

.. only:: jclouds

    .. literalinclude:: ../samples/jclouds/JCloudsNova.java
        :start-after: step-1
        :end-before: step-2

.. only:: fog

    You can use the interactive Ruby shell :code:`irb` to paste the
    code below:

    .. literalinclude:: ../samples/fog/getting_started.rb
        :language: ruby
        :start-after: step-1
        :end-before: step-2

.. only:: libcloud

    You can use an interactive Python shell by calling :code:`python
    -i` or :code:`ipython`.

    .. literalinclude:: ../samples/libcloud/getting_started.py
        :start-after: step-1
        :end-before: step-2

.. only:: openstacksdk

     You can use an interactive Python shell by calling :code:`python -i`
     or :code:`ipython`.

    .. code-block:: python

        .. literalinclude:: ../samples/openstacksdk/getting_started.py
            :start-after: step-1
            :end-before: step-2

.. only:: pkgcloud

    To try it, use an interactive Node.js shell by calling :code:`node` or add
    the following code to a script.

    .. literalinclude:: ../samples/pkgcloud/getting_started.js
        :start-after: step-1
        :end-before: step-2

.. only:: dotnet

    To use the OpenStack .NET SDK, add the following code in the required
    namespace section.

    .. code-block:: c#

        using net.openstack.Core.Domain;
        using net.openstack.Core.Providers;
        using net.openstack.Providers.Rackspace;

    Because all service endpoints use the Identity Service for authentication
    and authorization, place the following code in the 'void Main()'
    entry-point function.

    .. literalinclude:: ../samples/dotnet/getting_started.cs
        :language: c#
        :dedent: 3
        :start-after: step-1
        :end-before: step-2

.. only:: shade

    Create a configuration file to store your user name, password,
    project_name in :file:`~/.config/openstack/clouds.yml`.

    .. literalinclude:: ../samples/shade/clouds.yml
        :language: yaml

    You can use an interactive Python shell by calling :code:`python -i`
    or :code:`ipython`.

    .. literalinclude::  ../samples/shade/getting_started.py
        :start-after: step-1
        :end-before: step-2

.. only:: openstacksdk

    .. note:: If you receive the exception
              :code:`openstack.exceptions.HttpException: HttpException:
              401 Client Error: Unauthorized,` while trying to run one
              of the following API calls please double-check your
              credentials.

This code creates the :code:`conn` object that you can use in the next
steps.

Flavors and images
~~~~~~~~~~~~~~~~~~

To launch an instance, which is just a virtual machine, you choose
a flavor and an image. The flavor represents the size of the instance,
including the number of CPUs and amount of RAM and disk space. An
image is a prepared OS installation from which you clone your
instance. When you boot instances in a public cloud, larger flavors
can be more expensive than smaller ones in terms of monetary cost.

To list the images that are available in your cloud, run some API calls:

.. only:: jclouds

    .. literalinclude:: ../samples/jclouds/JCloudsNova.java
        :start-after: step-2
        :end-before: step-3

    This code returns output like this:

    .. code-block:: none

         Image{id=5e1c34f6-4044-4d19-88df-968ab2ca3df6, name=Debian-8.1, links=[Link{relation=SELF, href=https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/images/5e1c34f6-4044-4d19-88df-968ab2ca3df6}, Link{relation=BOOKMARK, href=https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/images/5e1c34f6-4044-4d19-88df-968ab2ca3df6}, Link{relation=ALTERNATE, type=application/vnd.openstack.image, href=http://10.64.141.201:9292/4ad04455f5e6431c895340bf3630e7bb/images/5e1c34f6-4044-4d19-88df-968ab2ca3df6}], updated=Thu Aug 27 12:25:22 PDT 2015, created=Thu Aug 27 12:12:47 PDT 2015, tenantId=null, userId=null, status=ACTIVE, progress=100, minDisk=0, minRam=0, server=null, metadata={build=1, distro=Debian, quicklaunch=False, revision=8.1-1, verified=True, verified_by=7, version=8.1}}
         Image{id=e81771c0-2944-405c-ba92-3deb0e1b4ce3, name=CentOS-7.0, links=[Link{relation=SELF, href=https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/images/e81771c0-2944-405c-ba92-3deb0e1b4ce3}, Link{relation=BOOKMARK, href=https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/images/e81771c0-2944-405c-ba92-3deb0e1b4ce3}, Link{relation=ALTERNATE, type=application/vnd.openstack.image, href=http://10.64.141.201:9292/4ad04455f5e6431c895340bf3630e7bb/images/e81771c0-2944-405c-ba92-3deb0e1b4ce3}], updated=Tue Aug 11 13:17:31 PDT 2015, created=Tue Aug 11 13:05:54 PDT 2015, tenantId=null, userId=null, status=ACTIVE, progress=100, minDisk=0, minRam=0, server=null, metadata={build=10, distro=Centos, quicklaunch=False, revision=7.0-10, verified=True, verified_by=6, version=7.0}}
         Image{id=90d5e049-aaed-4abc-aa75-60c2b1ed6516, name=Ubuntu-14.04, links=[Link{relation=SELF, href=https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/images/90d5e049-aaed-4abc-aa75-60c2b1ed6516}, Link{relation=BOOKMARK, href=https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/images/90d5e049-aaed-4abc-aa75-60c2b1ed6516}, Link{relation=ALTERNATE, type=application/vnd.openstack.image, href=http://10.64.141.201:9292/4ad04455f5e6431c895340bf3630e7bb/images/90d5e049-aaed-4abc-aa75-60c2b1ed6516}], updated=Wed Aug 05 09:38:59 PDT 2015, created=Wed Aug 05 09:26:21 PDT 2015, tenantId=null, userId=null, status=ACTIVE, progress=100, minDisk=0, minRam=0, server=null, metadata={build=16, distro=Ubuntu, quicklaunch=False, revision=14.04-16, verified=True, verified_by=5, version=14.04}}
         Image{id=f044ae8f-e0e1-4fb4-baff-0363c19a6638, name=CoreOS, links=[Link{relation=SELF, href=https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/images/f044ae8f-e0e1-4fb4-baff-0363c19a6638}, Link{relation=BOOKMARK, href=https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/images/f044ae8f-e0e1-4fb4-baff-0363c19a6638}, Link{relation=ALTERNATE, type=application/vnd.openstack.image, href=http://10.64.141.201:9292/4ad04455f5e6431c895340bf3630e7bb/images/f044ae8f-e0e1-4fb4-baff-0363c19a6638}], updated=Thu May 21 16:17:53 PDT 2015, created=Thu May 21 15:24:28 PDT 2015, tenantId=null, userId=null, status=ACTIVE, progress=100, minDisk=0, minRam=0, server=null, metadata={}}
         Image{id=2827d7cc-8cbb-4ce9-9b61-dadc2436144e, name=Fedora-20, links=[Link{relation=SELF, href=https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/images/2827d7cc-8cbb-4ce9-9b61-dadc2436144e}, Link{relation=BOOKMARK, href=https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/images/2827d7cc-8cbb-4ce9-9b61-dadc2436144e}, Link{relation=ALTERNATE, type=application/vnd.openstack.image, href=http://10.64.141.201:9292/4ad04455f5e6431c895340bf3630e7bb/images/2827d7cc-8cbb-4ce9-9b61-dadc2436144e}], updated=Tue Mar 17 11:09:36 PDT 2015, created=Tue Mar 17 10:58:31 PDT 2015, tenantId=null, userId=null, status=ACTIVE, progress=100, minDisk=0, minRam=0, server=null, metadata={}}
         Image{id=42a0101d-31c1-4c09-a70d-8f75f887ee27, name=Fedora-21, links=[Link{relation=SELF, href=https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/images/42a0101d-31c1-4c09-a70d-8f75f887ee27}, Link{relation=BOOKMARK, href=https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/images/42a0101d-31c1-4c09-a70d-8f75f887ee27}, Link{relation=ALTERNATE, type=application/vnd.openstack.image, href=http://10.64.141.201:9292/4ad04455f5e6431c895340bf3630e7bb/images/42a0101d-31c1-4c09-a70d-8f75f887ee27}], updated=Mon Mar 16 09:54:02 PDT 2015, created=Thu Mar 12 09:52:16 PDT 2015, tenantId=null, userId=null, status=ACTIVE, progress=100, minDisk=0, minRam=0, server=null, metadata={}}
         Image{id=683e165f-96b1-4ee5-8747-d15aade0dbff, name=CentOS-6.6, links=[Link{relation=SELF, href=https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/images/683e165f-96b1-4ee5-8747-d15aade0dbff}, Link{relation=BOOKMARK, href=https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/images/683e165f-96b1-4ee5-8747-d15aade0dbff}, Link{relation=ALTERNATE, type=application/vnd.openstack.image, href=http://10.64.141.201:9292/4ad04455f5e6431c895340bf3630e7bb/images/683e165f-96b1-4ee5-8747-d15aade0dbff}], updated=Wed Mar 18 14:54:52 PDT 2015, created=Fri Mar 06 17:41:34 PST 2015, tenantId=null, userId=null, status=ACTIVE, progress=100, minDisk=0, minRam=0, server=null, metadata={}}
         Image{id=de4d521b-6630-4361-8b9a-b2fa640cdfa0, name=CentOS-6.5, links=[Link{relation=SELF, href=https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/images/de4d521b-6630-4361-8b9a-b2fa640cdfa0}, Link{relation=BOOKMARK, href=https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/images/de4d521b-6630-4361-8b9a-b2fa640cdfa0}, Link{relation=ALTERNATE, type=application/vnd.openstack.image, href=http://10.64.141.201:9292/4ad04455f5e6431c895340bf3630e7bb/images/de4d521b-6630-4361-8b9a-b2fa640cdfa0}], updated=Wed Mar 18 14:53:58 PDT 2015, created=Mon Nov 17 14:42:15 PST 2014, tenantId=null, userId=null, status=ACTIVE, progress=100, minDisk=0, minRam=0, server=null, metadata={}}
         Image{id=5011c04a-f760-4dc5-9772-7e30d98647e6, name=Ubuntu-12.04-Precise, links=[Link{relation=SELF, href=https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/images/5011c04a-f760-4dc5-9772-7e30d98647e6}, Link{relation=BOOKMARK, href=https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/images/5011c04a-f760-4dc5-9772-7e30d98647e6}, Link{relation=ALTERNATE, type=application/vnd.openstack.image, href=http://10.64.141.201:9292/4ad04455f5e6431c895340bf3630e7bb/images/5011c04a-f760-4dc5-9772-7e30d98647e6}], updated=Mon Oct 27 15:05:35 PDT 2014, created=Mon Oct 27 15:02:18 PDT 2014, tenantId=null, userId=null, status=ACTIVE, progress=100, minDisk=0, minRam=0, server=null, metadata={}}
         Image{id=b4efbc2a-6130-4f2e-b436-55a618c4de20, name=Debian-7.0-Wheezy, links=[Link{relation=SELF, href=https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/images/b4efbc2a-6130-4f2e-b436-55a618c4de20}, Link{relation=BOOKMARK, href=https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/images/b4efbc2a-6130-4f2e-b436-55a618c4de20}, Link{relation=ALTERNATE, type=application/vnd.openstack.image, href=http://10.64.141.201:9292/4ad04455f5e6431c895340bf3630e7bb/images/b4efbc2a-6130-4f2e-b436-55a618c4de20}], updated=Wed Oct 15 15:42:52 PDT 2014, created=Mon Jul 14 12:02:15 PDT 2014, tenantId=null, userId=null, status=ACTIVE, progress=100, minDisk=10, minRam=1024, server=null, metadata={}}

.. only:: fog

    .. literalinclude:: ../samples/fog/getting_started.rb
        :language: ruby
        :start-after: step-2
        :end-before: step-3

    This code returns output like this:

    .. code-block:: none

        {"images"=>[{"id"=>"5e1c34f6-4044-4d19-88df-968ab2ca3df6",
        "links"=>[{"href"=>"https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/images/5e1c34f6-4044-4d19-88df-968ab2ca3df6",
        "rel"=>"self"},
        {"href"=>"https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/images/5e1c34f6-4044-4d19-88df-968ab2ca3df6",
        "rel"=>"bookmark"},
        {"href"=>"http://10.64.141.226:9292/4ad04455f5e6431c895340bf3630e7bb/images/5e1c34f6-4044-4d19-88df-968ab2ca3df6",
        "type"=>"application/vnd.openstack.image", "rel"=>"alternate"}],
        "name"=>"Debian-8.1"}, {"id"=>"e81771c0-2944-405c-ba92-3deb0e1b4ce3",
        "links"=>[{"href"=>"https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/images/e81771c0-2944-405c-ba92-3deb0e1b4ce3",
        "rel"=>"self"},
        {"href"=>"https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/images/e81771c0-2944-405c-ba92-3deb0e1b4ce3",
        "rel"=>"bookmark"},
        {"href"=>"http://10.64.141.226:9292/4ad04455f5e6431c895340bf3630e7bb/images/e81771c0-2944-405c-ba92-3deb0e1b4ce3",
        "type"=>"application/vnd.openstack.image", "rel"=>"alternate"}],
        "name"=>"CentOS-7.0"}, {"id"=>"90d5e049-aaed-4abc-aa75-60c2b1ed6516",
        "links"=>[{"href"=>"https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/images/90d5e049-aaed-4abc-aa75-60c2b1ed6516",
        "rel"=>"self"},
        {"href"=>"https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/images/90d5e049-aaed-4abc-aa75-60c2b1ed6516",
        "rel"=>"bookmark"},
        {"href"=>"http://10.64.141.226:9292/4ad04455f5e6431c895340bf3630e7bb/images/90d5e049-aaed-4abc-aa75-60c2b1ed6516",
        "type"=>"application/vnd.openstack.image", "rel"=>"alternate"}],
        "name"=>"Ubuntu-14.04"},
        {"id"=>"f044ae8f-e0e1-4fb4-baff-0363c19a6638",
        "links"=>[{"href"=>"https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/images/f044ae8f-e0e1-4fb4-baff-0363c19a6638",
        "rel"=>"self"},
        {"href"=>"https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/images/f044ae8f-e0e1-4fb4-baff-0363c19a6638",
        "rel"=>"bookmark"},
        {"href"=>"http://10.64.141.226:9292/4ad04455f5e6431c895340bf3630e7bb/images/f044ae8f-e0e1-4fb4-baff-0363c19a6638",
        "type"=>"application/vnd.openstack.image", "rel"=>"alternate"}],
        "name"=>"CoreOS"}, {"id"=>"2827d7cc-8cbb-4ce9-9b61-dadc2436144e",
        "links"=>[{"href"=>"https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/images/2827d7cc-8cbb-4ce9-9b61-dadc2436144e",
        "rel"=>"self"},
        {"href"=>"https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/images/2827d7cc-8cbb-4ce9-9b61-dadc2436144e",
        "rel"=>"bookmark"},
        {"href"=>"http://10.64.141.226:9292/4ad04455f5e6431c895340bf3630e7bb/images/2827d7cc-8cbb-4ce9-9b61-dadc2436144e",
        "type"=>"application/vnd.openstack.image", "rel"=>"alternate"}],
        "name"=>"Fedora-20"}

.. only:: libcloud

    .. literalinclude:: ../samples/libcloud/getting_started.py
        :start-after: step-2
        :end-before: step-3

    This code returns output like this:

    .. code-block:: python

        <NodeImage: id=840b74a6-500a-45f0-92fc-250a4650cb2d, name=CoreOS, driver=OpenStack  ...>
        <NodeImage: id=a8484bc7-f6fb-45fd-9dfb-409c7c6e9bc5, name=Debian-8.3, driver=OpenStack  ...>
        <NodeImage: id=152f56d5-f5df-4d2f-8be0-70f79122c2af, name=Debian-7.9, driver=OpenStack  ...>
        <NodeImage: id=f53d69f6-4b15-4ef6-af70-e0c37e314c64, name=Fedora-23, driver=OpenStack  ...>
        <NodeImage: id=12f6a911-00a2-42eb-8712-d930da2da81f, name=Ubuntu-16.04, driver=OpenStack  ...>
        <NodeImage: id=5091963c-6132-4590-a5c5-ec720a2afe88, name=test, driver=OpenStack  ...>
        <NodeImage: id=e0139772-0aa3-43d5-a480-ba438bb6fc8e, name=volume-testing-snap, driver=OpenStack  ...>
        <NodeImage: id=873e4bab-ed23-4096-83fb-ee8b0dd2f5a3, name=Ubuntu-15.10, driver=OpenStack  ...>
        <NodeImage: id=03f89ff2-d66e-49f5-ae61-656a006bbbe9, name=Ubuntu-14.04, driver=OpenStack  ...>
        <NodeImage: id=c1e8c5b5-bea6-45e9-8202-b8e769b661a4, name=CentOS-7, driver=OpenStack  ...>
        <NodeImage: id=10ff94ea-18dc-4790-8ac8-84e6ac9f3132, name=CentOS-6, driver=OpenStack  ...>


.. only:: pkgcloud

    .. literalinclude:: ../samples/pkgcloud/getting_started.js
        :start-after: step-2
        :end-before: step-3

    This code returns output like this:

    .. code-block:: none

        id: 6c7f5627-ca40-4781-ac34-4d9af53d4b29
        name: Fedora 22 - Updated
        created: 2015-08-17T03:53:17Z
        updated: 2015-08-17T04:53:12Z
        status: ACTIVE

        ...
        id: 2cccbea0-cea9-4f86-a3ed-065c652adda5
        name: Ubuntu 14.04
        created: 2015-08-13T02:25:10Z
        updated: 2015-08-13T02:43:38Z
        status: ACTIVE

.. only:: dotnet

    .. literalinclude:: ../samples/dotnet/getting_started.cs
        :language: c#
        :dedent: 3
        :start-after: step-2
        :end-before: step-3

    This code returns output like this:

    .. code-block:: none

        Image Id: dce1a289-2ad5-4aaa-a7a6-fe30adc2094e - Image Name: snap1
        Image Id: 97f55846-6ea5-4e9d-b437-bda97586bd0c - Image Name: cirros-0.3.4-x86_64-uec
        Image Id: 3e0e8270-0da4-4fec-bfc7-eeb763604cad - Image Name: cirros-0.3.4-x86_64-uec-ramdisk
        Image Id: 0b151382-d2f1-44d7-835b-6408bd523917 - Image Name: cirros-0.3.4-x86_64-uec-kernel

.. only:: shade

    .. literalinclude:: ../samples/shade/getting_started.py
        :start-after: step-2
        :end-before: step-3

    This code returns output like this:

    .. code-block:: none

        checksum: 750a56555d4ec7303f5dc33b007ff632
        container_format: bare
        created_at: '2014-07-14T19:02:15Z'
        direct_url:
        rbd://7e14670e-a6f8-445b-b632-4b79bafc4781/masseffect-images/b4efbc2a-6130-4f2e-b436-55a618c4de20/snap
        disk_format: raw
        file: /v2/images/b4efbc2a-6130-4f2e-b436-55a618c4de20/file
        id: b4efbc2a-6130-4f2e-b436-55a618c4de20
        min_disk: 10
        min_ram: 1024
        name: Debian-7.0-Wheezy
        owner: 0bacd8121bb548698f340455b38bf561
        protected: false
        schema: /v2/schemas/image
        size: 5242880000
        status: active
        tags: []
        updated_at: '2014-10-15T22:42:52Z'
        visibility: public

.. only:: openstacksdk

    .. literalinclude:: ../samples/openstacksdk/getting_started.py
        :start-after: step-2
        :end-before: step-3

    You should see output something like this:

    .. code-block:: python

        openstack.image.v1.image.Image(attrs={u'name': u'ubuntu-14.04', u'container_format': u'bare', u'disk_format': u'qcow2', u'checksum': u'6d8f1c8cf05e1fbdc8b543fda1a9fa7f', u'id': u'cb6b7936-d2c5-4901-8678-c88b3a6ed84c', u'size': 258540032}, loaded=True)
        ...

.. note:: You may notice that the image IDs change often even if the
          names of the images remain the same as the images are
          updated over time.

You can also get information about available flavors using API calls:

.. only:: jclouds

    .. literalinclude:: ../samples/jclouds/JCloudsNova.java
        :start-after: step-3
        :end-before: step-4

    This code returns output like this:

    .. code-block:: none

         Flavor{id=100, name=subsonic, links=[Link{relation=SELF, href=https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/flavors/100}, Link{relation=BOOKMARK, href=https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/flavors/100}], ram=1024, disk=80, vcpus=1, swap=Optional.of(), rxtxFactor=Optional.of(1.0), ephemeral=Optional.of(0)}
         Flavor{id=200, name=supersonic, links=[Link{relation=SELF, href=https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/flavors/200}, Link{relation=BOOKMARK, href=https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/flavors/200}], ram=2048, disk=80, vcpus=1, swap=Optional.of(), rxtxFactor=Optional.of(1.0), ephemeral=Optional.of(0)}
         Flavor{id=300, name=lightspeed, links=[Link{relation=SELF, href=https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/flavors/300}, Link{relation=BOOKMARK, href=https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/flavors/300}], ram=4096, disk=80, vcpus=2, swap=Optional.of(), rxtxFactor=Optional.of(1.0), ephemeral=Optional.of(0)}
         Flavor{id=400, name=warpspeed, links=[Link{relation=SELF, href=https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/flavors/400}, Link{relation=BOOKMARK, href=https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/flavors/400}], ram=8192, disk=80, vcpus=4, swap=Optional.of(), rxtxFactor=Optional.of(1.0), ephemeral=Optional.of(0)}
         Flavor{id=500, name=hyperspeed, links=[Link{relation=SELF, href=https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/flavors/500}, Link{relation=BOOKMARK, href=https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/flavors/500}], ram=16384, disk=80, vcpus=8, swap=Optional.of(), rxtxFactor=Optional.of(1.0), ephemeral=Optional.of(0)}
         Flavor{id=600, name=ridiculous, links=[Link{relation=SELF, href=https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/flavors/600}, Link{relation=BOOKMARK, href=https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/flavors/600}], ram=32768, disk=80, vcpus=16, swap=Optional.of(), rxtxFactor=Optional.of(1.0), ephemeral=Optional.of(0)}
         Flavor{id=700, name=ludicrous, links=[Link{relation=SELF, href=https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/flavors/700}, Link{relation=BOOKMARK, href=https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/flavors/700}], ram=65536, disk=80, vcpus=32, swap=Optional.of(), rxtxFactor=Optional.of(1.0), ephemeral=Optional.of(0)}
         Flavor{id=800, name=plaid, links=[Link{relation=SELF, href=https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/flavors/800}, Link{relation=BOOKMARK, href=https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/flavors/800}], ram=131072, disk=80, vcpus=64, swap=Optional.of(), rxtxFactor=Optional.of(1.0), ephemeral=Optional.of(0)}

.. only:: fog

    .. literalinclude:: ../samples/fog/getting_started.rb
        :language: ruby
        :start-after: step-3
        :end-before: step-4

    This code returns output like this:

    .. code-block:: none

        {"flavors"=>[{"id"=>"100",
        "links"=>[{"href"=>"https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/flavors/100",
        "rel"=>"self"},
        {"href"=>"https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/flavors/100",
        "rel"=>"bookmark"}], "name"=>"subsonic"}, {"id"=>"200",
        "links"=>[{"href"=>"https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/flavors/200",
        "rel"=>"self"},
        {"href"=>"https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/flavors/200",
        "rel"=>"bookmark"}], "name"=>"supersonic"}, {"id"=>"300",
        "links"=>[{"href"=>"https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/flavors/300",
        "rel"=>"self"},
        {"href"=>"https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/flavors/300",
        "rel"=>"bookmark"}], "name"=>"lightspeed"}, {"id"=>"400",
        "links"=>[{"href"=>"https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/flavors/400",
        "rel"=>"self"},
        {"href"=>"https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/flavors/400",
        "rel"=>"bookmark"}], "name"=>"warpspeed"}, {"id"=>"500",
        "links"=>[{"href"=>"https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/flavors/500",
        "rel"=>"self"},
        {"href"=>"https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/flavors/500",
        "rel"=>"bookmark"}], "name"=>"hyperspeed"}, {"id"=>"600",
        "links"=>[{"href"=>"https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/flavors/600",
        "rel"=>"self"},
        {"href"=>"https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/flavors/600",
        "rel"=>"bookmark"}], "name"=>"ridiculous"}, {"id"=>"700",
        "links"=>[{"href"=>"https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/flavors/700",
        "rel"=>"self"},
        {"href"=>"https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/flavors/700",
        "rel"=>"bookmark"}], "name"=>"ludicrous"}, {"id"=>"800",
        "links"=>[{"href"=>"https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/flavors/800",
        "rel"=>"self"},
        {"href"=>"https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/flavors/800",
        "rel"=>"bookmark"}], "name"=>"plaid"}]}

.. only:: libcloud

    .. literalinclude:: ../samples/libcloud/getting_started.py
        :start-after: step-3
        :end-before: step-4

    This code returns output like this:

    .. code-block:: python

        <OpenStackNodeSize: id=100, name=gp1.subsonic, ram=1024, disk=80, bandwidth=None, price=0.0, driver=OpenStack, vcpus=1,  ...>
        <OpenStackNodeSize: id=200, name=gp1.supersonic, ram=2048, disk=80, bandwidth=None, price=0.0, driver=OpenStack, vcpus=1,  ...>
        <OpenStackNodeSize: id=300, name=gp1.lightspeed, ram=4096, disk=80, bandwidth=None, price=0.0, driver=OpenStack, vcpus=2,  ...>
        <OpenStackNodeSize: id=400, name=gp1.warpspeed, ram=8192, disk=80, bandwidth=None, price=0.0, driver=OpenStack, vcpus=4,  ...>
        <OpenStackNodeSize: id=50, name=gp1.semisonic, ram=512, disk=80, bandwidth=None, price=0.0, driver=OpenStack, vcpus=1,  ...>
        <OpenStackNodeSize: id=500, name=gp1.hyperspeed, ram=16384, disk=80, bandwidth=None, price=0.0, driver=OpenStack, vcpus=8,  ...>

.. only:: pkgcloud

    .. literalinclude:: ../samples/pkgcloud/getting_started.js
        :start-after: step-3
        :end-before: step-4

    This code returns output like this:

    .. code-block:: none

        id: c46104de-d5fd-4567-ab0b-3dcfd117bd99
        name: m2.xlarge
        ram: 49152
        disk: 30
        vcpus: 12

        ...
        id: cba9ea52-8e90-468b-b8c2-777a94d81ed3
        name: m1.small
        ram: 2048
        disk: 20
        vcpus: 1

.. only:: dotnet

    .. literalinclude:: ../samples/dotnet/getting_started.cs
        :language: c#
        :dedent: 3
        :start-after: step-3
        :end-before: step-4

    This code returns output like this:

    .. code-block:: none

        Flavor Id: 1 - Flavor Name: m1.tiny
        Flavor Id: 2 - Flavor Name: m1.small
        Flavor Id: 3 - Flavor Name: m1.medium
        Flavor Id: 4 - Flavor Name: m1.large
        Flavor Id: 42 - Flavor Name: m1.nano
        Flavor Id: 5 - Flavor Name: m1.xlarge
        Flavor Id: 84 - Flavor Name: m1.micro

.. only:: shade

    .. literalinclude:: ../samples/shade/getting_started.py
        :start-after: step-3
        :end-before: step-4

    This code returns output like this:

    .. code-block:: none

        HUMAN_ID: true
        NAME_ATTR: name
        OS-FLV-DISABLED:disabled: false
        OS-FLV-EXT-DATA:ephemeral: 0
        disk: 80
        ephemeral: 0
        human_id: supersonic
        id: '200'
        is_public: true
        links:
        -   href:
            https://compute.dream.io:8774/v2/5d013ac5962749a49af7ff18c2fb228c/flavors/200
            rel: self
        -   href:
            https://compute.dream.io:8774/5d013ac5962749a49af7ff18c2fb228c/flavors/200
            rel: bookmark
        name: supersonic
        os-flavor-access:is_public: true
        ram: 2048
        swap: ''
        vcpus: 1

.. only:: openstacksdk

    .. literalinclude:: ../samples/openstacksdk/getting_started.py
        :start-after: step-3
        :end-before: step-4

    You should see output something like this:

    .. code-block:: python

        openstack.compute.v2.flavor.FlavorDetail(attrs={u'name': u'm1.tiny', u'links': [{u'href': u'http://controller:8774/v2/96ff6aa79e60423d9848b70d5475c415/flavors/1', u'rel': u'self'}, {u'href': u'http://controller:8774/96ff6aa79e60423d9848b70d5475c415/flavors/1', u'rel': u'bookmark'}], u'ram': 512, u'OS-FLV-DISABLED:disabled': False, u'vcpus': 1, u'swap': u'', u'os-flavor-access:is_public': True, u'rxtx_factor': 1.0, u'OS-FLV-EXT-DATA:ephemeral': 0, u'disk': 1, u'id': u'1'}, loaded=True)

        openstack.compute.v2.flavor.FlavorDetail(attrs={u'name': u'm1.small', u'links': [{u'href': u'http://controller:8774/v2/96ff6aa79e60423d9848b70d5475c415/flavors/2', u'rel': u'self'}, {u'href': u'http://controller:8774/96ff6aa79e60423d9848b70d5475c415/flavors/2', u'rel': u'bookmark'}], u'ram': 2048, u'OS-FLV-DISABLED:disabled': False, u'vcpus': 1, u'swap': u'', u'os-flavor-access:is_public': True, u'rxtx_factor': 1.0, u'OS-FLV-EXT-DATA:ephemeral': 0, u'disk': 20, u'id': u'2'}, loaded=True)

        openstack.compute.v2.flavor.FlavorDetail(attrs={u'name': u'm1.medium', u'links': [{u'href': u'http://controller:8774/v2/96ff6aa79e60423d9848b70d5475c415/flavors/3', u'rel': u'self'}, {u'href': u'http://controller:8774/96ff6aa79e60423d9848b70d5475c415/flavors/3', u'rel': u'bookmark'}], u'ram': 4096, u'OS-FLV-DISABLED:disabled': False, u'vcpus': 2, u'swap': u'', u'os-flavor-access:is_public': True, u'rxtx_factor': 1.0, u'OS-FLV-EXT-DATA:ephemeral': 0, u'disk': 40, u'id': u'3'}, loaded=True)

        ...

Choose an image and flavor for your instance. For example, pick one of
about 1GB RAM, 1 CPU, and a 1GB disk. This example uses the Ubuntu
image with a small flavor, which is a safe choice.

First, tell the connection to get a specified image by using the ID of the
image that you picked in the previous section:

.. only:: jclouds

    .. literalinclude:: ../samples/jclouds/JCloudsNova.java
        :start-after: step-4
        :end-before: step-5

.. only:: fog

    .. literalinclude:: ../samples/fog/getting_started.rb
        :language: ruby
        :start-after: step-4
        :end-before: step-5

.. only:: libcloud

    .. literalinclude:: ../samples/libcloud/getting_started.py
        :start-after: step-4
        :end-before: step-5

    This code returns output like this:

    .. code-block:: python

        <NodeImage: id=90d5e049-aaed-4abc-aa75-60c2b1ed6516, name=Ubuntu-14.04, driver=OpenStack  ...>

.. only:: pkgcloud

    .. literalinclude:: ../samples/pkgcloud/getting_started.js
        :start-after: step-4
        :end-before: step-5

    This code returns output like this:

    .. code-block:: none

        id: 2cccbea0-cea9-4f86-a3ed-065c652adda5
        name: Ubuntu 14.04
        created: 2015-08-13T02:25:10Z
        updated: 2015-08-13T02:43:38Z
        status: ACTIVE

.. only:: dotnet

    .. literalinclude:: ../samples/dotnet/getting_started.cs
        :language: c#
        :dedent: 3
        :start-after: step-4
        :end-before: step-5

    This code returns output like this:

    .. code-block:: none

        Image Id: 97f55846-6ea5-4e9d-b437-bda97586bd0c - Image Name: cirros-0.3.4-x86_64-uec

.. only:: shade

    .. literalinclude:: ../samples/shade/getting_started.py
        :start-after: step-4
        :end-before: step-5

    This code returns output like this:

    .. code-block:: none

        checksum: da578dd59289a35a0ac7744a0bd85cf5
        container_format: bare
        created_at: '2014-10-27T22:05:37Z'
        direct_url:
        rbd://7e14670e-a6f8-445b-b632-4b79bafc4781/masseffect-images/c55094e9-699c-4da9-95b4-2e2e75f4c66e/snap
        disk_format: raw
        file: /v2/images/c55094e9-699c-4da9-95b4-2e2e75f4c66e/file
        id: c55094e9-699c-4da9-95b4-2e2e75f4c66e
        min_disk: 0
        min_ram: 0
        name: Ubuntu-14.04
        owner: 0bacd8121bb548698f340455b38bf561
        protected: false
        schema: /v2/schemas/image
        size: 10737418240
        status: active
        tags: []
        updated_at: '2014-10-27T22:08:55Z'
        visibility: public

.. only:: openstacksdk

    .. literalinclude:: ../samples/openstacksdk/getting_started.py
        :start-after: step-4
        :end-before: step-5

    You should see output something like this:

    .. code-block:: python

        openstack.image.v1.image.Image(attrs={u'name': u'ubuntu-14.04', u'container_format': u'bare', u'disk_format': u'qcow2', u'checksum': u'6d8f1c8cf05e1fbdc8b543fda1a9fa7f', u'id': u'cb6b7936-d2c5-4901-8678-c88b3a6ed84c', u'size': 258540032}, loaded=True)

Next, choose which flavor you want to use:

.. only:: jclouds

    .. literalinclude:: ../samples/jclouds/JCloudsNova.java
        :start-after: step-5
        :end-before: step-6

.. only:: fog

    .. literalinclude:: ../samples/fog/getting_started.rb
        :language: ruby
        :start-after: step-5
        :end-before: step-6

.. only:: libcloud

    .. literalinclude:: ../samples/libcloud/getting_started.py
        :start-after: step-5
        :end-before: step-6

    This code returns output like this:

    .. code-block:: python

        <OpenStackNodeSize: id=100, name=subsonic, ram=1024, disk=80, bandwidth=None, price=0.0, driver=OpenStack, vcpus=1,  ...>

.. only:: pkgcloud

    .. literalinclude:: ../samples/pkgcloud/getting_started.js
        :start-after: step-5
        :end-before: step-6

    This code returns output like this:

    .. code-block:: none


        id: cba9ea52-8e90-468b-b8c2-777a94d81ed3
        name: m1.small
        ram: 2048
        disk: 20
        vcpus: 1

.. only:: dotnet

    .. literalinclude:: ../samples/dotnet/getting_started.cs
        :language: c#
        :dedent: 3
        :start-after: step-5
        :end-before: step-6

    This code returns output like this:

    .. code-block:: none

        Flavor Id: 2 - Flavor Name: m1.small

.. only:: shade

    Because shade accepts either the ID or name in most API calls, specify the
    name for the flavor:

    .. literalinclude:: ../samples/shade/getting_started.py
        :start-after: step-5
        :end-before: step-6

    This code returns output like this:

    .. code-block:: none

        HUMAN_ID: true
        NAME_ATTR: name
        OS-FLV-DISABLED:disabled: false
        OS-FLV-EXT-DATA:ephemeral: 0
        disk: 80
        ephemeral: 0
        human_id: subsonic
        id: '100'
        is_public: true
        links:
        -   href:
            https://compute.dream.io:8774/v2/5d013ac5962749a49af7ff18c2fb228c/flavors/100
            rel: self
        -   href:
            https://compute.dream.io:8774/5d013ac5962749a49af7ff18c2fb228c/flavors/100
            rel: bookmark
        name: subsonic
        os-flavor-access:is_public: true
        ram: 1024
        swap: ''
        vcpus: 1

.. only:: openstacksdk

    .. literalinclude:: ../samples/openstacksdk/getting_started.py
        :start-after: step-5
        :end-before: step-6

    You should see output something like this:

    .. code-block:: python

        openstack.compute.v2.flavor.Flavor(attrs={u'name': u'm1.small', u'links': [{u'href': u'http://controller:8774/v2/96ff6aa79e60423d9848b70d5475c415/flavors/2', u'rel': u'self'}, {u'href': u'http://controller:8774/96ff6aa79e60423d9848b70d5475c415/flavors/2', u'rel': u'bookmark'}], u'ram': 2048, u'OS-FLV-DISABLED:disabled': False, u'vcpus': 1, u'swap': u'', u'os-flavor-access:is_public': True, u'rxtx_factor': 1.0, u'OS-FLV-EXT-DATA:ephemeral': 0, u'disk': 20, 'id': u'2'}, loaded=True)

Now, you can launch the instance.

Launch an instance
~~~~~~~~~~~~~~~~~~

Now you can create the instance:

.. only:: jclouds

    .. literalinclude:: ../samples/jclouds/JCloudsNova.java
        :start-after: step-6
        :end-before: step-7

.. only:: fog

    .. literalinclude:: ../samples/fog/getting_started.rb
        :language: ruby
        :start-after: step-6
        :end-before: step-7

.. only:: libcloud

    .. literalinclude:: ../samples/libcloud/getting_started.py
        :start-after: step-6
        :end-before: step-7

    This code returns output like this:

    .. code-block:: python

        <Node: uuid=3c8fd6fc02916c26d75b5e9bfa53d8b1315671be, name=testing, state=PENDING, public_ips=[], private_ips=[], provider=OpenStack ...>

.. only:: openstacksdk

    .. literalinclude:: ../samples/openstacksdk/getting_started.py
        :start-after: step-6
        :end-before: step-7

    You should see output something like:

    .. code-block:: python

        openstack.compute.v2.server.Server(attrs={'flavorRef': openstack.compute.v2.flavor.Flavor(attrs={u'name': u'm1.small', u'links': [{u'href': u'http://controller:8774/v2/96ff6aa79e60423d9848b70d5475c415/flavors/2', u'rel': u'self'}, {u'href': u'http://controller:8774/96ff6aa79e60423d9848b70d5475c415/flavors/2', u'rel': u'bookmark'}], u'ram': 2048, u'OS-FLV-DISABLED:disabled': False, u'vcpus': 1, u'swap': u'', u'os-flavor-access:is_public': True, u'rxtx_factor': 1.0, u'OS-FLV-EXT-DATA:ephemeral': 0, u'disk': 20, 'id': u'2'}, loaded=True), 'name': 'testing', 'imageRef': openstack.image.v1.image.Image(attrs={u'name': u'ubuntu14.04', u'container_format': u'bare', u'disk_format': u'qcow2', u'checksum': u'6d8f1c8cf05e1fbdc8b543fda1a9fa7f', u'id': u'cb6b7936-d2c5-4901-8678-c88b3a6ed84c', u'size': 258540032}, loaded=True), 'id': u'a1700b84-dc9a-434e-8f7a-40852e97781c'}, loaded=False)

.. only:: pkgcloud

    .. literalinclude:: ../samples/pkgcloud/getting_started.js
        :start-after: step-6
        :end-before: step-7

    This code returns output like this:

    .. code-block:: none

        0d7968dc-4bf4-4e01-b822-43c9c1080d77

.. only:: dotnet

    .. literalinclude:: ../samples/dotnet/getting_started.cs
        :language: c#
        :dedent: 3
        :start-after: step-6
        :end-before: step-7

    This code returns output like this:

    .. code-block:: none

        Instance Id: 4e480ef1-68f0-491f-b237-d9b7f500ef24 at net.openstack.Core.Domain.Link[]

.. only:: shade

    .. literalinclude:: ../samples/shade/getting_started.py
        :start-after: step-6
        :end-before: step-7

If you list existing instances you'll see the newly created one in
there:

.. only:: jclouds

    .. literalinclude:: ../samples/jclouds/JCloudsNova.java
        :start-after: step-7
        :end-before: step-8

.. only:: fog

    .. literalinclude:: ../samples/fog/getting_started.rb
        :language: ruby
        :start-after: step-7
        :end-before: step-8

.. only:: libcloud

    .. literalinclude:: ../samples/libcloud/getting_started.py
        :start-after: step-7
        :end-before: step-8

.. only:: pkgcloud

    .. literalinclude:: ../samples/pkgcloud/getting_started.js
        :start-after: step-7
        :end-before: step-8

.. only:: dotnet

    .. literalinclude:: ../samples/dotnet/getting_started.cs
        :language: c#
        :dedent: 3
        :start-after: step-7
        :end-before: step-8

.. only:: shade

    .. literalinclude:: ../samples/shade/getting_started.py
        :start-after: step-7
        :end-before: step-8

The newly created instance will look something like:

.. only:: openstacksdk

    .. literalinclude:: ../samples/openstacksdk/getting_started.py
        :start-after: step-7
        :end-before: step-8

.. only:: jclouds

    .. code-block:: none

        Server{id=2346a21b-f080-4ec0-bba2-accdab95a479, name=jcloudsdemo, links=[Link{relation=SELF, href=https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/servers/2346a21b-f080-4ec0-bba2-accdab95a479}, Link{relation=BOOKMARK, href=https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/servers/2346a21b-f080-4ec0-bba2-accdab95a479}], uuid=null, tenantId=4ad04455f5e6431c895340bf3630e7bb, userId=17949030c6f4400b99f367fc1dc051ea, updated=Mon Sep 14 10:52:55 PDT 2015, created=Mon Sep 14 10:52:11 PDT 2015, hostId=2d51aec2cb6a97d232ff6866139c70e8e615accf8694c30cb2cb032e, accessIPv4=null, accessIPv6=null, status=ACTIVE, image=Resource{id=90d5e049-aaed-4abc-aa75-60c2b1ed6516, name=null, links=[Link{relation=BOOKMARK, href=https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/images/90d5e049-aaed-4abc-aa75-60c2b1ed6516}]}, flavor=Resource{id=100, name=null, links=[Link{relation=BOOKMARK, href=https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/flavors/100}]}, keyName=null, configDrive=null, addresses={private-network=[Address{addr=2607:f298:6050:8450:f816:3eff:febc:4091, version=6}, Address{addr=10.10.10.76, version=4}]}, metadata={}, extendedStatus=Optional.of(ServerExtendedStatus{taskState=null, vmState=active, powerState=1}), extendedAttributes=Optional.absent(), diskConfig=Optional.of(MANUAL), availabilityZone=Optional.of(iad-1)}

.. only:: libcloud

    .. code-block:: python

        <Node: uuid=3c8fd6fc02916c26d75b5e9bfa53d8b1315671be, name=testing, state=RUNNING, public_ips=[], private_ips=[], provider=OpenStack ...>

.. only:: openstacksdk

    .. code-block:: python

        openstack.compute.v2.server.ServerDetail(attrs={u'OS-EXT-STS:task_state': u'scheduling', u'addresses': {}, u'links': [{u'href': u'http://controller:8774/v2/96ff6aa79e60423d9848b70d5475c415/servers/a1700b84-dc9a-434e-8f7a-40852e97781c', u'rel': u'self'}, {u'href': u'http://controller:8774/96ff6aa79e60423d9848b70d5475c415/servers/a1700b84-dc9a-434e-8f7a-40852e97781c', u'rel': u'bookmark'}], u'image': {u'id': u'cb6b7936-d2c5-4901-8678-c88b3a6ed84c', u'links': [{u'href': u'http://controller:8774/96ff6aa79e60423d9848b70d5475c415/images/cb6b7936-d2c5-4901-8678-c88b3a6ed84c', u'rel': u'bookmark'}]}, u'OS-EXT-STS:vm_state': u'building', u'OS-SRV-USG:launched_at': None, u'flavor': {u'id': u'2', u'links': [{u'href': u'http://controller:8774/96ff6aa79e60423d9848b70d5475c415/flavors/2', u'rel': u'bookmark'}]}, u'id': u'a1700b84-dc9a-434e-8f7a-40852e97781c', u'user_id': u'59f76712914b44819cf311af43946079', 'imageRef': openstack.compute.v2.image.Image(attrs={u'id': u'cb6b7936-d2c5-4901-8678-c88b3a6ed84c', u'links': [{u'href': u'http://controller:8774/96ff6aa79e60423d9848b70d5475c415/images/cb6b7936-d2c5-4901-8678-c88b3a6ed84c', u'rel': u'bookmark'}]}, loaded=False), u'OS-DCF:diskConfig': u'MANUAL', u'accessIPv4': u'', u'accessIPv6': u'', u'progress': 0, u'OS-EXT-STS:power_state': 0, u'OS-EXT-AZ:availability_zone': u'nova', u'config_drive': u'', u'status': u'BUILD', u'updated': u'2015-10-12T13:45:37Z', u'hostId': u'', u'OS-SRV-USG:terminated_at': None, u'key_name': None, 'flavorRef': openstack.compute.v2.flavor.Flavor(attrs={u'id': u'2', u'links': [{u'href': u'http://controller:8774/96ff6aa79e60423d9848b70d5475c415/flavors/2', u'rel': u'bookmark'}]}, loaded=False), u'name': u'testing', u'created': u'2015-10-12T13:45:37Z', u'tenant_id': u'96ff6aa79e60423d9848b70d5475c415', u'os-extended-volumes:volumes_attached': [], u'metadata': {}}, loaded=True)

.. only:: pkgcloud

    .. code-block:: none

        ...
        id: '0d7968dc-4bf4-4e01-b822-43c9c1080d77',
        name: 'testing',
        status: 'PROVISIONING',
        progress: 0,
        imageId: '2cccbea0-cea9-4f86-a3ed-065c652adda5',
        adminPass: undefined,
        addresses: {},
        metadata: {},
        flavorId: '3',
        hostId: 'b6ee757ed678e8c6589ae8cce405eeded89ac914daec73e45a5c50b8',
        created: '2015-06-30T08:17:39Z',
        updated: '2015-06-30T08:17:44Z',
        ...

.. only:: dotnet

    .. code-block:: none

        Instance Id: 4e480ef1-68f0-491f-b237-d9b7f500ef24 at net.openstack.Core.Domain.Link[]

.. only:: fog

    .. code-block:: none

        [  <Fog::Compute::OpenStack::Server
            id="3dfd7960-a76e-47af-afe1-f40c95d93544",
            instance_name=nil,
            addresses={},
            flavor={"id"=>"100",
        "links"=>[{"href"=>"https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/flavors/100",
        "rel"=>"bookmark"}]},
            host_id="a1dc2b7b8b78a71c437e361f8beb2f0c3577891219b319ce2439847b",
            image={"id"=>"90d5e049-aaed-4abc-aa75-60c2b1ed6516",
        "links"=>[{"href"=>"https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/images/90d5e049-aaed-4abc-aa75-60c2b1ed6516",
        "rel"=>"bookmark"}]},
            metadata=    <Fog::Compute::OpenStack::Metadata
              [
                      
              ]
            >,
            links=[{"href"=>"https://compute.dream.io:8774/v2/4ad04455f5e6431c895340bf3630e7bb/servers/3dfd7960-a76e-47af-afe1-f40c95d93544",
        "rel"=>"self"},
        {"href"=>"https://compute.dream.io:8774/4ad04455f5e6431c895340bf3630e7bb/servers/3dfd7960-a76e-47af-afe1-f40c95d93544",
        "rel"=>"bookmark"}],
            name="testinstance",
            personality=nil,
            progress=0,
            accessIPv4="",
            accessIPv6="",
            availability_zone="iad-1",
            user_data_encoded=nil,
            state="BUILD",
            created=2015-09-10 17:30:22 UTC,
            updated=2015-09-10 17:30:28 UTC,
            tenant_id="4ad04455f5e6431c895340bf3630e7bb",
            user_id="17949030c6f4400b99f367fc1dc051ea",
            key_name=nil,
            fault=nil,
            config_drive="",
            os_dcf_disk_config="MANUAL",
            os_ext_srv_attr_host=nil,
            os_ext_srv_attr_hypervisor_hostname=nil,
            os_ext_srv_attr_instance_name=nil,
            os_ext_sts_power_state=0,
            os_ext_sts_task_state="spawning",
            os_ext_sts_vm_state="building"
          >]

.. only:: shade

   .. code-block:: none

       HUMAN_ID: true
        NAME_ATTR: name
        OS-DCF:diskConfig: MANUAL
        OS-EXT-AZ:availability_zone: iad-1
        OS-EXT-STS:power_state: 1
        OS-EXT-STS:task_state: null
        OS-EXT-STS:vm_state: active
        OS-SRV-USG:launched_at: '2015-07-20T20:31:10.000000'
        OS-SRV-USG:terminated_at: null
        accessIPv4: ''
        accessIPv6: ''
        addresses:
            private-network:
            -   OS-EXT-IPS-MAC:mac_addr: fa:16:3e:60:f5:cd
                OS-EXT-IPS:type: fixed
                addr: 2607:f298:6050:4e14:f816:3eff:fe60:f5cd
                version: 6
            -   OS-EXT-IPS-MAC:mac_addr: fa:16:3e:60:f5:cd
                OS-EXT-IPS:type: fixed
                addr: 10.10.10.14
                version: 4
        config_drive: ''
        created: '2015-07-20T20:30:23Z'
        flavor:
            id: '100'
            links:
            -   href:
                https://compute.dream.io:8774/5d013ac5962749a49af7ff18c2fb228c/flavors/100
                rel: bookmark
        hostId: f71865b497e6fa71063e292b11846eb64b5a41cd5c00fbb7465b6a48
        human_id: testing
        id: 67ecebdc-daff-4d84-bd04-bc76c67b48ec
        image:
            id: c55094e9-699c-4da9-95b4-2e2e75f4c66e
            links:
            -   href:
                https://compute.dream.io:8774/5d013ac5962749a49af7ff18c2fb228c/images/c55094e9-699c-4da9-95b4-2e2e75f4c66e
                rel: bookmark
        key_name: null
        links:
        -   href:
            https://compute.dream.io:8774/v2/5d013ac5962749a49af7ff18c2fb228c/servers/67ecebdc-daff-4d84-bd04-bc76c67b48ec
            rel: self
        -   href:
            https://compute.dream.io:8774/5d013ac5962749a49af7ff18c2fb228c/servers/67ecebdc-daff-4d84-bd04-bc76c67b48ec
            rel: bookmark
        metadata: {}
        name: testing
        networks:
            private-network:
            - 2607:f298:6050:4e14:f816:3eff:fe60:f5cd
            - 10.10.10.14
        os-extended-volumes:volumes_attached: []
        progress: 0
        security_groups:
        -   name: default
        status: ACTIVE
        tenant_id: 5d013ac5962749a49af7ff18c2fb228c
        updated: '2015-07-20T20:31:10Z'
        user_id: bfd3dbf1c8a242cd90884408de547bb9

Before you continue, you must do one more thing.

Destroy an instance
~~~~~~~~~~~~~~~~~~~

Cloud resources, such as running instances that you no longer use, can
cost money. To avoid unexpected expenses, destroy cloud resources.

.. only:: jclouds

    .. literalinclude:: ../samples/jclouds/JCloudsNova.java
        :start-after: step-8
        :end-before: step-9

.. only:: fog

    .. literalinclude:: ../samples/fog/getting_started.rb
        :language: ruby
        :start-after: step-8
        :end-before: step-9

.. only:: libcloud

    .. literalinclude:: ../samples/libcloud/getting_started.py
        :start-after: step-8
        :end-before: step-9

.. only:: pkgcloud

    .. literalinclude:: ../samples/pkgcloud/getting_started.js
        :start-after: step-8
        :end-before: step-9

.. only:: dotnet

    .. literalinclude:: ../samples/dotnet/getting_started.cs
        :language: c#
        :dedent: 3
        :start-after: step-8
        :end-before: step-9

.. only:: shade

    .. literalinclude:: ../samples/shade/getting_started.py
        :start-after: step-8
        :end-before: step-9

.. only:: openstacksdk

    .. literalinclude:: ../samples/openstacksdk/getting_started.py
        :start-after: step-8
        :end-before: step-9

If you list the instances again, the instance disappears.

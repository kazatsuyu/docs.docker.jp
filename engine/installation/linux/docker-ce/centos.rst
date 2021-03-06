.. -*- coding: utf-8 -*-
.. URL: https://docs.docker.com/engine/installation/linux/docker-ce/centos/
.. SOURCE:
   doc version: 17.09
      https://github.com/docker/docker.github.io/blob/master/engine/installation/linux/docker-ce/centos.md
.. check date: 2016/11/25
.. Commits on Oct 25, 2017 4b356427472793ddbb7cb824adc774ba082975ff
.. ----------------------------------------------------------------------------

.. Get Docker CE for CentOS

==============================
CentOS 用 Docker CE の入手
==============================

.. sidebar:: 目次

   .. contents:: 
       :depth: 3
       :local:

.. To get started with Docker CE on CentOS, make sure you meet the prerequisites, then install Docker.

CentOS 用 Docker CE を利用するには、 :ref:`動作条件に一致 <ce-prerequisites>` するか確認してから、  :ref:`Docker をインストール <ce-install>` してください。

.. Prerequisites

.. _ce-prerequisites:

動作条件
==========

.. Docker EE customers

Docker EE を利用する方は
------------------------------

.. To install Docker Enterprise Edition (Docker EE), go to Get Docker EE for CentOS instead of this topic.

Docker エンタープライズ版（Docker EE）のインストールは、 **このページではなく** CentOS 用 Docker EE 設定をご覧ください。

.. To learn more about Docker EE, see Docker Enterprise Edition.

Docker EE の詳細を学ぶには、  `Docker Enterprise Edition（英語） <https://www.docker.com/enterprise-edition/>`_ をご覧ください。


.. OS requirements

OS 動作条件
--------------------

.. To install Docker CE, you need a maintained version of CentOS 7. Archived versions aren’t supported or tested.

Docker CE をインストールするには、メンテナンスされている CentOS 7 が必要です。アーカイブされた（過去の）バージョンはサポートやテストを行っていません。

.. The centos-extras repository must be enabled. This repository is enabled by default, but if you have disabled it, you need to re-enable it.

``centos-extras`` リポジトリの有効化が必要です。このリポジトリはデフォルトでは有効ですが、もし無効にしている場合、 `再度有効化 <https://wiki.centos.org/AdditionalResources/Repositories>`_ する必要があります。

.. Uninstall old versions

.. _uninstall-old-versions:

古いバージョンのアンインストール
----------------------------------------

.. Older versions of Docker were called docker or docker-engine. If these are installed, uninstall them, along with associated dependencies.

Docker の古いバージョンの名前は ``docker`` または ``docker-engine`` です。もしこれらがインストールされている場合は、関連する依存関係を含めてアンインストールします。

.. code-block:: bash

   $ sudo yum remove docker \
                     docker-common \
                     docker-selinux \
                     docker-engine

.. It’s OK if yum reports that none of these packages are installed.

`yum` で対象パッケージがインストールされていないと表示されれば、問題ありません。

.. The contents of /var/lib/docker/, including images, containers, volumes, and networks, are preserved. The Docker CE package is now called docker-ce.

イメージ、コンテナ、ボリューム、ネットワークは ``/var/lib/docker/`` の中に保存されたままです。また、 Docker CE のパッケージは ``docker-ce`` と呼びます。


.. Install Docker CE

.. _ce-install:

Docker CE のインストール
==============================

.. You can install Docker CE in different ways, depending on your needs:

必要に応じて Docker CE のインストール方法を選べます。

..    Most users set up Docker’s repositories and install from them, for ease of installation and upgrade tasks. This is the recommended approach.
..    Some users download the RPM package and install it manually and manage upgrades completely manually. This is useful in situations such as installing Docker on air-gapped systems with no access to the internet.
.. In testing and development environments, some users choose to use automated convenience scripts to install Docker.

* ほどんどの利用者は Docker のリポジトリをセットアップし、インストールする方法です。インストールやアップグレード作業を簡単にします。こちらが推奨です。
* 一部のユーザは RPM パッケージをダウンロードし、手動でのインストールや、更新の作業を完全に手動で行います。こちらは Docker をインターネットにアクセスできない領域でインストールしたい場合に便利です。
* テストや開発環境のユーザでは、Docker のインストールを自動化された :ref:`便利なスクリプト <convenience-scripts>` を使えます。

.. Install using the repository

リポジトリを使ったインストール
------------------------------

.. Before you install Docker CE for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

新しいホストマシン上に Docker CE を初めてインストールする前に、Docker リポジトリのセットアップが必要です。その後、リポジトリから Docker のインストールやアップグレードができます。

.. Set up the repository

リポジトリのセットアップ
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. Install required packages. yum-utils provides the yum-config-manager utility, and device-mapper-persistent-data and lvm2 are required by the devicemapper storage driver.

1. 必要なパッケージをインストールします。 ``yum-utils`` は ``yum-config-manager`` ユーティリティを提供します。そして、 ``device-mapper-persistent-data`` と ``lvm2``  は ``devicemapper`` ストレージ・ドライバの使用に必要です。

.. code-block:: bash

   $ sudo yum install -y yum-utils device-mapper-persistent-data lvm2

.. Use the following command to set up the stable repository. You always need the stable repository, even if you want to install builds from the edge or testing repositories as well.

2. 以降のコマンドでは **stable** （安定版）リポジトリをセットアップします。もしも **edge** や **testing** リポジトリからビルドしたものをインストールしたい場合でも、常に **stable** リポジトリが必要です。

.. code-block:: bash

   $ sudo yum-config-manager \
       --add-repo \
       https://download.docker.com/linux/centos/docker-ce.repo

.. Optional: Enable the edge and testing repositories. These repositories are included in the docker.repo file above but are disabled by default. You can enable them alongside the stable repository.

3. **オプション** :  **edge** や **testing** リポジトリを有効化します。これらのリポジトリは ``docker.repo`` ファイルに含まれますが、デフォルトでは無効です。stable リポジトリと併用して有効化できます。

.. code-block:: bash

   $ sudo yum-config-manager --enable docker-ce-edge

.. code-block:: bash

   $ sudo yum-config-manager --enable docker-ce-test

.. You can disable the edge or testing repository by running the yum-config-manager command with the --disable flag. To re-enable it, use the --enable flag. The following command disables the edge repository.

``yum-config-manager`` コマンド実行時に ``--disable`` フラグを使えば、 **edge** や **testing** リポジトリを無効にできます。再び有効にするには ``--enable`` フラグを使います。以下のコマンドは **edge** リポジトリを無効化します。

.. code-block:: bash

   $ sudo yum-config-manager --disable docker-ce-edge

..        Note: Starting with Docker 17.06, stable releases are also pushed to the edge and testing repositories.

.. note::

   Docker 17.06 以降、 stable リリースは **edge** と **testing** リポジトリにも送られます。

..    Learn about stable and edge channels.

:doc:`stable と edge チャンネルについて学ぶ </engine/installation/index>`

.. Install Docker CE

.. _install-docker-ce:

Docker CE のインストール
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

..    Update the yum package index.

1. ``yum`` パッケージ・インデックスを更新します。

.. code-block:: bash

   $ sudo yum makecache fast

..        Warning: If you have multiple Docker repositories enabled, installing or updating without specifying a version in the yum install or yum update command will always install the highest possible version, which may not be appropriate for your stability needs.

.. attention::

   複数の Docker リポジトリを有効にすると、 ``yum install`` または ``yum update`` コマンドでバージョン指定をしなければ、常に最新バージョンをインストールします。そのため、安定性が必要な場合には、適切ではない場合があります。

.. If this is the first time you have refreshed the package index since adding the Docker repositories, you will be prompted to accept the GPG key, and the key’s fingerprint will be shown. Verify that the fingerprint is correct, and if so, accept the key. The fingerprint should match 060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35.

Docker リポジトリを追加後、パッケージ一覧の更新が初めての場合は、 GPG 鍵を受け入れるかどうかの確認と、鍵のフィンガープリント（fingerprint；指紋）を表示します。フィンガープリントが正しいものであると確認したら、鍵を受け入れます。鍵のフィンガープリントが ``9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88`` と一致するのを確認します。

.. Docker is installed but not started. The docker group is created, but no users are added to the group.

Docker をインストールしますが、起動しません。 ``docker`` グループを追加します、グループに所属しているユーザはいません。

..    On production systems, you should install a specific version of Docker CE instead of always using the latest. List the available versions. his example uses the sort -r command to sort the results by version number, highest to lowest, and is truncated.

2. プロダクション（本番向け）システムでは、Docker CE 最新版を使う代わりに、特定のバージョンをインストールすべきでしょう。利用可能なバージョンを一覧表示します。例では ``sort -r`` コマンドを使い、バージョン番号の結果を高いものから低いものへとソートします。また、表示を簡略化します。

.. code-block:: bash

   $ yum list docker-ce --showduplicates | sort -r
   
   docker-ce.x86_64            17.09.ce-1.el7.centos             docker-ce-stable

.. The contents of the list depend upon which repositories are enabled, and will be specific to your version of CentOS (indicated by the .el7 suffix on the version, in this example). Choose a specific version to install. The second column is the version string. You can use the entire version string, but you need to include at least to the first hyphen. The third column is the repository name, which indicates which repository the package is from and by extension its stability level. To install a specific version, append the version string to the package name and separate them by a hyphen (-).

　こちらには有効なリポジトリを表示します。また、特定の CentOS バージョンのものを表示します（この例では ``.el7`` が付いているバージョンを表示 ）。インストールするバージョンを選択します。２列目はバージョンの文字列です。どのバージョンを指定する時も、文字列の前にハイフン記号が必要です。３列目はリポジトリ名です。ここにはパッケージがどのリポジトリを使うかを示し、パッケージ名には安定性とバージョン番号を表示します。特定のバージョンをインストールするには、パッケージ名にハイフン記号（ ``-`` ）でバージョン文字列を追加します。

.. Note: The version string is the package name plus the version up to the first hyphen. In the example above, the fully qualified package name is docker-ce-17.06.1.ce.

.. note::

   バージョン文字列を指定するには、パッケージ名にハイフンを加え、その次にバージョン情報を書きます。先ほどの例では、正式なパッケージ名は ``docker-ce-17.09.0.ce`` になります。


.. code-block:: bash

   $ sudo yum install docker-ce-<正式なバージョン名>

.. Start Docker.

3. Docker を起動します。

.. code-block:: bash

   $ sudo systemctl start docker

..    Verify that docker is installed correctly by running the hello-world image.

4. ``docker`` が正しくインストールされているのを確認するため、 ``hello-world`` イメージを実行します。

.. code-block:: bash

   $ sudo docker run hello-world

..    This command downloads a test image and runs it in a container. When the container runs, it prints an informational message and exits.

このコマンドはテスト用イメージをダウンロードし、コンテナ内で実行します。コンテナを実行したら、情報を表示したあと終了します。

.. Docker CE is installed and running. You need to use sudo to run Docker commands. Continue to Linux postinstall to allow non-privileged users to run Docker commands and for other optional configuration steps.

Docker CE をインストールし、実行しています。Docker コマンドの実行には ``sudo`` が必要です。 引き続き :doc:`/engine/installation/linux/linux-postinstall` から、特権のないユーザで Docker コマンドを実行できるようにしたり、他のオプション設定を進めます。

.. Upgrade Docker CE

Docker CE のアップグレード
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. To upgrade Docker CE, follow the installation instructions, choosing the new version you want to install.

Docker CE をアップグレードするには :ref:`インストール手順 <install-docker-ce>` に従い、インストールしたい新しいバージョンを入れてください。


.. Install from a package

パッケージからインストール
------------------------------

.. If you cannot use Docker’s repository to install Docker CE, you can download the .rpm file for your release and install it manually. You will need to download a new file each time you want to upgrade Docker CE.

Docker CE のインストールに Docker のリポジトリが使えない場合、 ``.rpm`` ファイルをダウンロードし、手作業でインストールできます。Docker CE をアップグレードしたい場合は、新しいファイルのダウンロードが毎回必要です。

..     Go to https://download.docker.com/linux/centos/7/x86_64/stable/Packages/ and download the .rpm file for the Docker version you want to install.

1. https://download.docker.com/linux/centos/7/x86_64/stable/Packages/に移動し、インストールしたい Docker バージョンの ``.rpm`` ファイルをダウンロードします。

..        Note: To install an edge package, change the word stable in the URL to edge. Learn about stable and edge channels.

.. note::

   **edge**  パッケージをインストールするには、URL 中の ``stable`` の文字を ``edge`` にします。 :doc:`stable と edge チャンネルを学ぶにはこちら </engine/installation/index>`  。


..    Install Docker CE, changing the path below to the path where you downloaded the Docker package.

2. Docker CE をインストールするには、以下のパスの場所を Docker パッケージをダウンロードした場所に変更します。

.. code-block:: bash

   $ sudo yum install /path/to/package.rpm

3. Docker を起動します。

.. code-block:: bash

   $ sudo systemctl start docker

..    Verify that Docker CE is installed correctly by running the hello-world image.

4. Docker CE が正しくインストールされているのを確認するため、 ``hello-world`` イメージを実行します。

.. code-block:: bash

   $ sudo docker run hello-world

..    This command downloads a test image and runs it in a container. When the container runs, it prints an informational message and exits.

このコマンドはテスト用イメージをダウンロードし、コンテナ内で実行します。コンテナを実行したら、情報を表示したあと終了します。

.. Docker CE is installed and running. You need to use sudo to run Docker commands. Continue to Post-installation steps for Linux to allow non-privileged users to run Docker commands and for other optional configuration steps.

Docker CE はインストールされ、実行しています。Docker コマンドの実行には ``sudo`` が必要です。 引き続き :doc:`/engine/installation/linux/linux-postinstall` から、特権のないユーザで Docker コマンドを実行できるようにしたり、他のオプション設定を進めます。


.. Upgrade Docker CE

Docker CE のアップグレード
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. To upgrade Docker CE, download the newer package file and repeat the installation procedure, using yum -y upgrade instead of yum -y install, and pointing to the new file.

Docker CE をアップグレードするには、新しいパッケージ・ファイルをダウンロードし、インストール手順の ``yum -y install`` の代わりに ``yum -y upgrade`` を実行します。また、新しいファイルに置き換えます。


.. _convenience-scripts:

便利なスクリプトを使うインストール
----------------------------------------

.. Docker provides convenience scripts at get.docker.com and test.docker.com for installing stable and testing versions of Docker CE into development environments quickly and non-interactively. The source code for the scripts is in the docker-install repository. Using these scripts is not recommended for production environments, and you should understand the potential risks before you use them:

Docker は開発環境に対して迅速かつ非対話的に Docker CE の安定版・テスト版をインストールするために、 `get.docker.com <https://get.docker.com/>`_ と `test.docker.com <https://test.docker.com/>`_ で便利なスクリプトを提供しています。このスクリプトのソースコードは ``docker-install`` `リポジトリ <https://github.com/docker/docker-install>`_ にあります。 **プロダクション環境でのスクリプトの利用は推奨しません** 。また、利用前に、以下の潜在リスクがあるのを理解すべきでしょう。

..    The scripts require root or sudo privileges in order to run. Therefore, you should carefully examine and audit the scripts before running them.
    The scripts attempt to detect your Linux distribution and version and configure your package management system for you. In addition, the scripts do not allow you to customize any installation parameters. This may lead to an unsupported configuration, either from Docker’s point of view or from your own organization’s guidelines and standards.
    The scripts install all dependencies and recommendations of the package manager without asking for confirmation. This may install a large number of packages, depending on the current configuration of your host machine.
    Do not use the convenience script if Docker has already been installed on the host machine using another mechanism.

* スクリプトの実行には ``root`` か ``sudo`` 権限が必要です。そのため、スクリプトを実行する前に、調査と正常性に対して十分にご注意ください。
* スクリプトは自動的に Linux ディストリビューションとバージョン、パッケージ管理システムの検出を試みます。また、スクリプトにはインストール時に何からしらパラメータを渡せません。このため、サポートされていない設定に至ったり、Docker が意図しない、あるいは皆さんの組織のガイドラインや標準から外れたりする場合があります。
* スクリプトを実行すると、パッケージ・マネージャが示す依存関係や推奨パッケージを、すべて自動的にインストールします。これにより、ホストマシン上の設定によっては、非常に多くのパッケージや依存関係のインストールが行われる場合があります。
* 既にホスト・マシン上で別の手法による Docker をインストール済みの環境では、この便利なスクリプトは使用しないでください。

.. This example uses the script at get.docker.com to install the latest stable release of Docker CE on Linux. To install the latest testing version, use test.docker.com instead. In each of the commands below, replace each occurrence of get with test.

次の例は Linux に Docker CE の最新安定版リリースのインストールに、 `get.docker.com`_ のスクリプトを使います。最新テスト版を使いたい場合は、代わりに `test.docker.com`_ を指定します。その場合はコマンド中の ``get`` を ``test`` に置き換えて実行します。

.. warning::

   スクリプトを実行する前に、インターネットからダウンロードしたスクリプトをご確認ください。

.. code-block:: bash

   $ curl -fsSL get.docker.com -o get-docker.sh
   $ sudo sh get-docker.sh
   
   <output truncated>
   
   If you would like to use Docker as a non-root user, you should now consider
   adding your user to the "docker" group with something like:
   
     sudo usermod -aG docker your-user
   
   Remember that you will have to log out and back in for this to take effect!
   
   WARNING: Adding a user to the "docker" group will grant the ability to run
            containers which can be used to obtain root privileges on the
            docker host.
            Refer to https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface
            for more information.

.. Docker CE is installed. It starts automatically on DEB-based distributions. On RPM-based distributions, you need to start it manually using the appropriate systemctl or service command. As the message indicates, non-root users are not able to run Docker commands by default.

これで Docker CE をインストールしました。 ``DEB`` をベースとしたディストリビューションでは、自動的に開始します。 ``RPM`` ベースのディストリビューションでは、適切な ``systemctl`` や ``service`` コマンドを使い、手動で実行する必要があります。メッセージが表示されているように、デフォルトでは root ではないユーザは Docker コマンドを実行できません。

.. Upgrade Docker after using the convenience script

便利なスクリプトを使った後の Docker アップグレード
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. If you installed Docker using the convenience script, you should upgrade Docker using your package manager directly. There is no advantage to re-running the convenience script, and it can cause issues if it attempts to re-add repositories which have already been added to the host machine.

便利なスクリプトを使って Docker をインストールしている場合は、パッケージ・マネージャをとして直接アップグレードを試みるべきでしょう。便利なスクリプトを再度実行する利点は何らありません。また、スクリプトの再実行により、ホストマシン上に既に追加されているリポジトリを再追加するため、何か問題となる可能性があります。

.. Uninstall Docker CE

Docker CE のアンインストール
==============================

..    Uninstall the Docker CE package:

1. Docker CE パッケージをアンインストールします。

.. code-block:: bash

   $ sudo yum remove docker-ce

..    Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:

2. ホスト上のイメージ、コンテナ、ボリューム、その他にカスタマイズした設定ファイルは自動的に削除されません。全てのイメージ、コンテナ、ボリュームを削除するには：

.. code-block:: bash

   $ sudo rm -rf /var/lib/docker

.. You must delete any edited configuration files manually.

編集した設定ファイルは全て手動で削除する必要があります。

.. Next steps

次のステップ
====================

..    Continue to Post-installation steps for Linux
    Continue with the User Guide.

* :doc:`/engine/installation/linux/linux-postinstall` に進む
* :doc:`ユーザガイド </engine/userguide/index>` に進む


.. seealso:: 

   Get Docker CE for CentOS
      https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#uninstall-docker-ce


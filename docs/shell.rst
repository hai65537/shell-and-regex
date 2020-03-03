シェルの基礎
============

.. contents:: 目次
    :depth: 2

定義
----

基本用語
~~~~~~~~

-  POSIX:
   UNIXを始めとするオペレーティングシステムに共通のAPIを定めた標準規格．
-  標準入力 (stdin): 通常はキーボードを指す
-  標準出力 (stdout): 通常はディスプレイを指す
-  標準エラー出力 (stderr): 通常はディスプレイを指す
-  終了ステータス:
   0から255までの整数値．0はコマンドの実行が成功したことを，それ以外は失敗したことを表す．

コマンドラインの規則
~~~~~~~~~~~~~~~~~~~~

.. code:: shell

   $ command arg1 arg2 ...

-  各引数は1個以上の空白文字で区切られていなければならない．
-  ``-`` から始まるものはオプションとして扱う．
-  ファイルを引数として受け付ける場合，``-`` を引数に指定すると，標準入力を入力とする．
-  引数に ``--`` を指定することで，後続の引数はオプションとして解析されない．これはハイフンから始まるファイル名が存在するときなどに便利である．
-  GNU版のコマンドはロングオプションとして ``--`` から始まる文字列を受け付ける．
-  POSIXに規定されていないコマンドは必ずしも上記にしたがうとは限らない．

基本コマンド
------------

本節ではよく使われる基本的なコマンドを紹介する．なお，コマンドの実装はGNU版（Linux）とBSD版（macOS）が有名だが，それぞれに共通のオプションのうち，よく使うものだけを紹介する．中にはPOSIXに準拠していないものもあるため，使用するときは注意する必要がある．

POSIXコマンド
~~~~~~~~~~~~~

- `cat <shell-commands/cat.rst>`_
- `cp <shell-commands/cp.rst>`_
- `cd <shell-commands/cd.rst>`_
- `echo <shell-commands/echo.rst>`_
- `find <shell-commands/find.rst>`_
- `grep <shell-commands/grep.rst>`_
- `ln <shell-commands/ln.rst>`_
- `ls <shell-commands/ls.rst>`_
- `mkdir <shell-commands/mkdir.rst>`_
- `mv <shell-commands/mv.rst>`_
- `read <shell-commands/read.rst>`_
- `rm <shell-commands/rm.rst>`_
- `rmdir <shell-commands/rmdir.rst>`_
- `sed <shell-commands/sed.rst>`_
- `xargs <shell-commands/xargs.rst>`_

非POSIXコマンド
~~~~~~~~~~~~~~~

- `tar <shell-commands/tar.rst>`_
- `wget <shell-commands/wget.rst>`_
- `which <shell-commands/which.rst>`_

参考サイト
^^^^^^^^^^

- `<https://pubs.opengroup.org/onlinepubs/9699919799/idx/utilities.html>`_
- `<https://ss64.com/osx/>`_
- `<http://man7.org/linux/man-pages/dir_section_1.html>`_

シェルスクリプト
----------------

リダイレクト
~~~~~~~~~~~~

コマンドの出力をファイルへの入力にする機能．``>`` は上書き，``>>`` は追記を表す．

例）a.txtとb.txtの内容を結合してc.txtに記録する:

.. code:: shell

    cat a.txt b.txt > c.txt

例）すべてのファイルを列挙する．標準エラー出力に出力される，アクセスできないディレクトリを ``error.log`` に追記する:

.. code:: shell

    find / -type -f 2>> error.log

例）実行ファイルが存在するかどうかを確認し，存在する場合，``Exists!`` を出力する．このとき，``which`` コマンドの出力は必要ないため，標準出力と標準エラー出力をまとめて ``/dev/null`` に捨てる:

.. code:: shell

    which python &> /dev/null

パイプ
~~~~~~

コマンドの標準出力を次のコマンドの標準入力に渡すための機能．

例）a.txtの内容をソートし，重複する行を削除してb.txtに記録する:

.. code:: shell

    sort a.txt | uniq > b.txt

変数
~~~~

代入
^^^^

変数 ``a`` に文字列 ``"Hello World"`` を代入する:

.. code:: shell

    var="Hello World"

.. note:: ``=`` の周りに空白を含んではならない．

展開
^^^^

変数 ``name`` を ``echo`` に渡す:

.. code:: shell

    echo $name
    # or
    echo "${name}"

.. note:: シングルクォート内で変数は展開されない．

``if`` 文
~~~~~~~~~

.. code:: shell

    if false
    then
        echo "not reach"
    elif true
        echo 'True!'
    else
        echo not reach
    fi

``for`` 文
~~~~~~~~~~

.. code:: shell

    for p in 2 3 5 7
    do
        echo "The number $p is prime!"
    done

``while`` 文
~~~~~~~~~~~~

``case`` 文
~~~~~~~~~~~

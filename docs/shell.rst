シェルの基礎
============

.. contents:: 目次
    :depth: 2

定義
----

基本用語
~~~~~~~~

POSIX
    UNIXを始めとするオペレーティングシステムに共通のAPIを定めた標準規格．
標準入力 (stdin)
    通常はキーボードを指す
標準出力 (stdout)
    通常はディスプレイを指す
標準エラー出力 (stderr)
    通常はディスプレイを指す
終了ステータス
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

参考サイト
    - `<https://pubs.opengroup.org/onlinepubs/9699919799/idx/utilities.html>`_
    - `<https://ss64.com/osx/>`_
    - `<http://man7.org/linux/man-pages/dir_section_1.html>`_

本節ではよく使われる基本的なコマンドを紹介する．なお，コマンドの実装はGNU版（Linux）とBSD版（macOS）が有名だが，それぞれに共通のオプションのうち，よく使うものだけを紹介する．中にはPOSIXに準拠していないものもあるため，使用するときは注意する必要がある．

POSIXコマンド
~~~~~~~~~~~~~

- `awk <shell-commands/awk.rst>`_
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
- `test <shell-commands/test.rst>`_
- `xargs <shell-commands/xargs.rst>`_

非POSIXコマンド
~~~~~~~~~~~~~~~

- `tar <shell-commands/tar.rst>`_
- `wget <shell-commands/wget.rst>`_
- `which <shell-commands/which.rst>`_

シェル言語
----------

参考サイト
    - `<https://pubs.opengroup.org/onlinepubs/9699919799/idx/shell.html>`_

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

    which python &> /dev/null && echo 'Exists!'

パイプ
~~~~~~

コマンドの標準出力を次のコマンドの標準入力に渡すための機能．

例）a.txtの内容をソートし，重複する行を削除してb.txtに記録する:

.. code:: shell

    sort a.txt | uniq > b.txt

文字列
~~~~~~

エスケープ
^^^^^^^^^^

シェル言語の以下の記号は特別な意味を持つ::

    |   &   ;   <   >   (   )   $   `   \   "   '   <space>   <tab>   <newline>

状況によっては以下の記号も特別な意味を持つときがある::

    *   ?   [   #   ˜   =   %

コマンドの引数として渡すときは ``\`` を前置するか，次項のクォートを行う必要がある．

クォート
^^^^^^^^

クォートには ``'`` （シングルクォート）と ``"`` （ダブルクォート）を使うことができる．

シングルクォート内では，すべての文字は意味を失うため，基本的にシングルクォートを使うことで多くの特別な文字に関連するエラーは回避できる．シングルクォート内でシングルクォートを入れたい場合は以下のようにシングルクォート外でエスケープするかダブルクォートを用いる必要がある:

.. code:: shell

    echo 'Hello, I'\''m learning Bash.'
    # or
    echo "Hello, I'm learning Bash."

ダブルクォート内では，以下の文字が特別な意味を持つ::

    $   `   \

バックスラッシュは以下の文字が続くときエスケープし，それ以外はバックスラッシュがそのまま出力される::

    $   `   "   \   <newline>

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

``if`` 文は条件部分にコマンドを置く．

終了ステータスが0のとき真，0以外のとき偽．

``then`` は改行の代わりにセミコロンで区切ることで ``if`` と同じ行に置くことができる．

.. code:: shell

    if cond1
    then
        echo 'cond1 is true!'
    elif cond2; then
        echo 'cond2 is true!'
    else
        echo 'both are false'
    fi

``for`` 文
~~~~~~~~~~

例）拡張子が ``.png`` のファイルの横幅を800pxにする:

.. code:: shell

    for f in *.png; do
        convert "$f" -resize 800x "$f"
    done

``while`` 文
~~~~~~~~~~~~

``if`` と同様に条件が真である限り，実行し続ける．

.. code:: shell

    while cond; do
        echo 'cond is true.'
    done

``case`` 文
~~~~~~~~~~~

.. code:: shell

    case "$var" in
    (1)
        # `var` が1のときにマッチ
        echo 'one!'
        ;; # 省略不可能
    2)
        # 開き括弧は必須ではない
        echo 'two!'
        ;;
    3?)
        # `var` が30台の数字のときにマッチ
        echo 'thirty!'
        ;;
    *0|*5)
        # `var` が5の倍数のときにマッチ
        echo 'five!'
        ;;
    *)
        # 残ったすべてにマッチ
        echo 'No match!'
        # 最後は`;;`を省略できる
    esac

関数
~~~~

関数の定義は以下の通り:

.. code:: shell

    hoge() {
        # body
    }

呼び出しは通常のコマンドと同様に行う:

.. code:: shell

    hoge arg1 arg2

関数内で引数を使うには ``$1``，``$2`` などを用いる．

特別な変数
~~~~~~~~~~

.. list-table:: 特別な変数早見表
    :widths: 5, 50

    *   - ``$n``
        - ``n`` 番目の引数．``$0`` は自身の名前が入る．``n`` が10位上のときは ``n`` を ``{}`` で囲う．
    *   - ``$@``
        - 引数のリストに展開される．``"$@"`` は ``"$1" "$2" ...`` に展開される．
    *   - ``$*``
        - 引数のリストに展開される．``"$*"`` は ``"$1 $2 ..."`` に展開される．
    *   - ``$#``
        - 引数の個数．

算術演算
~~~~~~~~

形式
    ::

        $((expression))

例）1から100までのFizzBuzz:

.. code:: shell

    i=1
    while test $i -le 100; do # `i`が100以下(less than)である限り繰り返す
        if [ $(($i % 15)) -eq 0 ]; then # `[]`は`test`の別名
            echo fizzbuzz
        elif [ $((i % 3)) -eq 0 ]; then # $(()) の内側で変数を展開するときは`$`を省略してもよい
            echo fizz
        elif [ $((i % 5)) -eq 0 ]; then
            echo buzz
        else
            echo $i
        fi
        i=$((i + 1)) # `i`に1を加算して再代入
    done

コマンド出力の文字列埋め込み
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

例）最新でないPythonパッケージを一括で更新:

.. code:: shell

    pip install -U $(pip list -o | awk 'NR>2{print $1}')

.. note::

    ``pip list`` の出力は2行のヘッダに続いて，空白区切りで ``パッケージ名 バージョン`` が続く．``awk`` はパッケージ名だけを出力するために用いる．

.. warning::

    一部のパッケージは古いバージョンの別のパッケージに依存していることがある．必要とされるパッケージが最新になってしまうことで正しく動かなくなることもある．

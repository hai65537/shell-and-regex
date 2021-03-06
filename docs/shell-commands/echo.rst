=============================================
``echo`` - write arguments to standard output
=============================================

使い方
------

.. code::

    echo [-neE] [string]...

説明
----

引数を標準出力に出力し，改行するコマンド．引数が無ければ，改行だけが出力される．

オプション
----------

``echo`` は ``--`` をオプションとして受け付けない．

POSIX
~~~~~

存在しない．

非POSIX
~~~~~~~

``-n``
    改行を抑制する．
``-e``
    バックスラッシュによるエスケープシーケンスを有効にする．
``-E``
    バックスラッシュによるエスケープシーケンスを無効にする．規定の動作である．

オペランド
----------

*string*
    標準出力に出力される文字列．

    ``-e`` によって有効になるエスケープシーケンスは以下の通り．一部のエスケープシーケンスは端末によっては効果がなかったり，異なる動作をすることもある．

    ``\\``
        バックスラッシュ(``\``)1文字を出力する．
    ``\a``
        アラート (BEL) ``0x07``．
    ``\b``
        バックスペース (BS) ``0x08``．カーソル1つ左に移動する．
    ``\c``
        このエスケープシーケンス以降の文字を出力せずにコマンドを終了する．最後の改行も出力されない．
    ``\f``
        フォームフィード (FF) ``0x0C``．カーソルを1つ下に移動する．
    ``\n``
        改行 (LF) ``0x0A``．カーソルを次の行頭に移動する．
    ``\r``
        復帰 (CR) ``0x0D``．カーソルを行頭に戻す．
    ``\t``
        水平タブ (HT) ``0x09``．カーソルを移動後の位置が8の倍数なるように右に移動する．
    ``\v``
        垂直タブ (VT) ``0x0B``．カーソルを1つ下に移動する．
    ``\0NNN``
        8進数のアスキーコードを持つ文字を出力する． ``N`` は0個から3個の8進数．

標準入力
--------

使用されない．

入力ファイル
------------

なし．

標準出力
--------

引数が1文字のスペースで区切られ，最後の引数の後ろに改行文字が付く．

標準エラー出力
--------------

エラーレポートにのみ使用される．

出力ファイル
------------

なし．

終了ステータス
--------------

0
    成功
>0
    エラー

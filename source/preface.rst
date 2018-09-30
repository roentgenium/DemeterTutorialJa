前書き
======

この文章は何でないか
--------------------

1. この文章は X 線吸収微細構造 (XAFS) の原理，理論等について説明するものではありません．
2. この文章は，XAFS の真のパワーユーザによって書かれたものではありません．間違っている可能性を常に考慮し，少しでも疑問があれば，`Athena documentation <https://bruceravel.github.io/demeter/documents/Athena/index.html>`__  や `Artemis documentation <https://bruceravel.github.io/demeter/documents/Artemis/index.html>`__ といった本家のマニュアルや `Ifeffit Mailing List <http://cars9.uchicago.edu/mailman/listinfo/ifeffit/>`_ で真のパワーユーザのコメントを参照してください．`Ifeffit Mail Archive <http://www.mail-archive.com/ifeffit@millenia.cars.aps.anl.gov/>`__ ではメールアーカイブを検索することができます．場合によっては，著者による `Athena documentation 日本語訳 <http://www.moleng.kyoto-u.ac.jp/~moleng_04/asakura/ja/others/aug/index.html>`__ も役に立つかもしれません．
3. 繰り返しますが，この文章は，XAFS の真のパワーユーザによって書かれたものではありません．これから紹介する解析手順が，周りの XAFS 経験者あるいはあなたの指導教員とはいくらか異なっている可能性は大いにあります．著者の知る限り，XAFS 解析において絶対的といえる解析手順はなく，また，往々にして恣意性が入ることは否定できません．この文章の文責は著者にありますが，内容を保証するものではありません．

このチュートリアルは，分析手法の1つとして XAFS を利用し始めた人が一通りの解析を始められることを最大の目的としています．（著者の不勉強によるところもありますが，）数学的あるいは物理的な厳密さに欠ける表現もあります．

内容に関する指摘を歓迎いたします．エキスパートから初心者の方まで，どんな些細なことでもかまいません．早急に反映いたします．お気づきの点がありましたら京都大学朝倉 asakura@moleng.kyoto-u.ac.jp までご連絡ください．

Demeter/Ifeffit, Athena/Artemis に関する前置き
----------------------------------------------

この文章は，X線吸収微細構造 (XAFS) の解析ソフトの1つである Demeter/Ifeffit パッケージに含まれる Athena および Artemis の基本的な使い方について説明したものです．Demeter は Ifeffit package の後継版に当たります．

Athena や Artemis を実際に使い始めると，主に Web で検索したり，関連文献を探したりすることになると思います．また，Ifeffit という固有名詞を Web で検索したり，文献で見かけたりすると，2つの微妙に異なる意味で使われていることに気づくと思います．そこで，Athena および Artemis の説明に入る前に Ifeffit に関係するプログラム群について簡単に説明します．

Ifeffit という言葉で示される1つ目のものは "Interactive feffit" とでもいうべきもので，これは XAFS スペクトルの前処理と FEFF (University of Washington の J. J. Rehr 研究室で開発されている XAFS 理論計算プログラム) を用いたカーブフィッティングを行う "feffit" というプログラムを扱うためのライブラリ (Windows であれば Ifeffit.dll) およびコマンドラインプログラム (Windows であれば ifeffit.exe) を指しています．一方，もう1つの Ifeffit は，例えば論文等で Ifeffit package として言及される Athena, Artemis などを含む XAFS に関する解析プログラムあるいはユーティリティのパッケージを指しています．

本稿で説明する Athena および Artemis は Ifeffit package の後継版である Demeter package を用いています．

次に，Athena および Artemis のプログラムについて説明します．Athena および Artemis は "Interactive feffit" を GUI (Graphical User Interface) を通して使うためのプログラムであり，XAFS 解析用プログラムの中でも利用者が多いと思われるものの1つです．Athena および Artemis は GUI による操作が可能なため，（著者のような）初心者にも扱いやすいだけでなく，スペクトルが表示されるプロットウィンドウに対して，設定条件やフィッティング結果が即座に反映されるため，解析を進める上でも大きな助けにもなります．

次項からは，Athena および Artemis のごく基本的な解析方法について説明します．実試料の XAFS スペクトルを測定すると，測定自体が困難であったり，スペクトルの質が低かったり，解析の際に複雑なモデルについて考える必要があったりして，明確な結論を見いだすことが難しいことも少なくありません．しかし，XAFS が他の手法では得難い重要な情報を与える場合もあるでしょう．本チュートリアルが読者の一助になるよう願っています．

その他
------

文中では解析パラメータをどのように選ぶべきか，個人的な指針を示していますが，あくまで目安なので\ **これを絶対の基準としないで下さい**\ ．例えば，EXAFS スペクトルについてどの範囲でフーリエ変換するべきか1つをとっても，解析者によって異なる場合があります．

.. note::

   本 Web ページのライセンスは検討中です．もちろん無保証です．常識的な範囲でご自由にお使いください．また，本 Web ページを利用する際はできる限り，原典の URL http://www.moleng.kyoto-u.ac.jp/~moleng_04/asakura/ja/others/dtj/ (2018-09-30 現在) あるいはリンクを含めてください．

基本的には，以下の「操作」に書かれた手順でソフトを操作して下さい．

操作
~~~~

1. Athena を起動する．

チュートリアルで使うファイル
----------------------------

:download:`170918_sample.zip <_static/170918_sample.zip>`

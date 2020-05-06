# dockerfiles

* dockerfileという名前だけどsingularityの話します。

## Singularity

* とりあえずsingularityが気になった人は[これ](http://rnakato.hatenablog.jp/entry/2019/08/23/144656)読んでみてください(dockerより良いかもということが書かれています，特にスパコン環境内などはツールの設定がかなりめんどくさいので)。singularityの詳しい使い方は[SHIORKANE tutorial](https://supcom.hgc.jp/internal/mediawiki/Singularity_%E3%81%AE%E4%BD%BF%E3%81%84%E6%96%B9)(アカウントがないと読めませんが…)か[公式サイト](http://singularity.lbl.gov/quickstart)にまとまってます。

### monocle3をsingularityで試してみたお話

```
$ singularity pull docker://rnakato/monocle3
```

singularityはsingularity hubとdocker hubの両方からイメージをダウンロード（プル）できます。カレントディレクトリに`monocle3.simg`が保存されます。それで

```
$ singularity shell monocle3.simg
```

実行するとmonocle3の環境が整備されたshell内に入れます。ただこのshell内ではlocalのコマンドが使えなかったいするので(lessコマンドなどは使えなかったです)使用状況は限られちゃうかもしれません。viやR，Rscriptは使えたので自分はmonocle3.Rにコードをとりあえず書いて

```
> Rscript monocle3.R
```

これを実行しました。カレントディレクトリのファイルは自動でマウントされ，作られたfigやデータも保存される（この環境からexitしても見れる）のでdockerよりは大分便利かもしれません。

シングルセル解析で必要なツール一群をまとめたshubを自作してみんなで共有すれば，いちいちlocalにlibraryをダウンロードしてるかどうかや環境の違いで実行できないことがなくなりそうです。

### MoCaSeqをsingularityで試してみたお話

```
$ singularity pull docker://rolandradlab/mocaseq:latest
$ singularity inspect -r mocaseq-latest.simg
$ singularity run mocaseq-latest.simg
```

SHIROKANE上ではdockerは動かせない（？）ので`$ docker run hoge`はできない。ただ`$ singularity inspect -r <イメージ名>`でrunscriptが確認できる。runscriptは`docker run`したときに動くコード（だと思ってる）。`singularity run <イメージ名>`でrunscriptを実行できる。今回のrunscriptに対応する`MoCaSeq.sh`は引数を必要とするので，usageが表示される。そのusageに従って以下を実行すると`docker run`と同じ挙動を示した（はず）。

```
$ singularity run mocaseq-latest.simg --threads 48 --RAM 80 --test yes
```



# kaggler-pub-GCI-titanic

このリポジトリは11/9に作成されました。

# コンペ概要

松尾研GCI講義でのコンペである。内容はそこまで難しくない。初期段階で公開ノートブックがかなり強い。（0.ipynb）

期限は11/18の午後５時まで

本家titanicコンペと比較してcolumnsは同じでデータ量も同じだが、内容が違う。これは、本家のtrain_dataとtest_dataをごちゃまぜにしたのか？
ー＞だとしたら、本家コンペのprivate_scoreが高いものを引っ張ってこれば解決しそう。

もしくは、train_dataとtest_dataに重複がないかを検討

# 試したいことリスト

~~これはもう試しました~~

~~年齢をビン化処理をする~~ してみた。rfcはスコアが下がるがxgbが大幅にスコア向上した。

~~他の学習機でのスコア向上~~　もう試せるだけ試した感はある

・optunaを用いたイパーパラメーターチューニング（max_depth棟が大きくなってしまって過学習が起きてしまうのをどうするか要件等）

~~sex　*　Fareの列を作成するのがもしかしたら有効かもしれない~~ -> rfc以外でのスコア低減につながった。

~~0_8.ipynbについてrfc以外の学習機に対してもハイパーパラメータ最適化を施したい。~~　グリッドサーチはしない

~~苗字によって生き残ったかどうかというのはかなり制の相関があるとすれば、それをdictで保管しておいて、同一名があったときに関しては同じ名前にしてしまうのも手かと思う。これはEDAをして相関があるかどうかを確認する必要あり~~ 17.ipynbで検証

~~MrsがMrに含有されていることを把握した~~ 11/9以前に作成されたものに関しては要改善

・Ticket列に関して何かしらのデータを抽出できないか探ってみる。ー＞多分きつい

~~SCQ列の削除~~　9.ipynbのスコアで確認する

~~閾値の最適化をする~~　12.ipynbに対してしてみた。

~~GDBCの実装~~ gbcと同じ

・欠損していたかどうかを表す別の列の作成。欠損値をー１で補完する。欠損地のままgbcに突っ込む

~~・欠損値に関して、lgbcで推定して予測する。~~ 15.ipynbにて試してみた。これで、スコアが良くなってほしいなあ。

~~性別を分ける際に大人か子供かを表す列を追加。~~　あまりスコア向上しなかった。

~~家族が死んでいるか生きているかを表す列を追加~~　train_dataに関して過学習してしまっていて使い物にならない

Age, Fareに関して、中央値で補完するのがいいのではないか？

それぞれのモデルの精度をkaggleデータを使ってチューニングしたい。lgbのスコアが異常に低いのはなぜなのか

# 変更内容

## 11/9土曜日

### 0.ipynbについて、

運営から配布されたデータ。

LBで0.765程度のスコアが出る。

### 0_8.ipynbについて、

現在LBで過去最高のスコア0.8が出る。

pro.ipynbに比べてlrでの予測値をアンサンブルしただけ。

### 1.ipynbについて、

前処理は名前に対しての敬称列を作成。

Familyの数を表す列を作成。

Ageをビン化

models = ['rfc', 'svc', "xgb", "gbc"] 

これらのモデルをoptunaによってハイパーパラメーター最適化

LBでは0.778程度しか出ない。

private_scoreでは0.85程度を記録

### 2.ipynbについて

前処理で各列の積についても考慮に入れてみた。

importanceを取得してみると、sex * Fare の要素が非常に大きな重要度を持っていることが判明した。

pycaretでtop3モデルgbc、ridge、xgboostのアンサンブルを作った。

なぜか、まったくスコアが出ない。private_scoreは0.83程度。

LBでは0.76程度しか出ない

pycaretを使うという策は断念。

### 3.ipynbについて

0_8.ipynbの派生形ノートブックになっている。

これは前処理の段階でAgeをビン化処理を施した。

privete_scoreでは0.82を記録しているが、LBでは0.797となっている。

あまりよくない処理なのかもしれない

### 4.ipynbについて

3.ipynbの派生形

xgbでの予想もしてみたが、パラメータチューニングなしで0.8に届かなかった。

没。鬱。

ただ、lrも0.806しかないので、そんなに精度はかわらないんじゃないかなー

### 5.ipynbについて

0_8.ipynbの派生形

敬称を抽出している。

学習機に関してはグリッドサーチをしたrfcのみ。private_scoreは0.825

lrが案外いいスコア0.821を記録している。

LBでは0.791を記録。アンサンブルしたらいいスコアが出そう。

### 6.ipynbについて

5.ipynbの派生形

学習機にxgbを追加。さらにrfc、lr、gbc、のアンサンブルで作成した。

private_scoreはどれも0.8を超えている。

LB_scoreは0.794だった。

gbc混ぜたのが良くなかったのかも

Family列が生成できていなかったのでFamily列を作って再検証してみたい

## 11/10 日曜日

## 7.ipynbについて

6.ipynbの派生形

LBは0.806。過去最高

主変更点は、名前列について、重要度が高いMrのみについてtrue or Falseで判定する列を作った。

アンサンブルに関してlrを排除し、rfc(0.831)とgbc(0.817)のみのアンサンブル（重みなし）

今後は試したいことリストに残っているものを片っ端から試していく

### 8.ipynbについて

7.ipynbの継承。Sex*Fareの列を新たに作ってみた。重要度は1位になったが多重共線性の問題でスコアはあまり上昇せず。

LB＿scoreは不明。

private_scoreに関してrfcは0.001だけ上昇したが他のモデルが壊滅的状況。おそらくLBでもあまりいいスコアは出ないだろう。

断念。

### 9.ipynbについて

7.ipynbの継承

C, S, Q のノイズ除去をした。重要度を示すグラフに数値も表示されるように改善した。結果private_scoreではrfcとmlpcのスコアが上昇した。この結果を受け、rfc、mlpc、gbcのアンサンブルをしている。

LB_scoreに関しては~~11/10の午後三次に採点される見通し~~　0.794

なかなか、スコアが上がらなかった中、先0.806を記録してうれしくなっているが、0.85を記録するのは恐ろしく遠いことを確認して絶望している。どうすればいいんだろう。

また、締め切りまで１週間程度＋提出回数が少ないことから、そろそろハイパーパラメータのチューニングをしていきたいと思っている。

### 10.ipynbについて

9.ipynbの継承。Ageをビン化してみた。rfc、mlpcのスコアが減少した。xgbのスコアが上昇した。

そこで、rfc(0.833), gbc(0.821),xgb(0.832)のアンサンブルを作成した。LBでのscore確認は~~11/10の午後九時になる見通し~~ 0.788

正直な感想、スコアは上昇する気がしている（願望）とりあえず、これからのAHCに備えたいし、SMBCコンペのほうもsubmitできるノートブック作成に取り掛かりたい気もしている。そのためにはハイパーパラメータチューニングをしなきゃなーという気持ちになっている。

### 11.ipynbについて

7.ipynbの派生形。Q列を消した。結果rfcが0.828、gbcが0.817になった。

gbcの重要度も表記するように変更。所感はあまりスコアは上がらない気がしている。何か良いデータ列の作成方法がないか模索中。

ところでいつハイパーパラメータの調節をするんだという話が出てきている。

### 12.ipynbについて

7.ipynbの継承。optunaでthresholdの最適化を施した。結果、0.54くらいの閾値がいいっぽい。LB_scoreに関しては~~11/11の午前三時にわかる見通し~~　0.794

モデルに関してはrfcとgbcのアンサンブルでいいかなーと思っている。これからは学習機にハイパーパラメータ最適化を施していく予定。

## 11/11 月曜日

### 13.ipynbについて

12.ipynbの継承

dfとkaggle_dataとの整合性を確認した。おそらく、同一データが使用されているはず。（？）

## 14.ipynbについて あなたの正解率は0.7870813397129187です

7.ipynbと同じ。変更点なし。ただのコピーです。

### 15.ipynbについて

7.ipynbの継承。Age列に関して、LGBMによる推測を行った。うまくいくようならFare列に対しても推測を行ってもいいかも。

ところで、ハイパーパラメータのチューニングはいつやるんですか！？早くやっておきましょう。

rfc(0.827),gbc(0.825),xgb(0.825)のアンサンブルをとっている。LB_scoreは~~11/11の午後三時に採点される~~　0.791

### 16.ipynbについて あなたの正解率は0.784688995215311です

7.ipynbの継承。最終予測に対して、再度rfc, gbcに予測を突っ込んだ。ここもハイパーパラメータの調整ができそうではある。

LBスコアに関しては~~11/11の午後9時に採点がされる見通し。~~　0.8

まじでハイパーパラメータのチューニングはいつやんだいｗ。

##  11/12 月曜日

### 17.ipynbについて

14.ipynbの継承。後処理で苗字で列を作成し、同一苗字の人が存在する場合は家族での多数決をとる。

LBに関しては~~12日の午前3時に採点される見通し~~　0.703　無理

### 18.ipynbについて

14.ipynbの継承。

保存ミスをしたのでgithubのファイルに関しては削除した。

スコア自体も0.4自体しか取れていなかった。原因は、家族がいない人に対しても家族の死亡推定を0.5としていたからなのか、df_testに対する予測もなぜか０がとても多かった。

### 19.ipynbについて あなたの正解率は0.722488038277512です

18.ipynbの継承

家族がいない人に関しては家族の死亡率をー１に設定した。採点は11/12の午後九時に採点される見通し

まじでハイパーパラメータのチューニングをしろ、

### 20.ipynbについて　あなたの正解率は0.7894736842105263です

14.ipynbの継承。

採点スコアは0.803

Mr列を廃止してalone列を追加した。

### 21.ipynbについて

https://www.kaggle.com/code/cdeotte/titanic-using-name-only-0-81818

上のURLのコピー、なぜかうまく動かないのでどうしたものか。

### 22.ipynbについて　あなたの正解率は0.7607655502392344です

19.ipynbの継承。https://www.kaggle.com/code/chumajin/lgbm-starter-with-optunaを参考にLGBMのモデルを追加してみた。

LBスコアに関しては11/13の午後3時に採点される見通し。(?)

多分採点されてない！？

### 23.ipynbについて　あなたの正解率は0.7703349282296651です

22.ipynbを継承した。rfcに関してoptunaを使ったサーチに切り替えた。多分スコア下がってる感じする。あんまうれしくない

rfc_predのスコアは0.7679425837320574

lr_predのスコアは0.7751196172248804

mlpc_predのスコアは0.7535885167464115

knc_predのスコアは0.6483253588516746

gbc_predのスコアは0.7751196172248804

lgb_predのスコアは0.7607655502392344

predのスコアは0.7703349282296651

この数値が高いものを使ってアンサンブルしたらいいスコアが出そうですね笑やりましょう

### 24.ipynbについて

20.ipynbの継承。成人男性かそれ以外かという指標を追加したが、スコアは減少した。残念。

### 25.ipynbについて　あなたの正解率は0.7894736842105263です

24.ipynbの継承。catboostを使用してみたが、かなり精度いい。

ただ、0.79を超えるには何かもう一工夫がいりそう。どうしたものか。

LB_score = 0.797

kaggleのデータに対して正解率をグラフ表示して解析をしたほうがいいのではないか？

### 26.ipynbについて　catのスコアは0.7918660287081339
catboost単体での予想に変更した。rfc等のモデルを採用するとなぜかスコアが減少してしまう。どうしたいいのかなあという感じ。

LBのスコアに関しては11/15午前3時に採点される見通し。

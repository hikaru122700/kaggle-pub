# kaggler-pub-GCI-titanic

このリポジトリは11/9に作成されました。

# コンペ概要

松尾研GCI講義でのコンペである。内容はそこまで難しくない。初期段階で公開ノートブックがかなり強い。（0.ipynb）

期限は11/18の午後５時まで

本家titanicコンペと比較してcolumnsは同じでデータ量も同じだが、内容が違う。これは、本家のtrain_dataとtest_dataをごちゃまぜにしたのか？
ー＞だとしたら、本家コンペのprivate_scoreが高いものを引っ張ってこれば解決しそう。

もしくは、train_dataとtest_dataに重複がないかを検討

# 試したいことリスト

年齢をビン化処理をする

他の学習機でのスコア向上

optunaを突かttあハイパーパラメーターチューニング（max_depth棟が大きくなってしまって過学習が起きてしまうのをどうするか要件等）

sex　*　Fareの列を作成するのがもしかしたら有効かもしれない

0_8.ipynbについてrfc以外の学習機に対してもハイパーパラメータ最適化を施したい。

苗字によって生き残ったかどうかというのはかなり制の相関があるとすれば、それをdictで保管しておいて、同一名があったときに関しては同じ名前にしてしまうのも手かと思う。これはEDAをして相関があるかどうかを確認する必要あり。

MrsがMrに含有されていることを把握した。11/9以前に作成されたものに関しては要改善

# 変更内容

## 11/9土曜日

### 0.ipynbについて、

運営から配布されたデータ。

LBで0.765程度のスコアが出る。

## 0_8.ipynbについて、

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



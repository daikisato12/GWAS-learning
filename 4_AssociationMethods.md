### 線形モデル, 線形混合モデル


### Multivariate analysis
これまでのGWASでは, 一つの表現型を目的変数とする**univariate**な手法が一般的だが, 複数の表現型を同時に利用することは多くの場合で可能であるし, 複数の表現型を目的変数とする**multivariate**な手法の方が統計的な検出力も高まると言われている.

#### [Jiang & Zeng 1995 _Genetics_](https://www.genetics.org/content/140/3/1111.long)

#### [Shriner 2012 _Front. Genet._](https://www.frontiersin.org/articles/10.3389/fgene.2012.00001/full)

#### [Stephens 2013 _PLoS ONE_](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0065245)
手法内の[誤植](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0213951)に注意.

#### [Galesloot et al. 2014 _PLoS ONE_](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0095923)

#### [Turchin & Stephens 2019 _PLoS Genet_](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1008431)
上記Stephens 2013の手法を13のGWASデータに用いて, 再解析したという内容. 解析プログラムは`bmass`というRパッケージにまとめている.

d個の表現型について, 各SNPとそれぞれの表現型を無相関, 直接的相関, 間接的相関という3つのカテゴリーに分ける. すると各SNPと表現型との関係性として3^d種類のカテゴライズができる. この関係性をモデル (<img src="https://latex.codecogs.com/gif.latex?\gamma" title="\gamma" />) と呼び, どのモデルがデータと一致するか, 帰無モデル (全ての表現型と無相関) に反するものはどれかを特定するのが目的となる. 帰無モデルに対するモデル<img src="https://latex.codecogs.com/gif.latex?\gamma" title="\gamma" />の支持はベイズファクター (<img src="https://latex.codecogs.com/gif.latex?BF_\gamma" title="BF_\gamma" />) で表現される. この値が大きいほど, モデル<img src="https://latex.codecogs.com/gif.latex?\gamma" title="\gamma" />の相対的な妥当性が大きいということになる. P値でなくベイズファクターを用いることで, 異なるモデル間の比較や結合 (それらのweighted averageで求められる; 以下の式参照) が容易になる. weightsは各モデルの妥当性を反映している. `bmass`では経験的ベイズアプローチを取り入れており, データから適切なweightsを学習してくれる.<br><br>

<img src="https://latex.codecogs.com/gif.latex?BF_{av}&space;\equiv&space;\sum_\gamma&space;w_\gamma&space;BF_\gamma" title="BF_{av} \equiv \sum_\gamma w_\gamma BF_\gamma" /><br>
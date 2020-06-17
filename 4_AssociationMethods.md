### 線形モデル, 線形混合モデル

### Gene (region, pathway)-level association analysis
一般的なGWASにおいて, 実際には形質に影響しないSNPであっても, 効果サイズは計算されてしまう. この偽陽性をどう制御するかについて近年議論がなされている. 一つの対処法として, SNP単位ではなく, それらがenrichしている遺伝子や領域, あるいは機能的なパスウェイを検出するという考え方がある.

#### [Liu et al. 2010 _AJHG_](https://www.cell.com/ajhg/fulltext/S0002-9297(10)00312-5)
`VEGAS`という手法の提案.

#### [Wu et al. 2010 _AJHG_](https://www.cell.com/ajhg/fulltext/S0002-9297(10)00248-X)
#### [Wu et al. 2011 _AJHG_](https://www.cell.com/ajhg/fulltext/S0002-9297(11)00222-9)
`SKAT`という手法の提案.

#### [Inonita-Laza et al. 2013 _AJHG_](https://www.sciencedirect.com/science/article/pii/S0002929713001766)

#### [Carbonetto & Stephens 2013 _PLoS Genet._](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1003770)

#### [Leeuw et al. 2015 _PLoS Comput. Biol._](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1004219)
`MAGMA`という手法の提案.

#### [Lamparter et al. 2016 _PLoS Comput. Biol._](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1004714)
`Pascal`という手法の提案.

#### [Nakka et al. 2016 _Genetics_](https://www.genetics.org/content/204/2/783)
`PEGASUS`という手法の提案.

#### [Wang et al. 2017 _Genetics_](https://www.genetics.org/content/207/3/883)
`COMBAT`という手法の提案.

#### [Zhu & Stephens 2018 _Nat. Commun._](https://www.nature.com/articles/s41467-018-06805-x)

#### [Chen et al. 2020 _PLoS Genet._](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1008855)
`gene-ε`という手法の提案.

### Multivariate analysis
これまでのGWASでは, 一つの表現型を目的変数とする**univariate**な手法が一般的だが, 複数の表現型を同時に利用することは多くの場合で可能であるし, 複数の表現型を目的変数とする**multivariate**な手法の方が統計的な検出力も高まると言われている.

#### [Jiang & Zeng 1995 _Genetics_](https://www.genetics.org/content/140/3/1111.long)

#### [Shriner 2012 _Front. Genet._](https://www.frontiersin.org/articles/10.3389/fgene.2012.00001/full)

#### [Stephens 2013 _PLoS ONE_](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0065245)
手法内の[誤植](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0213951)に注意.

#### [Galesloot et al. 2014 _PLoS ONE_](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0095923)

#### [Turchin & Stephens 2019 _PLoS Genet._](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1008431)
上記Stephens 2013の手法を13のGWASデータに用いて再解析したという内容. 解析プログラムは`bmass`というRパッケージにまとめている.

d個の表現型について, 各SNPとそれぞれの表現型を無相関, 直接的相関, 間接的相関という3つのカテゴリーに分ける. すると各SNPと表現型との関係性として3^d種類のカテゴライズができる. この関係性をモデル (<img src="https://latex.codecogs.com/gif.latex?\gamma" title="\gamma" />) と呼び, どのモデルがデータと一致するか, 帰無モデル (全ての表現型と無相関) に反するものはどれかを特定するのが目的となる. 帰無モデルに対するモデル<img src="https://latex.codecogs.com/gif.latex?\gamma" title="\gamma" />の支持はベイズファクター (<img src="https://latex.codecogs.com/gif.latex?BF_\gamma" title="BF_\gamma" />) で表現される. この値が大きいほど, モデル<img src="https://latex.codecogs.com/gif.latex?\gamma" title="\gamma" />の相対的な妥当性が大きいということになる. P値でなくベイズファクターを用いることで, 異なるモデル間の比較や結合 (それらのweighted averageで求められる; 以下の式参照) が容易になる. weightsは各モデルの妥当性を反映している. `bmass`では経験的ベイズアプローチを取り入れており, データから適切なweightsを学習してくれる.<br><br>

<img src="https://latex.codecogs.com/gif.latex?BF_{av}&space;\equiv&space;\sum_\gamma&space;w_\gamma&space;BF_\gamma" title="BF_{av} \equiv \sum_\gamma w_\gamma BF_\gamma" /><br>
### Polygenic score (PGS)とは？
ある形質についてのGWASの結果から得られた各SNPsの効果サイズを, 独立なサンプルの各SNPのアリル数に掛け合わせたスコア (`1_OveralPicture.md`参照). 個体の形質に対する遺伝的な傾向/リスクを評価することができる. 各個体iについて以下の式で定義され, その個体の, 注目している表現型に対する遺伝的なリスクを表す (aij = {0, 1, 2}の値を取り,それぞれ遺伝子型 0/0, 0/1, 1/1に対応). <br>
<img src="https://latex.codecogs.com/gif.latex?PGS_i&space;=&space;\sum&space;_{j=1}&space;^M&space;a_{ij}w_j" title="PGS_i = \sum _{j=1} ^M a_{ij}w_j" />

### Discovery / Target sampleの相違について
PGSの計算に用いるSummary Statistics (要は各SNPsの効果サイズ) を得たGWASのサンプル (discovery sample) と, それを掛け合わせ, 表現型を予測したいサンプル (target sample) は独立であることが求められる ([Wray et al. 2013](https://www.nature.com/articles/nrg3457)). この時, 両者に遺伝的な隔たりが大きいと正確な予測ができないことが知られている. 以下の論文が参考になる.

#### [Scutari et al. 2016](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1006288)
Training (discovery) sampleとTarget sampleの遺伝的距離 (Fstあるいは平均血縁度) が遠いほど, 予測能力が下がることを示した. この予測能力の低下はアリル頻度とLDの違いに起因する.

#### [Martin et al. 2017](https://www.cell.com/ajhg/fulltext/S0002-9297(17)30107-6)
例えば身長はヨーロッパ集団からの遺伝的な距離が離れるにつれてPGSが低くなる. アフリカ集団は実際の表現型としては身長は低くないが, PGSで計算すると低くなってしまう. コアレセントシミュレーションを用いてゲノムデータを作成し, GWASを行う → PGSの計算 を再現してみると, 予想通り, 同じ集団をDiscovery/Target sampleに用いた時に, 実際の表現型とPGSの相関が最も強くなった.

*続報として, 上記研究では集団動態のシミュレーションに誤りがあったとする[論文](https://www.biorxiv.org/content/10.1101/2020.06.04.131284v1)が最近出た. 非アフリカ集団がアフリカ集団から生じたという仕様になっておらず, 集団間の遺伝的分化が大きく推定されていたという. 上記の結論に変わりはないが, Discovery sampleと異なる集団の遺伝子型を使って計算するPGSの正確さに, 報告されていたほどの大きな減少はないとのこと.

#### [Grinde et al. 2018](https://onlinelibrary.wiley.com/doi/full/10.1002/gepi.22166)
European ancestry (EA) の集団で行われたGWAS結果を, Hispanic/Latino集団に用いてPGSを計算した結果, 概ね予測性能は良かったが, 血圧に関してはそうではなかった.

#### [Curtis 2018](https://journals.lww.com/psychgenetics/Fulltext/2018/10000/Polygenic_risk_score_for_schizophrenia_is_more.2.aspx)
ヨーロッパ集団を用いた統合失調症のGWASから計算されたPRSを祖先の異なる集団に用いると, 統合失調症という表現型そのものよりも祖先の集団による差が大きい.

#### [Martin et al. 2019](https://www.nature.com/articles/s41588-019-0379-x)
もっとヨーロッパ集団以外のデータを増やさないとね, というコメンタリー論文.

#### [Duncan et al. 2019](https://www.nature.com/articles/s41467-019-11112-0)
(図1) 2008-2017年に出版された733のPolygenic scoreを使った研究を調べた結果, 67%がヨーロッパに祖先を持つ集団のみを対象, 19%がアジア (ほぼ東アジア; 中国や日本) 集団のみを対象, 3.8%のみがアフリカ系やヒスパニック, 現地人 (ネイティブアメリカンや中東)) 集団だった.

(図2) PGSのEffect size (実際の表現型に対するPGSの説明可能性？) を祖先異なる複数の集団で出している先行研究のGWASデータをForest plotにしてまとめているという図.　図1と同様, これらの研究のDiscovery sampleの大半はヨーロッパ祖先集団が占めているので, その影響が大きいと思われる. ヨーロッパ集団での効果サイズと他集団での効果サイズの比を取ると, 他集団では低い → ヨーロッパ祖先集団から計算されたPGSを他集団の表現型予測に使うと予測性能は大幅に落ちることが分かった. 特にアフリカ集団で差が大きい, これはヨーロッパ集団との遺伝的距離が他の集団に比べて大きいことに由来していると考えられる. この図, 説明があまりに少ないし, 元論文の引用もちゃんとされていないので非常に分かりづらい. また元論文でPGSの計算に使われているSNPsの基準も分かりづらい (P < 5.0×10^-8？).

ちなみに, 連続形質yの分散のうち, あるSNPの遺伝子型xで説明される割合をR^2とし, これは決定係数と呼ばれる (相関係数の二乗). アリル頻度がpの時, <br>
<img src="https://latex.codecogs.com/gif.latex?R^2&space;=&space;2p&space;(1-p)\beta" title="R^2 = 2p (1-p)\beta" /><br>
<img src="https://latex.codecogs.com/gif.latex?R^2&space;\fallingdotseq&space;0.5p(1-p)(log(OR))^2" title="R^2 \fallingdotseq 0.5p(1-p)(log(OR))^2" /><br>
が[成り立つらしい](http://103.253.147.127/PUBLICATIONS/riken.110916.pdf).

(図4) 同じヨーロッパでもDiscovery datasetがGIANTの時はPGSと実際の身長との相関が高いが, UKBの時はそうではない. ← 下記のようにGIANTは集団構造の補正が十分でない可能性が示唆されており, これが原因かもしれない. Discovery :ヨーロッパ / Target: アジア の組み合わせの時はそこそこ予測性能が良いのに対して, 逆の時はそうでもない. ← Discoveryに使われているアジアのデータセットはサンプルサイズがそこまで多くなく ([He et al. 2015](https://academic.oup.com/hmg/article/24/6/1791/685282); N = 93,236だけど), 検出力が高くないことが原因かもしれない. 

(図3) Clumping (LD pruningに近い; P値の小さいSNPsの周囲で連鎖不平衡にあるSNPs同士をclump (群) とし, それらをPGSの計算から取り除く) に用いるLDの閾値や, P値の閾値によってPGSの結果が変わる. Target sampleと同じ集団のLDを元にclumpingした時に最もPGSの分布幅は狭まる. また, より多くのSNPsを使う (clumpingの際, LDの閾値を上げる/SNPのP値の閾値を上げる) ほどPGSの分布が集団間でバラつく (逆に言えば, 使うSNPsを厳しく選定するほど, 集団間でPGSの値にあまり違いがなくなる).

では, PGSの値が異なることは何に起因するのか？
1. 遺伝的不動に由来する真の違い (Martin et al. 2017; 反論は[Guo et al. 2018](https://www.nature.com/articles/s41467-018-04191-y): driftの効果はPGSの差を生むほど強くない)
2. 自然選択に由来する真の違い
3. 遺伝子と環境との相互作用 (遺伝子型の効果が集団によって異なる) に由来する真の違い
4. Discovery/Target sampleに存在する集団構造に由来するバイアス
5. Discovery/Targetに用いられる集団や, PGSの計算過程によるバイアス. 具体的には, LDの構造や変異頻度の情報が不完全であること (genotypingやimputationに由来する), またそれらが集団によって異なること
6. GWASの際の効果サイズ推定におけるランダムなエラー.

現在のところ, データ不足のため様々な要因が正確に理解されていないので, 集団間のPGSの違いから何かを結論づけるのには注意が必要. 特に環境の効果はもっと調べる必要がある. 社会的地位や経験等が影響してくる精神的な表現型なら尚更.

### GWAS時点で遺伝的集団構造が十分に補正されていないという問題点
GIANTやR15-sibsとUKBのデータセットをそれぞれ使ったPRSの傾向が一致しないことがあり, GWAS時点でデータセット内部の集団構造が十分に補正されていないという問題点が指摘されている. 

#### [Sohail et al. 2019](https://elifesciences.org/articles/39702)

#### [Uricchio et al. 2019](https://onlinelibrary.wiley.com/doi/full/10.1002/evl3.97)
GIANTデータセットの集団構造の補正が不十分であった可能性を示す.

#### [Berg et al. 2019](https://elifesciences.org/articles/39725)
ヨーロッパ集団で身長に対してpolygenic adaptationが起きていると言われているが, それまでGIANTのデータを使って計算されてきたPGSをUKBのデータ (こちらの方が集団構造が薄い) を使って再評価した結果, シグナルは消えたという. GWASの時点の集団構造の補正がPGSの計算に不十分であった可能性を示す. 


### PGSの計算について
- `LDpred`: [元論文](https://www.cell.com/ajhg/fulltext/S0002-9297(15)00365-1), [リンク](https://github.com/bvilhjal/ldpred). 
- `PRSice` [元論文](https://academic.oup.com/bioinformatics/article/31/9/1466/200539) / [PRSice2](https://academic.oup.com/gigascience/article/8/7/giz082/5532407), [リンク](https://www.prsice.info).
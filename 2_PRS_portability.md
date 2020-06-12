### Polygenic score (PGS)とは？
ある形質についてのGWASの結果から得られた各SNPsの効果サイズを, 独立なサンプルの各SNPのアリル数に掛け合わせたスコア (`1_OveralPicture.md`参照). 個体の形質に対する遺伝的な傾向/リスクを評価することができる. 


### Discovery / Target sampleの相違について
PGSの計算に用いるSummary Statistics (要は各SNPsの効果サイズ) を得たGWASのサンプル (discovery sample) と, それを掛け合わせ, 表現型を予測したいサンプル (target sample) は独立であることが求められる ([Wray et al. 2013](https://www.nature.com/articles/nrg3457)). この時, 両者に遺伝的な隔たりが大きいと正確な予測ができないことが知られている. 以下の論文が参考になる.

#### [Scutari et al. 2016](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1006288)
Training (discovery) sampleとTarget sampleの遺伝的距離 (Fstあるいは平均血縁度) が遠いほど, 予測能力が下がることを示した. この予測能力の低下はアリル頻度とLDの違いに起因する.

#### [Martin et al. 2017](https://www.cell.com/ajhg/fulltext/S0002-9297(17)30107-6)


*続報として, 上記研究では集団動態のシミュレーションに誤りがあったとする[論文](https://www.biorxiv.org/content/10.1101/2020.06.04.131284v1)が最近出た.

#### [Grinde et al. 2018](https://onlinelibrary.wiley.com/doi/full/10.1002/gepi.22166)
European ancestry (EA) の集団で行われたGWAS結果を, Hispanic/Latino集団に用いてPGSを計算した結果, 概ね予測性能は良かったが, 血圧に関してはそうではなかった.

#### [Curtis 2018](https://journals.lww.com/psychgenetics/Fulltext/2018/10000/Polygenic_risk_score_for_schizophrenia_is_more.2.aspx)
ヨーロッパ集団を用いた統合失調症のGWASから計算されたPRSを祖先の異なる集団に用いると, 統合失調症という表現型そのものよりも祖先の集団による差が大きい.

#### [Martin et al. 2019](https://www.nature.com/articles/s41588-019-0379-x)
もっとヨーロッパ集団以外のデータを増やさないとね, というコメンタリー論文.

#### [Duncan et al. 2019](https://www.nature.com/articles/s41467-019-11112-0)
2008-2017年に出版されたPolygenic scoreを使った研究を調べた結果, 67%がヨーロッパに祖先を持つ集団のみを対象, 19%が東アジア集団のみを対象, 3.8%のみがアフリカ系やヒスパニック, 現地人集団だった.

ヨーロッパ祖先集団から計算されたPGSを他集団の表現型予測に使うと予測性能は大幅に落ちることが分かった.

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
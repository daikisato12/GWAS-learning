### ゲノムワイド関連解析とは？
個体の一塩基多型 (Single Nucleotide Polymorphisms: SNPs)の遺伝子型と表現型との相関を統計的に解析する手法. ヒトの疾患の遺伝的要因同定のために使われることが多いが, 生物の (遺伝的に規定される) あらゆる表現型の遺伝的背景を探ることができ, 育種・農学分野や進化・生態学分野においても有力な解析手法である.

### 解析における一般的な注意点
解析に用いる個体
- call rateの低い (e.g. <0.98) 個体を除く.
- 血縁関係にある個体 (e.g. PI_HAT > 0.12, p(IBD = 0) < 0.05 etc.) を除く.
- (ヒトの場合) 自己申告と遺伝的な性が一致しない人を除く. 
- 表現型との混交を避けるため, 同一集団に由来する遺伝的背景が均一な個体を使い, 内部の集団構造が存在しないことが重要. PCAにより, 各集団から外れる個体を除くなど.

解析に用いるSNPs
- call rateの低い (e.g. <0.99) SNPsを除く.
- ジェノタイピングのエラーを避けるため, マイナーアリル頻度（Minor Allele Frquency: MAF）が低い (e.g. <1%) SNPsは排除する. サンプル数が多ければ, 低頻度のアリルもエラーでなく捉えることができるため, この閾値はサンプル数に依存する.
- (計算の煩雑さのため?) 遺伝子型が2つの (Biallelicな) SNPsのみを対象とする.
- Hardy–Weinberg Equilibriumに沿わない (e.g. p < 1×10^–6) SNPsは除く.

慣習みたいなもの
- 0.05を100万 (一般に解析に用いるSNPsが100万あるとして) で割った, 5.0 × 10^-8 がSNPの有意性を示すP値の閾値として使われることが多い. 1.0 × 10^-6 をsuggestive levelとすることもある.

### 使われる手法
基本的にはどれも線形混合モデルをベースとしており, 大きくは変わらない (気がする) が, サンプル数/SNP数の増加に伴い, 計算時間とメモリ使用量が膨大に増えてきたので, それを抑えつつ, 真のシグナルを捉えられるよう微調整が加えられてきたというイメージ. [この論文](https://bmcgenomics.biomedcentral.com/articles/10.1186/s12864-020-6552-x)のイントロで各手法の (長所) 短所が紹介されている. 量的形質なら`BOLT-LMM`, 2値形質 (疾患かどうか等) なら`SAIGE`が現時点 (2020/6) ではベストという印象を受けた.
#### Variant-baseモデル
- `GCTA`: [元論文](https://www.cell.com/ajhg/fulltext/S0002-9297(10)00598-7). 2011年. genome-wide complex trait analysisの略. 線形混合モデルを用いた関連解析の他に, 相加的な遺伝的変異の効果により表現型分散をどれほど説明できるか, 遺伝率の計算も行うことができる. Genome-based Restricted Maximum Likelihood (GREML) という.
- `FaST-LMM`: [元論文](https://www.nature.com/articles/nmeth.1681). 2011年. 混合モデルでは計算に時間がかかりすぎるという問題点をM < Nになるようマーカー自体を減らすというアイデアで実装している.
- `GRAMMAR-Gamma`: [元論文](https://www.nature.com/articles/ng.2410). 2012年.
- `GEMMA`: [元論文](https://www.nature.com/articles/ng.2310). 2012年. Genome-wide efficient mixed-model analysisの略.
- `BOLT-LMM`: [元論文](https://www.nature.com/articles/ng.3190), [Webページ](https://data.broadinstitute.org/alkesgroup/BOLT-LMM/#x1-30001.1). 2015年. Bayesian linear mixed-model associationを採用. GWASに使われる既存の混合モデルでは計算に時間がかかりすぎる (O(M^2N)あるいはO(MN^2)) のをO(MN)の繰り返しまでに改良している (ここをM < Nになるようマーカー自体を減らすというアイデアで実装したのが`FaST-LMM`) . また, 既存のLMMのように全てのSNPsが独立な正規分布に従う小さな効果サイズを持つと仮定するのではなく, あらかじめ二つの事前正規分布を仮定するベイズ過程によって, それぞれのSNPsの効果サイズの大小を正確に推定していく.
  - 最近の大きな論文では大抵使われている. 基本的にヒトを対象とする, 大きな (sample size > 5000) データセットに向いているらしい. sample size < 5000 の場合は`GCTA`あるいは`GEMMA`を推奨とのこと. 潜在的な集団構造/個人間の血縁度を補正してくれるのでPCAのスコアは入れる必要なし.
- `GMMAT`: [元論文](https://www.cell.com/ajhg/fulltext/S0002-9297(16)00063-X). 2016年. ここから**一般化**線形混合モデルに入ってくる (要は連続的な形質でなく, 疾患のような2値形質が対象なので, logisticなモデルになる). 後述の`SMMAT`と同じ開発者. O(MN^2)の計算時間とO(N^2)のメモリ使用量がかかるのがネック.
- `SAIGE`: [元論文](https://www.nature.com/articles/s41588-018-0184-y). 2018年. Scalable and Accurate Implementation of GEneralized mixed modelの略. binary dataに対して, 2020年時点で最新・比較的有力な手法. メモリ使用量や計算時間が大幅に削られ, 偏ったcase-control比でも解析できるのが, `GMMAT`からの改良点であり, 強み.
#### Gene (region)-baseモデル
頻度の低いアリルに関しては, variantベースの手法では検出力が低いという問題があるので, 遺伝子あるいは領域ベースの手法がよく用いられる.
- `MAGENTA`: [元論文](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1001058), [Webページ](https://software.broadinstitute.org/mpg/magenta/). 2010年. 遺伝子単位の相関解析手法の一種だが, enrichしているパスウェイの特定に使われる.
- `ANNOVAR`: [元論文](https://academic.oup.com/nar/article/38/16/e164/1749458), [Webページ](https://doc-openbio.readthedocs.io/projects/annovar/en/latest/). 2010年. 検出された遺伝的変異の遺伝子や機能のアノテーションに使われる. 
- `MAGMA`: [元論文](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1004219). 2015年.
- `FUMA`: [元論文](https://www.nature.com/articles/s41467-017-01261-5), [リンク](https://github.com/Kyoko-wtnb/FUMA-webapp/). 2017年. webベースのアプリケーション. eQTLやCADD score, Hi-C, GWAS catalogなど複数の情報を元に, GWAS summary statisticsや遺伝子リストをインプットにすると機能的なアノテーションをしてくれたり, gene set enrichmentや組織特異的発現などを可視化してくれる.
- `SMMAT`: [元論文](https://www.cell.com/ajhg/fulltext/S0002-9297(18)30465-8). 2019年. 遺伝子単位の相関解析手法. 集団構造や血縁関係を補正するため, 一般化線形混合モデルを採用. O(N^3)の計算時間とO(N^2)のメモリ使用量がネックだった (下の`SAIGE-GENE`では改善).
- `SAIGE-GENE`: [元論文](https://www.nature.com/articles/s41588-020-0621-6). 2020年. 2020年6月時点で一番新しい, 遺伝子 (or 領域) 単位の相関解析手法. 近縁関係にある個体も使うことができるので, サンプルサイズを増やすことができるという利点がある. `SAIGE`同様, 2値形質を対象とする.
#### その他
- `METAL`: [元論文](https://academic.oup.com/bioinformatics/article/26/17/2190/198154), [Webページ](https://genome.sph.umich.edu/wiki/METAL). GWASのメタ解析手法.
- `GIANT`: [GWASのSummary Statisticsをまとめているデータベース](https://portals.broadinstitute.org/collaboration/giant/index.php/GIANT_consortium_data_files). ここには主にanthropometric (身長やBMIなど体型に関わる) データが置かれており, これを使ったメタ解析もよく行われる. これ以外にも様々なデータベースにGWASのSummary Statisticsデータは置かれている. [この論文](https://www.nature.com/articles/ng.3406)など参照. 例として[PGC (Psychiatric Genomics Consortium)](https://www.med.unc.edu/pgc/download-results/) が挙げられる. [Neale Lab](http://www.nealelab.is/uk-biobank)がUK Biobankのデータをまとめている. 日本人のデータであればRIKENの[JENGER](http://jenger.riken.jp/en/)がまとめているものもある.

### 関連の解析手法
- `Genomic control (GC)`: [元論文](https://onlinelibrary.wiley.com/doi/abs/10.1111/j.0006-341X.1999.00997.x). population stratification (後述) によってインフレーションを起こした遺伝子型-表現型間の相関を, λ (inflation factor) で補正するというアイデア. λが1.1を超えると, 有意となるSNPsが多すぎる ≒ 集団の構造化が疑われるらしい.
- `LD score (LDSC)`: [元論文](https://www.nature.com/articles/ng.3211). あるSNPについて, (周囲の; しばしば1Mbの範囲にある) 他の全てのSNPsとのLD (r^2) を合計したものが, そのSNPのLD scoreである. LD scoreを用いることで, ある形質のSNP-baseの遺伝率を計算したり, [独立な形質間の遺伝的な相関を計算する](https://www.nature.com/articles/ng.3406)ことができる. 必要なデータはGWASのSummary StatisticsとLDのデータ ([1KGについて計算したもの](https://data.broadinstitute.org/alkesgroup/LDSCORE/)がよく使われる) だけであり, 簡便に計算することができる. MAFが小さい (<1%) SNPsはLD scoreの分散が大きいため除かれるほか, 効果サイズが非常に大きいものや, LDの範囲が非常に長いものも除かれる. また, LDの計算に用いられたサンプルと, GWASが行われたサンプルが遺伝的に離れているとbiasが生じるので, できる限り近いものを用いる.
  - よく使われるソフトウェア: `LDSC` ([リンク](https://github.com/bulik/ldsc)).
- `Polygenic score (PGS)`: 特に疾患を対象にする場合はpolygenic risk score (PRS), risk profile scoring, genetic risk scoring, またgenotype score, genetic scoringなどと呼ばれたりもする. 注目している形質に各SNPj (計M個のSNPs) が与える効果サイズwj (通常GWASの結果そのままでなく, PGSの計算に使うSNPsの値だけで標準化?) を線型結合した値として, 各個体iについて以下の式で定義され, その個体の, 注目している表現型に対する遺伝的なリスクを表す (aij = {0, 1, 2}の値を取り,それぞれ遺伝子型 0/0, 0/1, 1/1に対応). <br>
<img src="https://latex.codecogs.com/gif.latex?PGS_i&space;=&space;\sum&space;_{j=1}&space;^M&space;a_{ij}w_j" title="PGS_i = \sum _{j=1} ^M a_{ij}w_j" /> <br>
しかし当然, 単純な線形結合で表すので, 劣性/顕性以外の複雑な遺伝様式は仮定しないし, 遺伝子間の相互作用 (epistasis) も考えない.<br>
どのSNPsをPRSの計算に用いるかについて, GWASで有意となった (一般に閾値として p < 5.0×10^-8 が使われる) SNPsだけでもいいし, 全てのSNPsを使ってもいい. これは注目している表現型や目的による. polygenicな背景がないと考えられる表現型であれば, より厳しい閾値でSNPsを減らすのがいいが, そうでなければ逆である. 用いるSNPsを増やせば予測能力が上がる気がするが, 実際にはノイズも増やすので, ここにトレードオフが存在する.<br>
PGSの運用においては, まず"Discovery/Base"集団にGWASを行った上で, 独立した"Target"集団にその結果を用いて, その予測性能を確認するという一連の流れが取られる. この時, 様々なP値の閾値を設定し, PRSの計算に用いるSNPsを決めるようである. また, [Discovery集団とTarget集団が遺伝的に離れていると, 正確な予測ができないことが知られている](https://www.cell.com/ajhg/fulltext/S0002-9297(17)30107-6).
  - よく使われるソフトウェア: `LDpred` [元論文](https://www.cell.com/ajhg/fulltext/S0002-9297(15)00365-1), `PRSice` [元論文](https://academic.oup.com/bioinformatics/article/31/9/1466/200539) / [PRSice2](https://academic.oup.com/gigascience/article/8/7/giz082/5532407), [リンク](https://www.prsice.info).
- `Mendelian Randomization (MR)`: [元論文](https://academic.oup.com/ije/article/32/1/1/642797). 医学/疫学分野でよく行われるランダム化比較試験 (RCT; 被験者をランダムに2群に分け, 交絡要因の影響を群間で等しくした上で, 見たい治療や薬の効果を調べる) のランダム化の部分を遺伝的変異の情報を用いて行うというもの. 形質との関連に交絡要因を含まないことや逆の因果関係を持たないことから, 遺伝的変異を操作変数として形質に影響を及ぼす因子との関連を推定できると考えられる.<br>
例えば, コーヒー摂取が肺がんに及ぼす影響を知りたい. この時, 喫煙というコーヒー摂取と肺がん罹患の両者に影響する交絡因子が考えられる. これまでであれば, 喫煙量を群間でコントロールした上でコーヒー摂取/非摂取群を作り, 肺がんに及ぼす影響を調べていた. しかしMRでは, コーヒー摂取と強く相関する遺伝的変異をGWASで特定した上で, その遺伝子型で2群を分け, 肺がんリスクを群間で比較する, という研究になる. コーヒ摂取 (あるいは他の要因) と喫煙が遺伝的な相関を持たない限り, コーヒー摂取傾向のみが肺がんに与える影響を推定できると考えられる. <br>
MRにおいては, 1) 遺伝的変異はリスク因子と関連し, 2) 遺伝的変異は交絡因子とは独立であり, 3) 遺伝的変異はリスク因子を経てのみ, 調べたい表現型に影響する, という前提が重要であり, これに当てはまらない場合は利用できない. 例えば, 遺伝子が多面効果を持ち, 交絡因子と関連してしまう場合が挙げられる. また, 集団の構造やLDの問題など, 操作する遺伝的変異の選択においても注意が必要.

### 関連項目
- 線形混合モデル (LMM: Linear Mixed Model): SNPの効果を固定効果, 性別や年齢 (, 年齢^2, 場合によってはBMIも), 遺伝的な集団構造 (e.g. PC1–10) を変量効果として入れて解析を行うスタンダードなモデル. `PLINK`で解析可能. 基本的に前述の手法は全てこの改良版. 個体数N, 計M個のSNPsがあった時, 個体の表現型yは, 各SNPxの固定効果β (これを求めるのが目的となる), サンプル間の遺伝的距離 (kinship matrixあるいはgenetic relationship matrix (GRM)と呼ばれる) に基づくpolygenicな変量効果g, エラー項eで表される. <br>
  <img src="https://latex.codecogs.com/gif.latex?\begin{pmatrix}&space;y_{1}&space;\\&space;\cdot&space;\\&space;\cdot&space;\\&space;y_{N}&space;\end{pmatrix}&space;=&space;\begin{pmatrix}&space;x_{11}&space;&&space;\cdot&space;\cdot&space;\cdot&space;&&space;x_{1M}\\&space;\cdot&space;&&space;&&space;\cdot&space;\\&space;\cdot&space;&&space;&&space;\cdot&space;\\&space;x_{N1}&space;&&space;\cdot&space;\cdot&space;\cdot&space;&&space;x_{NM}&space;\end{pmatrix}&space;\begin{pmatrix}&space;\beta_{1}&space;\\&space;\cdot&space;\\&space;\cdot&space;\\&space;\beta_{M}&space;\end{pmatrix}&space;&plus;&space;\begin{pmatrix}&space;g_{1}&space;\\&space;\cdot&space;\\&space;\cdot&space;\\&space;g_{N}&space;\end{pmatrix}&space;&plus;&space;\begin{pmatrix}&space;e_{1}&space;\\&space;\cdot&space;\\&space;\cdot&space;\\&space;e_{N}&space;\end{pmatrix}" title="\begin{pmatrix} y_{1} \\ \cdot \\ \cdot \\ y_{N} \end{pmatrix} = \begin{pmatrix} x_{11} & \cdot \cdot \cdot & x_{1M}\\ \cdot & & \cdot \\ \cdot & & \cdot \\ x_{N1} & \cdot \cdot \cdot & x_{NM} \end{pmatrix} \begin{pmatrix} \beta_{1} \\ \cdot \\ \cdot \\ \beta_{M} \end{pmatrix} + \begin{pmatrix} g_{1} \\ \cdot \\ \cdot \\ g_{N} \end{pmatrix} + \begin{pmatrix} e_{1} \\ \cdot \\ \cdot \\ e_{N} \end{pmatrix}" /><br>
- Population stratification: 集団の構造化という. 同一の研究内で異なる集団由来のサンプルを含めてしまうこと. Hidden (cryptic) relatednessは, サンプル間に知られていなかった遺伝的な (血縁) 関係があること.
- Ridge/Lasso (回帰): 過学習を抑えるために正則化項の概念を入れた線形回帰. 過学習を抑えるためには, データセットをtraining setとvalidation setに (何度か) 分け, 結果のrobustnessを評価する方法もしばしば取られる.
- 正則化 (項): 回帰モデルにおいて, 各変数の (標準偏回帰) 係数の大きさ (これが大きいとデータに無理に当てはめていることになる) にペナルティを与えるため損失関数に入れる項.<br>
<img src="https://latex.codecogs.com/gif.latex?E_{OLS}&space;=&space;\sum&space;_{i=1}^{N}&space;(y_i&space;-&space;\widehat{y})^2" title="\sum _{i=1}^{N} (y_i - \widehat{y})^2" /><br>
これが線形回帰における基本的な損失関数 (最小2乗法) だが, Ridge回帰およびLasso回帰の場合は<br>
<img src="https://latex.codecogs.com/gif.latex?E_{Ridge}&space;=&space;\sum&space;_{i=1}^{N}&space;(y_i&space;-&space;\widehat{y})^2&space;&plus;&space;\frac{1}{2}&space;\lambda&space;\sum&space;_{k=1}^{K}&space;\beta_k&space;^2" title="E_{Ridge} = \sum _{i=1}^{N} (y_i - \widehat{y})^2 + \frac{1}{2} \lambda \sum _{k=1}^{K} \beta_k ^2" /><br>
<img src="https://latex.codecogs.com/gif.latex?E_{Lasso}&space;=&space;\sum&space;_{i=1}^{N}&space;(y_i&space;-&space;\widehat{y})^2&space;&plus;&space;\lambda&space;\sum&space;_{k=1}^{K}&space;\left&space;|\beta_k&space;\right&space;|" title="E_{Lasso} = \sum _{i=1}^{N} (y_i - \widehat{y})^2 + \lambda \sum _{k=1}^{K} \left |\beta_k \right |" /><br>
と, それぞれ項 (Ridge回帰では偏回帰係数の2乗和 (L2正則化項), Lasso回帰では偏回帰係数の絶対値の和 (L1正則化項)) が増えている. この二つは偏回帰係数の減衰の仕方が異なり, Ridge回帰は少しずつ偏回帰係数を小さくしていくのに対して, Lasso回帰は影響度の小さい説明変数の偏回帰係数を0にしていく. すなわち, Ridge回帰は全体の説明変数を使用しつつ偏回帰係数を小さくするのに対して、Lasso回帰は変数を選択して線形回帰を行っていると解釈できる. Lasso回帰の欠点として, データ数 (n) に対して特徴量 (p) が多い (p > n) 時は高々nこの辺数しか選択できない点, 相関の高い変数群がある時, その中の一つしか選択できない点が挙げられる. これを解決すべく考案されたのがElastic Net回帰 ([Zou & Hastie 2005](https://rss.onlinelibrary.wiley.com/doi/abs/10.1111/j.1467-9868.2005.00503.x))になる. L1正則化項とL2正則化項の和を考えることで, Lasso回帰の欠点をRidge回帰の性質で補う形になっている.

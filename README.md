## GWAS に関するメモ
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

### 使われる手法
- `線形混合モデル (LMM: Linear Mixed Model)`: SNPの効果 + 性別や年齢 (, 年齢^2), 遺伝的な集団構造 (e.g. PC1–10) を説明変数として入れて解析を行うスタンダードなモデル. `PLINK`で解析可能. 基本的に後述のモデルは全てこの改良版.
- `BOLT-LMM`: [元論文](https://www.nature.com/articles/ng.3190), [Webページ](https://data.broadinstitute.org/alkesgroup/BOLT-LMM/#x1-30001.1). Bayesian linear mixed-model associationを採用. GWASに使われる既存の混合モデルでは計算に時間がかかりすぎる (O(M^2N)あるいはO(MN^2)) のをO(MN)の繰り返しまでに改良している (ここをM < Nになるようマーカー自体を減らすというアイデアで実装したのが`FaST-LMM`) . また, 既存のLMMのように全てのSNPsが独立な正規分布に従う小さな効果サイズを持つと仮定するのではなく, あらかじめ二つの事前正規分布を仮定するベイズ過程によって, それぞれのSNPsの効果サイズの大小を正確に推定していく.
  - 最近の大きな論文では大抵使われている. 基本的にヒトを対象とする, 大きな (sample size > 5000) データセットに向いているらしい. sample size < 5000 の場合は`GCTA`あるいは`GEMMA`を推奨とのこと. 潜在的な集団構造/個人間の血縁度を補正してくれるのでPCAのスコアは入れる必要なし.
- `FaST-LMM`: [元論文](https://www.nature.com/articles/nmeth.1681). 混合モデルでは計算に時間がかかりすぎるという問題点をM < Nになるようマーカー自体を減らすというアイデアで実装している.
- `GEMMA`: [元論文](https://www.nature.com/articles/ng.2310). Genome-wide efficient mixed-model analysisの略.
- `GCTA`: [元論文](https://www.cell.com/ajhg/fulltext/S0002-9297(10)00598-7). genome-wide complex trait analysisの略.
- `MAGENTA`: [元論文](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1001058), [Webページ](https://software.broadinstitute.org/mpg/magenta/). 遺伝子単位の相関解析手法.
- `METAL`: [元論文](https://academic.oup.com/bioinformatics/article/26/17/2190/198154), [Webページ](https://genome.sph.umich.edu/wiki/METAL). GWASのメタ解析手法.

### 関連の解析手法
- `LD score regression (LDSR)`: よく使われるソフトウェア: `LDSC`.
- `Polygenic risk score (PRS)`: 
polygenic score (PGS), risk profile scoring, genetic scoring, genetic risk scoringなどと呼ばれたりもする. よく使われるソフトウェア: `LDpred`, `PRSice`.
- `Mendelian Randomization (MR)`: [元論文](https://academic.oup.com/ije/article/32/1/1/642797). 医学/疫学分野でよく行われるランダム化比較試験 (RCT; 被験者をランダムに2群に分け, 交絡要因の影響を群間で等しくした上で, 見たい治療や薬の効果を調べる) のランダム化の部分を遺伝的変異の情報を用いて行うというもの. 形質との関連に交絡要因を含まないことや逆の因果関係を持たないことから, 遺伝的変異を操作変数として形質に影響を及ぼす因子との関連を推定できると考えられる.<br>
例えば, コーヒー摂取が肺がんに及ぼす影響を知りたい. この時, 喫煙というコーヒー摂取と肺がん罹患の両者に影響する交絡因子が考えられる. これまでであれば, 喫煙量を群間でコントロールした上でコーヒー摂取/非摂取群を作り, 肺がんに及ぼす影響を調べていた. しかしMRでは, コーヒー摂取と強く相関する遺伝的変異をGWASで特定した上で, その遺伝子型で2群を分け, 肺がんリスクを群間で比較する, という研究になる.<br>
MRにおいては, 1)遺伝的変異はリスク因子と関連し, 2)遺伝的変異は交絡因子とは独立であり, 3)遺伝的変異はリスク因子を経てのみ, 調べたい表現型に影響する、という前提が重要であり, これに当てはまらない場合は利用できない. 例えば, 遺伝子が多面効果を持ち, 交絡因子と関連してしまう場合が挙げられる. また, 集団の構造やLDの問題など, 遺伝的変異の選択においても注意が必要である.

### 関連項目
- Ridge/Lasso (回帰): 過学習を抑えるために正則化項の概念を入れた線形回帰のこと.
- 正則化 (項): 線形回帰において, 各変数の標準偏回帰係数の大きさ (これが大きいとデータに無理に当てはめていることになる) にペナルティを与えるため損失関数に入れる項.<br>
<img src="https://latex.codecogs.com/gif.latex?E_{OLS}&space;=&space;\sum&space;_{i=1}^{N}&space;(y_i&space;-&space;\widehat{y})^2" title="\sum _{i=1}^{N} (y_i - \widehat{y})^2" /><br>
これが線形回帰における基本的な損失関数 (最小2乗法) だが, Ridge回帰およびLasso回帰の場合は<br>
<img src="https://latex.codecogs.com/gif.latex?E_{Ridge}&space;=&space;\sum&space;_{i=1}^{N}&space;(y_i&space;-&space;\widehat{y})^2&space;&plus;&space;\frac{1}{2}&space;\lambda&space;\sum&space;_{k=1}^{K}&space;\beta_k&space;^2" title="E_{Ridge} = \sum _{i=1}^{N} (y_i - \widehat{y})^2 + \frac{1}{2} \lambda \sum _{k=1}^{K} \beta_k ^2" /><br>
<img src="https://latex.codecogs.com/gif.latex?E_{Lasso}&space;=&space;\sum&space;_{i=1}^{N}&space;(y_i&space;-&space;\widehat{y})^2&space;&plus;&space;\lambda&space;\sum&space;_{k=1}^{K}&space;\left&space;|\beta_k&space;\right&space;|" title="E_{Lasso} = \sum _{i=1}^{N} (y_i - \widehat{y})^2 + \lambda \sum _{k=1}^{K} \left |\beta_k \right |" /><br>
と, それぞれ項 (Ridge回帰では偏回帰係数の2乗和 (L2正則化項), Lasso回帰では偏回帰係数の絶対値の和 (L1正則化項)) が増えている. この二つは偏回帰係数の減衰の仕方が異なり, Ridge回帰は少しずつ偏回帰係数を小さくしていくのに対して, Lasso回帰は影響度の小さい説明変数の偏回帰係数を0にしていく. すなわち, Ridge回帰は全体の説明変数を使用しつつ偏回帰係数を小さくするのに対して、Lasso回帰は変数を選択して線形回帰を行っていると解釈できる.










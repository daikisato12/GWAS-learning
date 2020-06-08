## GWAS に関するメモ
### ゲノムワイド関連解析とは？
個体の一塩基多型 (Single Nucleotide Polymorphisms: SNPs)の遺伝子型と表現型との相関を統計的に解析する手法. ヒトの疾患の遺伝的要因同定のために使われることが多いが, 生物の (遺伝的に規定される) あらゆる表現型の遺伝的背景を探ることができ, 育種・農学分野や進化・生態学分野においても有力な解析手法である.

### 解析における一般的な注意点
解析に用いる個体
- call rateの低い (e.g. <0.98) 個体を除く.
- 血縁関係にある個体 (e.g. PI_HAT > 0.12) を除く.
- (ヒトの場合) 自己申告と遺伝的な性が一致しない人を除く. 
- 表現型との混交を避けるため, 同一集団に由来する遺伝的背景が均一な個体を使い, 内部の集団構造が存在しないことが重要. PCAにより, 各集団から外れる個体を除くなど.

解析に用いるSNPs
- call rateの低い (e.g. <0.99) SNPsを除く.
- ジェノタイピングのエラーを避けるため, マイナーアリル頻度（Minor Allele Frquency: MAF）が低い (e.g. <1%) SNPsは排除する. サンプル数が多ければ, 低頻度のアリルもエラーでなく捉えることができるため, この閾値はサンプル数に依存する.
- (計算の煩雑さのため?) 遺伝子型が2つの (Biallelicな) SNPsのみを対象とする.

### 使われる手法
- `線形混合モデル (LMM: Linear Mixed Model)`: SNPの効果 + 性別や年齢, 遺伝的な集団構造 (e.g. PC1–10) を説明変数として入れて解析を行うスタンダードなモデル. `PLINK`で解析可能. 基本的に後述のモデルは全てこの改良版.
- `BOLT-LMM`: [元論文](https://www.nature.com/articles/ng.3190). Bayesian linear mixed-model associationを採用. GWASに使われる既存の混合モデルでは計算に時間がかかりすぎる (O(M^2N)あるいはO(MN^2)) のをO(MN)の繰り返しまでに改良している (ここをM < Nになるようマーカー自体を減らすというアイデアで実装したのが`FaST-LMM`) . また, 既存のLMMのように全てのSNPsが独立な正規分布に従う小さな効果サイズを持つと仮定するのではなく, あらかじめ二つの事前正規分布を仮定するベイズ過程によって, それぞれのSNPsの効果サイズの大小を正確に推定していく.
  - 最近の大きな論文では大抵使われている. 基本的にヒトを対象とする, 大きな (sample size > 5000) データセットに向いているらしい. sample size < 5000 の場合は`GCTA`あるいは`GEMMA`を推奨とのこと. 潜在的な集団構造/個人間の血縁度を補正してくれるのでPCAのスコアは入れる必要なし.
- `FaST-LMM`: [元論文](https://www.nature.com/articles/nmeth.1681). 混合モデルでは計算に時間がかかりすぎるという問題点をM < Nになるようマーカー自体を減らすというアイデアで実装している.
- `GEMMA`: [元論文](https://www.nature.com/articles/ng.2310). Genome-wide efficient mixed-model analysisの略.
- `GCTA`: [元論文](https://www.cell.com/ajhg/fulltext/S0002-9297(10)00598-7). genome-wide complex trait analysisの略.

### 関連の解析手法
- `LD score regression (LDSC)`: 
- `Polygenic risk score (PRS)`: 
polygenic score (PGS), risk profile scoring, genetic scoring, genetic risk scoringなどと呼ばれたりもする. よく使われるソフトウェア: `LDpred`, `PRSice`.
- `Mendelian Randomization (MR)`: [元論文](https://academic.oup.com/ije/article/32/1/1/642797). 

### 関連項目
- Ridge/Lasso (回帰): 過学習を抑えるために正則化項の概念を入れた線形回帰のこと.
- 正則化 (項): 線形回帰において, 各変数の標準偏回帰係数の大きさ (これが大きいとデータに無理に当てはめていることになる) にペナルティを与えるため損失関数に入れる項.<br>
<img src="https://latex.codecogs.com/gif.latex?E_{OLS}&space;=&space;\sum&space;_{i=1}^{N}&space;(y_i&space;-&space;\widehat{y})^2" title="\sum _{i=1}^{N} (y_i - \widehat{y})^2" /><br>
これが線形回帰における基本的な損失関数 (最小2乗法) だが, Ridge回帰およびLasso回帰の場合は<br>
<img src="https://latex.codecogs.com/gif.latex?E_{Ridge}&space;=&space;\sum&space;_{i=1}^{N}&space;(y_i&space;-&space;\widehat{y})^2&space;&plus;&space;\frac{1}{2}&space;\lambda&space;\sum&space;_{k=1}^{K}&space;\beta_k&space;^2" title="E_{Ridge} = \sum _{i=1}^{N} (y_i - \widehat{y})^2 + \frac{1}{2} \lambda \sum _{k=1}^{K} \beta_k ^2" /><br>
<img src="https://latex.codecogs.com/gif.latex?E_{Lasso}&space;=&space;\sum&space;_{i=1}^{N}&space;(y_i&space;-&space;\widehat{y})^2&space;&plus;&space;\lambda&space;\sum&space;_{k=1}^{K}&space;\left&space;|\beta_k&space;\right&space;|" title="E_{Lasso} = \sum _{i=1}^{N} (y_i - \widehat{y})^2 + \lambda \sum _{k=1}^{K} \left |\beta_k \right |" /><br>
と, それぞれ項 (Ridge回帰では偏回帰係数の2乗和(L2正則化項), Lasso回帰では偏回帰係数の絶対値の和(L1正則化項)) が増えている. この二つは偏回帰係数の減衰の仕方が異なり, Ridge回帰は少しずつ偏回帰係数を小さくしていくのに対して, Lasso回帰は影響度の小さい説明変数の偏回帰係数を0にしていく. すなわち, Ridge回帰は全体の説明変数を使用しつつ偏回帰係数を小さくするのに対して、Lasso回帰は変数を選択して線形回帰を行っていると解釈できる.










## GWAS に関するメモ
### ゲノムワイド関連解析とは？
個体の一塩基多型 (Single Nucleotide Polymorphisms: SNPs)の遺伝子型と表現型との相関を統計的に解析する手法. ヒトの疾患の遺伝的要因同定のために使われることが多いが, 生物の (遺伝的に規定される) あらゆる表現型の遺伝的背景を探ることができ, 進化・生態学分野においても有力な解析手法である.

### 解析における一般的な注意点
解析に用いる個体
- 血縁関係にある個体 (e.g. PI_HAT > 0.12) を除く.
- call rateの低い (e.g. <0.98) 個体 を除く.
- (ヒトの場合) 自己申告と遺伝的な性が一致しない人を除く. 
- 表現型との混交を避けるため, 同一集団に由来する遺伝的背景が均一な個体を使い, 内部の集団構造が存在しないことが重要. PCAにより, 各集団から外れる個体を除くなど.

解析に用いるSNPs
- ジェノタイピングのエラーを避けるため, マイナーアリル頻度（Minor Allele Frquency: MAF）が低い (e.g. <1%) SNPsは排除する. サンプル数が多ければ, 低頻度のアリルもエラーでなく捉えることができるため, この閾値はサンプル数に依存する.
- (計算の煩雑さのため?) 遺伝子型が2つの (Biallelicな) SNPsのみを対象とする.

### 使われる手法
- `線形混合モデル`: 最も基本的なモデル. SNPの効果 + 性別や年齢, 集団構造 (e.g. PC1–10)を説明変数として入れて解析を行う. `PLINK`で解析可能.
- `BOLT-LMM`: Bayesian linear mixed-model associationを採用. 基本的にヒトを対象とする, 大きな (サンプル数 > 5000)データセットに向いているらしい. sample size < 5000 の場合は`GCTA`あるいは`GEMMA`を推奨とのこと.

### 派生的な手法
- `LD score regression (LDSC)`
- `Polygenic risk score (PRS)`
polygenic score, risk profile scoring, genetic scoring, genetic risk scoringなどと呼ばれたりもする. よく使われるソフトウェア: `LDpred`, `PRSice`.
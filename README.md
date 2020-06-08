## GWAS に関するメモ
### ゲノムワイド関連解析とは？
個体の一塩基多型 (Single Nucleotide Polymorphisms: SNPs)の遺伝子型と表現型との相関を統計的に解析する手法. ヒトの疾患の遺伝的要因同定のために使われることが多いが, 生物の (遺伝的に規定される) あらゆる表現型の遺伝的背景を探ることができ, 進化・生態学分野においても有力な解析手法である.

### 解析における一般的な注意点
- 血縁関係にない個体を用いる.
- ジェノタイピングのエラーを避けるため, マイナーアリル頻度（Minor Allele Frquency: MAF）が1%以上のSNPsのみを対象とする. サンプル数が多ければ, 低頻度のアリルもエラーでなく捉えることができるため, この閾値はサンプル数に依存する.
- (計算の煩雑さのため?) 遺伝子型が2つの (Biallelicな) SNPsのみを対象とする.
- 表現型との混交を避けるため, 同一集団に由来する遺伝的背景が均一な個体を使い, 内部の集団構造が存在しないことが重要.

### 使われる手法
- `線形混合モデル`
- `BOLT-LMM`

### 派生的な手法
- `LD score regression (LDSC)`
- `Polygenic risk score (PRS)`
polygenic score, risk profile scoring, genetic scoring, genetic risk scoringなどと呼ばれたりもする. よく使われるソフトウェア: `LDpred`, `PRSice`.
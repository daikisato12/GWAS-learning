### Quality control
#### 個体のフィルタリング
- call rateの低い (e.g. <0.98) 個体を除く.
- 血縁関係にある個体 (e.g. PI_HAT > 0.12, p(IBD = 0) < 0.05 etc.) を除く.
- (ヒトの場合) 自己申告と遺伝的な性が一致しない人を除く. 
- 表現型との混交を避けるため, 同一集団に由来する遺伝的背景が均一な個体を使い, 内部の集団構造が存在しないことが重要. PCAにより, 各集団から外れる個体を除くなど.

#### SNPsのフィルタリング
- call rateの低い (e.g. <0.99) SNPsを除く.
- ジェノタイピングのエラーを避けるため, マイナーアリル頻度（Minor Allele Frquency: MAF）が低い (e.g. <1%) SNPsは排除する. サンプル数が多ければ, 低頻度のアリルもエラーでなく捉えることができるため, この閾値はサンプル数に依存する.
- (計算の煩雑さのため?) 遺伝子型が2つの (Biallelicな) SNPsのみを対象とする.
- Hardy–Weinberg Equilibriumに沿わない (e.g. p < 1×10^–6) SNPsは除く.

### Phasing
次世代シーケンスデータは二倍体生物の各haploidを区別することができない. これを区別することをphasingと呼び, 正確なphasingをすると, アリル特異的な効果を調べたり, 座位間の連鎖情報を利用することができるようになるので, 解析の幅が広がる. 以前は親類個体のゲノムデータがないと正確なphasingはできないと考えられてきたが, 現在では様々な推定手法が開発されている. レビュー論文も参照 ([Tewhey et al. 2011 Nat. Rev. Genet.](https://www.nature.com/articles/nrg2950); [Browning & Browning 2011 Nat. Rev. Genet.](https://www.nature.com/articles/nrg3054)).

- [`Eagle2`](https://www.nature.com/articles/ng.3679): `SHAPEIT2`より~20倍速く, ~10%正確らしい. Reference Panelを利用する.
- [`Beagle5`](https://www.cell.com/ajhg/fulltext/S0002-9297(18)30242-8): 
- [`SHAPEIT4`](https://www.nature.com/articles/s41467-019-13225-y): 最近開発されたらしい. `Beagle5`や`Eagle2`よりも高速.

### Imputation
GWASでは, 解析に用いることのできる遺伝的変異の数が多いほど検出力が上がると考えられる. また, 他のデータセットで使用されるSNPsとの重なりも増えることから, メタ解析も容易になる. 一方で, サンプル数が100万人単位にまで激増している現在, 全サンプルに対して全ゲノムシーケンスを施すのは不可能であり, 多くの研究ではExomeに限った数万~数10万程度のArrayやChipによるgenotypingが行われている. こうしたArrayデータでは遺伝子型が判明されなかった変異について, 同一集団のリファレンスパネル (周囲の座位との連鎖情報) を用いて推定するのがImputationである.
- [Haplotype Reference Panel](https://www.nature.com/articles/ng.3643): Imputationに使われるレファレンスパネル.
- [`Minimac3`](https://www.nature.com/articles/ng.3656): 
- [`MaCH`](https://onlinelibrary.wiley.com/doi/full/10.1002/gepi.20533): 
- [`IMPUTE2`](https://www.nature.com/articles/ng.2354)([`IMPUTE`](https://www.nature.com/articles/ng2088)): 

### 表現型データをどう扱うか
- 2値データ (例えば疾患の有無) や 単純な連続値 (体重や身長など) はそのまま扱えばいい.
- アンケートデータを元にした順序尺度 (`1.全く当てはまらない` ~ `5. 非常によく当てはまる`) などは色々とやり方がある.
  - (複数項目あれば) 合計得点を表現型とする.
  - (連続値に近似可能であれば) 各順位に点数を割り当てる (e.g. `１ヶ月の飲酒頻度は？`という質問に対して, `1. 全く飲まない`であれば0, `2. 月に1回以下`は0.12, `3. 月に1~3回`であれば0.46, `4. 週に1,2回`であれば1.5など, 1週間に飲む回数の期待値を割り当てる).
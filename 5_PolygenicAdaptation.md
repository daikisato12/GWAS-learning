### Polygenic adaptation, PGSと自然選択の関係
古典的なSelection scanでは, 少数の大きな効果を持つ遺伝子座は特定できても, (特にいわゆる複雑な) 形質全体に弱く働く選択を検出することはできない. 大規模なGWASが可能になった今, Polygenicに働く自然選択の検出はここ数年ホットなトピックとなっている.

#### [McVicker et al. 2009 _PLoS Genet._](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1000471)
他の霊長類とのゲノム比較によるヒトの遺伝的多様性の評価. 保存性の高い領域周辺では, ヒトや古人類の中立な多型は大幅に減少していた. ゲノム全体に遺伝的多様性を失わせる"background selection"が働いている証拠. Hitch-hikingともいわれるが, 有害なサイトに働く負の選択が周囲の連鎖した中立サイトの遺伝的多様性まで減少させるというもの (正の選択の場合も同様に多様性は減少するが, この時は単にpositive selectionと呼ばれる). background selectionを定量するB統計量の提案. <br><br>

<img src="https://latex.codecogs.com/gif.latex?B&space;=&space;B^{(v)}&space;=&space;\frac{\pi_e}{\pi_0}&space;=&space;\frac{N_e}{N_0}&space;\approx&space;\left&space;[&space;-\sum_x&space;\int_0^1&space;\frac{u_x&space;f_x(t)dt}{t(1&plus;(1-t)r_{x,v}/t)^2}&space;\right&space;]" title="B = B^{(v)} = \frac{\pi_e}{\pi_0} = \frac{N_e}{N_0} \approx \left [ -\sum_x \int_0^1 \frac{u_x f_x(t)dt}{t(1+(1-t)r_{x,v}/t)^2} \right ]" /><br>

サイト<img src="https://latex.codecogs.com/gif.latex?v" title="v" />で期待される塩基多様度を<img src="https://latex.codecogs.com/gif.latex?\pi_e" title="\pi_e" />, background selectionが働いていない場合に期待される中立座位の塩基多様度を<img src="https://latex.codecogs.com/gif.latex?\pi_0" title="\pi_0" />, 同様にそれぞれの座位における有効集団サイズの期待値を<img src="https://latex.codecogs.com/gif.latex?N_e" title="N_e" />, <img src="https://latex.codecogs.com/gif.latex?N_0" title="N_0" />とする. 選択されたサイト<img src="https://latex.codecogs.com/gif.latex?x" title="x" />について, <img src="https://latex.codecogs.com/gif.latex?u_x" title="u_x" />はサイト<img src="https://latex.codecogs.com/gif.latex?x" title="x" />における世代あたりの (ハプロイドの) 有害変異の割合, <img src="https://latex.codecogs.com/gif.latex?r_{x,v}" title="r_{x,v}" />はサイト<img src="https://latex.codecogs.com/gif.latex?v" title="v" />と<img src="https://latex.codecogs.com/gif.latex?x" title="x" />の間の世代あたり組換え率. <img src="https://latex.codecogs.com/gif.latex?t" title="t" />はヘテロ接合体に対する選択の強さを示す. <img src="https://latex.codecogs.com/gif.latex?(1-t)" title="(1-t)" />はサイト<img src="https://latex.codecogs.com/gif.latex?x" title="x" />の有害アリルをヘテロで持つ個体の適応度であり, 非有害アリルをホモで持つ個体に対する相対的な値となる. <img src="https://latex.codecogs.com/gif.latex?f_x(t)" title="f_x(t)" />はサイト<img src="https://latex.codecogs.com/gif.latex?x" title="x" />において異なる強さの選択を受けるアリルの確率密度. ヒトゲノムのデータは[ここ](http://www.phrap.org/othersoftware.html)からダウンロード可能.

#### [Pritchard et al. 2010 _Curr. Biol._](https://www.cell.com/current-biology/fulltext/S0960-9822(09)02070-3)

#### [Kemper et al. 2014 _BMC Genomics_](https://bmcgenomics.biomedcentral.com/articles/10.1186/1471-2164-15-246)
自然選択の検出により少数の表現型に寄与する遺伝子座は特定できるが, 全体としては, 形質に影響する遺伝子座と自然選択の間に相関は見つけられない. 複雑な形質は古典的な自然選択の痕跡を残さないため, そうした遺伝子座を自然選択の検出により見つけることは難しい.

#### [Berg & Coop 2014 _PLoS Genet._](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1004412)
集団レベルでのPGS (この研究ではmean genetic valueと呼ばれる) は以下の式で表される (Fisher 1930). 計L個の各SNP (の効果アリル) がαの効果サイズを持つ時, 各SNP (の効果アリル) の集団内での頻度pを用いて, その集団mについてmean genetic value/effective sizeが求められる. <br><br>
<img src="https://latex.codecogs.com/gif.latex?Z_m&space;=&space;2\sum&space;_{l=1}^L&space;\alpha_lp_{ml}" title="Z_m = 2\sum _{l=1}^L \alpha_lp_{ml}" /><br>

本研究では, genetic valueの同時 (確率) 分布を求める集団遺伝学的なモデリングにより, 集団構造の効果を取り除くことで, 遺伝的浮動の効果ではなく自然選択がどのように遺伝子座に働くかを示している. 有意に選択が働いている遺伝子座を特定するだけでなく, 選択が働いている集団を特定することも可能.

#### [Field et al. 2016 _Science_](https://science.sciencemag.org/content/354/6313/760)
SDSを開発した論文. 最近の選択によりアリル頻度が変化した時, その二つのアリルそれぞれのコアレセント時間の分布に違いが生まれるだろうというアイデア. 一気に広がったハプロタイプ (tip lengthが短い) は周辺のシングルトンの密度が低いのに対し, 選択を受けていない (tip lengthが相対的に長い) ハプロタイプは, 周辺に多くのシングルトンがあるため, 最近傍のシングルトンまでの距離が短い. この最近傍シングルトンまでの距離から, tip lengthを最尤推定する. 標準化前のSDS*は, 各個体のコアレセントツリーから, 祖先型/派生型アリルを持つ個体の平均tip length (<img src="https://latex.codecogs.com/gif.latex?\hat{t}_A" title="\hat{t}_A" />および<img src="https://latex.codecogs.com/gif.latex?\hat{t}_D" title="\hat{t}_D" />; 各アリルについて最尤推定される) により, 以下の式で定義される. [コードのリンクはこちら](https://github.com/yairf/SDS). <br><br>
<img src="https://latex.codecogs.com/gif.latex?SDS*&space;=&space;log(\frac{\hat{t}_A}{\hat{t}_D})" title="SDS* = log(\frac{\hat{t}_A}{\hat{t}_D})" />

#### [Jain & Stephan 2017 _Genetics_](https://www.genetics.org/content/206/1/389.long)

#### [Gazal et al. 2017 _Nat. Genet._](https://www.nature.com/articles/ng.3954)

#### [Sanjak et al. 2018 _PNAS_](https://www.pnas.org/content/115/1/151)

#### [Novembre & Barton 2018 _Genetics_](https://www.genetics.org/content/208/4/1351)

#### [Okada et al. 2018 _Nat. Commun._](https://www.nature.com/articles/s41467-018-03274-0)

#### [Racimo et al. 2018 _Genetics_](https://www.genetics.org/content/208/4/1565)

#### [Zeng et al. 2018 _Nat. Genet._](https://www.nature.com/articles/s41588-018-0101-4)

#### [Guo et al. 2018 _Nat. Commun._](https://www.nature.com/articles/s41467-018-04191-y)
多くの研究がヒト集団間の遺伝的分化における自然選択の働きを示唆しているが, 自然選択のシグナルが特定の複雑な形質にenrichしているかどうか調べるというのは簡単なことではない. それは, 1) ほとんどの複雑な形質はそれぞれが弱い効果を持つ多数の遺伝子座から成り立っており, 遺伝的な分化のシグナルはそれらの間で薄まり, 完全な選択的一掃を探索する手法では検出できない. 2) 遺伝的分化は環境要因によってマスクされうる からである. 

まず表現型と相関するSNPsが集団間でアリル頻度に (遺伝的浮動から期待されるよりも有意な) 分化を示すかどうかを調べた. 集団間でアリル頻度に有意な分化が見られた形質に関しては, その方向性をPGSの計算/リスクアリルの平均頻度によって調べた. また, あるSNPが自然選択を受けているのであれば, そのSNPのLDスコア (1Mbの範囲にある他のSNPsとのr^2の合計値) もまた集団間で分化を示すはず → これも調べた. 

Fstエンリッチメント解析 (表現型と相関するSNPsが高いFstを示すかどうか; [Zhang et al. 2013](https://www.sciencedirect.com/science/article/pii/S2212066113000173))の結果, 身長, BMI, 統合失調症と相関するSNPsは, MAF/LD scoreのマッチするコントロールのSNP群と比べて平均のFst (EUR/EAS/AFRの3集団間で計算) が高かった. → これらの形質は自然選択を受けている (この時点ではそこまで言えないのでは？ ヨーロッパ集団のGWASの結果をもとに選抜した (**ヨーロッパ集団では**それらの形質と相関している) SNPsが, 3集団間で遺伝的浮動よりは有意に分化しているというだけ). ちなみに本研究で使われているWrightのFstは, <img src="https://latex.codecogs.com/gif.latex?\bar{p}" title="\bar{p}" /> を全集団でのアリル頻度, <img src="https://latex.codecogs.com/gif.latex?p_i" title="p_i" />をi番目の集団でのアリル頻度として, 以下の式で与えられる. <br><br>
<img src="https://latex.codecogs.com/gif.latex?F_{ST}&space;=&space;\frac{\bar{p}(1-\bar{p})-\sum&space;c_ip_i(1-p_i)}{\bar{p}(1-\bar{p})}&space;=&space;\frac{\bar{p}(1-\bar{p})-\overline{p(1-p)}}{\bar{p}(1-\bar{p})}" title="F_{ST} = \frac{\bar{p}(1-\bar{p})-\sum c_ip_i(1-p_i)}{\bar{p}(1-\bar{p})} = \frac{\bar{p}(1-\bar{p})-\overline{p(1-p)}}{\bar{p}(1-\bar{p})}" /><br>

次に, PGSの計算により遺伝的分化の方向性を調べた. LD clumpingで選抜したSNP群と, 同数のMAF/LD scoreがマッチするSNP群 (さらにそれを10,000セット)とを使い, それぞれPGSを計算するとEUR集団の身長PGSはEAS/AFR集団より高いし, 統合失調症のPGSはAFR集団で高い. これらはいずれもそれまでの先行研究に一致する. ただ上述のように, discovery sampleがEURなのにtarget sampleの異なる集団間の比較はそもそも成り立つのかという問題がある. そこで, 上のPGSの結果をより直接的に, trait-increasing allelesの頻度 (fTIAs) 比較で確かめる. 各SNPの効果の方向性は集団構造の影響を受けづらいからである. ← これはいいかも. 結果, 身長を高くするアリルはEASに比べてEUR集団で頻度が高い / 統合失調症はEURに比べてAFR集団で頻度が高い / BMIはEURに比べてEASで頻度が高い傾向があった.

_統合失調症 (おそらく他の精神疾患も) の遺伝的リスクがAFR集団で高いとなるのは, これまでの先行研究でも指摘されているし, 自分の注目しているSNPでもそう. 実際の表現型としてもそういう報告がある ([Schwartz & Blankenship 2014](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4274585/); [Bresnahan et al. 2007](https://academic.oup.com/ije/article/36/4/751/665657)) が本当だろうか？ 身長の話同様, 何か交絡要因を考慮できていない等の技術的な問題があるような気がしてしまう. また, 繰り返し指摘されているように, Socioeconomicな要因は確かに大きい. アフリカ集団を対象にした100万人規模のGWASを待ちたい._

問題点が5つ, 
1. Fst enrichment testは高度にpolygenicな形質に対しては検出力が下がる.　(図3でコントロールのSNPsでも期待値の0からずれている (集団間で頻度の差を示している) ことから分かるように) コントロール群のSNPsには, 表現型に影響するcausal variantsとLDにあるものが偶然含まれる可能性があるから.
2. 何度も指摘されているように, GWASのサンプルがEUR集団であるということ. EURと非EUR集団で遺伝的な異質性があると, 非EUR集団でのPGSの計算はバイアスを受けることになる. ← しかし, fTIAの計算でも概ねPGSの傾向と一致していたので, 大きな問題は無さそうとのこと. 
3. 形質と相関する全てのSNPsについてFst (あるいはLD score) を計算しているので, 今回見られた集団間の遺伝的分化が, 異なる遺伝子座に働く自然選択によるものなのか, 同一の遺伝子座だが異なるアリルに働く自然選択によるのか, あるいは同一のアリルだが, 自然選択の程度が集団間で異なるのか, を区別できない. ← ここのロジックはまだちょっと理解できていない. 
4. 異なるアリルが集団間で選択を受けているように思えるが, background selectionの可能性 (遺伝的分化が強いSNPsは, 負の選択を受けるcausal variantsとLDにあるのでは) も捨てきれない. 実際`SLiM`を使ったforward simulationでは, background selectionが遺伝的多様性を減少させ, 負の選択を受ける変異とLDにある変異について, 集団間の遺伝的分化を高めることが示された. 
5. FstやLD scoreにより自然選択を示したのが本研究だが, これらは**いつ**選択が働いたのか, あるいは他の種類の自然選択が生じているのかについては分からない. これについては最近開発された[Admixture graphを用いた手法 (Racimo et al. 2018)](https://www.genetics.org/content/208/4/1565)を参照.


#### [Edge & Coop 2019 _Genetics_](https://www.genetics.org/content/211/1/235)

#### [Uricchio et al. 2019 _Evol. Lett_](https://onlinelibrary.wiley.com/doi/full/10.1002/evl3.97)
ヒトの複雑な形質に働く自然選択を議論する上で先行研究が見落としがちだったmutational bias (新たな変異がある形質を増加させたり減少させやすいという傾向) に着目している. こうしたバイアスがあると, 若い (頻度の低い) アリルにばかりTIAがある, みたいな状況が生まれるかもしれない. アリル頻度と効果サイズの関係性を定量する要約統計量を提案する.

各SNPの派生型アリルの効果サイズと頻度をプロットしたβDAF plotを見ると, mutational biasと注目している形質に対する選択が分かる. 中立を仮定した帰無仮説では, trait-increasingな効果とtrait-decreasingの効果は同じ確率で生じるので, どのアリル頻度のbinでもβDAFのカーブは平らになるはず. 選択がなくても, mutational biasが強い場合はβの平均値が0からずれる. このβDAF plotのカーブの下側の面積を求め (要は任意の範囲のアリル頻度のbinについて計算されたβの合計値), 以下の通りSβと定義する. <br><br>
<img src="https://latex.codecogs.com/gif.latex?S_{\beta}(f_i,&space;f_j)&space;=&space;\sum_{y&space;\in&space;Y_{ij}}&space;\bar{\beta}(y)" title="S_{\beta}(f_i, f_j) = \sum_{y \in Y_{ij}} \bar{\beta}(y)" /><br>

選択が働かず, mutational biasのみが存在する時は上述の通り, どのアリル頻度のbinでもβの値は同じになるので, Sβの値も単に各binのβの平均値にbinの数を掛け合わせたものになる (y軸をSβ(0,x), xを横軸にとるグラフを考えると, 単調増加するグラフとなる).

形質に長期間の安定化選択 (最適値があり, そこから下がるのも上がるのも有害となる) が働いている時, TIAとTDAは同等に有害である. さらにmutational biasが存在しない場合, TIAもTDAも同じ確率で生じるので, Sβの期待値は0になる. trait-increasing/decreasingどちらかにmutational biasが存在する場合は, 0から外れた値になる. この時 (選択とmutational biasがどちらも存在する時), 選択により効果の大きなアリルは頻度が上がりづらいので, アリル頻度の低いbinほどβの平均値が大きくなりやすい.

さらに, 進化の過程で形質の最適値にシフトが生じた場合を考える. 例えば形質が増える/高まる方向にシフトが生じると, 頻度の高いSNPの平均効果サイズはプラスになり, Sβ(0,x)のプロットは単調なグラフでなくなり, Sβ(0,1)はプラスに転じる. シフトの時期が古くなればなるほど, TIAは浮動で固定したり消えたりするので, このシグナルは弱くなる.

また, mutational biasが存在せず中立の時, Sβの期待値は0になるが, その分散はcausal alleleとnon-causal alleleの間のLDに依存する. 表現型に全く影響を与えないアリルであっても, このLDによりeffect sizeを持つからである. これを考慮するため, ([ゲノムを1703個に分割した](https://academic.oup.com/bioinformatics/article/32/2/283/1743626)) 各LDブロックについて, そこに含まれる全てのSNPsの効果サイズの符号を (同じ確率で) そのまま or 逆転させることにより, LDによるアリル頻度の分布はそのままに, 効果サイズをランダムにしたSβの帰無分布を作成することができる.

この研究でも, GIANTのデータでは見られたいくつかの自然選択のシグナルがUKBだと見えないことを報告している. しかしBMIとEducational attainmentに関しては同様に観察された模様.

#### [Schoech et al. 2019 _Nat. Commun._](https://www.nature.com/articles/s41467-019-08424-6)

#### [Höllinger et al. 2019 _PLoS Genet._](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1008035)

#### [Chen et al. 2020 _AJHG_](https://www.cell.com/ajhg/pdfExtended/S0002-9297(20)30161-0)
`2_PRS_portability.md `で説明した Berg et al. 2019; Sohail et al. 2019等を受けて, 身長に対するpolygenic adaptationに疑義が生じている. 特にGIANTなどサンプル数が小さな研究で集団構造の補正が不十分であったため, 効果サイズにバイアスが生じていた可能性が指摘されている. 本研究では, ヨーロッパの中で身長の低いサルディーニャ集団に注目し, GIANT, UKBに加えて, Biobank Japan (BBJ) の東アジア集団のデータセットも使い, この疑問を再検証した. その結果, サルディーニャ集団は中立なSNPsセットで計算した時よりも有意に低いPGS (集団レベルの身長に関わるpolygenic score) を示し, またそのPGSはイギリス集団と10,000年前から分化し始めたことが分かった. PGSベースの手法ではヨーロッパ集団に身長に対する適応進化のシグナルは見られなかったが, ハプロタイプベースの手法 (tSDS) で確認された. 

3つの手法を使っている: 1) excess variance test, 2) polygenic height score trajectory, 3) tSDS analysis.

1. Qx testという手法 ([Berg & Coop 2014](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1004412)). 推定されたPGS (population-level) が集団間で遺伝的浮動から期待されるよりも大きな分散を示すかどうかを調べる. <br><br>
<img src="https://latex.codecogs.com/gif.latex?Q_x&space;=&space;\frac{\vec{Z'}^TF^{-1}\vec{Z'}}{2V_A}" title="Q_x = \frac{\vec{Z'}^TF^{-1}\vec{Z'}}{2V_A}" /><br><br>
ここで, <img src="https://latex.codecogs.com/gif.latex?\vec{Z}" title="\vec{Z}" /> は調べたい集団におけるgenetic value (population-level PGS)であり, <img src="https://latex.codecogs.com/gif.latex?F" title="F" /> は集団間のアリル頻度の相関行列になる. <img src="https://latex.codecogs.com/gif.latex?V_A" title="V_A" /> は祖先 (global) 集団全体の相加遺伝分散を示す. F行列を作るため, 身長と相関するSNPとMAF, [組換え率](http://ftp.1000genomes.ebi.ac.uk/vol1/ftp/technical/working/20130507_omni_recombination_rates/), background selection ([B値](http://www.phrap.org/othersoftware.html)) のマッチするランダムな20,000個のSNPを取ってきて計算している. <img src="https://latex.codecogs.com/gif.latex?Q_x" title="Q_x" /> 統計量は自由度 M-1 (Mは集団の数) のカイ二乗分布に従い, ここから漸近的にP値を推定する. 集団間で有意に分散が大きいとき, 異なる自然選択が集団間で生じていると期待される. <br>
さらに, どの集団が過分散に寄与しているか調べるため, Z szoreを計算する. M個の集団の中から1つを選択し, それを除いた残りの集団について計算したgenetic valuesから, genetic valueの平均と分散の期待値を計算する. それらを全集団に行い, 計算されたM個の平均と分散について共分散行列を作成する.

#### [Liu et al. _bioRxiv_](https://www.biorxiv.org/content/10.1101/357483v2)

#### [Holland et al. _bioRxiv_](https://www.biorxiv.org/content/10.1101/705285v8)

### 関連項目
- Blocked-jackknife法: tSDSとGWASのP値との相関を計算する際に使われたりするので, 以下ではその説明をするが, jackknife法自体は汎用的な考え方. ゲノム全体を (あるいはLDを考慮して; [この論文](https://academic.oup.com/bioinformatics/article/32/2/283/1743626)の[データ](https://bitbucket.org/nygcresearch/ldetect-data/src/master/)を使うこともある) 数百のブロックに分け, それぞれのブロックについて, それ以外の全てのブロックに含まれるSNPについて相関係数を計算する, という手順をブロックの数だけ繰り返す. ブロックの数をbとし, 上記i番目のブロックを除いた計算により求められるスピアマンの相関係数を<img src="https://latex.codecogs.com/gif.latex?\hat{\rho}^b_{(-i)}" title="\hat{\rho}^b_{(-i)}" />とすると, <br><br>
<img src="https://latex.codecogs.com/gif.latex?\bar{\rho^b}&space;=&space;\frac{1}{b}&space;\sum&space;_{i=1}^b&space;\hat{\rho}^b_{(-i)}" title="\bar{\rho^b} = \frac{1}{b} \sum _{i=1}^b \hat{\rho}^b_{(-i)}" /><br><br>
で平均の相関係数が求められ, さらに,<br><br>
<img src="https://latex.codecogs.com/gif.latex?\hat{\sigma}^2&space;=&space;\frac{b-1}{b}&space;\sum_{i=1}^b&space;(\hat{\rho}^b_{(-i)}&space;-&space;\bar{\rho^b})" title="\hat{\sigma}^2 = \frac{b-1}{b} \sum_{i=1}^b (\hat{\rho}^b_{(-i)} - \bar{\rho^b})" /><br><br>
からスピアマンの相関係数の点推定値の標準誤差が求められる. ここから, <img src="https://latex.codecogs.com/gif.latex?\hat{\rho}" title="\hat{\rho}" />は標準偏差<img src="https://latex.codecogs.com/gif.latex?\hat{\sigma}" title="\hat{\sigma}" />の正規分布に従う, すなわち<br><br>
<img src="https://latex.codecogs.com/gif.latex?\hat{\rho}&space;\sim&space;N(0,&space;\hat{\sigma})" title="\hat{\rho} \sim N(0, \hat{\sigma})" /><br><br>
を仮定して, <img src="https://latex.codecogs.com/gif.latex?H_0:\rho&space;=&space;0" title="\rho = 0" /> という帰無仮説を検定してP値を求める.
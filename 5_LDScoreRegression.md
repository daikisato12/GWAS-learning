### Linkage Disequilibrium


### LD score regression
あるpolygenicな基盤を持つ表現型を考えると, 各SNP (MAFを<img src="https://latex.codecogs.com/gif.latex?p" title="p" />とする)の効果サイズは, 分散が<img src="https://latex.codecogs.com/gif.latex?1/p(1-p)" title="1/p(1-p)" />に比例する分布から独立に得られると仮定できる. この時, あるSNP<img src="https://latex.codecogs.com/gif.latex?j" title="j" />について期待されるカイ二乗統計量は以下で表される.<br><br>

<img src="https://latex.codecogs.com/gif.latex?E[\chi^2&space;|&space;l_j]&space;=&space;Nh^2l_j&space;/&space;M&space;&plus;&space;Na&space;&plus;&space;1" title="E[\chi^2 | l_j] = Nh^2l_j / M + Na + 1" /><br>

ここで<img src="https://latex.codecogs.com/gif.latex?N" title="N" />はサンプルサイズ, <img src="https://latex.codecogs.com/gif.latex?M" title="M" />はSNPの総数, <img src="https://latex.codecogs.com/gif.latex?h^2" title="h^2" />は遺伝率なので, <img src="https://latex.codecogs.com/gif.latex?h^2/M" title="h^2/M" />はSNPあたりの平均遺伝率である. <img src="https://latex.codecogs.com/gif.latex?a" title="a" />はcryptic relatednessやpopulation stratificationといった交絡要因の強さの指標である. また, 注目しているSNPと他の全てのSNPとのLD(r^2)の和である<br><br>

<img src="https://latex.codecogs.com/gif.latex?l_j&space;=&space;\sum_k&space;r_{jk}^2" title="l_j = \sum_k r_{jk}^2" /><br>

がSNP<img src="https://latex.codecogs.com/gif.latex?j" title="j" />のLD scoreと定義される. 

#### [Bulik-Sullivan et al. 2015 _Nat. Genet._](https://www.nature.com/articles/ng.3211)
LD score regressionを開発した論文.

#### [Finucane et al. 2015 _Nat. Genet._](https://www.nature.com/articles/ng.3404)
Stratified LD score regressionを開発した論文.
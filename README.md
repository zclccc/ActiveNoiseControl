これは島村研究室内で情報共有をするための資料です．

# 能動騒音制御(Active Noise Control)

### 音を音で消すというロマン，能動騒音制御．


## 1. 概要

騒音は20世紀後半の交通車両や工場の増加と共に問題となってきた．
それに伴い，騒音をどう抑制するかが大きな課題となり，様々な騒音低減手法が考案されてきた．

最も簡単で古典的な騒音制御法として，静的騒音制御がある．
静的騒音制御は，吸音材や遮音材，反音材等を使って壁を形成し，物理的に騒音を遮断する方法である．
ただし静的騒音制御は空間的・物理的なコストが大きく，設置場所の制約が大きい（図１）．
加えて，この手法は消音効果が低く，最近では**能動騒音制御**による消音手法も併用されることが多い．

能動騒音制御とは，騒音に対して逆位相となる**制御音**を放射し，干渉させることで音響空間内で消音を達成する手法である（図２）．

能動騒音制御手法は機材構成や使用環境で制御手法が大きく異なる．
下に，現在実用化されている能動騒音制御を分類する．
狭義の能動騒音制御は３つ目のディジタル能動騒音制御 (適応フィルタ)を指す．
まずは **アナログ能動騒音制御** と **ディジタル能動騒音制御（固定フィルタ）** を説明する．

- アナログ能動騒音制御
- ディジタル能動騒音制御（固定フィルタ）
- ディジタル能動騒音制御（適応フィルタ）



### アナログ能動騒音制御

旧来のノイズキャンセリングヘッドホン・イヤホンでは，アナログ回路を用いた能動騒音制御が主流であった([Link1](http://blog.livedoor.jp/sce_info3-craft/archives/6786242.html), [Link2](https://www.google.co.jp/url?sa=t&rct=j&q=&esrc=s&source=web&cd=4&ved=2ahUKEwiK2P6a9MLhAhWMgrwKHRIMCfQQFjADegQIABAC&url=https%3A%2F%2Fsucra.repo.nii.ac.jp%2Findex.php%3Faction%3Dpages_view_main%26active_action%3Drepository_action_common_download%26item_id%3D16772%26item_no%3D1%26attribute_id%3D24%26file_no%3D1%26page_id%3D26%26block_id%3D52&usg=AOvVaw2xQgOBrE7McDo1Q15RQ1Ze) ).

アナログ能動騒音制御の構成は図３のとおりである．**参照マイクロホン** で取得した騒音（**一次音源**）から，逆位相となる **二次音源** を **制御スピーカ** から放射し，**制御点** で消音を達成する．
ただし，一次音源は制御点に達するまでの間に時間が経過し，変化している．参照マイクロホンから制御点までの経路を**一次経路**と呼び，制御スピーカから制御点までの経路を **二次経路** と呼ぶ．制御スピーカから放射する二次音源は，この一次経路と二次経路の特性も考慮して**コントローラ** を設計する必要がある．

ヘッドホンやイヤホンであれば，機器の形状が定まっているため，一次経路と二次経路も変化しない．
したがって，設計段階で一次経路と二次経路を測定してしまえば，コントローラでの制御も容易である．
このコントローラをアナログ回路で実現したものがアナログ能動騒音制御である．

### ディジタル能動騒音制御 (固定フィルタ)

アナログ回路はアナログ素子の特性上，制御できる周波数帯域が狭い（せいぜい数100Hz～数kHz）．
そこで最近では，広い周波数帯域も制御可能なディジタル回路を用いたディジタル能動騒音制御が主流となっている．([Link3](https://www.sony.jp/headphone/special/d-nc/), [Link4](https://www.sony.com.au/electronics/support/res/manuals/4145/41456810M.pdf))．

ディジタル能動騒音制御では，図４のようにアナログ回路が"AD変換"，"DSPコントローラ"，"DA変換"に置き換わる．
DSPコントローラ内では **ディジタルフィルタ** により **参照信号** を **制御信号** に変換する．

AD変換とDA変換にはある程度処理時間が必要なため，一次経路が短いと一次音源が制御点に到達するまでに二次音源の生成が間に合わず，消音できない場合もある．


## 能動騒音制御

ここでは狭義の能動騒音制御（適応フィルタによる騒音制御法）について説明する．  
能動騒音制御には，機材構成によりさらに２つの種類に分類される．

- フィードフォワード能動騒音制御
- フィードバック能動騒音制御

### 2. フィードフォワード能動騒音制御

ダクト音の騒音制御や，車内の騒音制御には，**フィードフォワード能動騒音制御** が用いられる．
図５にフィードフォワード能動騒音制御の構成を示す．制御点に **誤差マイクロホン** を設置し，
消音が達成しているかを確認する．

ダクトや車内は音響空間が変化するため，一次経路も時々刻々と変化する．
したがって，コントローラの処理も時々刻々と調整する必要がある．

コントローラ内のディジタルフィルタは，一般に **Filtered-X LMSアルゴリズム**[[link]()] により更新される．

## Filtered-X LMSアルゴリズム

コントローラ内のディジタルフィルタはFiltered-X LMSアルゴリズムにより時々刻々と更新される．  
このように逐次的に係数が更新されるフィルタのことを適応フィルタと呼ぶ．

フィルタ係数は，誤差マイクロホンで取得される **誤差信号** が0になるよう更新すればよい．
Filtered-X LMSアルゴリズムでは，**誤差信号の時間平均パワーを最小化する**ようフィルタ係数を更新する．
誤差信号の時間平均パワー *J* は以下の式で定義される．

J = E[ ]

ここで *x* は一次経路を通過した一次音源， *y* は二次経路を通過した二次音源を指す．
また *E[・]* は時間平均を意味する演算子であり，例えば以下の式で計算される．

E[x] <- a * E[x] + (1-a) * x




---
tags:
  - note
---
蛍光版の全体進捗
実験
EDTAを使った実験、テスト期間の関係で2週間後以降に実験

検出器
カメラの作動は確認できたがそこから先に進めてない
新しいラズパイを調達したのでそっちでも試す

ラマンの抽出液
蛍光色素の使い方の新規性

karura全体gitに


20241225サイエンスmtg内容
富山の実験で確認したいことは何か
　必要なデータ、検証したいこと
染色に必要な時間
　ThermoFisherのやつを参考に

取得後の明るさ判定
レーザー(daiko)Fukikoから引き継ぎ、デモ機を貸してもらう、そのあとの交渉(借りる機関の延長、もらえるかなど)
7日とかまでに追いメールどうなってるか
ラボ確保でき次第実験(計画書作成、試薬発注、実験等)

ラマンがなくなったので生体分子の議論がしづらい


21:00~　検出候補はあるのか、試薬候補、生死のみのばあいできる議論、その正当性(こっちメイン)
抽出液の保存について


検出器全体の感じ（明るい状況→暗い状況）

ラボでやってそうな感じ

試験管に並べた状況をカメラで撮った映像（反応）

検出器に乗せた状態の写真
写真を赤枠で囲む
明るさデータ
だからこれは光っているといえる

月曜mtgで
話したことのまとめ
hoechstかdapiか、他との組み合わせ(どんな議論ができるかも含めて)
生命の定義も含めて、自分たちのプランを決定、裏付けが必要
hoechstだけでは生細胞とはいえない→だから？のロジック

レーザー使用感報告→渉外

なんで水にピークが出るのか

TMA-DPHを用いた場合、先にPBS等をある程度いれておいてそこに試料を投入することで検出はできるのか

Sudaniiiのときどのような理由で取れなかったのか、どれくらいアセトニトリル濃度なら脂質抽出できたと考えられるか

TMA-DPHの扱い
　室温で保存(Thermofisher, https://www.thermofisher.com/order/catalog/product/jp/ja/T204)
　
TMA-DPHの作用機序(Sudan3と同じことにならないか)
　Sudan3の機序: 色素が脂質に溶解する
　TMA-DPHの機序: TMA基がリン脂質に静電気的に結合、DPH基で蛍光
　多分同じことはおきない
mattさんからtakuchanに 他のシステムと統合する旨の連絡あり、その管理を行う

砂漠に存在する微生物に細胞壁は存在するか(EDTAの意義について)
TMA-DPHの廃棄における扱い
TMA-DPHの溶媒について(実際にとれるのか)



PBSのところの精査(Yao)
mattさんとhayatoさん(Takuchan)

Fukiko: 広大のところの整理

SAR Docs
生命の3要素(遺伝子, 代謝, 外界との仕切り)
遺伝子, 外界との仕切り
遺伝子は地表・地下の環境DNAの分解速度とかもからめていきたい
細胞膜はMIDと併せて(できれば)



抽出液も考える

SAR Video
試薬とサンプルが入る様子から光る様子までの動画(見栄え)
カメラで見たときの様子(その辺の土で)
↑SAで撮ってる様子(できれば)

タスク整理
1. SAR
2. TMA-DPHの廃棄に関する扱い
3. matlab

matlab

晴天時
川底
RGB
・川底は反射により白っぽくなるケースが多い→全閾値を170以上に制限すると機械的に認識しやすくなる
HSV
・同上→Sは0-0.1, Vは0.7-1
YCbCr
・Y:175-, Cb:125- Cr:Free
1. matlabを使い、最適な動きをコードとして書き出し（画像入力したら結果が出力されるような形に)
2. 閾値の調整


イメージの領域分割
画像をアプリで開くところまでしか自動化できなさそう

大興確認メール

タスク整理
1. SAR Docs(2/4中)
2. TMA-DPHの挙動に関する検討(2/5mtgまで)
3. Matlab(2/9まで) 
4. 電通面接資料


TMA-DPHと硫酸塩
・JAXA研究チームの議論によりTMA-DPHによる細胞膜検出が硫酸塩環境下にて検討されていたため多分大丈夫
TMA-DPHとNTP
・TMA-DPHは静電相互作用により固定されるが、蛍光の機序的には対象に疎水部が必要である。一方NTPには疎水部が存在しないため、蛍光しない





% ワークスペースにエクスポートされたマスクの変数名を入力
maskVariableName = input('ワークスペースにエクスポートしたマスクの変数名を入力してください: ', 's');

% ワークスペースからマスクを取得
try
    mask = evalin('base', maskVariableName);
catch
    error('ワークスペースに指定された変数名のマスクが見つかりませんでした。');
end

% マスクが論理型（binary mask）であることを確認
if ~islogical(mask)
    error('エクスポートされたマスクは論理型（binary mask）である必要があります。');
end

% マスクを画像に適用
maskedImage = originalImage; % マスクを適用する画像の初期化 (必要に応じてコピー)

% 画像の型に合わせてマスクを適用
if isa(originalImage, 'uint8')
    maskedImage = uint8(double(originalImage) .* double(mask));
elseif isa(originalImage, 'uint16')
    maskedImage = uint16(double(originalImage) .* double(mask));
elseif isa(originalImage, 'double') || isa(originalImage, 'single')
    maskedImage = originalImage .* cast(mask, class(originalImage));
else
    error('サポートされていない画像型です。');
end


% 出力画像のパスを入力
outputname = input('マスクを適用した画像の出力パスを入力してください: ', 's');
outputPath = ([outputname,'.jpeg'])
% マスクを適用した画像をエクスポート
imwrite(maskedImage, outputPath);

disp('マスクを適用した画像をエクスポートしました。');


.Files



東大ラボに向けて、やりたい実験・内容まとめ、妥協できるライン(大腸菌使えないなど)
温湿度計実験に向けた準備（回路など、必要な部品と作業、土日)

回路系とか、ビーム径絞る目的以外

重ね合わせを先に
, 'ColorChannels', [0 0 1]
\v91


土壌中のeDNAは無期限に保存される可能性



1. 存在の有無の確認を重視し、わかりやすい蛍光にした
2.実際に蛍光している様子
3.検出器の構造説明
4.実際にカメラで撮った様子
5.ラボでの風景(雑多な説明はここ
6.試薬について（試薬、対象、選定理由など
7.上の情報の補足


extraction
negative effects がぼんやりしすぎ、ラボユースとの関連性
easy to use がざっくりすぎ
pHの調整のところを盛り込む

fluorescence reaction
蛍光の新しさを強調(呈色でない理由, 

DNAを使うとしたら、、、のところの根拠が欲しい(Yuniの送ってくれたやつとか)
DNAが見つかればextantなのか本当に考える

何を検出するのか
膜の重要性
なぜ膜を検出しようと考えたのか
(膜のExtantについて, 予備的に)
MIDとの違い(見た目、成分的, MID入れるならここまで書かないともったいない)

プロトコルの改良により反応速度を上げた感
検出器本体の形(ほぼ同じ励起、蛍光のものを使うことで省スペースで2つできる)
10分の話は消す


今回我々は全く新しい検出方法として、生命分子の検出に蛍光反応を導入した。従来の呈色反応の場合、サンプルに含まれるバイオマーカーが少量の場合、色の変化が小さく、バイオマーカーの存在を明確にすることが困難であった。一方蛍光反応では、バイオマーカーが少量であっても、センサーで光を検知することで、結果を明確にすることができる。また、この分析法は用いる蛍光試薬の励起波長・蛍光波長を近づけることで、一つの装置で複数のバイオマーカーを検出することができる。

生命が広く持つ要素として、遺伝物質と膜構造が挙げられる。現在生命の起源としてよく知られているのはRNAワールド仮説だが、RNAは死後の分解が早く、構造的に不安定であるため、現実的に検出することは困難である。また、他の種のポリ電解質を広範に検出することも技術的に困難である。一方、DNAは地球上の生物が遺伝情報として保持し、またこれを検出する方法も複数確立されている。加えて、地球-火星間で隕石を通じ生命が移動した可能性も否定できない。そのため、火星であっても地球型生命の痕跡を探すことは有意義である。


同様に地球上で細菌類が膜の材料として広く用いているリン脂質を検出することも有意義である。地球上の生物の多くは膜材料としてエステル脂質をベースとしたリン脂質を用いている。もちろん、古細菌のようにグリセロールとエーテル脂質を用いる生物も存在するが、先述の地球と火星の生物が共通の祖先をもつ可能性を考慮すると、リン脂質を材料とする細胞膜を持つ可能性は十分にあると考えられる。実際に、過去の火星探査では土壌からリン・リン酸塩が検出されており、この可能性を補強する。


富山実験のプロトコル(収量向上)
東大ラボに向けて、やりたい実験・内容まとめ、妥協できるライン(大腸菌使えないなど)

call C:\dev\ros2\ros2-windows\local_setup.bat
ros2 run demo_nodes_cpp talker

call C:\dev\ros2\ros2-windows\local_setup.bat
ros2 run demo_nodes_py listener

11行くらいで

ssh science@10.160.46.180

指定領域の合計値

20-27

and procure other chemicals


組織構造
動きが企業みたい
・お金とか、開発の構造とか
・イベントとか、開発以外もしてる
教育のことはそこまで重視しない
Keisukeの成長
技術的成長・マネジメント(重視)
次世代リーダー育成

・KARURAの差別化
組織的マネジメント
・独自の強み
分野横断型の活動(工学・理学の融合)
・リーダーシップ
技術
マネジメント力
・言及すべきこと
資金調達

国際開発を行うことができる即戦力の育成(学生のうちからやるメリット)
同世代との競争
早いうちからやることで、業界間との軋轢の解消の経験

漠然と大きなことを1つ言った後、具体的なこと

手動 160
river
crack


活動の中で、学生たちは具体的にどのようなスキルや知識を習得し、どのように成長すると考えていますか？
KARURA Projectの活動が、社会にどのような影響を与えると考えていますか？
KARURA Projectの活動を、将来的にどのように発展させていきたいと考えていますか？(活動規模の拡大などについて)
KARURA Projectの活動を通じて得られた経験や成果を、どのように社会に還元していく計画ですか？
助成金の使途について、具体的な内訳を教えてください。
KARURA Projectの活動を通じて、学生たちはどのようなキャリアパスを描けると考えていますか？
KARURA Projectの活動において、特に力を入れている点や、他の学生団体との差別化ポイントがあれば教えてください。(強み、独自性)
チームメンバー間のコミュニケーションを円滑に行うために、どのような工夫をしていますか？
卒業生は、KARURA Projectの活動で得た経験をどのように活かしていますか？
KARURA Projectの活動理念やビジョンについて、改めて教えてください。
KARURA Projectの活動において、今後の課題や展望があれば教えてください。


TMA-DPHで得られると予想していた430nmの蛍光が得られませんでした。

理由を考えてください。補足資料として、Gemini 2.0 Pro Experimental が考えた原因等と、実験に関する対話のログ、実験のプロトコルを添付します。



Gemini 2.0 Pro Experimental が考えた原因

-----------------------------------------

TMA-DPHの蛍光が検出できなかった理由として、チャットログの内容とTMA-DPHの性質を踏まえると、以下の要因が考えられます。



光源の問題:

不適切な励起波長: Teruさんが使用したHgランプは、365nm付近にピークを持つものの、可視光領域にも幅広い波長の光を含んでいます。TMA-DPHの励起波長は365nmであるため、Hgランプの他の波長の光が干渉し、目的の蛍光が観察しにくくなった可能性があります。特に、Teruさんが指摘しているように、Hgランプの光によってサンプルが青白く見えてしまっていることから、励起光が強すぎたり、他の波長の影響で本来の蛍光色が隠れてしまったりしている可能性が高いです。

レーザーの故障: 本来使用する予定だったレーザーが故障していたため、適切な励起光を照射できなかったことが根本的な原因です。

サンプル調製の問題:

細胞膜/脂質成分の不足: Teruさんが考察しているように、サンプル中にTMA-DPHが結合する細胞膜や脂質成分が十分に存在しなかった可能性があります。TMA-DPHは脂質二重層に結合して蛍光を発するため、ターゲットとなる脂質が少ないと蛍光が弱くなります。

DMSOの影響:凝固: 実験室の温度が低く、DMSOが凝固（ゲル状に変化）したタイミングがあったとTeruさんが述べています。DMSOはTMA-DPHを溶解させるために重要ですが、凝固によってTMA-DPHが均一に分散せず、局所的な濃度ムラが生じた可能性があります。

濃度: 最終濃度が10^-7 Mになるように設定したとのことですが、これがTMA-DPHの蛍光を検出するのに適切な濃度であったかどうかの確認が必要です。

EDTAの影響

　　　　・Teruさんが指摘しているように、細胞膜に作用するEDTAが、リン脂質に影響を与えている可能性を検討するべき。

インキュベーション条件:

温度と時間: 40℃、15分間のインキュベーションは、TMA-DPHが脂質二重層と相互作用するのに十分な条件であったかどうか、文献などで確認する必要があります。

その他の要因:

pHの影響: サンプルのpHがTMA-DPHの蛍光特性に影響を与えた可能性も考えられます。

共存物質の影響: 土壌との混合物を使用しているため、土壌中の成分がTMA-DPHの蛍光を阻害した可能性も否定できません。

対策案

Yuna Horieさんが提案しているように、まずは理想的な環境（例えば、レシチンなどの脂質を含むリポソームなど）でTMA-DPHの蛍光を確認することが重要です。

また、大腸菌を用いる場合でも、大腸菌の濃度を上げる、または、細胞膜をより多く含む画分をサンプルとして使用するなどの工夫が必要かもしれません。

そして最も重要なのは、適切な波長のレーザー光源を確保することです。

-----------------------------------------
対話ログ
-----------------------------------------
[金曜日 22:23] Teru: @Yuna Horie @Tom お疲れ様です。今日、担当のsarの動画を取りました。ただ、もらったレーザーが動かなく、意図した動画はとれていません。指定された電圧・電流に設定したところ、電流が流れず、レーザーとして機能していないことが分かりました（電気の先生と一緒にいろいろ試しましたが、レーザーが出てきませんでした）。代用として、とりあえず、Hgランプが入った光源で蛍光の様子を取ってみました。とりあえずスレッドに。

[金曜日 22:28] Teru: https://drive.google.com/drive/folders/17ed7_CpZJecTgLB4XFAAtWxn2RxHSoZl
[金曜日 22:30] Teru: ヘキストは緑色の蛍光、もう一つのやつは未反応でした。試料は大腸菌濃度10^-2まで希釈したもの（OD600=0.5）と土壌との混合物を使っているので、反応は起こるはずです。

[土曜日 01:26] Yuna Horie: お疲れ様です！！！まじかあレーザー使えないのとtma dph が見えないのかなりきついな
・もちろん入れてくれてると思うけど、DMSOはtma dphの方に入れてくれた？
・インキュベート時間はどれくらい取りましたか？
・細胞壁があると厳しいとかってあるのかな(細胞膜検出なので)@Tom 考えられる原因出してみてください！

[土曜日 08:02] Yuna Horie: @Teru ちなみにラボでの作業ショットは撮れましたか？

[土曜日 11:47] Yuna Horie: @Teru 何度もごめん〜！昨日の映像を名付けてもらえるととても助かる！

[土曜日 13:54] Teru: ああ、ラボ、、ほんとすいません、撮り忘れてます。ただ、去年のSARの時に頼まれてたラボの動画があって、結局去年は使わなかったはずなので、今年使うのもありだとおもいます。ただ、かっこよくない、、、笑 https://drive.google.com/drive/folders/1W7ypNC1PKULlLycLrq4niyB4BaVWKG9q
[土曜日 13:55] Teru: 名前付け了解です やっておきます ⇒完了 (edited)

[土曜日 13:58] Teru: TMA DPHが見えない理由はサンプル中にあんまり細胞膜とか、脂質成分がなかった、、説も考えられますが。DMSOに溶解して、最終濃度が10^-7 Mになるように設定してやってみました。ただ、DMSOは融点が25℃？くらいで、実験室が寒くて、放置していると少しゲル状になってしまったタイミングがありました。すぐい温水に入れて溶解しましたが、本番はあったかいので大丈夫だとは思いますが、、【追記】見えなかった理由はいろいろと考えられますが、まずは理想的な環境を再現して、どのような色がみられるのかを確かめたらいいかと思いました。細胞膜に作用するということで、EDTAの働きで細胞膜のリン脂質部分には影響がないのか、そもそも今の用途ではできそうかもう少し調べたらいけそうかなって思いました。 (edited)

[土曜日 13:59] Teru: インキュベートは約40℃、15分間です。水溶液内の温度はそれくらいに設定しました。

[土曜日 23:29] Yuna Horie: @Teru ありがとうー ちなみにblank2-4とかTMA-DPHがシアンっぽい色になってるのはなんだろう（ビーカーのみだとだいぶ青く見えるよねえ）

[日曜日 05:00] Yuna Horie: あと、ヘキストが緑になったのは未反応のヘキストのせいだと思います

[日曜日 06:33] Yuna Horie: 火曜に再度トライしてもらうことは可能ですか？
・ヘキストの量を減らすか検出対象の量を増やして、ほぼ反応するようにすることで緑ではなく青っぽい蛍光を得る
・大腸菌や土壌は一旦おいておいて、TMA-DPHの理想環境（対象レシチンとか？）で試してみる
それぞれ蛍光が取れているところは見せたくて,,,お願いできませんか

[日曜日 13:36] Teru: おそらく光源の色だと思います。365、540付近にピークを持つHgランプを使ってたので、青白いレーザに見えてます。ビーカーのみの時の色がレーザーの色そのものです。

[日曜日 13:39] Teru: こっちの雪がだいぶひどいのと、時間的に火曜日は厳しいです。青っぽい蛍光を得るためには、365 nm付近のみをだすレーザーが必要だけど、こっちに送ってもらったレーザーが機能していないので、それができそうにないのが現状です。大腸菌濃度とかを増やしてヘキストをやっても、光源はHgランプの可視光も含むものなので、同じような結果が得られると思います。それから、TMA-DPHの対象となる物質は持ち合わせていないので、そっちも厳しいと思います、、市販されているものを買ってやるのは無理でもないけど、そもそもの光源が可視光を含んでいるから、見たい蛍光は見えないと思います
-



TMA-DPH
DMSOの膜に対する影響があった可能性は小さい
どちらかといえばTMA-DPHに影響した可能性の方があるが、実際のところ影響するかは未知数

バイオマーカーの量が少ない場合

"C:\Users\daxia\Desktop\Temporary Files\KARURA\Geology\Reference_Yuna"
"C:\Users\daxia\Desktop\Temporary Files\
KARURA\Geology\batfunc_crack.m"


そもそも遺伝物質を検出することにした理由を入れる必要がある→そこからRNAでなくDNAを選んだ理由
(of life. While the RNA)
While からworthwhile を2行くらいに
Compared to からof fluorescenceを1文に、前半要らない

DNAをはじめとする遺伝物質は分解が早く、故に検出することができれば、生命の存在の有力な証拠となる。また、遺伝物質を保護することができる細胞膜のようなものが検出することができれば、生命の存在を示す補助となるだろう。よく検出対象とされるRNAは比較的不安定かつ検出が難しいため、今回我々は比較的安定的なDNAを検出対象とした。隕石を介した惑星間生命移動の可能性を考慮すると、地球上の生物が遺伝物質として用いるDNAを検出することは有意義である。

Genetic material, such as DNA, degrades quickly. Therefore, its detection would be strong evidence for the existence of life. Furthermore, the detection of structures like cell membranes, which can protect genetic material, would serve as supporting evidence for the existence of life. RNA, which is often the target of detection, is relatively unstable and difficult to detect. Therefore, we focused on detecting the comparatively more stable DNA. Considering the possibility of interplanetary transfer of life via meteorites, detecting DNA, which is used as genetic material by organisms on Earth, is significant. Similarly, detecting phospholipids would also be significant.

Genetic material and membrane structure are two widely held elements of life. While the RNA world hypothesis is a leading origin of life theory, RNA's instability[1] and difficulty in detection pose challenges. Given DNA's stability and detectability, plus the possibility of interplanetary life transfer via meteorites[2], searching for DNA on Mars is worthwhile. Similarly, it is also meaningful to detect phospholipids, which are widely used as membrane materials by bacteria on Earth.

Genetic material, such as DNA, degrades quickly. Therefore, if it can be detected, it serves as a strong indicator of the presence of life. Furthermore, if cell membrane-like structures that protect genetic material can be detected, this would provide supporting evidence for the presence of life. RNA, which is often the target of detection, is relatively unstable and difficult to detect. Therefore, we focused on detecting DNA, which is comparatively more stable. Considering the possibility of interplanetary transfer of life via meteorites, it is significant to detect DNA, which is used as genetic material by life on Earth. Similarly, detecting phospholipids would also be significant.


この方法はaの理由により反応を鋭敏に検出する

惑星間生命移動を考慮すると、地球上の生物が膜の材料としても用いるリン脂質も有効な検出対象となる。
検出可能性

Freelance Data Analyst (Bioinformatics),  Professional in genetic biology and plant physiology

地球上の生物はリン脂質を膜材料とし、これは生物を外的要因から守る。膜材料となりうる物質はリン脂質のほかにもあるが、火星にリンが大量に存在することを考えると、火星の生物がリン脂質を膜材料に用いる可能性は高い。検出可能性を考慮すると、リン脂質を検出対象とすることは合理的である。

Lives on earth use phospholipids as the ingridients of their membrain, and it protect themselves from factors outside. Consider about huge amount of phosphate on mars, the possibility of life on mars use phospholipids as the ingridients of their membrain. Also from the side of detection possibility, select the phospholipids 

DNAの安定性と火星でDNAが形成された可能性を考慮すると
加えて、火星で核酸塩基からDNAが合成された可能性や、隕石による惑星間生命移動の可能性は火星でのDNA探査を理由づける。

5803

1点お聞きしたいです。
富山で大腸菌を用いていましたが、あれはKARURAで標本を買って培養したものですか? 
それとも高専に使用させてもらっているものなのでしょうか?
今回は使いませんが、場合によっては今後実験を行う中で大腸菌を用いることもあるかと思うので、その辺どうなのかちょっと知りたいです。

タスク整理

・磁場を計測
地球は地磁気由来で強い、月は残留地場由来(そのもの)で弱い
・表面の凸凹を微隕石由来であるといえる?

密度: サイズの推定方法、密度から何が言えるか(平均密度と比較など
体積：液体につけて水位差をはかる
密度：アルキメデスの原理

やること(3/11)
TRCの準備(岩石サイズ算出の計算式, 楕円近似する?, 密度と月の内部進化の表, 3/14まで)

実験とか、他の部分と統合してのテストも行う必要がある
蛍光mtg前にひな形を作成しておく

なんや知らんが、加入できんのじゃが、どうしてくれるんじゃ

今日のタスク
1. 所用試薬量の計算
2. 今後のスケジュール


事前にやるべきこと
・東京実験(なるはや, 4/6~)
各種試薬と標的物質の対応グラフ探し
退光の影響調査
廃液処理の手順

・組み込み実験(4/12まで)
遮光環境とローバー内の差分
ー遮光環境は東京実験
画像のみと数値のみの場合のデータ量比較
ーすぐできる(Mattさんあたりに確認)
赤の迷光(反射)対策
ーリサーチメイン
遮光するものの設計
ー実物を触ってみないとわからない、SAとの干渉も考慮


東京実験でやること
1st. 各種試薬の蛍光確認: 最優先
2nd. レーザーの動作確認: Mattさんにお願いしたい、データは大興に問い合わせ中
3rd: 遮光状態の確認(遮光するものができ次第)
4th: 各種試薬の収量向上

早稲田 Hanako 新しく入ってくれる Slackで堀江さんと3人のグループ 剛性生物学 6, 7月が忙しい


カメラ映像を一定間隔でスクショして特定のフォルダに入れる→解析
下向きのウェブカメラ：止まった時だけスクショする形→コマンド入力で処理開始(ワンフレームだけ)
dji o3: 常時、数秒おき、パソコンに飛ばせるか確認
#340にサイエンスとしての補足(Mattさんのスレッド)
特に13の実験で使いたい
ウェブカメから静止画を抜き出す形がいい（無理ならスクショ)

早めにカーテンの素材を確認(遅くとも2, 3日前)

@Hanako Koseki 
初めまして、蛍光検出器サブチームのTomです。
これからよろしくお願いします:bow:

早速ですが、HanakoさんとYunaさんを交えて、Zoomで一度お話しする機会をいただければと思います。
蛍光班でやりたいことであったり、Hanakoさんの専門など、いろいろ伺いたいです。
お手数ですが、以下のリンクから、参加可能な日程の入力をお願いします。
もし参加不可であれば、ご連絡ください。
http://tonton.amaneku.com/list.php?id=20250406054551_dQDk60

大興に修理ないし検査をお願いするにあたり、レーザーの動作確認について、以下2点を確認したいです。
・納入仕様書の範囲内にて電圧をかけて動かなかったか
・用いた安定化電源は定電圧でなかったか

水: 17:00-18:30以外
木: 14:00-

スライド下2文のやつの内容確認


リターンの文面を急ぎ目で

Science Plan
実験結果から考えられる閾値についてなど
Background Research

来週の日曜までに第1稿

この先生に頼んで実験協力してもらったなどの話は多分ok

Hoechst 

4/21
1. Science Docs
2. ROS
3. Chem Safety

今回の大会では、DNA検出にHoechst 33342、リン脂質検出にTMA‑DPHを用いることにした。Hoechst 33342は生体膜や核膜を透過して生細胞・死細胞双方の核を迅速に染色でき、TMA‑DPHは室温でわずか数分のインキュベーションのみでリン脂質特有の蛍光を立ち上がらせるため、限られた競技時間内において迅速かつ安定した検出を両立できる。他の試薬だと染色時間や透過性、あるいは使用時のpH条件が試験管や液体輸送システムと合わず安定性を損ないやすいが、これらはpH 7–8の範囲で安定し、PPやPLA製のチューブにも試薬成分が付着しにくく、繰り返しの試行にも影響を及ぼさない。装置の光学系は365 nm帯のUV‑LEDをレンズフィルターで365nm付近の光を集光し、波長選択性の高いダイクロイックミラーで直角に反射させつつサンプルへ照射し、そのまま同ミラーを透過した蛍光をもう一つのレンズフィルターで460nm付近に絞り込んでRaspberry Pi Camera V2で撮像する構成になっている。得られた画像はローバー内のRaspberry Pi 4Bで解析され、あらかじめ設定した領域ごとのピクセルカウントをリアルタイムに算出することで、迷光の影響を抑制しつつ定量性を確保している。ボックス型・シート型の遮光ユニットで周囲を覆うことで太陽光や他の機器の光に影響されず、正確な実験結果を取得することができる。


土壌サンプル: 20, 40

東京大学
島先生

いつも大変お世話になっております。
KARURAプロジェクトの大下でございます。

この度はご多忙の中、実験室をご利用させていただく機会を賜り、誠にありがとうございました。
おかげさまで、当初予定しておりました実験のうち、蛍光分光光度計を用いた実験はおおむね完了いたしました。

一方で、弊団体が大会にて使用を予定していたレーザー光源に不具合が生じたため、レーザーおよびコアユニットを用いた実験を実施することができておりません。

つきましては、誠に恐縮ではございますが、引き続き実験室を利用させていただきたくお願い申し上げます。
具体的には、5月19日の週まで、週2日程度の頻度で利用させていただければと考えております（4月と同様のペースを想定しております）。

お忙しいところ誠に恐れ入りますが、何卒ご検討いただけますと幸いに存じます。
引き続きどうぞよろしくお願い申し上げます。


KARURAプロジェクト
大下

全体チャンネルに、みんな個人が受け持ってる企業や個人宛(名刺交換して互いに認知している人とか)にクラファンお願いメールする旨リマインド(4/30朝予約投稿したい)
メールテンプレはKurenaさんがやってくれる

レーザーは現地に取りに行く(本番使う回路なども持っていく)


Could you send me the product list of the reagents?

DNAなど、生命のいた証拠となりうる物質があったときに光る蛍光色素を利用し、生命探査を行います。
自作の蛍光検出器を用い、実験を重ね最適な反応条件を探っています。

These enable both rapid and stable detection within the limited competition time. 

抽出液中のEDTA濃度はとりあえず1mMにした、そこからの最適化は行っていない


DNA検出はextantかextinctな生き物の痕跡の検出を目的に行っている。我々は生物の構成要素の一つとして遺伝物質を挙げている。現在メジャーなのはRNAワールド仮説だが、我々はDNAを検出することにした。DNAは構造的にRNAより安定であり、個体の死滅後も長期間残り続ける。一方でRNAは固体の死滅後早いものだと30分ほどで分解する。検出可能性が高いことが理由である。DNA検出のため、我々はHoechst33342を用いる。これはDNAのAT配列副溝と特異的に結合し、355nm付近の励起光を照射することで、479nm付近の蛍光を発する。

Hoechst 33342 のスタンダードプロトコルは37Cでの30分のインキュベーションだ。しかし、今回の大会だと時間が足りないため、短い時間で効率よく反応を進める必要がある。抽出溶液並びに染色液を55℃まで加熱することで、5分のインキュベーションでも十分な実験結果を得ることに成功した。また、蛍光をカメラで撮影し、反応が起こる領域のピクセルカウントを計測することで、蛍光強度を定量的に解析することができる。（検出限界について書く）

核酸塩基は隕石などからも検出されており、非生物的合成法も見つかっていないため、DNAの存在のみで判断できるものではないものの、生命の存在の有力な証拠となる。他の化学種の分析と併せて、土壌に生命が存在するかどうかを判断する。

----------

細胞膜検出はextantな生命の痕跡の検出を目的に行っている。我々は生物の構成要素の一つとして外界との仕切りを挙げている。今回はリン脂質を検出する。膜構造を取りうる物質は様々あるが、リン脂質はは既知の生物の細胞膜や核膜を構成し、外界との仕切りの役割を果たす物質であり、また検出も他の物質と比べ容易であることから、今回はリン脂質を選定した。リン脂質検出のため、我々はTMA-DPHを用いる。これはTMA-DPH分子のTMA基が細胞膜の疎水性部位に埋没し、355nm付近の励起光を照射することで、479nm付近の蛍光を発する。

TMA-DPHのスタンダードプロトコルは室温での3分のインキュベーションだ。しかし、この条件で得られる蛍光は、今回用いる蛍光検出器で検出するには弱いため、蛍光強度を強める必要がある。抽出溶液並びに染色液を55℃まで加熱し5分間インキュベーションすることで、十分な蛍光強度を得ることに成功した。また、蛍光をカメラで撮影し、反応が起こる領域のピクセルカウントを計測することで、蛍光強度を定量的に解析することができる。（検出限界について書く）

リン脂質単体では生命の決定的な痕跡とは言えないものの、DNA等の情報と併せて、土壌に生命が存在するかどうかを検討する。

----------

検出対象(DNA, リン脂質)の抽出を行うための抽出液として、当初スタンダードプロトコルと同様にPBSを用いる予定だったが、蛍光強度を計測したところ適切な蛍光強度が得られなかった。この原因を細胞壁が染色液の細胞壁への到達を妨げているためであると考え、キレート剤であるEDTAを1mMの濃度になるよう添加し再度実験したところ、良好な蛍光強度が得られた。また濃度を変化させ実験したところ、10mMの濃度でEDTAを添加した場合が最も良好な蛍光が得られた。これはEDTAを飽和量まで添加したためであると考えららえる。

----
この液体は採取した土壌サンプルを溶解させキュベットまで輸送する。EDTAを10mMの濃度で溶解させた抽出液を50mlのシリンジ2本に分けローバーに搭載する。具体的には、100mlのPBSに37.2mg EDTA・Na2を溶解させる。


Using actual soil samples, we varied the incubation time, temperature, and sample concentration of the test solution, and measured fluorescence spectra under each condition. Incubation times of 5 and 15 minutes, and temperatures of 30, 35, 40, 45, 50, 55, and 60°C were tested. Based on the resulting fluorescence intensities and practical considerations, the optimal condition was determined to be incubation at 55°C for 5 minutes after the addition of the test solution.

時間条件
5, 15
サンプル量
20, 40, 60, 80
温度条件
30, 35, 40, 45, 50, 55, 60

天然の土壌サンプルを用いて、インキュベーション時間、温度、試験溶液のサンプル量、抽出液中EDTA濃度を変化させ、それぞれの条件下で蛍光スペクトルを測定した。インキュベーション時間は5分、15分、温度は30℃、35℃、40℃、45℃、50℃、55℃、60℃、サンプル量は抽出液・染色液合計1ml中20mg, 40mg, 60mg, 80mg、抽出液中EDTA濃度は0.1mM, 1mM, 5mM, 7mM, 10mM, 25mM, 50mM, 100mMでテストした。その結果、実際のミッション時間を考慮し、EDTA濃度10mMの濃度の抽出液を用い、抽出液・染色液合計時に40mg/ml の濃度となるサンプルに対し、試験液添加後55℃で5分間インキュベートすることで、短時間で強力な蛍光強度を得ることができる、最適条件であると決定された。

Fluorescence spectra were measured under different conditions using natural soil samples, varying incubation time, temperature, sample volume of test solution, and EDTA concentration in the extract solution. Incubation times of 5 and 15 minutes, temperatures of 30°C, 35°C, 40°C, 45°C, 50°C, 55°C, and 60°C, sample amounts of 20 mg, 40 mg, 60 mg, and 80 mg in 1 ml of total extract and stain solution, and EDTA concentrations in the extract solution of 0.1 mM, 1 mM, 5 mM, 7 mM, 10 mM, 25 mM, 50 mM, 100mM. Considering the actual mission time, it was determined that the optimal conditions for obtaining strong fluorescence intensity in a short time were obtained by incubating the samples at 55°C for 5 minutes after adding the test solution, using an EDTA concentration of 10 mM and a total concentration of 40 mg/ml of the extract and staining solution. The optimal condition was determined to be a short incubation at 55°C for 5 minutes after the addition of the test solution.



Hoechst 33342と同様の実験を、同じ場所で採取された土壌サンプルを用いて行ったところ、同じ最適条件が確認された： 55℃で5分間, 試料40mg/ml, 抽出液中EDTA濃度10mM。

A similar experiment to Hoechst 33342 was performed with soil samples collected at the same location, confirming the same optimal conditions: 55°C for 5 min, 40 mg/ml sample, 10 mM EDTA concentration in the extractant.

加えて東京大学でEDTAの濃度を変化させ実験を行い、10mM EDTA PBS Solution が抽出液として最適であると結論付けた。
In addition, experiments were conducted at the University of Tokyo with varying concentrations of EDTA, and it was concluded that 10 mM EDTA PBS Solution was the best extraction solution.
    julius

JULIUS(1)                                                            JULIUS(1)



名前
           julius
          - 大語彙連続音声認識エンジン

概要
       julius [-C jconffile] [options...]

内容
       Julius は数万語を対象とした大語彙連続音声認識を行うことのできるフリー
       の認識エンジンです．単語N-gramを用いた2パス構成の段階的探索により高精
       度な認識を行うことができます．また，小規模語彙のための文法ベースの認識
       や単単語認識も行うことができます．認識対象としてマイク入力，録音済みの
       音声波形ファイル，特徴抽出したパラメータファイルなどに対応しています．

       コアの認識処理は，全て JuliusLib ライブラリとして提供されています．
       JuliusはJuliusLibを用いる音声アプリケーションの一つです．

       Julius を用いて音声認識を行うには，音響モデル，単語辞書，および言語モ
       デルが必要です．

設定
       Julius および JuliusLib コアエンジンの設定（動作選択，設定，モデル指
       定， パラメータ変更など）は，すべてここで説明する「オプション」で指定す
       る． Julius に対しては，これらのオプションをコマンドライン引数として直
       接指 定するか，あるいはテキストファイル内に記述したものを "-C" につづけ
       て指定する．このオプションを記述したテキストファイルは "jconf 設定ファ
       イル" と呼ばれる．

       JuliusLib を用いる他のアプリケーションにおいても，JuliusLib内の認識 エ
       ンジンへの動作指定は，同様にこのオプション群で行う．jconf 設定ファイル
       に設定内容を記述して，それをメイン関数の最初で
       j_config_load_file_new(char *jconffile); で呼び出 すことで，JuliusLib
       内の認識エンジンに設定をセットすることができる．

       なお，jconf 設定ファイル内では，相対ファイルパスはその jconf ファイル
       の位置からの相対パスとして解釈される（カレントディレクトリではない）．
       注意されたい．

       以下に各オプションを解説する．

   Julius アプリケーションオプション
       JuliusLib とは独立した，アプリケーションとしての Julius の機能に関する
       オプションである．認識結果の出力，文字コード変換，ログの設定，モジュー
       ルモードなどを含む．これらのオプションは，JuliusLib を組み込んだ他のア
       プリケーションでは使用できないので注意すること．

        -outfile
           認識結果を個別のファイルに出力する．入力ファイルごとの認識結果を，
           その拡張子を ".out" に変えたファイルに保存する． (rev. 4.0)

        -separatescore
           認識結果で言語スコアと音響スコアを個別に出力する．指定しない場
           合，和の値が認識結果のスコアとして出力される．

        -callbackdebug
           コールバックがJuliusLibから呼ばれたときにコールバック名を 標準出力
           に出力する．デバッグ用である．(rev.4.0)

        -charconv  from to
           出力で文字コードの変換を行う．from は言語モデルの文字セットを，to
           は出 力の文字セットを指定する．文字セットは，Linux では iconv で用
           いられるコード名である．Windows では，コードページ番号あるいはいか
           に示すコード名のどれかである： ansi, mac, oem, utf-7, utf-8, sjis,
           euc．

        -nocharconv
           文字コード変換を行わない．-charconv の指定を リセットする．

        -module  [port]
           Julius を「サーバモジュールモード」で起動する．TCP/IP 経由でク ライ
           アンとやりとりし，処理の制御や認識結果・イベントの通知が行 え
           る．port はポート番号であり，省略時は 10500 が用いられる．

        -record  dir
           区間検出された入力音声をファイルに逐次保存する． dirは保存するディ
           レクトリを指定する． ファイル名は，それぞれの処理終了時のシステム時
           間から YYYY.MMDD.HHMMSS.wavの形で保存される．ファ イル形式は16bit,
           1チャンネルのWAV形式である． なお，入力がGMM等によって棄却された場
           合も記録される．

        -logfile  file
           通常 Julius は全てのログ出力を標準出力に出力する． このオプションを
           指定することで，それらの出力を指定ファイルに 切替えることができ
           る．(Rev.4.0)

        -nolog
           ログ出力を禁止する．標準出力には何も出力されなくなる． (Rev.4.0)

        -help
           エンジン設定，オプション一覧などのヘルプを出力して終了する．

   全体オプション
       全体オプションは，モデルや探索以外のオプションであり， 音声入力・音検
       出・GMM・プラグイン・その他の設定を含む． 全体オプションは， 音響モデ
       ル(-AM)・言語モデル(-LM)・デ コーダ(-SR)などのセクション定義の前に定義
       するか， -GLOBAL のあとに指定する．

       オーディオ入力
            -input
           {mic|rawfile|mfcfile|outprob|adinnet|vecnet|stdin|netaudio|esd|alsa|oss}
               音声入力ソースを選択する．音声波形ファイルの場合は fileあるい
               はrawfileを指定 する．HTK 形式の 特徴量ファイルを認識する場合
               はhtkparamあるい はmfcfile を指定する．HTKパラメータファイル を
               出力確率ベクトルとして読み込む場合は outprobを指定する．起動後
               にプロンプトが表れ るので，それに対してファイル名を入力する．
               adinnet では， adintool などのクライアントプロセス から音声デー
               タをネットワーク経由で受け取ることができる． vecnetでは，HTK特
               徴量/出力確率ベクトルをネッ トワーク経由で受け取ることができ
               る． netaudio はDatLinkのサーバから， stdinは標準入力からの音声
               入力を認識する． esdは，音声デバイスの共有手段として多くの
               Linuxのデスクトップ環境で利用されている EsounD daemon からの入
               力を認識する．

            -filelist  filename
               (-input rawfile|mfcfile|outprob 時) filename内に列挙されている
               全てのファ イルについて認識を順次行う． filenameには認識する入
               力ファイル名 を1行に1つずつ記述する．

            -notypecheck
               入力の特徴量ベクトルの型チェックを無効にする．通常 Julius は入
               力の型が音響モデルとマッチするかどうかをチェックし，マッチしな
               い とエラー終了する．このオプションはそのチェックを回避する．な
               んらかの 理由で型チェックがうまく動作しないときに使用する．

            -48
               48kHzで入力を行い，16kHzにダウンサンプリングしながら認識する．
               これは 16kHz のモデルを使用しているときのみ有効である． ダウン
               ダンプリングの内部機能は sptk から 移植された． (Rev. 4.0)

            -NA  devicename
               DatLink サーバのデバイス名 (-input netaudio).

            -adport  port_number

               -input adinnet 使用時，接続を受け付ける adinnet のボート番号を
               指定する．(default: 5530)

            -nostrip
               音声取り込み時，デバイスやファイルによっては，音声波形中に振幅
               が "0" となるフレームが存在することがある．Julius は通常，音声
               入力に含まれるそのようなフレームを除去する．この零サンプル除去
               が うまく動かない場合，このオプションを指定することで自動消去を
               無効化することができる．

            -zmean ,  -nozmean
               入力音声ストリームに対して直流成分除去を行う．全ての音声処理の
               の前段として処理される． -zmeansourceオプションも見よ．

            -lvscale  factor
               音声入力の振幅の事後スケーリング

            -chunk_size
               音声入力のバッファ長をサンプル数で指定可能（デフォル
               ト：1000）．この値を小さくすると遅延を小さくできるが，小さすぎ
               ると不安定になる．

       レベルと零交差による入力検知
            -cutsilence ,  -nocutsilence
               レベルと零交差による入力検知を行うかどうかを指定する．デフォル
               トは，リアルタイム認識（デバイス直接入力およびネットワーク入
               力） では on, ファイル入力では off である．このオプションを指定
               する ことで，例えば長時間録音された音声ファイルに対して音声区間
               検出 を行いながら認識を行うこともできる．

            -lv  thres
               振幅レベルのしきい値．値は 0 から 32767 の範囲で指定する．
               (default: 2000)

            -zc  thres
               零交差数のしきい値．値は１秒あたりの交差数で指定する．
               (default: 60)

            -headmargin  msec
               音声区間開始部のマージン．単位はミリ秒． (default: 300)

            -tailmargin  msec
               音声区間終了部のマージン．単位はミリ秒． (default: 400)

       入力棄却
           入力長，あるいは平均パワーによる入力の事後棄却が行える． 平均パワー
           による棄却は，デフォルトでは無効化されており，ソースからコンパイ ル
           する際に configureに --enable-power-reject を指定することで有効とな
           る． リアルタイム認識時で，かつ特徴量でパワー項を持つ場合のみ使用で
           きる．

            -rejectshort  msec
               検出された区間長がmsec以下の入力を 棄却する．その区間の認識は中
               断・破棄される．

            -rejectlong  msec
               検出された区間長がmsecより長い入力を 棄却する．その区間の認識は
               中断・破棄される．

            -powerthres  thres
               切り出し区間の平均パワーのしきい値．(Rev.4.0)

               このオプションはコンパイル時に --enable-power-rejectが指定され
               たときに 有効となる．

       GMM / GMM-VAD
            -gmm  hmmdefs_file
               GMM定義ファイル．3状態（出力状態が１つのみ）のHMMとして定義す
               る．形式はHTK形式で与える．形式や使用できる特徴量の制限は音響
               モデルと同じである． なお，GMMで用いるMFCC特徴量の設定は，
               -AM_GMMのあとに音響モデルと同様に指定する．こ の特徴量設定は音
               響モデルと別に，明示的に指定する必要があること に注意が必要であ
               る．

            -gmmnum  number
               GMM指定時，計算するガウス分布数を指定する．フレームごとにGMMの
               出力確率を求める際，各モデルで定義されている混合ガウス分布のう
               ち，このnumberで指定した数の上位ガ ウス分布の確率のみを計算す
               る．小さな値を指定するほどGMMの計算 量を削減できるが，計算精度
               が悪くなる．(default: 10)

            -gmmreject  string
               GMMで定義されているモデル名のうち，非音声として棄却すべきモデ
               ルの名称を指定する．モデル名を複数指定することができる．複数指
               定する場合は，空白を入れずコンマで区切って一つの stringとして指
               定する．

            -gmmmargin  frames
               (GMM_VAD) GMM VAD による区間検出の開始部マージン．単位はフレー
               ム数で指定する．(default: 20) (Rev. 4.0)

               このオプションは--enable-gmm-vad付きでコンパイル されたときに有
               効となる．

            -gmmup  value
               (GMM_VAD) 音声区間の開始とみなす VAD スコアの閾値．VADスコアは
               (音声GMMの最大尤度 - 非音声HMMの最大尤度) で表される．
               (Default: 0.7) (Rev.4.1)

               このオプションは--enable-gmm-vad付きでコンパイル されたときに有
               効となる．

            -gmmdown  value
               (GMM_VAD) 音声区間の終了とみなす VAD スコアの閾値．VADスコアは
               (音声GMMの最大尤度 - 非音声HMMの最大尤度) で表される．
               (Default: -0.2) (Rev.4.1)

               このオプションは--enable-gmm-vad付きでコンパイル されたときに有
               効となる．

       デコーディングオプション
           デコーディングオプションは，使用する認識アルゴリズムに関する設定を
           行う オプションである．この設定はエンジン全体に対する設定であり，全
           ての認識 処理インスタンスで共通の設定となる．探索の幅や言語重みなど
           の個々のデコー ディング設定については，認識処理インスタンスごとに指
           定する．

            -realtime ,  -norealtime
               入力と並行してリアルタイム認識を行うかどうかを明示的に指定す
               る． デフォルトの設定は入力デバイスに依存し，マイクロフォン等の
               デバ イス直接認識，ネットワーク入力，および DatLink/NetAudio 入
               力の 場合は ON, ファイル入力や特徴量入力についてはOFFとなってい
               る．

       その他
            -C  jconffile
               jconf設定ファイルを読み込む．ファイルの内容がこの場所に展開され
               る．

            -version
               バージョン情報を標準エラー出力に出力して終了する．

            -setting
               エンジン設定情報を標準エラー出力に出力して終了する．

            -quiet
               出力を抑制する．認識結果は単語列のみが出力される．

            -debug
               (デバッグ用) モデルの詳細や探索過程の記録など，様々な デバッグ
               情報をログに出力する．

            -check  {wchmm|trellis|triphone}
               デバッグ用のチェックモードに入る．

            -plugindir  dirlist
               プラグインを読み込むディレクトリを指定する．複数の場合は コロン
               で区切って並べて指定する．

            -outprobout  file
               計算された出力確率行列をHTK形式ファイルに保存（debug）．

   複数モデル認識のためのインスタンス宣言
        -AM  name
           音響モデルインスタンスを新たに宣言する．以降の音響モデルに関す る設
           定はこのインスタンスに対するものと解釈される． name にはインスタン
           スにつける名前を 指定する（既にある音響モデルインスタンスと同じ名前
           であってはい けない）. (Rev.4.0)

        -LM  name
           言語モデルインスタンスを新たに宣言する．以降の言語モデルに関す る設
           定はこのインスタンスに対するものと解釈される． name にはインスタン
           スにつける名前を 指定する（既にある言語モデルインスタンスと同じ名前
           であってはい けない）. (Rev.4.0)

        -SR  name am_name lm_name
           認識処理インスタンスを新たに宣言する．以降の認識処理や探索に関 する
           設定はこのインスタンスに対するものと解釈される． name にはインスタ
           ンスにつける名前を 指定する（既にある認識処理インスタンスと同じ名前
           であってはいけ ない）．am_name, lm_name にはそれぞれこのインスタン
           スが使用する音響モデルと言語モデルのインスタンスを名前，あるい は
           ID 番号で指定する．(Rev.4.0)

        -AM_GMM
           GMM使用時に，GMM計算のための特徴量抽出パラメータを，この宣言の あと
           に指定する．もし GMM 使用時にこのオプションでGMM用の特徴量 パラメー
           タを指定しなかった場合，最後に指定した音響モデル用の特 徴量がそのま
           ま用いられる． (Rev.4.0)

        -GLOBAL
           全体オプション用のセクションを開始する．-AM, -LM, -SR などのインス
           タンス 宣言を用いる場合，音声入力設定などの全体オプションは，これら
           の 全てのインスタンス定義よりも前か，あるいはこのオプションの あと
           に指定する必要がある．この全体オプション用のセクションは， jconf 内
           で何回現れても良い． (Rev.4.1)

        -nosectioncheck ,  -sectioncheck
           複数インスタンスを用いる jconf において，オプションの位置チェッ ク
           の有効・無効を指定する．有効である場合，ある種類のインスタン スの宣
           言がされたあとは，他のインスタンス宣言が現れるまで，その インスタン
           スのオプションしか指定できない（例： -AM のあと，他の -AMや -LMなど
           が現れるまで，音響モデルオプションしか 指定できない．他のオプション
           があらわれた場合はエラーとなる）． また，全体オプションは全てのモデ
           ルインスタンスの前に指定する必 要がある．デフォルトでは有効になって
           いる．(Rev.4.1)

   言語モデル (-LM)
       このグループには，各モデルタイプごとに指定するオプションが含まれてい
       る． 一つのインスタンスには一つのモデルタイプだけが指定可能である．

       N-gram
            -d  bingram_file
               使用するN-gramをバイナリファイル形式で指定する． バイナリ形式へ
               の変換は mkbingram を 使用する．

            -nlr  arpa_ngram_file
               前向き (left-to-right) のN-gram 言語モデルを指定する．
               arpa_ngram_file はARPA標準形式のファ イルである必要がある．

            -nrl  arpa_ngram_file
               後ろ向き (right-to-left) のN-gram 言語モデルを指定する．
               arpa_ngram_file はARPA標準形式のファ イルである必要がある．

            -v  dict_file
               N-gram，または文法用の単語辞書ファイルを指定する．

            -silhead  word_string  -siltail  word_string
               音声入力両端の無音区間に相当する「無音単語」エントリを指定す
               る． 単語の読み（N-gramエントリ名），あるいは"#"+単語番号（辞書
               ファ イルの行番号-1）で指定する．デフォルトはそれぞれ "<s>",
               "</s>" である．

            -mapunk  word_string
               unknown に対応する単語名を指定する．デフォルトは， "<unk>" ある
               いは "<UNK>" である．この単語は， 認識辞書において N-gram にな
               い単語を指定した場合にマッピングされ る単語である．

            -iwspword
               ポーズに対応する無音単語を辞書に追加する．追加される単語の内容
               は オプション-iwspentryで変更できる．

            -iwspentry  word_entry_string

               -iwspword指定時に追加される単語エントリの内容 を指定する．辞書
               エントリと同じ形式で指定する．(default: "<UNK> [sp] sp sp")

            -sepnum  number
               木構造化辞書の構築時に線形登録する単語数を指定する．(default:
               150)

            -adddict  dicfile
               起動時に辞書を追加で読み込む．

            -addword  entry_string
               起動時に単語エントリを追加で読み込む．

       文法
           -gramや-gramlistで文法を複数回指定す ることで，一つのインスタンス内
           で複数の文法を用いることができる． （旧Juliusのオプション -dfa, -v
           の 組合せは単一の文法のみ指定可能である）

            -gram  gramprefix1[,gramprefix2[,gramprefix3,...]]
               認識に使用する文法を指定する．文法はファイル（辞書および構文制
               約 有限オートマトン）のプレフィックスで指定する．すなわち，ある
               認 識用文法がdir/foo.dictと dir/foo.dfa としてあるとき，
               dir/fooのように拡張子を省いた名前で指定する． 文法はコンマで区
               切って複数指定することができる．また繰り返し 使用することでも複
               数指定できる．

            -gramlist  list_file
               認識に使用する文法のリストをファイルで指定する． list_fileに
               は， -gram と同様の文法プレフィックスを1行に１つず つ記述す
               る．また，このオプションを繰り返し使用することで，複数 のリスト
               ファイルを指定できる．なお，リスト内で文法を相対パスで 指定した
               場合，それは，そのリストファイルからの相対パスとして解 釈される
               ことに注意が必要である．

            -dfa  dfa_file  -v  dict_file
               認識に使用する文法の構文制約オートマトンと辞書をそれぞれ指定す
               る． (Julius-3.x との互換性のための古いオプションであり，使用す
               べきでない）

            -nogram
               それまでに -gram，-gramlist， -dfa および -v で 指定された文法
               のリストをクリアし，文法の指定なしの状態 にする．

            -adddict  dicfile
               起動時に辞書を追加で読み込む．

            -addword  entry_string
               起動時に単語エントリを追加で読み込む．

       単単語
            -w  dict_file
               単単語認識で用いる単語辞書を指定する．ファイル形式は単語N-gram
               や文法と同一である．辞書上の全ての単語が認識対象となる．
               (Rev.4.0)

            -wlist  list_file
               単語辞書のリストを指定する．list_file には1行に一つずつ，使用す
               る単語辞書のパスを記述する．相対パスを 用いた場合，それはそ
               のlist_fileから の相対パスとして解釈される． (Rev.4.0)

            -nogram
               それまでに -w あるいは -wlistで 指定された辞書のリストをクリア
               し，指定なしの状態に戻す．

            -wsil  head_sil_model_name tail_sil_model_name sil_context_name
               単単語認識時，音声入力の両端の無音モデルおよびそのコンテキスト
               名を指定する． sil_context_nameとして NULLを指定した場合，各モ
               デル名がそのまま コンテキストとして用いられる．

            -adddict  dicfile
               起動時に辞書を追加で読み込む．

            -addword  entry_string
               起動時に単語エントリを追加で読み込む．

       User-defined LM
            -userlm
               プログラム中のユーザ定義言語スコア計算関数を使用することを宣言
               する．(Rev.4.0)

       その他の言語モデル関連
            -forcedict
               単語辞書読み込み時のエラーを無視する．通常Juliusは単語辞書内に
               エラーがあった場合そこで動作を停止するが，このオプションを 指定
               することで，エラーの生じる単語をスキップして処理を続行する こと
               ができる．

   音響モデル・特徴量抽出 (-AM) (-AM_GMM)
       音響モデルオプションは，音響モデルおよび特徴量抽出・フロントエンド処理
       に関する設定を行う．特徴量抽出，正規化処理，スペクトルサブトラクション
       の 指定もここで行う．

       音響HMM関連
            -h  hmmdef_file
               使用するHMM定義ファイル． HTK の ASCII 形 式ファイル，あるい
               はJulius バイナリ形式のファイルのどちらかを 指定する．バイナリ
               形式へは mkbinhmm で 変換できる．

            -hlist  hmmlist_file
               HMMlistファイルを指定する．テキスト形式，あるいはバイナリ形式
               のどちらかを指定する．バイナリ形式へは mkbinhmmlist で変換でき
               る．

            -tmix  number
               Gaussianpruning の計算状態数を指定する．小さ いほど計算が速くな
               るが，音響尤度の誤差が大きくなる．See also -gprune. (default:
               2)

            -spmodel  name
               文中のショートポーズに対応する音韻HMMの名前を指定する．このポー
               ズ モデル名は，-iwsp, -spsegment, -pausemodelsに関係する．ま
               た，文法使用時に スキップ可能なポーズ単語エントリの識別にも用い
               られる． (default: "sp")

            -multipath   -nomultipath
               状態間遷移を拡張するマルチパスモードを有効にする．オプション指
               定がない場合，Julius は音響モデルの遷移をチェックし，必要であ
               れば自動的にマルチパスモードを有効にする．このオプション
               は，ユー ザが明示的にモードを指定したい場合に使用する．

               この機能は 3.x ではコンパイル時オプションであったが，4.0 より
               実行時オプションとなった．(rev.4.0)

            -gprune  {safe|heuristic|beam|none|default}
               使用する Gaussian pruning アルゴリズムを選択する． noneを指定す
               ると Gaussian pruning を無効化 しすべてのガウス分布について厳密
               に計算する． safe は上位 N 個を計算する． heuristic と beam
               はsafe に比べてより積極的な枝刈りを行うため計算量削減の効果が大
               きいが， 認識精度の低下を招く可能性がある．defaultが 指定された
               場合，デフォルトの手法を使う．(default: tied-mixture model の場
               合，standard 設定ではsafe，fast設 定ではbeam．tied-mixture でな
               い場合 none).

            -iwcd1  {max|avg|best number}
               第1パスの単語間トライフォン計算法を指定する． max 指定時，同じ
               コンテキストのトライフォン集合の 全尤度の最大値を近似尤度として
               用いる．avg は 全尤度の平均値を用いる．best number は上位 N 個
               の トライフォンの平均値を用いる． デフォルトは，一緒に使用され
               る言語モデルに依存する．N-gram使用 時には best 3，文法使用時は
               avgとなる．もしこの音響モデルが異なるタイプの 複数の言語モデル
               で共有される場合は，後に定義されたほうのデフォルトが デフォルト
               値として用いられる．

            -iwsppenalty  float

               -iwspによって末尾に付加される単語末ショートポー ズの挿入ペナル
               ティ．ここで指定した値が，通常単語の末尾から単語 末ショートポー
               ズへの遷移に追加される．

            -gshmm  hmmdef_file
               Gaussian Mixture Selection 用のモノフォン音響モデルを指定する．
               GMS用モノフォンは通常のモノフォンから mkgshmm によって生成でき
               る．

            -gsnum  number
               GMS 使用時，対応するトライフォンを詳細計算するモノフォンの 状態
               の数を指定する． (default: 24)

       特徴量抽出パラメータ
            -smpPeriod  period
               音声のサンプリング周期を指定する．単位は，100ナノ秒の単位で指
               定する．サンプリング周期は -smpFreq でも指定 可能．(default:
               625)

               このオプションは HTK の SOURCERATE に対応する．同じ値が指定でき
               る．

               複数の音響モデルを用いる場合，全インスタンスで共通の値を指定す
               る必要 がある．

            -smpFreq  Hz
               音声のサンプリング周波数 (Hz) を指定する．(default: 16,000)

               複数の音響モデルを用いる場合，全インスタンスで共通の値を指定す
               る必要 がある．

            -fsize  sample_num
               窓サイズをサンプル数で指定 (default: 400)．

               このオプションは HTK の WINDOWSIZE に対応する．ただし値はHTKと
               異なり，(HTKの値 / smpPeriod) となる．

               複数の音響モデルを用いる場合，全インスタンスで共通の値を指定す
               る必要 がある．

            -fshift  sample_num
               フレームシフト幅をサンプル数で指定 (default: 160)．

               このオプションは HTK の TARGETRATE に対応する．ただし値はHTKと
               異なり，(HTKの値 / smpPeriod) となる．

               複数の音響モデルを用いる場合，全インスタンスで共通の値を指定す
               る必要 がある．

            -preemph  float
               プリエンファシス係数 (default: 0.97)

               このオプションは HTK の PREEMCOEF に対応する．同じ値が指定でき
               る．

            -fbank  num
               フィルタバンクチャンネル数．(default: 24)

               このオプションは HTK の NUMCHANS に対応する．同じ値が指定でき
               る．指定しないときのデフォルト値が HTKと異なっていることに注意
               （HTKでは22）．

            -ceplif  num
               ケプストラムのリフタリング係数. (default: 22)

               このオプションは HTK の CEPLIFTER に対応する．同じ値が指定でき
               る．

            -rawe ,  -norawe
               エネルギー項の値として，プリエンファシス前の raw energy を使用
               する / しない (default: disabled=使用しない)

               このオプションは HTK の RAWENERGY に対応する． 指定しないときの
               デフォルトがHTKと異なっていることに注意（HTKで はenabled)．

            -enormal ,  -noenormal
               エネルギー項の値として，発話全体の平均で正規化した正規化エネル
               ギー を用いるかどうかを指定する．(default: -noenormal)

               このオプションは HTK の ENORMALISE に対応する． 指定しないとき
               のデフォルトがHTKと異なっていることに注意（HTKで はenabled)．

            -escale  float_scale
               エネルギー正規化時の，対数エネルギー項のスケーリング係数．
               (default: 1.0)

               このオプションは HTK の ESCALE に対応する．デフォルト値がHTKと
               異なっていることに注意（HTKでは 0.1）．

            -silfloor  float
               エネルギー正規化時の，無音部のエネルギーのフロアリング値．
               (default: 50.0)

               このオプションは HTK の SILFLOOR に対応する．同じ値が指定でき
               る．

            -delwin  frame
               一次差分計算用のウィンドウフレーム幅．(default: 2)

               このオプションは HTK の DELTAWINDOW に対応する．同じ値が指定で
               きる．

            -accwin  frame
               二次差分計算用のウィンドウフレーム幅．(default: 2)

               このオプションは HTK の ACCWINDOW に対応する．同じ値が指定でき
               る．

            -hifreq  Hz
               MFCCのフィルタバンク計算時におけるバンド制限を有効化する．この
               オプションではカットオフ周波数の上限値を指定する． -1 を指定す
               ることで無効化できる．(default: -1)

               このオプションは HTK の HIFREQ に対応する．同じ値が指定できる．

            -lofreq  Hz
               MFCCのフィルタバンク計算時におけるバンド制限を有効化する．この
               オプションではカットオフ周波数の下限値を指定する． -1 を指定す
               ることで無効化できる．(default: -1)

               このオプションは HTK の LOFREQ に対応する．同じ値が指定できる．

            -zmeanframe ,  -nozmeanframe
               窓単位の直流成分除去を有効化／無効化する． (default: disabled)

               このオプションは HTK の ZMEANSOURCE に対応する．-zmean も参照の
               こと．

            -usepower
               フィルタバンク解析で振幅の代わりにパワーを使う．(default:
               disabled)

       正規化処理
            -cvn
               ケプストラム分散正規化 (cepstral variance normalization; CVN)
               を有効にする．ファイル入力では，入力全体の分散に基づいて正規化
               が行われる．直接入力ではあらかじめ分散が得られないため，最後の
               入力の分散で代用される．音声信号入力でのみ有効である．

            -vtln  alpha lowcut hicut
               周波数ワーピングを行う．声道長正規化 (vocal tract length
               normalization; VTLN) に使用できる．引数はそれぞれワーピング 係
               数，周波数上端，周波数下端であり，HTK設定の
               WARPFREQ，WARPHCUTOFF および WARPLCUTOFF に対応する．

            -cmnload  file
               起動時にケプストラム平均ベクトルを fileから読み込む．ファイルは
               -cmnsave で保存されたファイルを指定する．これ は MAP-CMN におい
               て，起動後最初の発話においてケプストラム平均 の初期値として用い
               られる．通常，2発話目以降は初期値は，直前の 入力の平均に更新さ
               れるが，-cmnnoupdateを指定 された場合，常にこのファイルの値が各
               発話の初期値として用いられ る．

            -cmnsave  file
               認識中に計算したケプストラム平均ベクトルを fileへ保存する．すで
               にファイルがあ る場合は上書きされる．この保存は音声入力が行われ
               るたびに上書きで 行われる．

            -cmnupdate   -cmnnoupdate
               実時間認識時，初期ケプストラム平均を入力ごとに更新するかどうか
               を指定する．通常は有効 (-cmnupdate) であり， 過去5秒間の入力の
               平均を初期値として更新する． -cmnnoupdate が指定された場合，更
               新は行われず， 初期値は起動時の値に固定される．-cmnload で初期
               値 を指定することで，常に同じ初期値を使うようにすることができ
               る．

            -cmnmapweight  float
               MAP-CMN の初期ケプストラム平均への重みを指定する．値が大きいほ
               ど初期値に長時間依存し，小さいほど早く現入力のケプストラム平均
               を用いるようになる．(default: 100.0)

       フロントエンド処理
            -sscalc
               入力先頭の無音部を用いて，入力全体に対してスペクトルサブトラク
               ションを行う．先頭部の長さは-sscalclenで指定する． ファイル入力
               に対してのみ有効である．-ssload と 同時に指定できない．

            -sscalclen  msec

               -sscalcオプション指定時，各ファイルにおいて ノイズスペクトルの
               推定に用いる長さをミリ秒で指定する．(default: 300)

            -ssload  file
               ノイズスペクトルをfileから読み込ん でスペクトルサブトラクション
               を行う． fileはあらかじめ mkssで作成する．マイク入力・ネット
               ワーク入 力などのオンライン入力でも適用できる．-sscalcと 同時に
               指定できない．

            -ssalpha  float

               -sscalcおよび-ssload用の 減算係数を指定する．この値が大きいほど
               強くスペクトル減算を行うが， 減算後の信号の歪も大きくな
               る．(default: 2.0)

            -ssfloor  float
               スペクトルサブトラクションのフロアリング係数を指定する．スペク
               トル減算時，計算の結果パワースペクトルが負となってしまう帯域に
               対しては，原信号にこの係数を乗じたスペクトルが割り当てられる．
               (default: 0.5)

       その他の音響モデル関連オプション
            -htkconf  file

               HTK Config ファイルを解析して，対応する特徴量抽出オプションを
               Julius に自動設定する．file は HTK で音響モデル学習時に使用した
               Config ファイルを指定する．なお， Julius と HTK ではパラメータ
               のデフォルト値が一部異なるが， このオプションを使用する場合，デ
               フォルト値も HTK のデフォルト に切替えれられる．

   認識処理・探索 (-SR)
       認識処理・探索オプションは，第1パス・第2パス用のビーム幅や言語重みのパ
       ラメータ，ショートポーズセグメンテーションの設定，単語ラティス・CN 出力
       用設定，forced alignment の指定，その他の認識処理と結果出力に関するパラ
       メータを含む．

       第1パスパラメータ
            -lmp  weight penalty
               (N-gram使用時) 第1パス用の言語スコア重みおよび挿入ペナルティ．
               ペナルティは負であれば単語挿入を抑制し，正であれば単語挿入を促
               進する．

            -penalty1  penalty
               (文法使用時) 第1パス用の単語挿入ペナルティ． (default: 0.0)

            -b  width
               第1パス探索の枝刈り (rank pruning) のビーム幅を指定する．単位
               は HMM ノード数である． デフォルト値は音響モデルやエンジンの設
               定による．モノフォン 使用時は400, トライフォン使用時は800，トラ
               イフォンでかつ setup=v2.1 のときは 1000 となる．

            -bs  width
               第1パスのscore pruningのスコア幅を指定する．rank pruning（-b
               width）と併用可能．デフォルトはオフ．

            -nlimit  num
               第1パスでノードごとに保持する仮説トークンの最大数．通常は 1 で
               固定されており変更できない．コンパイル時に configureで
               --enable-wpairおよび --enable-wpair-nlimit が指定されているとき
               のみ変更できる．

            -progout
               第1パスで，一定時間おきにその時点での最尤仮説系列を出力する．

            -proginterval  msec

               -progoutの出力インターバルをミリ秒で指定する． (default: 300)

       第2パスパラメータ
            -lmp2  weight penalty
               (N-gram使用時) 第2パス用の言語スコア重みおよび挿入ペナルティ．
               ペナルティは負であれば単語挿入を抑制し，正であれば単語挿入を促
               進する．

            -penalty2  penalty
               (文法使用時) 第2パス用の単語挿入ペナルティ． (default: 0.0)

            -b2  width
               第2パス探索における仮説展開回数の上限を指定する．単位は 仮説
               数．(default: 30)

            -sb  float
               第2パスの仮説尤度計算時のスコア幅を指定する．単位は対数尤度差
               である．(default: 80.0)

            -s  num
               仮説のスタックサイズを指定する．(default: 500)

            -n  num

               num個の文仮説数が見付かるまで探索を 行う．得られた仮説はスコア
               でソートされて出力される （-outputも見よ）．デフォルト値はコン
               パイル時 のエンジン設定によって変わり，fast 版では 1,
               standard版では10 である．

            -output  num
               見つかったN-best候補のうち，結果として出力する文仮説の数を 指定
               する．-nも参照のこと．(default: 1)

            -m  count
               探索打ち切りのための仮説展開回数のしきい値を指定する．
               (default: 2000)

            -lookuprange  frame
               第2パスの単語展開時に，接続しうる次単語候補を見付けるための 終
               端時刻の許容幅をフレーム数で指定する．値を大きくするほど その周
               辺の多くの仮説を次単語候補として仮説展開が行われるように なる
               が，探索が前に進みにくくなることがある．(default: 5)

            -looktrellis
               仮説展開を第1パスの結果単語トレリス上に絞る．

       ショートポーズセグメンテーション
            -spsegment
               ショートポーズセグメンテーションを有効にする． (Rev.4.0)

            -spdur  frame
               無音区間判定のためのしきい値を指定する．無音単語が一位仮説とな
               るフレームがこの値以上続いたとき，無音区間として入力が区切られ
               る．(default: 10)

            -pausemodels  string
               「無音単語」を定義するための音響モデルにおける無音モデルの名前
               を指定する．コンマで区切って複数の名前を指定できる． このオプ
               ションが指定されない場合，文法を用いた認識では -spmodel で指定
               されるモデルのみを読みとする単 語が無音単語とされる．ま
               た，N-gramではこれに加えて -silhead および -siltail で 指定され
               る単語も無音単語として扱われる．(Rev.4.0)

            -spmargin  frame
               デコーダベースVADにおいて，アップトリガ時の巻戻し幅をフレーム
               数で指定する．(Rev.4.0)

               このオプションはconfigureに --enable-decoder-vadを付けてコンパ
               イルしたとき のみ有効である．

            -spdelay  frame
               デコーダベースVADにおいて，アップトリガ判定の遅延幅をフレーム
               数で指定する．(Rev.4.0)

               このオプションはconfigureに --enable-decoder-vadを付けてコンパ
               イルしたとき のみ有効である．

       単語ラティス / confusion network 出力
            -lattice ,  -nolattice
               単語グラフ（ラティス）の出力を有効化/無効化する．

            -confnet ,  -noconfnet
               Confusion network の出力を有効化/無効化する．confusion network
               は単語グラフから生成されるため，有効時は同時に -lattice も有効
               化される．(Rev.4.0)

            -graphrange  frame
               グラフ生成において近傍の同一単語仮説をマージする．開始フレーム
               および終了フレームの位置の差がそれぞれ frame以下の同一単語仮説
               についてマー ジする．その際，スコアは高いほうのものが残され
               る．値が -1 の場 合，マージは一切行われない．値を大きくするほど
               コンパクトなグラ フが生成されるが，スコアの誤差が大きくなる．こ
               のオプションは -confnetにも影響する．(default: 0)

            -graphcut  depth
               生成されたグラフに対して，深さによるカットオフを行う．
               depthは，あるフレームにおいて存在可 能な単語数の上限を指定す
               る．Julius では，第2パスの探索が不安定 な場合，一部分が極端に深
               いグラフが生成されることが稀にあり，こ のオプションによってそれ
               を抑制することができる．-1 を指定する ことでこの機能は無効化さ
               れる．(default: 80)

            -graphboundloop  count
               事後的に行われる単語グラフの境界時間調整において，振動による 無
               限ループを防ぐための打ち切り値を指定する．(default: 20)

            -graphsearchdelay ,  -nographsearchdelay
               巨大グラフ生成用にアルゴリズムをチューニングする．このオプショ
               ンが有効時，Julius は第1文仮説が見つかる前のグラフ生成時の仮説
               中断を行わないように，グラフ生成アルゴリズムを変更する．これ
               は， ビーム幅や探索範囲を極端に大きくして巨大なワードグラフを生
               成し ようとするときに，グラフの精度を改善することがあ
               る．(default: disabled)

       複数文法/複数辞書認識
           文法や単単語認識において，一つのインスタンスで複数の文法や辞書を用
           いる 場合に指定できるオプションである．

            -multigramout ,  -nomultigramout
               複数文法あるいは複数辞書を用いて認識を行う場合，通常の Julius
               は全ての文法/辞書の中から最尤仮説を出力する．このオプションを
               指定することで，与えられた個々の文法/辞書ごとに一位仮説を 出力
               することができる．(default: disabled)

       Forced alignment
            -walign
               認識結果を用いて，入力に対する単語単位の forced alignment を行
               う．単語の境界フレームと平均音響尤度が出力される．

            -palign
               認識結果を用いて，入力に対する音素単位の forced alignment を行
               う．音素ごとの境界フレームと平均音響尤度が出力される．

            -salign
               認識結果を用いて，入力に対するHMMの状態単位の forced alignment
               を行う．状態ごとの境界フレームと平均音響尤度が出力される．

       その他
            -inactive
               認識処理インスタンスを一時停止状態 (inactive state) で起動す
               る． (Rev.4.0)

            -1pass
               第1パスのみを実行する．このオプションを指定した場合，第2パスは
               実行されない．

            -fallback1pass
               通常，第2パスの探索が失敗したとき，Julius は認識結果無しで終了
               する．このオプションを指定することで，そのような第2パスの失敗時
               に， 第1パスの最尤仮説を最終結果として出力することができる． （
               これはJulius-3.xでのデフォルトの振る舞いである）

            -no_ccd ,  -force_ccd
               音響モデルを音素コンテキスト依存モデルとして扱うかどうかを明示
               的に指定する．デフォルトはHMM中のモデル名から自動判断される．


            -cmalpha  float
               確信度計算のためのスコアのスムージング係数．(default: 0.05)

            -iwsp
               （マルチパスモード時のみ有効）単語間にショートポーズモデルを 挟
               み込んだ認識処理を行う．このオプションを指定すると，辞書上の 全
               単語の末尾に，スキップ可能なショートポーズモデルが付加される．
               このショートポーズモデルはコンテキストを考慮せず，また前後の 音
               素のコンテキストにも表れない．付加するショートポーズモデルは
               -spmodel で指定できる．

            -transp  float
               透過単語に対する追加の挿入ペナルティを指定する．(default: 0.0)

            -demo

               -progout -quietと同等．

            -mbr

            -nombr

            -mbr_wwer

            -mbr_weight

ENVIRONMENT VARIABLES
        ALSADEV
           (マイク入力で alsa デバイス使用時) 録音デバイス名を指定する． 指定
           がない場合は "default"．

        AUDIODEV
           (マイク入力で oss デバイス使用時) 録音デバイス名を指定する． 指定が
           ない場合は "/dev/dsp"．

        PORTAUDIO_DEV
           (portaudio V19 使用時) 録音デバイス名を指定する． 具体的な指定方法
           は adinrec の初期化時にログに出力されるので参照のこと．

        LATENCY_MSEC
           Linux (alsa/oss) および Windows で，マイク入力時の遅延時間をミ リ秒
           単位で指定する．短い値を設定することで入力遅延を小さくでき る
           が，CPU の負荷が大きくなり，また環境によってはプロセスやOSの 挙動が
           不安定になることがある．最適な値はOS やデバイスに大きく 依存す
           る．デフォルト値は動作環境に依存する．

EXAMPLES
       使用例については付属のチュートリアルをご覧下さい．

SEE ALSO
       julian(1), jcontrol(1), adinrec(1), adintool(1), mkbingram(1),
       mkbinhmm(1), mkgsmm(1), wav2mfcc(1), mkss(1)

       http://julius.sourceforge.jp/[1]

DIAGNOSTICS
       正常終了した場合，Julius は exit status として 0 を返します．エラーが見
       付かった場合は異常終了し， exist status として 1 を返します． 入力ファ
       イルが見つからない場合やうまく読み込めなかった場合は，そのファ イルに対
       する処理をスキップします．

BUGS
       使用できるモデルにはサイズやタイプに若干の制限があります．詳しく はパッ
       ケージに付属のドキュメントを参照してください． バグ報告・問い合わせ・コ
       メントなどは GitHub のサイト までお願いします．

COPYRIGHT
       Copyright (c) 1991-2013 京都大学 河原研究室

       Copyright (c) 1997-2000 情報処理振興事業協会(IPA)

       Copyright (c) 2000-2005 奈良先端科学技術大学院大学 鹿野研究室

       Copyright (c) 2005-2013 名古屋工業大学 Julius開発チーム

AUTHORS
       Rev.1.0 (1998/02/20)
           設計：河原達也と李 晃伸 (京都大学)

           実装：李 晃伸 (京都大学)

       Rev.1.1 (1998/04/14), Rev.1.2 (1998/10/31), Rev.2.0 (1999/02/20),
       Rev.2.1 (1999/04/20), Rev.2.2 (1999/10/04), Rev.3.0 (2000/02/14),
       Rev.3.1 (2000/05/11)
           実装：李 晃伸 (京都大学)

       Rev.3.2 (2001/08/15), Rev.3.3 (2002/09/11), Rev.3.4 (2003/10/01),
       Rev.3.4.1 (2004/02/25), Rev.3.4.2 (2004/04/30)
           実装：李 晃伸 (奈良先端科学技術大学院大学)

       Rev.3.5 (2005/11/11), Rev.3.5.1 (2006/03/31), Rev.3.5.2 (2006/07/31),
       Rev.3.5.3 (2006/12/29), Rev.4.0 (2007/12/19), Rev.4.1 (2008/10/03),
       Rev.4.1.5 (2010/06/04), Rev.4.2 (2011/05/01), Rev.4.2.1 (2011/12/25),
       Rev.4.2.2 (2012/08/01), Rev.4.2.3 (2013/06/30), Rev.4.3 (2013/12/25)
           実装：李 晃伸 (名古屋工業大学)

THANKS TO
       このプログラムは Rev.3.1 まで，情報処理振興事業協会(IPA)独創的情報技術
       育 成事業「日本語ディクテーションの基本ソフトウェアの開発」(代表者：鹿
       野 清宏 奈良先端科学技術大学院大学教授)の援助を受けて行われました．
       Rev.3.4.2までは「情報処理学会 連続音声認識コンソーシアム」において公開
       さ れました．

       3.x 時代のマルチプラットフォーム DLL版 は，板野秀樹氏(現名城大学)の手
       によって作成・公開されました．また，Windows Microsoft Speech API対応版
       は 住吉貴志氏(京都大学・当時)の手によるものです．

       そのほか，上記の協力・貢献してくださった方々，およびさまざまな助言・コ
       メントをいただく関係者各位に深く感謝いたします．

注記
        1. http://julius.sourceforge.jp/
           http://julius.sourceforge.jp/en/



                                  19/12/2013                         JULIUS(1)

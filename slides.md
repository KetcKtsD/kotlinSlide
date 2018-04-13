# Kotlin

---

# kotlinとは

--

2011年に発表されたばかりのJVM言語です

--

読み方は「ことりん」、 意味はやかん、 ロゴもやかん

--

語感がかわいい

--

ロシア サンクトペテルブルグのJetBrains社研究所生まれ

--

ロシア生まれとかどう考えてもかわいい

--

生まれた地に近いコトリン島から命名されました

--

__つまり__

--

# __かわいい(大事)__

---

# 主な特徴や機能

--

- 静的型付け  
- クロージャ  
- Null安全  
- 拡張関数  
- インライン関数  
- 単一式関数  
- ローカル関数

--

- ジェネリクス  
- フィールドがない(プロパティがある)
- 演算子オーバーロード  
- ミックスインと第一級デリゲーション  
- スマートキャスト  
- javaScriptへコンパイル可  
- Android上での動作  
- オープンソース(Apache License Version 2.0による)  

--

- JSへの変換

---

# Kotlinを始めよう

--

- 必要なもの
- [InteliJ IDEA](https://www.jetbrains.com/idea/) 
    * 学生ならタダで有料版が使えます使おう
- ある程度のJavaの知識(なくてもなんとかなるかな？)

---

__ここでブラウザ上でKotlin試せるよ！__

[https://try.kotlinlang.org/](http://try.kotlinlang.org/)

---

# Hello World

--
Javaの場合

    public class Main {
        public static void main(String args[]){
            System.out.println("ほげらりおん");
        }
    }

Kotlinの場合

    fun main(args: Array<String>){
        println("ほげらりおん")
    }

--

HelloWorldから分かるJavaとは違うところ
--
- 関数の宣言にはfunctionの略であるfunを用いる
- 変数の宣言は「変数名: 型名」
- 関数はクラスに属さなくても良い
- セミコロンがいらない

---

# 基本的な事について

---

### 可視性について
Kotlinの可視性のアノテーションは4種類あります

--

- public
    * すべてのクラス(またはファイル)に公開

- internal 
    * 同一コンパイル単位内に公開

- protected
    * 自身と自身を継承したクラスに公開
    
- private
    * 公開しない

--

これらアノテーションはクラス、コンストラクタ、プロパティと関数等に付与することができます  

特にこれらを指定しない場合、デフォルトの可視性はpublicです

---

## Kotlinで扱う基本的な型

---

### Any

KotlinのすべてクラスはAnyクラスを継承しています  
特にスーパータイプの宣言がない場合はAnyがデフォルトのスーパークラスとなります  

--

Any is not java.lang.Object

--

Anyクラスはメンバとして以下のメソッドが定義されています

- equals(other: Any?): Boolean
    * 構造の等価性を判断する

- hashCode(): Int
    * ハッシュコードを得る

- toString(): String
    * 文字列で表す

--

equalsメソッドは == に置き換えることができます

---

### Unit型

Javaで言うvoidのような物  
関数等が特に決まって返す値がない場合に返す  
オブジェクト  
明示的に返さなくても良い  

---

### 数値型

| 型 | ビット幅|
|:-|:-|
|Byte|8|
|Short|16|
|Int| 32 |
|Long| 64 |
|Float|32|
|Double|64|

--

Kotlinでは文字は数値ではありません  

--

### 注意点
KotlinにはJavaのようにプリミティブな型は存在せず、すべてがオブジェクトです  
そして数値型の暗黙的な型変換は存在しません  
そもそも小さな型は大きな型のサブタイプではありません  

__例__    

    val a: Int = 1
    val b: Double = a //コンパイルエラー

--

明示的な型変換を行う必要がある
    
    val a: Int = 1
    val b: Double = a.toDouble() //Double型に明示的に変換

--

全ての数値型は次の変換が可能です

* toByte()
* toShort()
* toInt()
* toLong()
* toFloat()
* toDouble()
* toChar()

---

### 真偽値型
__Boolean__

組み込み演算子としてJavaと同じく || と && と ! が提供されています

---

### 文字型
__Char__

Javaのchar型とほぼ一緒ですが  
数値としては扱われません  

    val c: Char = 'あ' //Javaのchar型と同じく ' で囲って表現
    val a: Int = c.toInt() //明示的にInt型に変換可

---

### 文字列型
__String__

String型には二種類の文字列リテラルがあります  

テンプレート式を使用できます  

--

インデックス演算でアクセスでき、forループでぶん回すこともできます。  

--

### エスケープ文字列

Javaと一緒のやつ
    
    val str: String = "ほげらりおん\n" //エスケープシーケンスもJavaと一緒

--

### 生文字列
生の文字列をそのまま認識します """ で囲って表現
    
    val str: String = """ 
    ほげほげほげ
    ほげほげ
    ほげほげ
    \n
    """ //エスケープシーケンスも無視してそのまま認識されます

--

先頭の空白をtrimMargin関数で削除することも可能です  

    val str: String = """ 
    |ほげほげほげ
    |ほげほげ
    |ほげほげ
    """.trimMargin()

デフォルトの接頭辞は'|'ですが、'trimMargin(">")'のようにして  
パラメータを与えるとそれを接頭辞として扱うこともできます。
    

--

### テンプレート式
文字列の文字列への組み込み

${式} または $変数名 を使用する

    val str1: String = "ほげらりをん"
    val str2: String = "つよい"
    val str3: String = "${str1 + 'は'}$str2" //ほげらりをんはつよい

---

### Range
範囲を表す型

--

- CharRange
    * Char型の範囲を表す
- IntRange
    * Int型の範囲を表す
- LongRange
   * Long型の範囲を表す

--

例

    1.rangeTo(4) //1~4の範囲を表すIntRangeを得る
    1L.rangeTo(4L) //1~4の範囲を表すLongRangeを得る
    'a'.rangeTo('z') //a~zを表すCharRangeを得る
    4.downTo(1) // 4~1の範囲を表すIntRangeを得る

--

rangeTo() は .. downTo() はdownTo に置き換えられる

    1..4 //1~4の範囲を表すIntRange
    1L..4L //1~4の範囲を表すLongRange
    'a'..'z' //a~zを表すCharRange
    4 downTo 1 // 4~1の範囲を表すIntRange

--
ある数刻みでのRangeを得たい場合はstepを使います

    1..4 step 2//1~4の範囲を2刻みで表すIntRange
    1L..4L step 2L//1~4の範囲を2刻みで表すLongRange
    'a'..'z' step 2 //a~zを一つ飛ばしで表すCharRange
    4 downTo 1 step 2 // 4~1の範囲を2刻みで表すIntRange
    

--

rangeTo() downTo() 以外でRangeを得たい場合は  
各クラスのProgression系クラスのfromClosedRange関数を使用します

--

例

    IntProgression.fromClosedRange(1, 4, 1)
    
第一引数から第二引数の範囲のレンジを得る、第三引数はstep

--

Rangeは  
オブジェクト  (in or !in)  range  
と書くことでBooleanを返す式にすることができます

--

例

    println(3 in 1..3) //true   3は1~3の範囲に含まれている？
    println(3 !in 1..3) //false 3は1~3の範囲に含まれない？

---

### 配列
__Array__
- Array型を用いる

- 配列操作のためのプロパティやメンバ関数を提供

- ジェネリクスを用いるため不変です（Javaの配列は共変）

--

arrayOf,arrayOfNulls,emptyArray等の標準ライブラリ関数を使用して作成できます  

--

例

    val intArray: Array<Int> = arrayOf(1, 2, 3, 4)
    intArray.set(3,5) //indexが3の要素に5を代入
    intArray.forEach {//関数リテラルを用いてリストを順走査するメソッド
        println(it)  //要素を順番に出力
    }

--

### さっきの共変不変のくだり
### 役に立つはずだから興味があれば聞いてね

--

# 共変とは？

--

広い型から狭い型への変換を行うこと  

またはそれが可能であること

--

この説明では、子クラスから親クラスへの変換を行うこと、
またはそれが可能であることを指します

--

それらを踏まえて以下のコードを見てみましょう

--

Javaの例

    //IntegerクラスはNumberクラスを継承しています
    val Integer[] intArray = {1, 2 ,3 ,4}  
    val Number[] numArray = intArray //危険な操作だけどコンパイル通っちゃう
    numArray[0] = 1.2 //numArrayの要素は実体はIntegerなのでランタイムエラー

--

Kotlinの例

    //IntクラスはNumberクラスを継承しています
    val intArray: Array<Int> = arrayOf(1, 2, 3, 4)
    val numArray: Array<Number> = intArray //ここでコンパイルエラー

--

kotlinの配列は無論共変としても扱える

    val intArray: Array<Int> = arrayOf(1, 2, 3, 4)
    val numArray: Array<out Number> = intArray //共変は out 型 と記述する

--

ではKotlinの配列を共変にするとJavaの配列と同じようにランタイムエラーが起こり得るのか？

--

答えはNo

--

_配列を共変にした時点でsetメソッドは見えなくなります_

--

なぜ？

--

Javaの例から分かる通り、共変なオブジェクトに値の  
入力が行えるのは問題がります

--

つまり

--

__共変なオブジェクトへは値の入力が行えるべきではない__

--

それを示すのがoutというキーワードであり、それが共変を表す所以です


共変の逆は反変で in となります

---

# 構文

---

## 変数宣言

--

Kotlinの変数には二種類あります

--

再代入可のやつと再代入不可のやつ

--

宣言に用いるワードは前者がvar後者がval

例

    val valHoge: String = "hoge" //再代入不可
    var varHoge: String = "hoge" //再代入可

宣言の構文は (var or val) (変数名): (型名) = (初期化式)

--

いちいち型名書くのがだるい?

--

もちろん省略できます！

--

省略しました

    val hoge = "hoge"

--

初期化時に入ってくる型が明確(型推論が可能)な場合に省略が可能です 

--

    var hoge = "hoge"
    hoge = 1

始めに言った通り、Kotlinは静的型付けです  
これは許容されません  


---

## インスタンスの生成

--

Javaと同じくコンストラクタを使用しますが

new は不要です

--

例

    val hoge = Hoge() //Hogeクラスのインスタンスを生成
    
宣言時の型の省略で代入式の両辺に  
型名を書く必要がなく

とてもスマート

--

しかし

--

型の省略は使い方次第でコードの可読性を上げも下げもするので注意！！！

---

## if式

--

if文では無くif式

--

つまりどういうこと？

--

式なので値を返します

--

Javaで例えるならば、三項演算子のような振る舞いをします

--

まずは普通のif文らしい例から

--

例

    val hoge = 3
    if (hoge != 3) {
        println("うぇい")
    } else { 
        println("そいや")
    }
    //そいや
--

もちろんif-else内での処理が一つだけなら{}は省略できます

    val hoge = 3
    if (hoge != 3) 
        println("うぇい")
    else 
        println("そいや") //そいや

--

値を返す

    val hoge = 3
    val fuga = if(hoge==3) hoge else 0
    
hogeが3ならばhoge そうでなければ0をfugaに代入

--

if式はを式として扱う場合は(変数に代入するなど)</br>elseの枝がなければなりません</br>
また、一つでも違う型を返す枝があれば</br>Anyとして扱われます</br>

--

ただし、全ての節が返す値の型が同一の</br>インターフェース、
またはクラスを継承していた場合はその型として扱われます

---

## ~~switch文~~  
## when式

--

Kotlinにはswitch文は存在せず、代わりにwhen式が存在します

--

if式同様にwhen式は値を返しますが同様に、  
elseの枝がないと値として変数に代入することはできません

--

例

    //hogeはInt型とする
     val hogehoge = when (hoge) {
         1 -> {
             "1"
         }
         2 -> "2" //処理が一つの場合{}を省略できる
         3, 4, 5 -> "3 or 4 or 5" //複数の値を指定できる
         in 6..10 -> "6 to 10" //レンジを条件として使用可能
         else -> "undefined" //else枝
        }

--

枝ごとにbreakしなくてもいいの？

--

when式はフォールスルーしません

---

### For文

--

Javaのようにカウンタをインクリメントしながら</br>繰り返しを制御したりするようなfor文ではありません  

--

Javaのfor-eachの様に、  
ある条件を満たしたオブジェクトから  
要素を順番に取り出して繰り返し処理を行います

--

### 条件とは
1. 関数、もしくは拡張関数にIteratorもしくは条件2を満たすオブジェクトを返すiterator()を持っている
2. 関数、もしくは拡張関数に要素を返すnext()とBooleanを返すhasNext()を持っている
3. これら3つの関数がoperatorとしてマークされていること  

--

1もしくは2の条件と3の条件を満たしたオブジェクトは


    for (変数: 型名 in オブジェクト) {//左辺の型名は省略可
       //処理
    } //処理が一つの場合は{}を省略できます

この形でfor文を使用できます

--

RangeはIteratorを継承したProgressionIterator系の  
オブジェクトをiterator()で返すのでfor文に用いることが可能です

--

例

    for(i in 1..5) print(i) // 12345
    for(i in 5 downTo 1) print(i)//54321
    for(i in 10 downTo 1 step 2) print(i)//108642
    for(i in 1..10 step 2) print(i)//13579

---

### while文

--

Javaと一緒  
条件が真の間ループを続ける  

    var hoge = 1
    while (hoge <= 4){
        println(hoge)
        hoge++
    }// 1 2 3 4
    
---

### do-while文

--

Javaと微妙に違います

--

果たして！！その違いとは！？

--

Javaと違ってdoブロック内で宣言をした変数にwhile()の条件式の部分からアクセスできます

--

例

    do{
        val hoge = 1
    } while(hoge > 1)

こんな感じ

--

__地味にありがたい...__

---

### 構造ジャンプ演算子

--

kotlinの構造ジャンプ演算子

- return
    * 最も内側の関数をリターンする
- continue
    * 最も近い外側のループの次のステップに進む
- break
    * 最も近い外側のループを終了する

--

Kotlinではラベルを用いてこれらの演算子を使用したループ(関数)が属するスコープの任意のループ(returnならば関数)に対して
演算子の効果を発揮させることができます  
(にほんごむずかしい)

--

例を見るのが早いですよね！！！

--

例
forループの場合

    hoge@ //ラベル
    for (i in 1..10) {//ループ1
        print("i = $i\n")
        for (j in 1..10) {//ループ2
            print("$j ")
            if (j == 5 && i == 3) {
                break@hoge //hogeラベルの付いたループに対してbreak
            }
        }
        print("\n")
    }

--

実行結果
    
    i = 1
    1 2 3 4 5 6 7 8 9 10 
    i = 2
    1 2 3 4 5 6 7 8 9 10 
    i = 3
    1 2 3 4 5 

--

ラベルを付与する場合は ラベル名@  
ラベルを指定して演算子を用いる場合は 演算子@ラベル名

--

__ラベルという目印のおかげでどのループに対して実行しているのかわかりやすい!!__

--

関数のリターンは後述する関数のところで


---

## クラスとオブジェクトについて

---

### クラスと継承

---

### クラス

--

Kotlinのクラスは以下の要素で構成されます


- 初期化ブロック

- コンストラクタ

- プロパティ

- 関数

- 内部クラス

- コンパニオンオブジェクト

--

Kotlinではクラス宣言にはclassキーワードを用います

例

    class Empty {
    }

--

更にクラスのbodyがない場合{}を省略できます

    class Empty  //最も短いクラス宣言

---

### コンストラクタ

--

### プライマリコンストラクタ

--

Kotlinではコンストラクタを定義する際にはconstructorというキーワードを用います  

--

クラス名の直後に定義されるコンストラクタを特にプライマリコンストラクタと呼びます

--

プライマリコンストラクタの例
   
    class Person constructor(name: String) {
    
    }

constructor()の後の{}はクラスのボディです  

コンストラクタの処理ブロックではありません

--

プライマリコンストラクタにはコードを  
含めることはできません  
コードを含めたい場合、代わりに初期化子ブロックを用います  
初期化子ブロックの先頭にはinitキーワードを付与します

--

例
 
    class Person constructor(name: String) {
         init { //コンストラクタに渡されるパラメータを出力
             println(name)
         }
    }

--

プライマリコンストラクタに可視性を指定しない場合  
constructorキーワードは省略できます


    class Person(name: String){
        init {
           println(name)
        }
    }
    
簡潔になりました

--

更にプライマリコンストラクタに  
渡されたパラメータは  
初期化ブロックでそのまま  
使用することができます

    class Person(firstName: String, lastName: String){
        val fullName = "$lastName $firstName" // プロパティ fullNameを初期化
    }

--

更に更に！プライマリコンストラクタに  
渡された値を  
そのままクラスのプロパティとして  
持つことができます

    class Person(val firstName: String, 
                 val lastName: String, 
                 var age: Int)
    //読み取り専用のプロパティとしてfirstNameとlastNameを持つ
    //書き換え可能プロパティとしてageを持つ
    
ただ渡されたデータを保持するだけのような  
クラスを定義する際に便利

--

プライマリコンストラクタは必ずしも定義する必要はありません  

--

### セカンダリコンストラクタ

--

プライマリコンストラクタ以外のコンストラクタです

--

セカンダリコンストラクタはプライマリコンストラクタが定義されている場合、
直接または間接的に  
処理を委任しなければなりません  

--

他のコンストラクタへの処理の委任は

constructor(仮引数...): this(実引数...){
}

と書きます

--

セカンダリコンストラクタを定義する場合はクラスのボディ内にコンストラクタを定義します


--

例
   
    class Person(firstName: String){
        init {
            println(firstName)
        }
              
        constructor(firstName: String, lastName: String):
        this(firstName){
            println(lastName)
        }
    }
--

コンストラクタの実行順は、委任先のコンストラクタが先に実行されます 
   
先の例のセカンダリコンストラクタを実行すると、firstNameが先に出力されlastNameが後に出力されます

--

なにもコンストラクタを定義しない場合、引数のないpublicなプライマリコンストラクタが自動的に生成されます

---

### 継承

--

Kotlinでの継承に関してのルール

--

子クラスは必ず親クラスのコンストラクタのいずれかを実行しなけばなりません  

--

親クラスには一部を除いて必ずopenアノテーションが付与されている必要があります

--

class クラス名 : 継承元クラス名  
この形で継承を実現できます

--

例

    open class Base(s: String)
    
    class Derived : Base {
        constructor(s: String): super(s) {
        
        }
    }

--

子クラスのプライマリコンストラクタで親のコンストラクタを実行したい場合は  

class クラス名(引数...) : 継承元クラス名(実引数...)  
とします

--

例

    open class Base(s: String)
    
    class Derived(s:String):Base(s)

--

コンストラクタの実行順は親が先、子が後となります

---

__抽象クラス__

--

後述するプロパティや関数を抽象宣言することが可能なクラス  
抽象クラスは継承されることで初めてインスタンスを生成することが可能になります

--

抽象クラス、プロパティ、関数の宣言には、  
abstractアノテーションを使います
  
  
例
    
    abstract class Hoge() {
        abstract val hoge: String
        abstract fun hoge()
    }
    

--

抽象クラスは継承されて初めてインスタンスの  
生成が可能になる  
つまり継承前提のクラスなのでopenアノテーションを  
付与しなくても継承できます

---

__インターフェース__

--

どのようなプロパティ、関数を持っているのかを定義するものです
インターフェースは抽象的な関数とプロパティを持つことができます  
インターフェースはインターフェース、またはクラスに継承して使用します

--

抽象クラスとの違いは、インターフェースは実態を持てないことです  
また、インターフェースはデフォルトの実装を持つことができます  
宣言にはinterfaceキーワードを使用します

--

例

    interface Hoge {
        val hoge: String
            get() = "hoge"//デフォルトの実装を持てるが実態は
                          //持てないため代入はできない
        fun fuga()
    
        fun piyo() {
            println(hoge)
        }
    }

--

インターフェースの要素はすべてpublicとなります

---
### プロパティ

--

前述したとおり、Kotlinのクラスはフィールドを持たず、代わりにプロパティを持ちます

--

プロパティって？

--

Javaで例えるならば、フィールドとアクセサ(getter/setter)を組合わせたような物です

--

プロパティを定義するにはプライマリコンストラクタの節で示したようにするか  
クラスのボディ内で定義する必要があります

--

プロパティの定義は

(可視性とか) (var or val) (プロパティ名) :(型) = (初期化式) 

と書きます

変数と同様に型推論が可能な場合に型名を省略できます

--

例

    //ほぼ使い回し
    class Person(firstName: String, lastName: String){
       //プロパティ fullNameを初期化
       private val fullName = "$lastName $firstName" 
    }

もちろん、Javaのフィールドの様にコンストラクタ内での初期化も可能です

--

### アクセサ

--

プロパティにはgetter/setterを定義可能です  

そして、このアクセサにも可視性のアノテーションを付与することが可能です  

--

アクセサはプロパティの定義の下に   

(可視性) get(){処理}  
(可視性) set(仮引数){処理}

このような形で定義します

--

getterの処理は最後にそのプロパティの型のオブジェクトを  
setterは最後にUnitを返す必要があります

--

アクセサ内で実行する処理が一つの場合は  

(可視性) get() = 処理  
(可視性) set(仮引数) = 処理

と書くことができます

--

アクセサの可視性だけ指定したくて処理は特にいらない場合は  

プロパティの定義  
(可視性) get  
(可視性) set

このように書けます

--

もちろんvalで定義されたプロパティはsetterを持つことができません

--

例

    val hoge: String //fugaを取得する読み取り専用プロパティ
        get() {
            return fuga
        }

    var fuga = "fuga"
        set(value){
            println(value)
            field = value//後述します
        }


--

アクセサからプロパティにアクセスするためにはバッキングフィールドを用います  
Kotlinはプロパティにバッキングフィールドが必要な場合、自動的にそれを生成します  

--

アクセサ内から単にプロパティ名を指定してプロパティにアクセスしようとすると  
再度アクセサを経由することになり再帰によるオーバーフローが起こりえます  

--

例

    //マイナスの値を受け付けないInt型プロパティ
    var fuga = 1
        set(value){
            if(value >= 0){
                field = value
            }
        }

バッキングフィールドによりアクセサ内から直接プロパティにアクセスすることができます

バッキングフィールドにアクセスするにはfieldを用います  

---

### 関数

--

kotlinの関数は必ず値を返さなければなりません  

目立って返す値がない場合にはUnitを返すようにします

--

関数を定義するにはfunctionの略のfunキーワードを用います

--

例

    fun hoge(): Unit{
    
    }

(可視性) fun (関数名)(仮引数....): 返り値の型 {処理}  
の形で宣言をします

--

Unitを返す場合に限り、戻り値の型は省略することができます

--

これは最もシンプルな関数定義の例です

    fun simple(){
    }

--

引数を与える場合はこんな感じ  

    fun simple(hoge: Hoge){
    }


--

また引数にはデフォルトの値を持たせる事ができます  

    fun simple(fuga: Fuga,hoge: Hoge = Hoge()){
    }
    
    //呼び出し
    simple(Fuga())

ただし引数の順序などの条件によっては名前付き引数を用いる必要が出てきます  

--

    fun simple(fuga: Fuga = Fuga(),hoge: Hoge){
    }
    
    //呼び出し
    simple(hoge = Hoge())

名前付き引数自体はいつでも使えます  
可読性等を考慮して使う必要がある場合にも使いましょう  

--

また可変数引数も使用可能です

    fun simple(vararg strings: String) {
        strings.forEach(::println)//可変数引数は配列として扱う
    }
    
    simple("1","12","123")
    simple(strings = *arrayOf("a","aa","aaaa"))//名前付き引数を使いたければこんな感じ

--

序盤に言ったように、関数は必ずしもクラスに属する必要はありません

トップレベルに宣言をしてそれをグローバルに用いることができます

--

また、トップレベルに関数を定義した際にはprotected以外の可視性を指定できます  

例えばprivateを指定すれば、関数を定義したファイル以外からその関数を視ることができなくなります

--

関数をクラス内で定義した場合、それはメンバ関数(いわゆるメソッド)となります  

メンバ関数へはそのクラスのインスタンスからアクセスします

--

例

    class Hoge{
        fun hoge(){
            println("ほげらりをん")
        }
    }

----

使用時

    val hoge = Hoge()
    hoge.hoge()

--

### 単一式関数

--

Kotlinには関数を宣言的に書くための仕組みとして、単一式関数と言う糖衣構文があります

簡単に言えば人間にとって、より直感的でより理解しやすい構文です

これにより関数を宣言的に書くことができます

--

例

通常の関数定義

    fun hoge(a: Boolean, b: Boolean): Boolean {
        return a && b
    }

----

上記関数を単一式関数に変換

    fun hoge(a:Boolean, b: Boolean): Boolean = a && b

(可視性) fun (関数名)(仮引数...): (返り値の型) = (単一の式)  
のように書くことで定義できます  

--

また、単一式関数は返り値の型を推論可能なのでそれを省略できます

例

    fun hoge(a:Boolean, b: Boolean) = a && b

更に簡潔になりました

--

### 高階関数

--

kotlinは高階関数を持てます

--

高階関数って何...？

--

値として受け渡したりできる関数のことを指します

--

ざっくり言ってしまえばオブジェクトとしての関数です

--

ここでは前述した関数を宣言された関数、高階関数は関数オブジェクトと呼ぶことにします

--

宣言された関数はそれをそのままオブジェクトとして扱うことはできません  

--

関数オブジェクトを作るには

    val function = fun(s: String) {
        println(s)
    }

このように通常の関数宣言から関数名を抜いたものを変数に代入するように記述します

ちなみにこの変数の型は (String) -> Unit 型です  
String型の引数を一つ受け取りUnitを返す関数型  
という事になります  

もう一つ、書き方があります  

--

例

    val function: (String) -> Int = {str ->
       str.toInt()
    }

この{}の部分を関数リテラルと呼びます

{ 仮引数... -> 処理 }  
という形で書きます

この例ではfunctionは引数にString型を受け取りIntを返す関数型のオブジェクトです  
基本的にreturnは使用できず、一番最後の処理で返る値が評価され、そのまま返り値になります  

--

関数リテラルは引数が一つの場合にそれを省略し、itで参照することができます

--

例

     val function: (String) -> Int = {
           it.toInt()
        }

--

関数オブジェクトを実行

--

関数オブジェクトを実行するにはinvoke()メソッドを使用します

例

    val function: (String) -> Int = {
               it.length
            }
    
    function.invoke("ほげ")

--

またinvoke()は省略することが可能です

    val function: (String) -> Int = {
               it.length
            }
    
    function("ほげ")

これにより直感的に関数オブジェクトを実行できる 

--

### 関数オブジェクトを関数に渡す

例

    fun hoge(function: (String) -> Unit) {
        function("hoge")
    }

    //使用
    hoge( {
        println(it)
    })

--

関数に関数リテラルを一つだけ渡す場合、()を省略できます

    fun hoge(function: (String) -> Unit) {
        function("hoge")
    }

    hoge {
        println(it)
    }

--

また関数、またはコンストラクタに渡す最後の引数が関数オブジェクトだった場合  
このような書き方ができます

    fun hoge(s: String, func: () -> Unit) {
        //特に何もしない
    }
    
    //使用
    hoge("hoge") {
        println("s")
    }

--

ここで突然、構造ジャンプ演算子 returnについて

--

関数に関数リテラルを渡すとき、その関数はネストしていますよね？

--

なので、ラベルを用いて終了する関数を指定することができます

--

ここではKotlinのArrayクラスのforEachを用いて例を示します

例

    //適当な値の入ったArray<Int>なオブジェクト
    val intArray = arrayOf(1, 2, 3, 4, 5, 6, 7, 8)
    
    intArray.forEach each@ {
            if (it == 5) {
                return@each //関数 forEachを終了
            }
        }

--

この場合ラベル名の宣言は省略可能です   
その場合のラベル名は関数名になります

--

例
    
    intArray.forEach {
        if (it == 5) {
            return@forEach
        }
    }


強い


---

コンパニオンオブジェクト

--

Kotlinにはstaticなメンバは存在しません

--

その代替手段としてクラス共通の  
オブジェクトとなる  
コンパニオンオブジェクトを使用します

--

コンパニオンオブジェクトはクラスのボディ内で宣言します

--

例 

    class Hoge {
   
        companion object {
             val HOGE = "HOGE"
             
             fun hoge(){
             }
       }
    }

--

コンパニオンオブジェクトには全ての可視性のアノテーションを付与することができます

ですがコンパニオンオブジェクト内のメンバにはprotectedのみ付与することができません

---

### Null安全

--

KotlinではNull許容型と非Null許容型は明確に区別されます

Null許容型には 末尾に? を付与します

--

例

    var hoge: Strung = null //これはダメ
    var fuga: String? = null //行ける

--

関数やコンストラクタの引数、関数の返り値にも指定可能

    fun hoge(s:String?):Int?{
        //省略
    }

--

null許容型オブジェクトのメンバは安全に呼び出すことができます  
これを安全呼び出しといいます
安全呼び出しには . の代わりに ?. や !!. を使用します

--

例

       val hoge = Hoge()
       
       hoge?.hoge //hogeがNullだった場合Nullが返る
       hoge!!.hoge //hogeがNullだった場合ぬるぽ！

---

### エルビス演算子 5：-)

左辺がnullなら右辺を評価し  
左辺が非nullならば左辺を評価する演算子 

    
    var hoge: Hoge? = null
    var fuga = hoge ?: Hoge()//右辺が評価
    
    fun hoge(hoge: Hoge?) {
        val hoge = hoge ?: return//入ってきた値がnullならおわる
    }


---

他にもdata class とかenumとか移譲とか拡張関数とかいろいろあるけど眠かった  

    fun String.toDate():Date {
        //なにかしらの方法でDateオブジェクトに変換する処理
    }
    
    "2018/04/04".toDate()


---
# Javaオブジェクト指向プログラミング  

# 第1章：クラスとインスタンス  

- クラス：オブジェクトの定義
    - クラス名、属性、操作で構成される
    - 属性：データ(クラス内で定義された変
    - 操作：属性の値を使って行われる処理(クラス内で定義されたメソッド)

- インスタンス(≒オブジェクト)：クラスから生成されたオブジェクト  
    - クラスで定義されている属性(データ)に値を代入したもの  

```java
class Employee{
    // フィールド
    int employeeID;    // 社員番号
    String name        // 氏名
    int salary;        // 基本給
    double multiplier; // ボーナスの基準月数

    // コンストラクタ
    Employee(int id. String na, int sal, double mu){
        employeeID = id;
        name = na;
        salaly = sal;
        multiplier = mu;
    }
    // コンストラクタ
    Employee(){} // 引数のないコンストラクタ（オーバロード）

    // メソッド
    void work(){
        System.out.println(name + "は働きました")
        // フィールドで定義した属性を使える
    }
    void calcSalary(){
        return salary
    }
}
```

## インスタンスの生成  
```java
class Company{
    public static void main(String[] args){
        // 引数の無いコンストラクタでインスタンスの作成
        Employee Yamada = new Employee() // インスタンス化
        // クラス名:Employee
        // 変数名:taro
        // コンストラクタ（クラス名と同じ）
        // フィールドの利用
        taro.name = "山田";
        System.out.println(name);

        // メソッドの利用
        taro.work();
        int answer = taro.calcSalary();

        // 引数のあるコンストラクタでインスタンスの作成
        Employee Yoshida = new Employee(200, "吉田", 40000, 2.0) // インスタンス化
    }    
}
```
- フィールドのデフォルト値  
    - 数値型は0
    - boolean型はfalse
    - char型は'\u0000'
    - 参照型はnull 

## コンストラクタ
- コンストラクタ:インスタンス生成時に実行される特別なメソッド  
    - **フィールドの初期化**
    - 名前はクラス名と同じ
    - 戻り値は指定できない 

## thisとthis()
- this:コンストラクタ作成の際、フィールド変数をそのまま使いたいときに便利  
    - this.フィールド変数名  
- this()：コンストラクタで定義された属性値をコンストラクタに持ってくることができる  
    - this()
    - this(引数1,引数2):引数のあるコンストラクタも呼べる

```java
class Employee{
    // フィールド
    int employeeID;    // 社員番号
    String name        // 氏名
    int salary;        // 基本給
    double multiplier; // ボーナスの基準月数

    // コンストラクタ
    Employee(){
        this.employeeID = 12345;
        this.name = "未登録";
        this.salaly = 330000;
        this.multiplier = 200000;
    }

    // コンストラクタ
    Employee(int id. String na, int sal, double mu){
        this.employeeID = EmployeeID;
        this.name = name;
        this.salaly = salaly;
        this.multiplier = multiplier;
    }

        // コンストラクタ
    Employee(int employeeId. String name){
        this(); // 引数のないコンストラクタの属性値がコピーされる

        // 追加で変更したい部分だけ変更  
        this.employeeID = employeeID; 
        this.name = name;
    }

    // メソッド
    void work(){
        System.out.println(name + "は働きました")
        // フィールドで定義した属性を使える
    }
    void calcSalary(){
        return salary
    }
}
```

## 静的メンバ  
- 静的メンバ：クラス内で共有したいフィールド値にstaticをつける  
    - 同一クラスで生成されたインスタンスで共有される  
    - クラス名.クラス変数
    - クラスメソッド(static ~())内でフィールド変数は扱えない(this.変数など)
- **インスタンスを生成していない際に、クラス内の変数やメソッドにアクセスしたい場合に使用**

```java
class Person{
    // フィールド
    String name;        // 氏名
    static int population=0; // 人口（インスタンス生成された回数）

    // コンストラクタ
    Person(){
        this.name = "未登録";
        Person.population++; // インスタンス生成時に1加算
    }

    // コンストラクタ
    Person(String name){
        this(); // population++を引き継ぐ
        this.name = name;
    }

    // メソッド
    void printName(){
        System.out.println(this.name); 
    }

    // スタティックメソッド(インスタンス未生成時にmainメソッドから呼び出し参照できる)
    static int getPopulation(){
        return Person.population; // スタティック変数へのアクセス(クラス名.変数)
    }
}
```

# パッケージとJava API  

##  パッケージとCLASSPATH  
- パッケージ：クラスを整理する仕組み
    - package パッケージ名;
    - import パッケージ名.クラス名;
    - パッケージ名の命名規則：jp.co.example（ドメインの逆順）
- CLASSPATH：クラス・パッケージを探す場所を指定  
    - CLATHPATH=フォルダパス1;フォルダパス2;

---

### package使用例
Employeeクラスを「app.staff」パッケージ内にある場合  
```java
package app.staff;
// workspace/app.staff/Employee.java
public class Employee{

}
```
  
異なるパッケージのクラスを利用する際は以下のように記述  
```java
class Company{
    public static void main (String[] args){
        app.staff.Employee Yamada = new app.staff.Employee();
    }
}
```

### インポート使用例
importを使用するとクラス名のみでインスタンス生成ができる  
```java
import app.staff.Employee;
class Company{
    public static void main (String[] args){
        Employee Yamada = new Employee();
    }
}
```
---
## Java API  

- Java API：[公式ドキュメント](https://docs.oracle.com/javase/jp/11/docs/api/index.html)

## Stringクラス
Stringの文字列比較の注意点  
```java
String str1 = "Hello";
String str2 = str1;
String new String("Hello");

System.out.println(str1==str2); // true
System.out.println(str1==str3); // false
// アドレス位置で比較している

System.out.println(str1.equals(str2)); // true
System.out.println(str1.equals(str3)); // true
// 文字列で比較している

```
# 第3章：カプセル化  
- カプセル化：オブジェクトの内部構造を秘匿
    - フィールドへの不正な値の代入を防ぐ
    - オブジェクトの独立性を高める
    - オブジェクトの再利用性を高める    
    - 保守性の高いプログラムを作ることができる  

## フィールドのカプセル化  
フィールドの値をカプセル化する場合はprivateを使用する
```java
public class Employee{
    static final double MULTIPLIER = 2.0;
    private static int number = 0;
    private int employeeID;
    private String name;
    private int salary;
}

class Company{
    public static void main (String[] args){
        Employee Yoshida = new Employee();
        Yoshida.salary = 5;  // privateでカプセル化されているのでコンパイルエラー
    }
}
```
## アクセス用メソッド
getメソッド：フィールドの値を取得するメソッド
```java
フィールドのデータ型 get フィールド名(){
    return this.フィールド名
}
```
setメソッド：フィールドの値を設定するメソッド
```java
void set フィールド名(フィールドのデータ型 引数名){
    // 入力値チェック
    if (条件式) {
        this.フィールド名=引数名;
    }
}
```
---

getメソッド・setメソッドの使用例  
```java
public class Employee{
    static final double MULTIPLIER = 2.0;
    private static int number = 0;
    private int employeeID;
    private String name;
    private int salary;

    int getEmployeeID(){
        return this.employeeID;
    }

    void setName(String name){
        if (name != null && name.length() > 0){
            // null文字は設定できないように
            this.name = name;
        }
    }
}

class Company{
    public static void main (String[] args){
        Employee Yoshida = new Employee();
        Yoshida.SetName("吉田");; // setメソッドを使って名前を設定  
    }
}
```

## メソッドのカプセル化
メソッドをカプセル化することでオブジェクト内部でのみメソッドを使用できるようにする
```java
public class Employee{
    static final double MULTIPLIER = 2.0;
    private static int number = 0;
    private int employeeID;
    private String name;
    private int salary;

    int getEmployeeID(){
        return this.employeeID;
    }

    void setName(String name){
        if (name != null && name.length() > 0){
            // null文字は設定できないように
            this.name = name;
            printString(this.name);
        }
    }
    private printString(String message){  // オブジェクト内部でのみ利用できる
        System.out.println(message)
    }
}
```
## クラスのカプセル化
- クラスのカプセル化
    - public クラス名：別のパッケージのクラスから参照できるようにする
    - クラス名：同じパッケージ内のみアクセス可能
```java
public class Employee{

}
```
### アクセス制御
|公開度合い|修飾子|説明|
|---|---|---|
|高|public|どこからでもアクセス可能|
|高|protected|同じパッケージ内とサブクラスからアクセス可能|
|低|無指定|同じパッケージ内からアクセス可能|
|低|private|同じクラス内のメンバからアクセス可能|

# 第4章：継承の基本  
- 継承：あるクラスが持つ属性と操作を他のクラスが受け継ぐこと(is-a関係)・親と子は1対多の関係  
    - スーパークラス：受け継ぎ元のクラス
    - サブクラス：受け継ぎ先のクラス
    - コンストラクタは受け継がない

---

スーパークラスとしてのEmployeeクラス
```java
public class Employee{
    static final double MULTIPLIER = 2.0;
    private static int number = 0;
    private int employeeID;
    private String name;
    private int salary;

    // コンストラクタ
    public Employee(){
        this.employeeID = 12345;
        this.name = "未登録";
        this.salaly = 330000;
        this.multiplier = 200000;
    }

    // コンストラクタ
    public Employee(int id. String na, int sal, double mu){
        this.employeeID = EmployeeID;
        this.name = name;
        this.salaly = salaly;
        this.multiplier = multiplier;
    }

    void setName(String name){
        if (name != null && name.length() > 0){
            // null文字は設定できないように
            this.name = name;
            printString(this.name);
        }
    }
}
```
サブクラスとしてのEngineerクラス
```java
public class Engineer extends Employee{

    private int overtime;

    public void setOvertime(int overtime){
        if (overtime >= 0){
            this.overtime = overtime;
        }
    }
}
```
  
メインメソッド
```java
class Company{
    public static void main (String[] args){
        Engineer Yoshida = new Engineer();

        Yoshida.SetName("吉田"); // スーパークラスのsetメソッドを使って名前を設定
        
        Yoshida.setOvertime(50); // サブクラスで定義したsetメソッドを使って残業時間を設定
        
    }
}
```

## 継承時のコンストラクタの動作
- サブクラスでインスタンスを生成する際、スーパークラスの引数なしコンストラクタ・デフォルトコンストラクタが先に呼び出される
```java
class SuperClass{
    SuperClass() {
        System.out.println("Super Class constructor");
    }
}

class SubClass extends SuperClass{
    SubClass() {
        // ここでスーパークラスの引数なしのコンストラクタが呼び出される
        System.out.println("Sub Class constructor");
    }
}

class Main{
    public static void main (String[] args){
        SubClass sub = new Subclass();
        // Super Class constructor
        // Sub Class constructor
    }
}
```
メインメソッドを実行するとスーパークラスの**引数なしの**コンストラクタが先に呼び出される
```console 
 $ Super Class constructor
 $ Sub Class constructor
```
---
- super(引数)：スーパークラスに引数なしのコンストラクタが無い際に、サブクラスのコンストラクタ内でスーパークラスの引数なしコンストラクタの呼び出しができる
    - サブクラスのコンストラクタの1行目に記述
    - 引数を指定すると、引数のあるスーパークラスのコンストラクタが呼び出される
    - super()の記載がないときはコンパイラによってsuper()が自動で追加される
```java
class SuperClass{
    String str;

    //SuperClass() {
    //    System.out.println("Super Class constructor");
    //}
    
    SuperClass(String str) {
        System.out.println("Super Class constructor" + str);
        this.str = str;
    }
}

class SubClass extends SuperClass{
    SubClass() {
        super("明示的にスーパークラスのコンストラクタを呼び出す");
        System.out.println("Sub Class constructor");
    }
}


class Main{
    public static void main (String[] args){
        SubClass sub = new SubClass("Sample");
        System.out.println(sub.str);
        // Super Class constructor
        // Sub Class constructor
    }
}
```
```console
 $ Super Class constructor Sample 明示的にスーパークラスのコンストラクタを呼び出す
 $ Sub constructor
 $ Sample
```

## オーバライド
- オーバライド：サブクラスで継承したメソッドを再定義して処理を書き換える
    - オーバーライドしたメソッドの優先順位が高くなる
- 条件：スーパークラスのメソッドと以下を同じにする
    - 戻り値の型
    - メソッド名
    - 引数とデータ型
    - スーパークラスのメソッドとアクセス制御が同じか緩い  
---
```java
public class Employee{
    private int salary;

    public int calcSalary(){ // 親クラスのメソッド
        return this.salary; 
    }
}
```
サブクラスとしてのEngineerクラス
```java
public class Engineer extends Employee{

    public int calcSalary(){ // オーバーライドで書き換え
        return super.calcSalary + 2000;
        /* 
        オーバーライドで上書きしたメソッドの優先順位が高い
        そのため、スーパークラスのcalcSalaryを呼ぶ際は
        super.calcSalary()として呼び出す
        */
    }
}
```
## クラスに対するfinal(参考)  
- 継承させたくないクラス、オーバーライドさせたくないメソッドにfinalを付与
    - finalクラス：アクセス修飾子 final class クラス名{ }
    - finalメソッド：アクセス修飾子 final 戻り値のデータ型 メソッド名(引数)

# 第5章：継承の応用  
## 抽象クラス
- 抽象クラス：サブクラスの使用を決定するためのクラス
    - サブクラスにオーバーライドを強要
    - abstructキーワード
    - **メソッドの処理を持たない**
        - 修飾子 abstract 戻り値の型 メソッド名(); 
    - **インスタンスの生成ができない**
    - 継承元のクラスにabstractを付ける・継承先の雛形となる
---
継承元にabstructを付けて、サブクラスにオーバーライドを強要
```java
public abstract class Employee{ // 抽象クラス
    private int salary;

    public void getSalary(){
        this.salary = salary
    }
    public int setSalary(int salary){
        if(salary>=150000){
        this.salary=salary;
        }
    }
    public abstract int calcSalary(); // 抽象メソッド・処理なし
}
```
サブクラスとしてのEngineerクラス  
抽象クラス内で抽象化されたメソッド「calcSalary」をオーバーライドで書き換え
```java
public class Engineer extends Employee{

    public int calcSalary(){ // オーバーライドで書き換え
        return getSalary() + 2000;
    }
}
```
メインメソッド  
抽象クラス「Employee」はインスタンス生成できない  
抽象クラスを継承したEngineerクラスはインスタンス生成できる
```java
class Main{
    public static void main (String[] args){
        // Employee Yoshida = new Employee(); 
        // コンパイルエラー（abstractクラスであるため）
        Engineer Yoshida = new Engineer();
        Yoshida.setSalary(200000);
        // 抽象クラスを継承したサブクラスはインスタンス生成できる
    }
}
```
---
## インターフェース  
- インターフェース：抽象メソッドだけをもつ抽象クラス
    - 修飾子 interface インターフェース名 { }
    - メソッド宣言すると、暗黙的にpublic abstractとなる
    - 変数宣言すると、暗黙的にpublic static finalとなる
---
Playerインタフェース
```java
public interface Player{
    void play(); // 再生
    void stop(); // 停止
    void fastForward(); // 早送り
    void rewind(); // 巻き戻し
}
```
Playerインタフェースを実装したDVDPlayerクラス
```java
public class DVDPlayer implements Player{
    public void play(){
        System.out.println("再生");
    }
    public void stop(){
        System.out.println("停止");
    }
    public void fastForward(){
        System.out.println("早送り");
    }
    public void rewind(){
        System.out.println("巻き戻し");
    }
}
```
メインメソッド
```java
public class User{
    public static void main (String[] args){
        DVDPlayer player = new DVDPlayer();
        player.play();
        player.stop();
        player.fastForward();
        player.rewind();
    }
}
```
---
継承とインタフェースは併用できる  
インタフェースは複数使える
```java
public class SubClass extends SuperClass implements interface1, interface2{

}
```
## オブジェクトクラス

# 第6章：ポリモーフィズム  

## ポリモーフィズムとは
- ポリモーフィズム：同じメッセージを送ってもインスタンスによって異なる動きをする性質  
    - インスタンス利用者はインスタンスの動作原理・実装を知る必要はない  
    - インスタンス利用者の使用性の向上  

## ポリモーフィズムの実装
```java
Enployee emp = new Engineer();   // 親の型 = new 子クラス();
Player player = new DVDPlayer(); // インタフェースの型 = new 子クラス();

Engineer emp = new Enployee();   // 逆は無理
DVDPlayer player = new Player(); // 逆は無理
```
---
親「Employee」を継承した２つの子クラス  
```java
public abstract class Employee{
    public abstract void work();
}

class Engineer extends Employee{
    public void work(){
        System.out.println("設計書を作成しました");
    }

    public void doTest(){
        System.out.println("テストを実施しました");
    }
}

class Sales extends Employee{
    public void work(){
        System.out.println("客先に行きました");
    }
}
```
**複数種類のサブクラスのインスタンスをスーパークラス型としてまとめて扱うことができる**  

見かけ上の型が親クラスになるので、**子クラス独自のメソッドは使用できない**  
```java
class Main{
    public static void main (String[] args){
        Employee[] emps = new Employee[2];
        emps[0] = new Engineer(); // Employee emps[0] = new Engineer();
        emps[1] = new Sales(); // Employee emps[0] = new Sales();

        for (Employee e : emps){
            System.out.print(e.getClass().getName() + " : ");
            e.work();
        }

        emps[0].doTest();         // コンパイルエラー
        /* 
        見かけ上の型が親クラスであり
        doTestはEngineerクラスのメソッドであるため
        */

        Engineer engineer = (Engineer)emps[0]; // ダウンキャスト
        engineer.doTest();
        ((Engineer)emps[0]).doTest(); // 直接ダウンキャストも可能

    }
}
```
実行例
```
 $ Engineer : 設計書を作成しました
 $ Sales : 客先に行きました
 $ テストを実施しました
 $ テストを実施しました
```

## 動的結合  

```java
public class Employee{
    public void work(){
        System.out.println("従業員は働きました");
    }
}

class Engineer extends Employee{
    public void work(){
        System.out.println("設計書を作成しました");
    }

    public void doTest(){
        System.out.println("テストを実施しました");
    }
}

class Programmer extends Engineer{
    public void work(){
        System.out.println("プログラムを作成しました");
    }
}
```

```java
class Main{
    public static void main (String[] args){
        Employee emp1 = new Employee();
        Employee emp2 = new Engineer();
        Employee emp3 = new Programmer();
        
        emp1.work(); // Employee: 従業員は働きました
        emp2.work(); // 設計書を作成しました
        emp3.work(); // プログラムを作成しました

        Employee emp4 = new Engineer();
        // コンパイルエラー
        emp4.doTest(); // 型のクラスにないとコンパイルエラー
        
        Engineer emp5 = new Programmer(); 
        // 実行可能
        emp5.doTest(); // 型のクラスにあると実行できる
    }
}
```
実行例
```
 $ Engineer : 従業員は働きました
 $ Sales : 設計書を作成しました
 $ プログラムを作成しました
```

# 第7章：例外処理

## エラーの種類
- 文法エラー：プログラム言語の構文に違反したことによりコンパイルが失敗する
- 実行エラー：プログラム実行途中に問題が生じ、プログラムが途中で強制終了する
    - ゼロ割
    - 配列のインデックスが範囲外
    - 型変換不可能
    - 存在しないファイルを開く
- 論理エラー：プログラムは最後まで動作するが予想した結果と異なる  

## 例外処理
- 例外処理：例外発生時の強制終了を防ぐために記述
    - try-catch文・try-catch-final文を使う
```java
class Main {
	public static void main(String[] args) {
		try {
			int x = Integer.parseInt(args[0]);
			int y = Integer.parseInt(args[1]);

			System.out.println("x / y : " + x / y);
		} catch (ArithmeticException | ArrayIndexOutOfBoundsException | NumberFormatException e) {
			System.out.println(e + "が発生");
		}
		System.out.println("正常終了");
	}
}
```

## 例外の種類
- Throwable：最も上位の例外
    - Exception：ソフトウェア系のエラー・チェック例外
        - RuntimeExeption：プログラム上のエラー・非チェック例外
    - Error：ハードウェア系のエラー（CPUなど）・非チェック例外

- **Exception系例外はtry-catch必須の例外**

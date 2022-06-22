# Javaシステムプログラミング  

# 付録A
## 書式付出力
- 書式付出力：桁数指定・右寄せといった書式を指定した表示
- System.out.printfメソッド  
    - System.out.printf(書式付き文字列, 引数);

変換形式の種類  
|変換形式|説明|
|---|---|
|%d|10進数の整数として表示|
|%f|10進数の浮動小数点数として表示|
|%s|文字列を表示|
|%t|日付・時刻を表示|

```java
class Company{
    public static void main(String[] args){
        int a=10,b=20;
        System.out.printf("%d + %d：%d \n", a,b,a+b);
        System.out.printf("%3$d + %2$d：%1$d \n", a+b,a,b);
        System.out.printf("%d + %<d：%d \n", a,a+a);
    }    
}
```
実行結果
```bash
10 + 20：30 
20 + 10：30 
10 + 10：20 
```
---
- 整数の書式指定  
    - %[引数の順番$][-+ 0,(][最小文字数]d

|フラグ|説明|
|---|---|
|-|左詰め|
|+|常に符号を含める|
| |正値の時先頭に空白|
|0|先頭部分に0を追加|
|,|桁の区切りを含める|
|(|負の数値を（）で囲む|

```java
class Company{
    public static void main(String[] args){
        int i = 12345;
        System.out.printf("%-10d%n",i); // 左詰め・10文字
        System.out.printf("%10d%n",i);  // 右詰め・10文字
        System.out.printf("%,10d%n",i); // カンマ・10文字
        System.out.printf("% ,10d%n",i);
        System.out.printf("%+,10d%n",i);
        System.out.printf("%(,10d%n",i);
        System.out.printf("%(,10d%n",i*-1); // 負の値を（）で囲む
    }    
}
```
実行結果
```bash
12345     
     12345
    12,345
    12,345
   +12,345
    12,345
  (12,345)
```
---
- 少数の書式指定  
    - %[引数の順番$][-+ 0,(][最小文字数][.小数点以下の桁数]f

```java
class Company{
    public static void main(String[] args){
        System.out.printf("%15f \n",Math.PI);
        System.out.printf("%15.10f \n",Math.PI);
        System.out.printf("%15.3f \n",Math.PI);
    }    
}
```
実行結果
```bash
       3.141593 
   3.1415926536 
          3.142 
```
---
- 文字列の書式指定  
    - %[引数の順番$][-][最小文字数][.最大文字数]s

```java
class Company{
    public static void main(String[] args){
        System.out.printf("%5s \n","AB");
        System.out.printf("%5s \n","ABCDEFG");
        System.out.printf("%5.5s \n","ABCDEFG");
    }    
}
```
実行結果
```bash
   AB 
ABCDEFG 
ABCDE 
```

# 第1章：ストリーム
- ストリーム：Javaと他の機器との間でのデータの送受信を連続的に行う仕組み
    - java.ioパッケージが必要
 - ストリームの使用手順  
    1. ストリームの**インスタンス生成**  
    2. データの読み込み・書き込み  
    3. **ストリームを閉じる(closeメソッド)**  
---

読み込む「from.txt」  
```txt
1001,世界太郎,30
1002,世界次郎,24
1003,世界三郎,22
```
FileReaderを使ったファイルの読み込み  
- コンストラクタ
    - FileReader：引数で指定したテキストファイルから読み込むためのストリームを生成する
- メソッド
    - int read()：１文字読み込む・文字コードを**数値で返す**・読み込み終了の際は-1を返す
    - void close()：ストリームを閉じる
```java
import java.io.FileReader;

public class FileRead1 {
    public static void main(String[] args) throws Exception { // 本来はtry-catchをする
        String from = "from.txt";

        FileReader fr = new FileReader(from); // 1. インスタンス生成

        int i = 0;
        while ((i = fr.read()) != -1) { // 2. ファイルの読み込み・「-1」は読み込み終了時の戻り値
            char c = (char) i; // 1文字ずつ文字を読み込む
            System.out.print(c); 
         // ファイル内の改行コードを読み込むため改行される
        }
        fr.close(); // 3. クローズ
    }
}
```
---
FileWriterを使ったファイルの書き込み
- コンストラクタ
    - FileWriter：引数で指定したテキストファイルに書き込むためのストリームを生成する
- メソッド
    - void write()：１文字書き込む
    - void close()：ストリームを閉じる

```java
import java.io.FileWriter;

public class FileWrite1 {
    public static void main(String[] args) throws Exception {
        String to = "to.txt";

        FileWriter fw = new FileWriter(to);

        fw.write("1001,世界太郎,30");
        fw.write("1002,世界次郎,24");
        fw.write("1003,世界三郎,22");

        fw.close();
    }
}
```

書き出された「to.txt」  
```txt
1001,世界太郎,301002,世界次郎,241003,世界三郎,22
```

---

FileReaderとFileWriterを同時に使用した例  
```java
import java.io.FileReader;
import java.io.FileWriter;

public class FileCopy1 {

    public static void main(String[] args) throws Exception {
        String from = "from.txt";
        String to = "to.txt";

        FileReader fr = new FileReader(from); // 1.インスタンス生成
        FileWriter fw = new FileWriter(to);   // 1.インスタンス生成

        int i = 0;
        while ((i = fr.read()) != -1) { // 2.ファイル読み込み・「−１」は読み込み終了時の戻り値
            char c = (char) i;
            System.out.print(c); // 1文字ずつ出力(改行コードも読み込まれる)
            fw.write(c);  // 読み込んだ文字を「to.txt」に書き込む（クローズ後にまとめてコピー）
            // fw.flush(); // １文字ずつコピー
            sleep();
        }
        fr.close(); // 3.クローズ
        fw.close(); // 3.クローズ
    }

    private static void sleep() {
        try {
            Thread.sleep(50); // ms 
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    }
```

## ファイル入出力のバッファリング
- バッファリング：入出力データを一時的にメモリに蓄える仕組み
    - 外部機器とのアクセス回数を減らすことで処理効率を向上
    - fw.write()とfw.flush()を考えると、fw.write()ではfw.close()後にまとめて書き込むので効率が良い
---
- コンストラクタ
    - BufferedReader(Reader in)：バッファリングを行う文字型入力ストリームを生成する
    - BufferedWriter(Writer in)
- メソッド
    - String readLine()：１行読み込む・読み込み終了後は「null」を返す
    - void write(String str)：文字列strを書き込む
    - void newLine：行区切り文字を書き込む
    - void close()：ストリームを閉じる

読み込むテキストファイル  
```txt
1001,世界太郎,30
1002,世界次郎,24
1003,世界三郎,22
```
- バッファリングを利用したファイルコピー
    - 行単位でファイル内容を読み込み「,」で区切られた3つめの数値の平均値を計算し端末に表示
```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;

public class FileCopy2 {
    public static void main(String[] args) throws Exception {
        String from = "from.txt";
        String to = "to.txt";

        BufferedReader reader = new BufferedReader(new FileReader(from)); // 引数はストリーム
        BufferedWriter writer = new BufferedWriter(new FileWriter(to)); // 引数はストリーム

        String line = null;
        int num = 0;
        int totalAge = 0;

        while ((line = reader.readLine()) != null) {
            // 1. ファイルへ書き込み・読み込み終了時はnullを返す
            writer.write(line);
            writer.newLine();
            writer.flush();
            sleep();

            num++;
            // lineには「0000,name,age」のように1行格納
            String[] fields = line.split(","); // 「,で区切る」
            String name = fields[1]; // 名前
            int age = Integer.parseInt(fields[2]); // 年齢
            System.out.printf("%sさん、%3d才%n", name, age);

            totalAge += age;
        }
        double aveAge = (double) totalAge / num;
        System.out.printf("%d人の平均年齢%6.2f才", num, aveAge);

        reader.close();
        writer.close();
    }

    private static void sleep() {
        try {
            Thread.sleep(50);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    }
```
実行結果
```bash
世界太郎さん、 30才
世界次郎さん、 24才
世界三郎さん、 22才
3人の平均年齢 25.33才
```

## 例外処理
- tryブロック：Reader・Writerを用いたファイル操作
- caychブロック：例外処理
- finallyブロック：close処理

```java
import java.io.*;
public class Ex11 {
    public static void main(String[] args) {
        // TODO 自動生成されたメソッド・スタブ
        String from = "./profile.txt";
        BufferedReader reader = null;
        int i = 0; // 行番号

        try {
            reader = new BufferedReader(new FileReader(from));
            String line = null;
            while((line = reader.readLine()) != null) {
                System.out.printf("%d: %s \n", i, line); // 行番号: 文字列 で表示
                i++;
            }
        
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        
        } finally { // finallyブロックの中でclose()
            try {
                if (reader != null) {
                    reader.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }	
    }
}
```

## 標準入出力  
- 標準入力：キーボードからの入力
- 標準出力：端末上への出力

---
キーボードから標準入力された文字列を標準出力するプログラム  
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class InputOutput {
    public static void main(String[] args) {
        BufferedReader key =
            new BufferedReader(new InputStreamReader(System.in));

        try {
            while (true) {
                System.out.print("何か入力（exitで終了）> ");
                String line = key.readLine(); // 標準入力
                if (line.equals("exit")) { // exitが入力されたら終了
                    break;
                }
                System.out.println(line); // 標準出力
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
実行例
```bash 
何か入力（exitで終了）> aaaa
aaaa
何か入力（exitで終了）> exit
```
---
Sccanerを使用した標準入力の例
```java
import java.util.Scanner;

public class ScannerSample {
    public static void main(String[] args) {
        @SuppressWarnings("resource") // Eclipse上のワーニングを出さないようにする処理
        Scanner scanner = new Scanner(System.in);
        int total = 0;

        System.out.println("入力した数値を加算します。");
        System.out.print("数値の入力(終了は数値以外)> ");

        while (scanner.hasNextInt()) { // int型変数が入力される限りループ
            total += scanner.nextInt();
            System.out.print("数値の入力(終了は数値以外)> ");
        }
        System.out.printf("合計値%5d%n", total);
    }
}
```
実行例
```bash
入力した数値を加算します。
数値の入力(終了は数値以外)> 5
数値の入力(終了は数値以外)> 5
数値の入力(終了は数値以外)> a
合計値   10
```

# 第２章：コレクション
- コレクション：複数のオブジェクトを保持するデータ構造
    - 基本データ型は保持できない
    - 同じクラスのオブジェクトを保持
    - 可変長：**配列のように事前に要素数を決める必要は無い**=動的に要素数を変化させられる  
    - 例：ECサイトのカート
- ArrayListクラス・HashMapクラスがよく使われる

## ArrayListクラス
- ArrayListクラス：要素数を指定せずに使える配列
    - **基本データ型を要素として持つことはできない**ので**ラッパークラスを使う**  

|基本データ型|ラッパークラス|
|---|---|
|boolean|Boolean|
|byte|Byte|
|char|Character|
|short|Short|
|int|Interger|
|long|Long|
|float|Float|
|double|Double|

---

ArrayListのサンプルプログラム  
```java
import java.util.ArrayList;
import java.util.Iterator;

public class sample {
    public static void main(String[] args) {
        // インスタンスの生成
        ArrayList<String> list = new ArrayList<>();
        
        // 要素の追加
        list.add("First");
        list.add("Second");
        list.add("Third");
        System.out.println("リストの要素" + list); // [First, Second, Third]
        
        list.add(0,"Inserted");
        System.out.println("挿入後のリストの要素:" + list); // [Inserted, First, Second, Third]
        
        // 要素の取得
        String element = list.get(0); // 引数で指定した添字の要素を取得
        System.out.println("element:" + element); // Inserted
        
        // 要素数の取得
        int size = list.size();
        System.out.println("リストのサイズ:" + size); // 4
        
        // 要素の削除
        list.remove(0);
        System.out.println("リストの0番目の要素を削除:" + list); // [First, Second, Third]

        // for文の利用
        for (int i = 0; i < list.size(); i++) {
            String s = list.get(i);
            System.out.println("s:" + s + " 文字数:" + s.length()); // s:String 文字数:int
        }
        
        // Iterator
        Iterator<String> i = list.iterator();
        while(i.hasNext()) {
            String s = i.next();
            System.out.println("s:" + s + " 文字数:" + s.length());
        }

        // ラッパークラスの利用
        ArrayList<Integer> list_int = new ArrayList<>();
        list_int.add(1);
        list_int.add(2);
        System.out.println("リストの要素:" + list_int);
    }
}
```
実行結果
```
要素の追加------------------------------------
リストの要素[First, Second, Third]
挿入後のリストの要素:[Inserted, First, Second, Third]

要素の取得-----------------------------------
element:Inserted

要素数の取得------------------------------------
リストのサイズ:4

要素の削除------------------------------------
リストの0番目の要素を削除:[First, Second, Third]

for文の利用------------------------------------
s:First 文字数:5
s:Second 文字数:6
s:Third 文字数:5

Iteratorの利用------------------------------------
s:First 文字数:5
s:Second 文字数:6
s:Third 文字数:5

ラッパークラスの利用------------------------------
リストの要素:[1, 2]
```

---
インスタンス生成したオブジェクトに対するArrayList

Employeeクラス  
```java
public class Employee implements Comparable<Employee> {
    private int id;
    private String name;
    private int salary;

    public Employee() {
    }

    public Employee(int id, String name, int salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getSalary() {
        return salary;
    }

    public void setSalary(int salary) {
        this.salary = salary;
    }

    public String toString() {
        return id + "番の" + name + "さん（給料：" + salary + "円）";
    }

    @Override
    public int compareTo(Employee o) {
        return this.name.compareTo(o.name);
    }
}
```

```java
import java.util.ArrayList;

public class sample {
    public static void main(String[] args) {
        ArrayList<Employee> list = new ArrayList<Employee>();

        Employee emp1 = new Employee(1, "山田", 230000);
        list.add(emp1);

        Employee emp2 = new Employee();
        emp2.setId(2); emp2.setName("吉田"); emp2.setSalary(300000);
        list.add(emp2);

        list.add(new Employee(3, "領家", 200000));

        int sum = 0;
        for (int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i)); 
            sum = sum + list.get(i).getSalary();
        }
        int avg = sum / list.size();
        System.out.println("平均給料 : " + avg + "円");
    }
}
```
実行結果
```
1番の山田さん（給料：230000円）
2番の鈴木さん（給料：300000円）
3番の佐藤さん（給料：200000円）
平均給与:243333円
```


## リストの操作
- Collectionsクラスを使ったリスト操作
    - Collections.reverse(list)：リストの要素を反転
    - Collections.shuffle(list)：リストの要素をシャッフル
    - Collections.sort(list)：リストの要素をソート
---

Collectionsを使ったサンプルプログラム  

```java
import java.util.ArrayList;
import java.util.Collections;
public class sort {
    public static void main(String[] args) {
        // インスタンスの生成
        ArrayList<Integer> list_int = new ArrayList<>();
        list_int.add(1);
        list_int.add(2);
        list_int.add(3);
        list_int.add(4);
        list_int.add(5);
        System.out.println("初期状態:" + list_int);
        
        // 反転
        Collections.reverse(list_int);
        System.out.println("反転:" + list_int);
        
        // シャッフル
        Collections.shuffle(list_int);
        System.out.println("シャッフル:" + list_int);
        
        // ソート
        Collections.sort(list_int);
        System.out.println("ソート:" + list_int);
    }
}
```

実行結果
```
初期状態:[1, 2, 3, 4, 5]
反転:[5, 4, 3, 2, 1]
シャッフル:[4, 2, 1, 3, 5]
ソート:[1, 2, 3, 4, 5]
```

## HashMapクラス
- HashMapクラス：キーと値を対応付けて保持
    - インデックスの代わりにキーで値を参照
    - キーの重複不可

|キー|値|
|---|---|
|りんご|100|
|ばなな|110|

---
HashMapクラスを使ったサンプルプログラム
```java

import java.util.*;

public class HashMap_sample {
    public static void main(String[] args) {
        HashMap<String, Integer> map = new HashMap<>();
        // 要素の追加
        map.put("りんご", 100);
        map.put("バナナ", 110);
        map.put("もも", 90);
        System.out.println("mapの要素:" + map);
        
        // 要素の取得
        int element = map.get("りんご");
        System.out.println("キー(りんご)の値:" + element);
        
        // サイズの取得
        int size = map.size();
        System.out.println("mapのサイズ:" + size);
        
        // キーの削除
        map.remove("りんご");
        System.out.println("りんごを削除したmapの要素:" + map);
        
        // キーと値の一覧取得
        Set<String> keySet = map.keySet();
        Collection<Integer> values = map.values();
        System.out.println("キーの一覧を取得:" + keySet);
        System.out.println("値の一覧を取得:" + values);
        
        // 拡張for文でキーの取得
        for (String key : map.keySet()) {
            System.out.println(key + ": " + map.get(key));
        }
        // 拡張for文で値の取得
        for (int value : map.values()) {
            System.out.println("value:" + value);
        }
        
        // キーが含まれるかどうかの判定
        System.out.println("現在のmap:" + map);
        System.out.println("りんご:" + map.containsKey("りんご")); // りんごは削除済み
        System.out.println("もも:" + map.containsKey("もも")); // ももはある
    }
}

```
実行結果
```
要素の追加-----------------------------------
mapの要素:{もも=90, りんご=100, バナナ=110}

要素の取得-----------------------------------
キー(りんご)の値:100

サイズの取得-----------------------------------
mapのサイズ:3

キーの削除-----------------------------------
りんごを削除したmapの要素:{もも=90, バナナ=110}

キーと値の一覧取得-----------------------------------
キーの一覧を取得:[もも, バナナ]
値の一覧を取得:[90, 110]

拡張for文でキーの取得-----------------------------------
もも: 90
バナナ: 110

拡張for文で値の取得-----------------------------------
value:90
value:110

キーが存在するかの判定-----------------------------------
現在のmap:{もも=90, バナナ=110}
りんご:false
もも:true
```
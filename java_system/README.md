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
    - 可変長
---
- ArrayListクラス：複数のオブジェクトを順序付けしてまとめて保持するデータ構造
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
    }
}
```

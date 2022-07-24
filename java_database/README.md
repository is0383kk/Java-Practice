# Javaデータベースプログラミング

# 第１章:JDBC API
## JDBC API
- JDBC API：データアクセスを提供するAPI
    - RDBMSに依存しないプログラムの作成が可能
    - java.sql・javax.sqlパッケージ
    - JDBCドライバ
        - RDBMSとの接続
        - RDBMSへのクエリー及び更新分の送信
        - 実行結果の処理
        - RDBMSとの接続解除

## JDBCドライバ
- JDBCドライバ：RDBMSにアクセスして操作
    - 「.jar」形式のファイル・CLASSPATHに設定が必要

# 第２章：データベース接続
## Driver managerを用いたデータベース接続
- Driver manager：JDBCドライバを管理するためのサービス
    - DriverManagerクラス：java.sqlパッケージで提供

---
DriverManagerクラスを用いてデータベースに接続・切断を行うプログラム  
```java
// java.sqlパッケージのインポート
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class Sample1_1 {
    public static void main(String[] args) {
        String jdbcUrl =
        "jdbc:mysql://localhost/javadb?serverTimezone=Asia/Tokyo"; // getConnectionの引数：JDBC URL
        String userId = "javaclient"; // getConnectionの引数：ID
        String password = "javapass"; // getConnectionの引数：パスワード

        Connection con = null;
        try { // 必ず付ける・コンパイルが終わらない
            // ①JDBCドライバのロード(JDBCをJDMの中に入れる処理)
            Class.forName("com.mysql.cj.jdbc.Driver"); 
            System.out.println("JDBCドライバのロード完了");

            // ②接続の確立（ConnectionはJavaとDBをつなぐための通り道）
            // DriverManager.getConnection(相手, ID, パスワード);
            con = DriverManager.getConnection(jdbcUrl, userId, password);
            System.out.println("接続しました");

            // ③SQL文の送信と結果の取得（次章以降で説明）

        } catch (ClassNotFoundException e) { // JDBCドライバのロードに失敗すると発生
            System.out.println("JDBCドライバをロードできません");
            e.printStackTrace();
        } catch (SQLException e) { // getConnectionに失敗すると発生
            System.out.println("getConnectionに失敗しました（JDBC url・ID・パスワードを確認してください）");
            e.printStackTrace();
        } finally { // 必ず実行される
            try {
                // ④接続の解除
                if (con != null) { // getConnectionは失敗時にnullを返すので、getConnectionできた際にclose
                    con.close();
                    System.out.println("切断しました");
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

## DataSouceを用いたデータベース接続
- DataManager：データベース接続のための文字列情報がハードコーディングされているため、RDBMSが変更された際、ソースコードの修正が必要

- DataSource：JNDI APIを使用する
    - java.sql.datasouce

## データベース情報の取得
- java.sql.DatabaseMetaDataオブジェクト
    - データベース情報

# 第３章：SQL文の実行
## Statementオブジェクト
- Statementオブジェクト：Javaを使ってRDBMSにSQL文を発行するためのオブジェクト
    - Statement
    - PreparedStatement
    - CallableStatement

## SELECT文の実行
DataManagerを使用したSELECT文の実行プログラム  
```java

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class Sample2 {
    public static void main(String[] args) {
        String jdbcUrl = "jdbc:mysql://localhost/javadb?serverTimezone=Asia/Tokyo";
        String userId = "javaclient";
        String password = "javapass";
        String strSQL = "select * from customers"; // SQL文の中に「;」はない

        try (Connection con = 
            DriverManager.getConnection(jdbcUrl, userId, password)) {
                
            // Statementオブジェクトの取得 
            /*
                *1. getConnectionより後に行う
                *  2. 引数がない
                */
            Statement stmt = con.createStatement();

            // SELECT文の実行
            /*
                * 1. 上で定義したstmtを使う
                * 2. SELECT文(strSQL)を引数に入れる
                */
            ResultSet rs = stmt.executeQuery(strSQL); // rsの中に結果が入る(Javaが受け入れられる量のデータ)

            // 検索結果の表示
            while (rs.next()) {
                // rs.getString(列名)：列を文字列で取得・getInt・getDateなどがある
                String custId = rs.getString("cust_id");
                String custName = rs.getString("cust_name");
                String custAddr = rs.getString("cust_addr");
                System.out.printf("%s\t%s\t%s%n", custId, custName, custAddr);
            }
            // オブジェクトの解放
            rs.close();
            stmt.close();

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

## PreparedStatementによるSQL文の実行
- PreparedStatement：コンパイル済みのSQL文を保持し、パラメータのみを指定することができる
    - SQLインジェクション対策を行うことができる

## ストアドプロシージャの実行


# 第４章：トランザクション
## トランザクション
- トランザクション：データベースにでのデータ操作処理単位
    - **Atomicity**：処理が「全て実施：Commit」・「全て未実施：Rollback」に状態を確定させることを保証
    - Consistency：処理が矛盾・不整合を発生させないことを保証
    - Isolation：複数トランザクションが「並行実行で副作用が生じない」ことを保証
    - Durability：トラブルによる停止などに対して操作結果が消失されないことを保証

## トランザクションの開始と終了
- トランザクションの開始


## トランザクションの分離レベル

# 第５章：O/Rマッピング
## O/Rマッピング
- O(bject)/R(elation)マッピング：オブジェクトのフィールドとテーブルの列との対応付けをすること
    - **インピーダンスミスマッチ**：データベースはリレーション・Javaではインスタンスを持つことでつながりを示す・両者の使用の違いのこと

## JDBCを使用したアプリケーションの問題点
- JDBCを使用したアプリケーションの問題点
    - プログラムが理解しづらい
        - JDBCに必要な手続きと業務処理が混在(システム変更しづらい要因)
    - システムの変更に対応しづらい
        - テーブル構造が変更された際、プログラムも修正が必要
- 上記の問題解決のためのデザインパターンが提案されている

## DAOパターン



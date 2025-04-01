AWS Fargateを使用してJavaアプリをコンテナ化して実行する手順をフルセットで説明します。以下は、サンプルのJavaアプリを作成し、それをAWS Fargateで稼働させるための詳細な手順です。

---
## **概要**
ここでは以下を行います：
1. Javaアプリケーションの構築
2. Dockerイメージの作成
3. AWS環境の準備とECS設定
4. AWS Fargateでコンテナを実行
特にJavaアプリケーションのビルド手順を詳細化します。

---
### **1. Javaアプリケーションの構築**
#### **サンプルプログラム**
簡単なHello WorldのWebアプリケーションを例にします。`Spring Boot`を使用して、HTTPサーバをJavaで構築します。
**ディレクトリ構成:**

```
spring-boot-hello/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── example/
│   │   │           └── hello/
│   │   │               └── HelloApplication.java
│   │   ├── resources/
│   │       └── application.properties
├── Dockerfile
├── pom.xml
```
#### **ステップ 1: Spring Bootプロジェクト作成**
まず、`Maven`を使用してSpring Bootプロジェクトを作成します。
**`pom.xml`**
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>hello</artifactId>
    <version>1.0</version>
    <packaging>jar</packaging>
    <properties>
        <java.version>17</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```
#### **ステップ 2: アプリケーションのコード**
**`src/main/java/com/example/hello/HelloApplication.java`**
```java
package com.example.hello;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
@SpringBootApplication
@RestController
public class HelloApplication {
    public static void main(String[] args) {
        SpringApplication.run(HelloApplication.class, args);
    }
    @GetMapping("/")
    public String hello() {
        return "Hello, AWS Fargate!";
    }
}
```
**`src/main/resources/application.properties`**
```properties
server.port=8080
```

---
### **2. Javaのビルド手順**
#### **ステップ 1: Mavenを使用したビルド**
作成した`pom.xml`とコードをプロジェクトディレクトリ内に配置したら、以下のコマンドを実行してビルドを行います。
```bash
# プロジェクトディレクトリに移動
cd spring-boot-hello
# Mavenによるビルド
mvn package
```
成功すると次のパスに「fat JAR」が生成されます（依存関係込み）。
```
target/hello-1.0.jar
```
これで、Javaアプリケーションのビルドは完了です。

---
### **3. Dockerイメージの作成**
#### **Dockerfileの作成**
プロジェクトルートに以下の`Dockerfile`を作成します。
**`Dockerfile`**
```Dockerfile
# OpenJDKのベースイメージ使用
FROM openjdk:17-jdk-slim
# アプリケーションJARファイルをコンテナにコピー
COPY target/hello-1.0.jar /app.jar
# ポートを指定
EXPOSE 8080
# JARファイルを実行
ENTRYPOINT ["java", "-jar", "/app.jar"]
```
#### **Dockerイメージのビルド**
以下のコマンドでDockerイメージを作成します。
```bash
# Dockerイメージのビルド
docker build -t hello-app .
```
#### **イメージの確認**
Dockerイメージが作成できたか確認します。
```bash
docker images
```

---
### **4. AWS環境の準備とECS設定**
AWS Fargateをセットアップするための手順を説明します。
#### **ステップ 1: Elastic Container Registry (ECR)のリポジトリ作成**
AWS Management ConsoleでElastic Container Registry (ECR)を作成し、Dockerイメージをプッシュします。
```bash
# AWS CLIでリポジトリを作成
aws ecr create-repository --repository-name hello-app
```
作成されたリポジトリURLを確認してイメージをプッシュします。
```bash
# ログイン
aws ecr get-login-password --region <REGION> | docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.<REGION>.amazonaws.com
# タグ付けしてプッシュ
docker tag hello-app:latest <ACCOUNT_ID>.dkr.ecr.<REGION>.amazonaws.com/hello-app:latest
docker push <ACCOUNT_ID>.dkr.ecr.<REGION>.amazonaws.com/hello-app:latest
```
#### **ステップ 2: ECSでタスク定義を作成**
1. AWS Management ConsoleでECSを開きます。
2. 新しいタスク定義を作成し、Launch Typeを「Fargate」に設定。
3. コンテナ設定で、以下を指定：
   - コンテナ名: `hello-app`
   - イメージ: `<ACCOUNT_ID>.dkr.ecr.<REGION>.amazonaws.com/hello-app:latest`
   - ポート: `8080`
#### **ステップ 3: サービス作成**
ECSで新しいサービスを作成し、Fargateを選択します。
- サブネットとセキュリティグループを適切に設定。
- 外部からアクセスする場合はHTTPにする。
#### **ステップ 4: 確認**
Fargateで動作するサービスが作成されているはずです。タスクの状態が「RUNNING」になったのを確認し、公開されたURLにアクセスしてメッセージ「Hello, AWS Fargate!」が表示されることを確認します。

---
### **まとめ**
これで、JavaアプリケーションをAWS Fargate上にデプロイする手順が完了しました。JavaのビルドからDocker化、そしてAWSへのデプロイまで学びました。必要に応じてECSのログやCloudWatchを確認してエラートラブルシューティングを行ってください。
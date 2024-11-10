# Ch3. 修飾詞

## 修飾詞 (Modifiers)

修飾詞用來控制類別、屬性、方法等的訪問權限和行為。主要分為 `存取修飾詞` 和 `非存取修飾詞`，它們可以影響代碼的可見性、繼承性和執行方式。

### 存取修飾詞（Access Modifiers）

存取修飾詞控制成員的可見性和訪問範圍，包括 `public`、`protected`、`private`、`default`，來達到`封裝`概念。

| 修飾詞         | 訪問範圍           | 用於類別/方法/屬性 |
|-------------|----------------|------------|
| `public`    | 任何類別都能訪問       | `類別、屬性、方法` |
| `protected` | 同一包內、以及子類別可以訪問 | `屬性、方法`    |
| `private`   | 只能在同一類別內訪問     | `屬性、方法`    |
| `默認(無修飾)`   | 同一包內可以訪問       | `類別、屬性、方法` |

**範例 :**

```java
public class Example {
    public int publicField;
    protected int protectedField;
    private int privateField;
    int defaultField; // default

    public void publicMethod() {
    }

    protected void protectedMethod() {
    }

    private void privateMethod() {
    }

    void defaultMethod() {
    } // default
}
```

### 非存取修飾詞 (Non-access Modifiers)

非存取修飾詞用於設定類別或方法的特定屬性，通常控制行為或優先級，包含以下常見修飾詞：

| 修飾詞            | 功能                      | 用於類別/方法/屬性 |
|----------------|-------------------------|------------|
| `static`       | 表示靜態成員，屬於類別而非物件         | 屬性、方法      |
| `final`        | 表示不可變或不可覆蓋              | 類別、屬性、方法   |
| `abstract`     | 表示抽象類別或方法，無具體實現         | 類別、方法      |
| `synchronized` | 表示同步方法，在多執行緒環境下互斥執行     | 方法         |
| `volatile`     | 表示多執行緒共享變數需及時刷新，避免快取    | 屬性         |
| `transient`    | 表示屬性在序列化過程中被忽略          | 屬性         |
| `native`       | 表示該方法由本地代碼實現（非 Java 代碼） | 方法         |
| `strictfp`     | 強制使用 IEEE 754 標準進行浮點數運算 | 類別、方法      |


## 重點整理

- 使用存取修飾詞來達到封裝概念，保護程式碼不被輕易呼叫、修改。
- `static` 能透過類別名稱方式呼叫，無需實例化。
- `final` 使方法不能覆寫，變數值不能修改。
- `abstract` 抽象類別或方法，無具體實現，`interface`就是抽象。
- `synchronized` 用來控制執行續，來到執行續安全。


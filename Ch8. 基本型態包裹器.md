# 基本型態包裹器

## 基本型態包裹器 (Wrapper Classes)

是用於包裝基本資料型態（如 `int`、`double` 等）的類別。`Java`中的基本型態並非物件，所以作用範圍很局限。
`包裹器類`將基本型態封裝成物件，以便在物件需求的情境中使用這些基本型態。

**用途：**

- 將基本型態轉換為物件：在`Java`中的集合框架（如 `ArrayList`）和`泛型`中只能使用物件類型，不能直接使用基本型態。
- 內建方法：包裝器類提供了許多內建的方法來處理基本型態，例如 `parseInt`、`toString` 等。
- 支援自動裝箱與拆箱：`Java` 支援`自動裝箱（autoboxing）`和`自動拆箱（unboxing）`，使基本型態和包裝器類可以自動互相轉換。

### 基本型態及其對應的包裝器類

| 基本型態      | 包裝器類        |
|-----------|-------------|
| `byte`    | `Byte`      |
| `short`   | `Short`     |
| `int`     | `Integer`   |
| `long`    | `Long`      |
| `float`   | `Float`     |
| `double`  | `Double`    |
| `char`    | `Character` |
| `boolean` | `Boolean`   |


### 自動裝箱（Autoboxing）和自動拆箱（Unboxing）

- 自動裝箱：將基本型態自動轉換為對應的包裝器物件。例如，`int` 自動轉換為 `Integer`。
- 自動拆箱：將包裝器物件自動轉換為對應的基本型態。例如，`Integer` 自動轉換為 `int`。

**範例：**

```java
Integer a = 10; // 自動裝箱：int 轉換為 Integer
int b = a;      // 自動拆箱：Integer 轉換為 int

List<Integer> list = new ArrayList<>();
list.add(20); // 自動裝箱：int 轉換為 Integer
```

### 包裝器類的常用方法

包裝器類提供了靜態方法來處理基本型態的轉換和運算。以下是幾個常見方法：

- 解析字串為基本型態：例如 `Integer.parseInt()`、`Double.parseDouble()`等可以將字串轉為對應的基本型態。

  ```java
  String numStr = "123";
  int num = Integer.parseInt(numStr);
  System.out.println(num); // 輸出: 123
  ```

- 轉換為字串：toString() 方法將基本型態轉換為字串。

  ```java
  Integer num = 123;
  String str = num.toString();
  System.out.println(str); // 輸出: "123"
  ```

- 常量：包裝器類有一些常量，例如`Integer.MAX_VALUE`、`Integer.MIN_VALUE`，表示該型態的最大值和最小值。
  ```java
  System.out.println(Integer.MAX_VALUE); // 輸出: 2147483647
  ``` 
  
- 比較兩個包裝器物件：`compareTo()`和`equals()`方法用於比較包裝器物件。
  ```java
  Integer a = 100;
  Integer b = 200;
  System.out.println(a.compareTo(b)); // 輸出: -1
  System.out.println(a.equals(b));    // 輸出: false
  ```

### 包裝器類的應用場景

- 集合框架：基本型態不能直接用於集合框架中，因此需要使用包裝器類來將基本型態轉為物件。
  ```java
  List<Integer> list = new ArrayList<>();
  list.add(10); // 使用 Integer 包裝器類
  ```
- 泛型：Java 泛型只支持物件類型，因此在使用泛型時必須使用包裝器類來封裝基本型態。
- 處理數據轉換：包裝器類提供了很多實用方法，例如將字串轉換為基本型態數值、處理最大最小值等，方便進行數據處理。

## 重點整理

- 包裹器是為了在需要物件的情境，進行包裝基本資料型態的處理，例如：`泛型`需求。
- `Java` 支援`自動裝箱（autoboxing）`和`自動拆箱（unboxing）`，使基本型態和包裝器類可以自動互相轉換，無需特別處理。
- 包裝器類提供了靜態方法來處理基本型態的轉換，例如：`Integer.parseInt()`、`Double.parseDouble()`。
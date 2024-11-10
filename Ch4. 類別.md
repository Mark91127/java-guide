# Ch4. 類別

## Class 類別

用來定義物件的設計圖。它是物件導向程式設計（OOP）的核心概念之一，用來描述物件的屬性（資料）和行為（方法）。簡單來說，`class` 是用來創建物件的模板，而物件則是 `class` 的實例。

**命名規則** 
- 類別名稱需以`大駝峰`命名，例如:`Example`、`Simple`。

**範例 :**
```java
class Example {
    
}
```

在`class`程式區塊裡可以包含以下資訊:

- package
- 成員變數 (屬性、全域變數)
- constructor
- method
- static block

### 包 (Package)

是一個用來組織類別、介面和子包的命名空間機制。允許開發者將類別進行分類和組織，並且可以避免類別名稱衝突。這樣的組織方式提高了代碼的可維護性、可擴展性，並且便於管理大型應用程式。

#### 用途
- 避免命名衝突：在不同的包中可以有相同名稱的類別，從而避免命名衝突。 例如:同個地址不會出現兩棟房子。
- 控制存取範圍：`package` 可以控制類別、方法、屬性的可見性，配合存取修飾詞（`public`, `private`, `protected` 等）來進行權限管理。

#### 命名規則
- 通常，`package`命名採用反向域名命名規範，例如：com.companyname.projectname，避免名稱衝突。
- `package`應該是小寫字母，並且由點（.）分隔，分別代表不同層級的包結構。

#### 宣告規則

- 必須在 `class` 第一行宣告。

```text
package package_name;
```

**範例 :**

```java
package com.example.myapp;

class Example {
    
}
```

此範例表示，`example` 這個 `class` 位於 `com.example.myapp` 資料夾中。

### 成員變數 (Member Variables)

是定義在類別內部、但位於方法外部的變數。成員變數是類別或物件的屬性，描述了該類別或物件的狀態。每個物件都有自己的一份獨立的成員變數值，除非它們被定義為靜態變數（static），讓所有物件共享同一個值。

#### 命名規則
- 變數名稱應使用`小駝峰`命名，例如:`cardNo`、`account`，常數例外。
- 常數應為名稱應全部為大寫，單詞用下劃線`_`分隔。

#### 宣告規則

以下是宣告的規則，`[]`代表選填，依情境選擇填寫。

```text
[存取修飾詞] [static] [final] 型態 變數名稱;
```
可以是任何有效的型態，包括基本資料型態（如 `int`, `double`, `boolean` 等）以及物件型資料型態（如 `String`, `自訂類別`等）。

**範例 :**

```java
package com.example.myapp;

class Example {
    
    public int i;
    public String str = "hello world";
    public static String str1 = "hello world";
    public static final String STATIC_STR = "hello world"; //因不可變，所以為常數
    
}
```


### 建構子 (Constructor)

是一個特殊的方法，用於在創建物件時初始化物件的狀態。建構子具有與類別相同的名稱，並且沒有回傳型態。每當創建類別的實例時，會呼叫該類別的建構子來初始化物件的成員變數。


#### 命名規則

- 名稱必須與類別名稱相同。

#### 宣告規則

以下是宣告的規則，`[]`代表選填，依情境選擇填寫。

```text
[存取修飾詞] 類別名稱 ([參數...]) {}
```
如果未建立有參數建構子，`Java`則會新增空參數建構子(預設建構子)。

**範例1 :**

```java
class Example {
    
    //隱藏的預設建構子 public Example() {}

    public static void main(String[] args) {
        Example example = new Example();
    }
    
}
```

當定義有參數建構子時，則會覆蓋空參數建構子。

**範例2 :**

```java
class Example {
    
    public String text;
    
    public Example(String text) {
        this.text = text;
    }

    public static void main(String[] args) {
        Example example = new Example(); // compiler error
        Example example = new Example("Hello World"); // success
    }
    
}
```

如果需要同時存在，則需要自行撰寫空參數建構子。

**範例3 :**

```java
class Example {
    
    public String text;

    public Example() {
        
    }
    
    public Example(String text) {
        this.text = text;
    }

    public static void main(String[] args) {
        Example example = new Example(); // success
        Example example = new Example("Hello World"); // success
    }
    
}
```

### 方法 (Method)

描述了物件或類別的行為。方法可以進行各種操作，如計算、修改物件狀態或處理輸入輸出等。每個方法都有一個名稱，並且可能會接受一些參數，也可以返回一個值。

#### 命名規則

- 名稱應該要有動詞，並且以`小駝峰`命名，例如:`run()`、`getData()`、`setData()`。

#### 方法簽章 (Method Signature)

是指方法的名稱、參數類型和參數的順序，這些共同決定了方法的唯一性。在 `Java` 中，方法簽章是用來區分不同方法的關鍵因素，特別是在方法重載的情境下，當同一個類別中有多個方法名稱相同但參數不同時，`Java` 會根據方法簽章來區分這些方法。

以下是宣告的規則，`[]`代表選填，依情境選擇填寫。

```text
[存取修飾詞] [abstract] [synchronized] [static] [final] 回傳值 方法名稱([參數...]) {}
```
**範例 :**

```java
class Example {
    
    public void run() {
        System.out.println("run");
    }
    
}
```

了解完上述資訊後，我們來設計衣服 `class`。

```java
class Clothes {

    private String type; // 衣服的類型，例如 "T-Shirt"、"Jacket" 等
    private String color; // 顏色
    private String size; // 尺寸，例如 "S", "M", "L", "XL"

    public Clothes(String type, String color, String size) {
        this.type = type;
        this.color = color;
        this.size = size;
    }

    public String getType() { return type; }
    public void setType(String type) { this.type = type; }

    public String getColor() { return color; }
    public void setColor(String color) { this.color = color; }

    public String getSize() { return size; }
    public void setSize(String size) { this.size = size; }

}
```

## 重點整理

- `class` 程式區塊內只能存在`package`、`成員變數 (屬性、全域變數)`、`constructor`、`method`、`static block`。`if`、`for`等只能存在方法區塊。
- `package` 宣告必須在 `class` 最上面。
- `成員變數` 可以是任何有效的型態。
- 如果未建立有參數建構子，`Java`則會新增空參數建構子(預設建構子)。
- 當定義有參數建構子時，則會覆蓋空參數建構子。
- 方法簽章的宣告方式。

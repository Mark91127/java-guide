# 字串

## 字串 (String)

`String` 是一個`不可變(immutable)`的`物件`，專門用來處理字串（字符序列）。是非常常用的類別，它在
`java.lang`基礎包中，並且擁有許多內建的方法來處理文本資料。

**宣告規則：**

- 較為廣泛使用，建立後將會直接放到`字串池`。
  ```java
  String text = "Hello World";
  ```

- `字串`也是`類別`，所以可以使用`new`關鍵字方式宣告。
  ```java
  String text = new String("Hello World");
  ```

**特性：**

- 不可變性(Immutability)：字串一旦創建，其內容`無法更改`。

所以當對字串進行修改（如拼接）時，實際上是創建了一個新的字串物件，而不是在原字串基礎上進行更改。

```java
public class StringExample {

  public static void main(String[] args) {
    String str1 = "Hello";
    str1 = str1 + " World";
    System.out.println(str1); // print Hello World
  }

}
```
**在這個例子中：**

- `String str1 = "Hello"`產生`Hello`字串物件。
- `str1 = str1 + " World"`將會重新參照到新的字串物件，並產生`Hello World`字串物件。

![string](image/string.svg)

## 字串池 (String Pool)

是一個池，用來存放字串字面量，讓相同字串可以共享同一個內存地址，而不會為每個字串字面量創建新的物件，
重復利用來達到節省內存的效果。

**特性：**

當創建字串字面量時，首先會檢查字串池中是否已經存在相同的字串內容。如果存在，則會重用池中的字串物件，並將變數指向該物件;
如果不存在，則會在字串池中創建該字串的新實例，來達到節省內存。

```java
public class StringExample {

  public static void main(String[] args) {
    String str1 = "Hello";
    String str2 = "Hello";
    String str3 = new String("Hello");

    System.out.println(str1 == str2); // true，str1 和 str2 指向相同的字串池物件
    System.out.println(str1 == str3); // false，str3 是通過 new 創建的新物件，儲存在堆區
    System.out.println(str1.equals(str3)); // true，內容相同

  }

}
```

在這個例子中：

- `==`用來比較記憶體位址是否相同，是否參照到同一個物件。
- `equals()`用來比較值是否相同。
- `str1` 和 `str2` 是相同的字串字面量，兩者共享同一個字串池中的物件，所以為`true`。
- `str3` 則是使用 `new String("Hello")` 明確創建新字串物件，因此即使內容相同，與字串池中的字串物件地址不同
，所以為`false`。


![stringpool](image/stringpool.svg)

### intern() 方法

如果希望手動將字串放入字串池中，可以使用`String`類的`intern()`方法。該方法會檢查字串池中是否已經存在相同內容的字串，
如果有，則返回池中的物件；如果沒有，則將該字串放入字串池，並返回該物件。

```java
public static void main(String[] args) {
    String str1 = "Hello";
    String str2 = new String("Hello");
    String str3 = str2.intern();

    System.out.println(str1 == str2);  // false
    System.out.println(str1 == str3);  // true
  }
```

在這個例子中：

- `str1` 和 `str2` 兩者為不同參照，所以為`false`。
- `str3` 則是使用 `str2.intern()` 手動把`Hello`放進字串池，但是`str1`已經存在，所以回傳`str1`的字串物件參照。

**intern前後差別：**

intern前：

```java
public static void main(String[] args) {
    String str1 = "Hello";
    String str2 = new String("Hello World");
  }
```

在這個例子中：

- 建立`Hello`字串物件，放入字串池。
- 建立`Hello World`字串物件，未放入字串池。

![stringpool_intern_before](/image/stringpool_intern_before.svg)

intern後：

```java
public static void main(String[] args) {
    String str1 = "Hello";
    String str2 = new String("Hello World");
    String str3 = str2.intern();
  }
```

在這個例子中：

- 建立`Hello`字串物件，放入字串池。
- 建立`Hello World`字串物件，未放入字串池。
- `str3` 則是使用 `str2.intern()` 手動把`Hello World`放進字串池，並回傳字串池的`Hello World`字串物件的參照。

![stringpool_intern](/image/stringpool_intern.svg)

## StringBuilder

`StringBuilder`是一個`可變`的字串類，用於在創建多個字串物件的情況下進行字串的高效修改。

**特性：**

- 是可變的，與`String`的特性相反，修改並不會產生新的字串物件。
- `StringBuilder`的長度會隨著內容的增減而自動調整。
- 透過方法進行簡易的字串修改，例如：`append`、`insert`等方法。
- 非同步，適合單線程環境中頻繁進行字串操作的情境。

`append()`：在末尾追加字串或其他類型。
```java
StringBuilder sb = new StringBuilder("Hello");
sb.append(" World");
System.out.println(sb.toString()); // 輸出: Hello World
```

`insert()`：在指定位置插入字串。
```java
StringBuilder sb = new StringBuilder("Hello World");
sb.insert(5, ",");
System.out.println(sb.toString()); // 輸出: Hello, World
```

`delete()`：刪除指定範圍的字元。
```java
StringBuilder sb = new StringBuilder("Hello, World");
sb.delete(5, 7);
System.out.println(sb.toString()); // 輸出: Hello World
```

`toString()`：將`StringBuilder`轉為`String`。
```java
StringBuilder sb = new StringBuilder("Hello World");
String str = sb.toString();
```

**重點整理：**

- `字串`是`不可變`的，由於其內容`無法更改`，所以會建立新的字串物件。
- `字串池`的目的是為了`重複利用`，達到節省記憶體消耗。
- `==`用來比較記憶體位址是否相同，是否參照到同一個物件。
- `equals()`用來比較值是否相同。
- 使用`intern()`方法，把`new String()`的物件手動放入字串池。
- `StringBuilder`是`可變的`，所以不會因為修改就產生額外的字串物件。
# Ch5. 物件

## 物件 (Object)

基於物件導向程式設計（OOP）概念的核心單位。`類別(class)` 透過實例化產生物件，代表一個具體的
`實例(instance)`，包含`屬性(attributes)`和`行為(methods)`。

**特性**

- 每個物件都是獨立的，例如屬性。
- 產生時會佔用一部份記憶體空間。
- 非靜態的屬性及方法，需透過實例化後呼叫。
- 所有物件一律都會繼承`Object`，並提供相關方法，例如：`toString()`、`equals(Object obj)`、`hashcode()`。
  
### 實例化 (Instantiation)

是創建物件的過程，通常使用 `new` 關鍵字來達成。以下是幾種常見的實例化方式和情境：

**1. 透過`new`關鍵字**

最常用的實例化方式，門檻相對較低。

**宣告規則**

- `類別名稱([參數...])` 取決於建構子。

```text
類別名稱 變數名稱 = new 類別名稱([參數...]) 
```

透過`new`關鍵字，將設計好的`class`，變成`物件`。

```java
Clothes clothes = new Clothes("T-Shirt", "pink", "S");
```

**2. 透過反射(Reflection)**

反射是較進階的功能，門檻相對較高，允許在執行時動態創建物件。可使用 `Class` 的 `newInstance()` 方法，或透過
`Constructor`
來進行更靈活的實例化。

```java
public class Example {

  public static void main(String[] args) {
    try {
      Class<?> carClass = Class.forName("com.example.Clothes");
      Clothes clothes3 = (Clothes) carClass.getDeclaredConstructor(String.class, String.class,
              String.class)
          .newInstance("T-Shirt", "pink", "S");
    } catch (Exception e) {
      e.printStackTrace();
    }
  }

}
```

透過`Class.forName`方法，可塞入相對應的類別名稱，動態生成物件。

### 堆(Heap) vs 堆疊(Stack)

在`JVM`的`Data Runtime Area`裡其中有`堆疊(Stack)`跟`堆(Heap)`兩個重要記憶體區塊，以下是特性介紹：

#### 堆疊(Stack)

在`JVM`中，`Stack`是每個執行緒的獨立記憶體區域，負責管理`方法呼叫`和`局部變量`
。每個執行緒在執行時都擁有自己的堆疊，且每當呼叫一個方法時，`JVM`
都會在`堆疊`中創建一個「`堆疊幀`」（Stack Frame），來保存此`方法`的`局部變量`、`操作數棧`、`返回地址`等資訊。

**特性：**

- 後進先出（LIFO）：堆疊區依據後進先出原則管理每個方法的執行。
  ![stack](/image/stack.svg)
- 執行緒私有：`Stack` 在每個執行緒中都是`獨立的`，互相不干擾，來達到執行緒安全環境。

**堆疊幀（Stack Frame）**

每個方法呼叫會在堆疊中創建一個`堆疊幀`，大致由以下三個部分構成：

![stack](/image/stackframe.svg)

**局部變量區（Local Variables）：**
- 用於存儲方法內的所有局部變量，包括方法參數和臨時變量。
- 每個變量根據其類型（int、float、引用類型等）分配對應的空間大小。
- 變量是按索引訪問的，而不是名稱。

**操作數棧（Operand Stack）：**
- 用於計算過程中的中間結果，例如方法內的算術和邏輯運算。
- 當執行字節碼指令時，操作數棧會被進行推入和彈出操作。
- 操作數棧可以存放不同類型的數據，但在操作時必須保持數據類型一致。

**幀數據（Frame Data）或 方法返回地址（Return Address）：**
- 存儲方法執行狀態，例如方法返回地址（上一個方法的指令位置）、捕獲異常的資訊和其他狀態數據。 
- 當方法執行完成並返回給呼叫者時，JVM 會根據返回地址回到之前的執行位置。

**StackOverflowError問題**

當呼叫方法造成無窮遞迴無法停止，導致堆疊不斷增長，最終超出`JVM`為堆疊分配的記憶體限制。

```java
public class Example {

  public static void recursiveMethod(int n) {
    if (n == 0)
      return;  // 終止條件
    recursiveMethod(n + 1); // 遞迴呼叫會使 n 越來越大
  }

  public static void main(String[] args) {
    recursiveMethod(1);
  }
}
```

上述範例就是終止條件錯誤，導致方法持續執行，最終拋出`StackOverflowError`。

```text
Exception in thread "main" java.lang.StackOverflowError
```

**如何避免StackOverflowError**

- 檢查遞迴終止條件：確保每個遞迴方法有正確的終止條件，並且能在合理的深度上觸發終止條件。
- 避免無窮遞迴：在遞迴設計中保持條件嚴謹，避免無窮遞迴。檢查參數更新或條件是否能實際控制遞迴深度。
- 使用迴圈代替遞迴：在一些情況下(如尾遞迴)，可以考慮將遞迴轉換為迴圈來降低堆疊使用。
- 增加 JVM 堆疊大小(僅在必要時)：如果無法避免深度遞迴，可以嘗試在 JVM 啟動時調整堆疊大小，例如使用 -Xss
  參數來增加堆疊記憶體大小，但這通常不是根本解決之道。

#### 堆(Heap)

在`JVM`中，`Heap`負責存儲所有`物件`和`陣列`的記憶體區域。並在`JVM`
啟動時分配，其記憶體可以根據需要動態擴展。

**特性：**

- 動態分配：物件在運行時動態分配於堆區。
- 執行緒公有：堆區在`JVM`中是全局的，所有執行緒共用同一個，所以需要注意執行緒不安全環境。

**堆區的結構：**

![heap](/image/heap.svg)

- 新生代（Young Generation）：新建立的物件會被分配在新生代，並分為`Eden`區和兩個`Survivor`區（`S0` 和
  `S1`）。當物件經歷多次`垃圾回收(GC)`仍存活時，會被移動到年老代。
- 年老代（Old Generation）：存放存活較長時間的物件。當年老代空間不足時，會觸發 `Major GC` 或 `Full GC`
  ，強制啟動回收機制。
- 永久代（Permanent Generation）/元空間（Metaspace）：JDK 8 以前，永久代（PermGen）用於存放`類別資訊`、`方法`等元數據。JDK 8
  之後改為`元空間(Metaspace)`，並移至`本機記憶體(native memory)`管理，來避免容易導致應用程序在類大量加載的情況下出現`OutOfMemoryError`。

**Stack與Heap比較**

以下是`Stack`跟`Heap`比較表格：

| 特性    | 堆區（Heap）         | 堆疊區（Stack）          |
|-------|------------------|---------------------|
| 存儲內容  | 物件、陣列            | 局部變量、堆疊幀            |
| 分配方式  | 動態分配，受垃圾回收管理     | 靜態分配，先進後出           |
| 記憶體大小 | 可以擴展（有最大限制）      | 大小固定                |
| 存取速度  | 相對較慢             | 相對較快                |
| 共用性   | 執行緒公用            | 執行緒私有               |
| 管理方式  | 由 JVM 的垃圾回收器自動管理 | JVM 自動管理，方法執行結束自動釋放 |

#### 物件導向概念 - 參照 (Reference)

在了解`參照`概念之前，先來看一段程式碼，您如何理解並說明這段程式碼？

```java
A a = new A();
```

如果您認為變數a就等於物件A，那就代表還沒完全了解`物件導向`的其中概念。

`變數`跟`物件`是透過`參照(Reference)`建立關聯。這代表對物件的操作通常是通過`參照`來進行的，而不是直接操作物件本身。

**特性：**
- 變數`參照`只能有一個，例如：一個變數只能參照一個物件，不能同時參照多個物件。
- 參照無需設定，只需透過宣告的方式會自動建立，例如：`A a = new A();`，從這個訊息得知`a`變數已經參照到`A`物件。

搭配上述對`Stack`及`Heap`的初步了解，透過簡單的解釋其中運作原理。

以下是各個步驟在`Stack`跟`Heap`裡的狀態：

- 單純宣告變數`obj`在`Stack` 儲存的狀態。

![reference1](/image/reference1.svg)

- 單純建立物件。

![reference2](/image/reference2.svg)

- 宣告並建立參照。

![reference3](/image/reference3.svg)

如果今天出現`obj = obj1`呢？

依照上述案例，就不能理解成 obj `等於` obj1，而是要理解成：

- `obj`的這個變數修改參照到`obj1`原本已經建立參照的物件
- `Object@3fee733d`將會被`GC`

![reference4](/image/reference4.svg)

**OutOfMemoryError問題**

又稱`OOM`，是 Java 程式在執行時發生的一種錯誤，表示`JVM`無法從系統中獲得足夠的記憶體來執行操作。
這種錯誤常見於大量資料處理、物件創建過多，或是記憶體洩漏等情況。

```java
public class HeapSpaceOutOfMemoryExample {
    public static void main(String[] args) {
        List<int[]> list = new ArrayList<>();
        while (true) {
            list.add(new int[1_000_000]); // 持續分配大量空間
        }
    }
}
```

- 堆區記憶體不足（Java Heap Space）
當程式不斷創建新的物件並持續佔用堆區（heap）記憶體，最終導致堆區滿而無法分配更多記憶體。
常見於長期運行的程式、物件池、快取（cache）或大量數據儲存等操作。
例如：瞬間建立大量物件導致堆區耗盡。

**如何避免OutOfMemoryError問題**

- 增加 JVM 記憶體大小： 可以透過 JVM 參數增加堆區大小，例如 -Xmx（最大堆大小）或 -XX:MaxMetaspaceSize（最大 Metaspace 大小）。
  ```text
  java -Xmx1024m -XX:MaxMetaspaceSize=256m MyApp
  ```
- 檢查和優化程式中的記憶體使用：
檢查物件的生命週期，確保不必要的物件能夠及時被回收。
對集合（如 List、Map）的容量進行管理，避免無限制地向集合中添加資料。
避免不必要的靜態變數，靜態變數持有的物件無法自動釋放。
- 使用記憶體分析工具： 使用 Java Memory Analyzer（MAT）、VisualVM 或其他分析工具來檢查記憶體分配和物件引用，以找出記憶體洩漏或記憶體占用過多的物件。
- 避免無窮集合或緩存增長： 應限制快取、集合等的大小，避免不斷累積無限數據導致記憶體耗盡。
使用例如 WeakHashMap 等弱引用結構來儲存臨時性資料，讓垃圾回收器可以釋放無需的資料。
- 確保釋放資源：
在使用 I/O 流或資料庫連線時，應及時關閉或釋放資源，以減少無用物件的佔用。
可以使用 try-with-resources 語法來保證資源的正確釋放。

### 垃圾回收機制 (Garbage Collection , GC)

是一項自動化的記憶體管理功能，負責釋放不再使用的物件所佔用的記憶體空間。透過垃圾回收機制，
可以減少手動管理記憶體的負擔，避免記憶體洩漏的問題，使應用程式更加穩定且有效率。

由於`Object@3fee733d`已無任何參照，所以會被回收，並釋放記憶體。

![reference2](/image/reference2.svg)


## 重點整理

- 通常透過`new`關鍵字實例化。
- `Stack`主要儲存方法訊息，且每個執行緒獨立。
- `StackOverflowError`發生原因，通常為方法使用遞佪且尚未有終止條件所導致。
- `Heap`主要儲存物件相關訊息，每個執行緒共用。
- `Reference`用來建立變數及物件的連結、關聯。
- `OutOfMemoryError`發生原因，通常為瞬間建立大量物件並且無法被回收。
- GC機制隨時監控並回收無用的物件，釋放記憶體及空間。


# Python 3.5 筆記

## 概述
* Python 3 之後，直譯器預期的原始檔案編碼必須是 UTF-8。（Python 2.x 預期的是 ASCII）
* 註解的寫法：井字號。
* 程式碼的大小寫是有區別的（case sensitive）。
* 所有東西都是**物件**，資料型態是物件，**函式也是一個物件**。
    * 想知道某個變數是何種物件時，可以透過 type() 函式取得。
    * 每個物件本身都會有個 \_\_dict\_\_ 屬性，當中記錄著類別或實例擁有的特性，可以透過 vars() 函式取得。
    * 註）type() 與 vars() 函式定義於 \_\_builtins\_\_ 模組中，不用特地 import 就可以直接呼叫。詳細請參考本文「模組」章節。
* 動態定型語言，變數本身並沒有型態資訊。變數只是個參考至實際物件的名稱。
* 物件導向機制支援**多重繼承**。
* 程式區塊（在 Python 中稱為 **Suite**）
    * 使用冒號（:）開頭，之後同一區塊範圍要有相同的縮排，不可混用不同空白數量，也不可混用空白與 Tab。
    * Python 的建議是使用四個空白做為縮排。
* DocStrings
    * 類似 javadoc 的自描述文件系統。
    * 套件層級的 DocStrings 要寫在套件對應的 \_\_init\_\_.py 檔案中。
    * 在函式、類別或模組定義的一開頭，使用三引號括起來的多行字串，會成為函式、類別或模組的 \_\_doc\_\_ 屬性，也就是會成為 help() 的輸出內容之一。
* 程式進入點
    * Python 是一種腳本語言，沒有統一的入口，基本上就是從第一行執行到最後一行。
    * 用一種模擬的方式來產生程式進入點。
        ``` Python
            if __name__ == “__main__”:
                # suite
        ```
        如果某個模組是被 Python 直譯器直接執行的，它的 \_\_name\_\_ 會被設定為 \_\_main\_\_，若模組是被 import 進來的，那麼  \_\_name\_\_ 就會是模組的名稱。[參考這個文章](http://blog.konghy.cn/2017/04/24/python-entry-program/)
* [官方開發文件](https://docs.python.org/3/)
* [良葛格](https://openhome.cc/Gossip/Python/)

## 資料型態

* 數值
    * int  # python 2.x 的整數分為 int 與 long
    * float   # 高精度小數請改用 decimal.Decimal  
    * True/False
    * complex
* 字串
    * str
    * byte
* 群集
    * list            # [1, 'two', True]   有序、可接受異質元素
    * tuple        # (1, 'two', True)  與 list 十分相似，但建立後不可再變動
    * set            # {'Hello', 'World’} 無序、不重覆、元素需為 hashable 物件
    * dict            # {'key': 'value’} 鍵值組

## 運算子
* 運算子 == 與 != 是用來比較物件實際的值、狀態的相等性，而運算子 is 與 is not 是用來比較兩個物件的參考是否相等。
* 加減乘除運算子 +, -, *, /
    * 實作方法 \_\_add\_\_(), \_\_sub\_\_(), \_\_mul\_\_(), \_\_truediv\_\_()
    * 應用於數值型態：加減乘除，特別小心浮點數
    * 應用於字串型態：用 + 運算子來串接字串，用 * 運算子來重覆字串
    * 應用於 list 和 tuple：用 + 運算子來串接 list/tuple，用 * 運算子來重覆 list
* 比較與指定運算子 >, >=, <, <=, ==, !=
    * 實作方法 \_\_gt\_\_(), \_\_ge\_\_(), \_\_lt\_\_(), \_\_le\_\_(), \_\_eq\_\_(), \_\_ne\_\_()
    * 註）實作 \_\_eq\_\_() 時通常也會實作 \_\_hash\_\_()
* 邏輯運算子 and, or, not
* 位元運算子 &, |, ^, ~, <<, >>
    * 應用於數值型態
    * 應用於 set

## 函式 Function
* 函式也是物件，可以指定給其它的變數。
* 呼叫函式時，不一定要依照參數宣告順序來傳入引數，而可以指定參數名稱來設定其引數值。
* 如果函式執行完畢但沒有使用 return 傳回值，或者使用了 return 結束函式但沒有指定傳回值，預設就會傳回 None。
* 函式中還可以定義函式，稱為區域函式（Local Function），區域函式可以直接存取它的外部函式的參數或區域變數，如此可以減少呼叫函式時引數的傳遞。
* 不支援函式重載（overloading），後定義的函式會覆改先前定義的函式。可藉助預設引數來有限度地模仿函式重載。
* 預設引數
    ```
    def greeting(gender, name="triumph")
        # suite
    ```
* 不定長度引數
    * 引數名稱前置一個星號（*）會將不定長度的引數打包成一個 tuple。
    * 引數名稱前置兩個星號（**）會將不定長度的引數打包成一個 dictionary。
    * 在一個函式中可同時使用 * 與 ** 。
* Lambda 函式

## 流程控制
### if … elif … else
``` Python
if condition1:
    # suite
elif condition2:
    # suite
else:
    # suite
```

### while … else
```
while condition:
    # suite
else
    # suite
    # 注意！就算 while 正常執行結束也會執行 else 區塊。
    # 若不想執行此區塊，需在 while 區塊中 break 來中斷迴圈。
    # 但強烈不建議使用這個區塊。
```
### for … in … else
```
for arg in iterable:
    # suite
else
    # suite
    # 注意！就算 if 正常執行結束也會執行 else 區塊。
    # 若不想執行此區塊，需在 if 區塊中 break 來中斷迴圈。
```

### for Comprehension
暫略

## 模組 Module
* 一個 *.py 檔案就是一個模組，透過 import 命令可以將其它模組引入。被 import 的模組中的程式碼會被執行。
* 在 \_\_builtins\_\_ 模組中的函式、類別等名稱，都可以不用 import 就直接取用，而且不用加上模組的名稱為前置。
* 想要知道一個模組中有哪些名稱，可以使用 dir() 函式。
* 當 import 某個模組而使得指定的 *.py 檔案被載入（且被執行）時，Python 直譯器會為它建力一個 module 實例，並建立一個模組名稱來參考它。dir(math) 實際上是查詢 math 名稱參考的 module 實例上，有哪些屬性名稱可以存取。
* 直譯器 import 一個模組時會略過所有以底線（underscore）開頭的成員，術語稱為 Module Private。
* 可以透過 sys.module 來知道目前已載入的模組名稱與實例有哪些。鍵的部分是模組名稱，值的部分是模組實例。
* 直譯器會根據 sys.path 來尋找可載入模組，其組成來自於：
    * 執行 Python 直譯器時的資料夾
    * PYTHONPATH 環境變數
    * Python 安裝中標準程式庫資料夾
    * PTH 檔案中列出的資料夾

## 套件 Package
* 套件資料夾中一定要有一個 \_\_init\_\_.py，該資料夾才會被視為一個套件。
* 可建立多層次套件，也就是套件中還有套件，在這種情況之下，每個擔任套件的資料夾與子資料夾中，各要有一個 \_\_init\_\_.py 檔案。
* 若要針對套件來撰寫 DocStrings，可以在套件相對應的 \_\_init\_\_.py 中使用三引號來撰寫。

## 物件導向
* 類別若沒有指定父類別，那麼就是繼承 object 類別。
* 可以進行**多重繼承**。多個父類別繼承下來的方法名稱沒有衝突時是最單純的狀況，有名稱衝突時就要注意搜尋順序：基本上是從子類別開始尋找名稱，接著是同一階層父類別由左至右搜尋，再至更上階層父類別由左至右搜尋，直到達到頂層為止。

## 類別 Class



```
class Account(若有父類別則在此括號內以逗號分隔):
    def __init__(self, name, number, balance):
        self.name = name
        self.number = number
        self.balance = balance

    def deposit(self, amount):
        # do something here

    def withdraw(self, amount):
        # do something here
```

### 特殊方法
主要為定義在 object 類別中的方法，請參考[官方文件](https://docs.python.org/3/reference/datamodel.html#special-method-names)。
* \_\_new\_\_ # 建構類別實例
* \_\_init\_\_  # 初始化
* \_\_del\_\_  # 在物件被刪除時自行定義清除相關資源的行為
* \_\_str\_\_  # 使用 print() 函式時回傳的內容，主要是給人類看的易懂格式
* \_\_repr\_\_ # 使用 REPL 時回傳的內容，主要是給程式、機器剖析用的特定格式
* \_\_hash\_\_ # hashable
* \_\_iter\_\_ # 回傳一個迭代器
* \_\_eq\_\_, \_\_ne\_\_ # 運算子 ==, !=
* \_\_gt\_\_, \_\_ge\_\_ # 運算子 >, >=
* \_\_lt\_\_, \_\_le\_\_ # 運算子 <, <=
* \_\_format\_\_
* \_\_bool\_\_

### 裝飾器

* @property
* @staticmethod 希望某個方法不被拿來做為綁定方法。
* @classmethod 第一個參數一定是接受所在類別的 type 實例。
* @abstractmethod

## 例外處理

```
try:
    # do something
except BaseException as err:
    # 如果 try 區塊中有 exception 就會執行
else:
    # 如果 try 區塊中沒有產生任何 exception 就會執行
finally:
    # 無論如何都會執行，就算 try 區塊中有 return 也會先執行 finally 之後再 return
```

* 在 Python 中，例外並不一定是錯誤。例如，當使用 for in 語法時，其實底層就運用到了例外處理機制。
* SystemExit、GeneratorExit、KeyboardInterrupt、StopIteration 等例外，更像是一種事件，代表著流程因位某個原因無法繼續而必須中斷。
* 所有例外都是 BaseException 的子類別。

## 測試

## 發行套件 Package distribution
在套件中新增一個名為 setup.py 的檔案，參考內容如下：
```
from distutils.core import setup

setup(
    name = “sample”,
    version = “1.0.0”,
    py_module = [“sample”],
    author = “triumph”,
    author_email = “foo@example.com”,
    url = “https://blog.suntri.com”,
    description = “some description"
) 
```
執行命令 ``` python3 setup.py sdist ``` 來建構發行套件檔。
執行命令 ``` python3 setup.py install ``` 將發行套件檔安裝為本地副本（local copy）
執行命令 ``` python3 setup.py upload ``` 將發行套件上傳至 PyPI

## 常用內建模組
* collections
* time
* datetime
* os # 處理檔案系統
* glob # 工作目錄下搜尋檔案
* csv # 處理 CSV 檔案
* json # 處理 JSON 檔案
* xml.dom, xml.sax # 處理 XML 檔案
* logging #
* re # 規則表示式 Regex
* unittest #  單元測試
* timeit # 效能評測
* threading, subprocess, multiprocessing # 多執行緒



## 細談群集 Collections
* Sequences type —> 均為 iterable, 共用方法有 len(), min(), max(), count()
    * Immutable —> 均為 hashable 所以可做為 set/dict 的鍵值
        * tuple
        * str
        * bytes
    * Mutable —> append(), clear(), copy(), extend(), insert(), pop(), remove(), reverse() 
        * list
        * range
        * bytearray
* Set type —> 元素均需為 hashable
    * Immutable 
         * forzenset
    * Immutable
        * set 
* Mapping type
    * deque # 在頻繁操作中比用 list 來的高效
    * nametuple
    * OrderedDict
    * defaultdict
    * Counter
    * ChainMap

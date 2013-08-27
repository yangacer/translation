中文版譯自 http://tecomp.sourceforge.net/index.php?file=doc/lang/tutorial.txt

# 簡介

本文件提供一個快速的 Eiffel 簡介，涵蓋了撰寫 Eiffel 程式的基本需求。

就如著名的 "The C programming Language" 作者 Brian Kernighan 與 Denis Ritchie
所言，學習程式語言最好的方法就是用該語言寫程式。因此我們會專注在一些簡單但實用
的程式。大部分簡介中的程式為上述名著中範例的 Eiffel 版本。

以下並非程式設計簡介而是針對如何以 Eiffel 撰寫程式，因此會假設讀者已具備撰寫
C, C++, 或 java 的基礎能力。

# Hello world

我們的第一個程式僅印出 

```
  Hello, world
```

要達到這個目的的 Eiffel 程式如下

```
class
    HELLO
create 
    make
feature
    make
        do
            io.put_string("Hello, world")
            io.put_new_line
        end
end
```

所有 Eiffel 程式碼都存在於類別 (class) 之中。
各個類別的程式碼存在於單一檔案。以上的程式碼
必須被寫到一個 "hello.e" (全小寫) 的檔案中。大小寫對 Eiffel 來說是相同的。然而
類別名稱通常都是以大寫表示，而特徵 (feature) 通常是以小寫表示。

編譯與執行 Eiffel 程式依系統與編譯器不同。本文的資訊以 UNIX 環境下的 tecomp 為
準。為編譯該程式，編譯器需要額外資訊，這些資訊可透過 ace-file 取得。例如前面程式
的 ace-file "hello.ace" 可含有以下資訊

```
root
    HELLO.make
cluster
    "./"
    "`path_to_tecomp_installation'/library/kernel"
end
```

目前的 tecomp 版本需給定安裝目錄的絕對路徑或相對路徑，未來版本會支援符號定義方式。你可以
鍵入以下命令來編譯與執行該程式

```
    tecomp hello.ace
```

然後它會印出

```
    Hello, world
```

接著來解釋一下這個程式。一個 Eiffel 程式由任意數量的類別組成。其中一個類別必須
是根類別 (root class) 、一個程序必須是根程序 (root procedure)。以上面的程式來說
根類別名為 HELLO 而跟程序為 make。

編譯器得知道如何找到類別；類別存在於通常以系統目錄實現的叢集 (cluster) 中。上面的
ace-file 定義了兩個叢集─ "./" (即當前目錄) 與 "`path_to_tecomp_installation'/library/kernel" 
(即 Eiffel 核心類別存放處。編譯器會在這些叢集中搜尋類別，當你的程式中
使用了一個搜尋不到的類別，編譯器則會提報錯誤。叢集中的 Eiffel 類別集合稱為 universe。

執行一個 Eiffel 程式從建立一個根型別的物件並呼叫其根程序開始 (此處型別與類別兩詞
為同義，它們只有在使用泛型時不同)。該跟程序可以創建任意數量的各種物件並呼叫任何已
建立物件的常式。

Eiffel 與許多現代語言相同，使用自由格式─程式語法中的空白字元是不重要的。退格
是為了人眼的可讀性而非編譯器限制。

名字像是 class, create, feature, do, 與 end 是語言中的關鍵字，它們是被保留的
；所有類別、特徵或變數不能使用這些保留字。

現在，我們來看看上面程式的結構

```
class
    HELLO   -- 類別名稱
create
    make    -- 建構程序
feature
    ... -- 類別特徵
end
```

這個框架表示一個 HELLO 類別的定義。型別為 HELLO 的物件只能以 make 建構程序建立。
所以特徵宣告在 feature...end 區塊。

一個特徵可以是常式或屬性。我們的簡單程式只有一個特徵稱為 make。該特徵為常式，
一個常式可以接受參數並回傳結果。沒有回傳結果的常式稱為命令 (command) 或程序
反之稱為查詢 (query)。make 這個常式為 HELLO 的建構程序是因為他被列在建構程序
的集合中(在 HELLO 中它是唯一一個)。

make 的程式碼

```
make
    do
        io.put_string("Hello, world")
        io.put_new_line
    end
```

只有兩個陳述式，```io.put_string("Hello, world")``` 呼叫 io 這個特徵；所有
Eiffel 中的類別都可以呼叫 io，因為該 io 為 STD_FILES 型別，此型別在 ANY 類別中
被定義而 ANY 是被所有的類別隱式繼承。既然 io 傳回的物件為 STD_FILES 型別
，STD_FILES 中的特徵得以被呼叫。

STL_FILES 有特徵 put_string 接受一個字串參數。該特徵 put_string 是一個命令因為
它沒有任何回傳值。它將字串參數輸出至標準輸出。而特徵 put_new_line 做的事情如字面
所述(譯：輸出換行)。

特徵的概念是 Eiffel 語言的基礎，因此以下解釋一些基本觀念：

一個特徵有兩種觀點─使用者或客戶端觀點，與實作觀點。由客戶端觀點，我們會區分查詢
與命令。一個查詢可接受零或多個參數並傳回一個值。雖然語言本身並未強迫，不過一個
查詢最好可以不含有副作用(譯：例如改變物件狀態，類似 C++ const member function 
概念)。一個命令一樣可以接受零或多個參數但沒有回傳值，它通常都被預期會改變物件
狀態。

因此，一個查詢可以屬性或函式實作，一個命令則需以程序實作。(?)

在兩個雙引號間的字串序列，像是 "Hello, world"，被稱為字串或字串常數。特殊字元如
換行或退格可藉由逸出序列包含在字串中。例如 %N 與 %T 為換行與退格的逸出序列。所以
我們也能這樣寫

```
io.put_string("Hello, world%N")
```

得到與下面程式同樣的輸出

```
io.put_string ("Hello, world")
io.put_new_line
```

果想要依照程式碼中的格式輸出含有多個換行的字串，可以使用逐字(verbatim)字串，
例如

```
io.put_string ("[
    usage: tecomp options ace_file
 
        options
            -t{p,v,e}{0,1,2,3}  trace parsing, validation,
                        execution with level 0,1,2,3
            -ws{0,1,2,3}        write statistics
         ]")
```

會完全依照程式碼中字串的格式做輸出(譯：保留所有空白字元)。

將多行逐字字串擺在 "[" 與 "]" 之間會移除每一行的"最長相同前綴空白字元
(longest common whitespace prefix)" (即向左對齊)。若擺在 "{" 與 "}" 間則會直接
複製所有字串而不移除任何前綴。逐字字串常數與此文件語法和許多 UNIX shell 
類似。

若想在原碼中的字串換行但不希望換行包含在字串時，可使用包裝字串。陳述式

```
io_put_string ("Hello, %
        %world")
```

與下面有相同輸出。

```
io.put_string("Hello, world%N")
```

在兩個 '%' 符號中的空白字元會在建構字串常數的過程中被忽略。

ANY 類別還有另一個特徵 print 用來將任何物件輸出到標準輸出。所以最短的 Hello world程式長得像這樣

```
class HELLO create make feature
    make 
        do
            print("Hello, world%N")
        end
end
```

# 區域變數、計算式與迴圈

下一個程式使用公式

```
degree Celsius = (5/9)( degree Fahrenheit - 32 )
```

印出以下的華氏攝氏溫度對照表：

```
    0       -17
    20      -6
    40      4
    60      15
    80      26
    100     37
    120     48
    140     60
    160     71
    180     82
    200     93
    220     104
    240     115
    260     126
    280     137
    300     148
```

此表可以下面的 Eiffel 程式輸出

```
class
    FAHR_CELSIUS 
        -- 印出華氏-攝氏對照表
create
    make
feature
    make
        local
            fahr: INTEGER -- 華氏溫度
        do
            from
                fahr := 0
            until
                fahr > 300
            loop
                io.put_character('%T')
                io.put_integer  (fahr)
                io.put_character('%T')
                io.put_integer  ((fahr-32) * 5 // 9)
                io.put_new_line
                fahr := fahr + 20
            end
        end
end
```

任何在 '--' 與行尾間的字元都會被編譯器忽略，這些字元可作為註解。

Eiffel 中，區域變數可在各個常式中的 do end 區塊前宣告。上面的程式中，區域變數
fahr 宣告為 INTEGER 型別；Eiffel 是強型別語言，因此任何變數，運算式等都必須
對應一個型別。 在 Eiffel 裡，一個 INTEGER 是介於 -2^31 至 2^31 - 1 之間的數值
，意即 INTEGER 至少有 32 位元。

INTEGER 為核心函式庫中的一個類別。

FAHR_CELSIUS 的程序 _make_  含有一迴圈，由初始化區段、終止條件、與迴圈本體組成。
在 Eiffel 裡一個迴圈運作如下。

- 執行初始化區段 (from ...)

- 測試終止條件 (until ...)

- 當條件為假，迴圈本體 (loop ...) 即被執行，之後再次測試終止條件

- 一旦終止條件為真，迴圈終止並執行迴圈(... end)之後的第一個陳述式

運算式 ```(fahr-32) * 5 // 9``` 是一個整數運算式，適用通用的計算規則。因此
fahr-32 必須加上括號。運算子 // 代表整數除法。

字元放在兩個單引號之前，'a' 代表字元 _a_. '%T' 代表特殊字元退格。

# 字元輸入與輸出

核心函式庫讓我們得以讀寫檔案；一個檔案可視為一連串以換行符號分隔的行，每一
行則為一個字元的序列。這樣的觀點是獨立於平台或作業系統的；在某些系統上
(如 Windows)，每一行其實是以兩個字元分隔，分別為 carriage return 與 linefeed。
核心函示庫會對應不同的系統，讓 Eiffel 程式處理的每一行都是以換行字元分隔。

每個 Eiffel 程式會開啟三個標準輸出入檔案，或稱文字串流(text stream)：
standard_input, standard_output, 與 standard_error。
依照預設，standard_input 對應到鍵盤，standard_output 與 standard_error 
對應到螢幕。當使用管線 (pipes) 或輸出入重導時，標準輸出入檔案也指向實體檔案
或暫存緩衝區。程式從 standard_input 讀取並寫出到 standard_output 時並不需要
關心檔案實際指向哪個資源。

類別 ANY 中的 io 查詢回傳一個 STD_FILES 型別的物件讓我們得以操作標準輸出入
檔案。STD_FILES 有許多特徵，以下的程式會用到的重要特徵包含：

```
end_of_file: BOOLEAN
    -- standard_input 在上一次讀取時是否已到達檔案結尾
read_character
    -- 從 standard_input 讀入下一個字元並讓它可透過 last_character 取得。
    -- 若沒有讀取到任何字元，令 end_of_file 為真。
    require
        not end_of_file
    ...
last_character: character
    -- 字元，在 read_character 被呼叫時讀取的字元

put_character (c: CHARACTER)
    -- 寫入字元 'c' 到預設輸出的結尾後

注意：EiffelStudio 的 STD_FILES 型別不支援 end_of_file。需使用
io.input.end_of_file。
```

上面只是複製核心函式庫文件 std_files.e 中的一部分。通常 Eiffel 的特徵都帶有
一小段標頭說明，描述特徵的用途與回傳值。

以上所述的四個特徵呈現出一般查詢與命令的分別。read_character 是一個命令，它
嘗試從 standard_input 讀取一個字元並讓 last_character 查詢可取得這個字元，或
是在狀況成立時令 end_of_file 為真，表示 standard_input 中沒有沒有更多的字元。
put_character 寫出一個字元到 default_output，預設為 standard_output。

對 put_character, put_string 等的呼叫可以交錯進行；輸出會依照呼叫的順序呈現。

命令 read_character 有一個前條件(precondiction) (譯：斷言(assertion)的一種)

```
    require not end_of_file
```

意即當上一次讀取輸入串流時遇到了檔案結尾，就不允許呼叫 read_character。

可透過 ace-file 來設定斷言的監控，例如

```
    root
        ...
    default
        assertions(all)
    cluster
        ...
    end
```

所有斷言，像是前條件都會在執行期被監控，這對程式的除錯是極佳的輔助。一旦程式
進入成熟並經過完整測試，我們可以改變監控模式為```assertion(no)```而不須更動
任何程式碼。

前條件乃條約式設計(Design by Contract)的一部分，這種設計在 Eiffel 中被廣放的
運用。一個特徵的前條件，建立了客戶端與供給端的一部分條約，並賦予客戶端一項義
務。

- 客戶端義務：只有在前條件成立時才呼叫一個特徵。

條約的另一部分，特過制定後條件(postcondition) 來賦予供給端責任。條約式設計
中，斷言可區分為前條件、後條件、類別不變性(class invariants)、迴圈不變性、
與檢定(checks)。相關資訊在之後的條約式設計章節會提及。

## 檔案複製

僅給予字元輸出入功能時，就能撰寫許多有用的程式。第一個程式是將所有的字元
從輸入複製到輸出。

```
class
    COPY
create
    make
feature
    make
        do
            from
                io.read_character
            until
                io.end_of_file
            loop
                io.put_character (io.last_character )
                io.read_character
            end
        end
end
```

程式碼說明了程式的用意：在迴圈中我們嘗試從輸入串流讀取第一個字元，終止條件
io.end_of_file 檢查是否到達輸入串流的結尾。只要結尾還未到達，剛剛讀取的字元
會以 io.pu_character(io.last_character) 寫入到輸出串流。

這個終止條件保證我們能滿足 read_character 的前條件，也就是不會超過串流結尾後
仍進行讀取。

## 字數計算

稍稍修改檔案複製程式可以讓我們計算輸入串流中的字元數目。

```
class 
    CHAR_COUNT
create
    make
feature
    make
        local
            nc: INTEGER -- 字元數目
        do
            from
                io.read_character
            until
                io.end_of_file
            loop
                nc := nc + 1
                io.read_character
            end

            io.put_string ("number of characters: ")
            io.put_integer (nc)
            io.put_new_line
        end
end
```

相對於將讀取到的字元寫入到輸出串流，我們遞增 nc 這個計數器，最後輸出讀取到的
字元數目。

Eiffel 裡面，大部分的型別有合理的預設值。所有 INTEGER 型態的變數會被初始化為
0 。因此並不需要明確初始化 nc 。

與 C 不同，Eiffel 並沒有遞增運算元(++)，你必須寫成```nc := nc + 1 ```以遞增
 nc 。

# 陣列與物件

為了展示陣列與物件的用途，我們來寫個統計數字、空白字元與其他字元出現次數的
程式。

輸入字元可區分為 12 個種類，為了儲存不同數字的出現次數，我們使用一個整數陣列
而非使用個別的變數。

```
class COUNT_DIGITS create make feature
    make
        local
            ndigit:     ARRAY[INTEGER]
            nwhite, nother: INTEGER
            c:      CHARACTER
            i:      INTEGER
        do
            create ndigit.make_filled(0, ('0').code, ('9').code)
                -- create array object
            from io.read_character until io.end_of_file loop
                c := io.last_character
                if '0' <= c and c <= '9' then
                    i := c.code
                    ndigit[i] := ndigit[i] + 1
                elseif c = ' ' or c = '%N' or c= '%T' then
                    nwhite := nwhite + 1
                else
                    nother := nother + 1
                end
                io.read_character
            end

            io.put_string ("digits = ")
            from i := ('0').code until i > ('9').code loop
                io.put_character(' ')
                io.put_integer  (ndight[i])
                i := i + 1
            end
            io.put_string(", white space = ");
            io.put_integer( nwhite )
            io.put_string(", other = ");
            io.put_integer( nother )
            io.put_new_line
        end
end
```

如果將以上程式碼當作輸入文字，程式輸出類似

```
    digits =  5 5 0 0 0 0 0 0 0 2, white space = 298, other = 58
```

宣告式```ndigit: ARRAY[INTEGER]```宣告 ndigit 是一個整數陣列，Eiffel 裡陣列
大小是在執行期指定而非編譯期。陳述式

```
    create ndigit.make_filled(0, ('0').code, ('9').code)
```

建立一個陣列物件，其下界索引 (lower index) 為字元 '0' 的編碼值，上界索引
為字元 '9' 的編碼值，即一個大小為 10 的陣列，且每個陣列元素的值為 0 。
相對與解釋 ARRAY 的特徵，我們來看看 array.e 中是怎麼定義 ARRAY 的。

```
class ARRAY[G] ... create make ... feature ...

    make_filled (value:G; l,u: INTEGER)
        -- 建立一個下界為小寫 L ，上界為 `u' 的陣列
        -- 並以 `value' 填滿之。
        -- 若 u < l 則陣列為空。

    lower: INTEGER
        -- 陣列索引下界
 
    upper: INTEGER
        -- 陣列索引上界
 
    count: INTEGER
        -- 元素個數
 
    item alias "[]" (i: INTEGER): G
        -- 第 i 個陣列元素
        require
             lower <= i
                         i     <= upper
        ...
        end
 
    put (v: G; i: INTEGER)
        -- 將 `v' 放到陣列的第 `i' 個位置
        require
             lower <= i and i <= upper
        ...
        end
end
```

這樣一來程式中與陣列相關的陳述式代表甚麼意思，應該就很清楚了。
ARRAY 類別是一個泛型 (generic) 類別，使用了泛型參數 G。 我們可套用任何型別到
G ，來宣告一個陣列。以下是合法的陣列宣告

```
a1: ARRAY[CHARACTER]
a2: ARRAY[INTEGER]
a3: ARRAY[ARRAY[INTEGER]]
```

a3 宣告了一個陣列的陣列，然而

``` 
a1: ARRAY[ARRAY] 
```

是不合法的。現在我們可以理解類別與型別的差異，(譯：牽涉到泛型時)ARRAY 是一個
類別，而 ARRAY[INTEGER] 是一個型別。對於非泛型類別來說，類別名稱同時代表一
個類別與型別。

item 這個特徵宣告時同時指定了別名 "[]"。這表示除了可以寫```ndigit.item(i)```
也可以使用```ndigit[i]```。

別名機制在基礎型別像是 INTEGER 也被使用。例如在 INTEGER 類別的原始碼中可以看
到類似宣告

```
plus alias "+" (other: INTEGER) : INTEGER
```

意即運算式```a + b```其實等同```a.plus(b)```也就是以 b 為參數，呼叫 a 物件的
特徵 plus (在 Eiffel 或稱為 target a) 。

回到數字統計程式。程式中的迴圈一次讀取一個字元，它必須決定一個字元是數字、空
白字元或者其他。作法上需要使用條件式，通常形式為：

```
if condiftion_1 then
    compound_1
elseif condition_2 then -- 零或更多 elseif 
    compound_2
else        --可有可無
    compound
end

注意：因為不須括號(譯：與 C 相較)，elseif 關鍵字不含任何空白！
```

一個複合 (compound) 是任一合法 Eiffel 陳述句的序列。條件式陳述句的行為與其他
語言，如 C, java 等相同。

字元能以通用的關係運算元作比較。每個字元有對應的編碼 (通常是 ascii 編碼) ，
CHARACTER 類別含有一個 code 查詢可回傳對應的字元編碼，條件
```'0' <= c and c <= '9' ```測試 c 是否為數字。

為表示特殊字元如換行等，Eiffel 字元常數能以逸出序列寫為 '%N' 與 '%T' 分別代
表換行與退格字元。

對常數使用特徵與進行運算時，有一點得先了解─字元常數 '0' 是一個型別為 CHARACTER
的運算式，因此，對字元常數所代表的物件，所有 CHARACTER 的特徵都可以被呼叫。

然而，無法直接以```'0'.code```來呼叫，因為這將造成模稜兩可的語法而無法解析。
要將常數當成物件來使用時，我們必須加上括號，意即，```('0').code```表示 '0' 字
元的編碼。

雖然這個程式有點矯情 (誰會想要統計檔案裡的數字？) 我們仍要寫一個不同版本來展
示更多 Eiffel 技巧 (譯：再矯情一下)。

上面的程式用條件式陳述句來判斷字元的種類。不考量 unicode 的狀況下其實我們只
有 256 個不同的字元，因此可用一個陣列來輔助判斷。

主要的想法是用一個大小為 256 的陣列，每一個陣列元數參照一個計數器，三個空白
字元的元素應該要參照到同一個空白字元計數器。對應數字的元素參照數字字元計數器
，其它元素則參照剩下的它類字元計數器。

設計一個計數物件不難：

```
class COUNTER_OBJECT feature
    value: INTEGER
    increment do value := value + 1 end
invariant
    value >= 0
end
```

類別與對應的型別只能是複製語意或是參照語意。COUNTER_OBJECT 類別使用參照語意。
INTEGER 類別是複製語意，它的宣告類似

```
expanded class INTEGER ... end
```

使用了 expanded 關鍵字來宣告具參照語意的類別。複製與參照語意間的差異性，對於
賦值、參數傳遞與使用 = 比對運算元而言，相當重要。

有參照語意的物件在被賦值與傳遞(call by reference)時，物件不會被複製，僅參照
被複製到目的地。比對運算元 = 只有在左右兩邊參照同一物件時為真。

若想要比對被參照物件的等價性(內容相同)，必須要使用等價比對運算元 ~ 。對於使用
複製語意的類別，比對運算元 = 與 ~ 是同義的。

在 COUNTER_OBJECT 中我們宣告了類別不變性。

```
invariant
    value >= 0
```

類別不變性可以宣告在類別的最後面 (最後一個特徵區塊後) 。它是一種恆常性條件，
表明在每個特徵被叫用前後，該恆常性條件必須被滿足。

當類別擁有許多屬性時，加上類別不變性是相當有利的。有時為了增加或改良特徵而
延伸(extend)類別，人們會忘了滿足不變性，這時打開斷言監控可以讓執行期監控
在不變性被違反時給予提示。

有了 COUNTER_OBJECT 類別，數字統計程式可以簡單完成，這裡先給出架構如下：

```
class COUNT_DIGIT2 create make feature {NONE}
    white_counter, other_counter: COUNTER_OBJECT
    char_counter:       ARRAY[COUNTER_OBJECT]

    make
        do
            initialize
            read_input
            write_statistics
        end
    iniialize
        ...

    read_input
        ...

    write_statistics
        ...
end
```

既然這個程式有點長，為了提供較好的可讀性與可維護性，我們將它切割成三個程序
initialize, read_input 與 write_statics。

initialize 程序初始化計數器物件與陣列，read_input 掃描輸入並妥善地遞增計數器
，而 write_statistics 在程式結束前給我們預期的輸出。

這三個程序須能夠存取計數器，因此我們將計數器作為屬性來使用，藉以避免參數
傳遞。

initialize 的程式碼如下：

```
initialize
    local
        co: COUNTER_OBJECT
        i: INTEGER
    do
        create white_counter; create other_counter
        create char_counter.make_filled(other_counter, 0, 255)

        from i:= ('0').code until i = ('9').code + 1 loop
            create co
            char_counter[i] := co
            i := i + 1
        end

        char_counter[('%N').code] := white_counter
        char_counter[('%T').code] := white_counter
        char_counter[(' ').code] := white_counter
    ensure
        char_counter.count = 256
    end
```

譯：digital_counter 不見了？為何是區域變數？

沒甚麼特別的地方，初始化計數器物件、前條件聲明 char_counter 已被恰當初始化。
之後的程序可以倚賴這個性質。

接著程序 read_input 更簡單

```
read_input
  require
    char_counter.count = 256
  local
    c: CHARACTER
  do
    from
      io.read_character
    until
      io.end_of_file
    loop
      c := io.last_character
      char_counter[c.code].increment
      io.read_character
    end
  end
```

每次讀取字元 c 時，只要透過```char_counter[c.code]```取得對應的參照並呼叫其特
徵 increment 即可。

前條件表明該程序預期 char_counter 陣列已被妥善初始化。若前條件未被滿足，
read_character 可能會越界存取 char_counter 陣列。

接著是 write_statistics

```
write_statistics
  require
    digit_counter.count = 10
  local
    i: INTEGER
  do
    io.put_string ("digits = ")
    from i := 0 until i = 10 loop
      io.put_character (' ')
      io.put_integer ( digit_counter[i].value )
      i := i + 1
    end
    io.put_string   ( ", white space = " )
    io.put_integer  ( white_counter.value )
    io.put_string   ( ", other = " )
    io.put_integer  ( other_counter.value )
    io.put_new_line
  end
```

TODO These 3 routine obviouly won't get compiled.

# 函式

目前我們僅止於撰寫程序；記得，泛用的名稱為常式。從使用者角度來看，常式一類是
命令，另一類是查詢 (有傳回值的特徵) ，查詢能實作為屬性或函式。

現在來寫個計算階乘數的函式，還記得數學定義是

```
n! = 1,           if n = 0
n! = n * (n-1)!,  if n > 0
```

Eiffel 裡可以撰寫遞迴函式，既然數學上以遞迴定義，以遞迴函式實作十分簡單

```
fac (n: INTEGER): INTEGER
  require
    n >= 0
  do
    if n = 0 then
      Result := 1
    else
      Result := n * fac(n - 1)
    end
  end
```

所有 Eiffel 函式都隱含一個預先宣告的區域變數 Result ，所以我們不需要額外宣告
此一變數，編譯器會自動幫你添加。Result 變數的型別取決於函式的傳回值型別，在
常式中你必須賦值給 Result (或建立 Result) ，Result 的值會被傳回函式的呼叫者。

注意：函式可以有零或多個參數，以下都是合法的函式

```
five: INTEGER do Result := 5 end
array_of_10_ints: ARRAY[INTEGER] do create Result.make_filled(0,0,9) end
```

使用者不需要知道 five 與 array_of_10_ints 是函式，他們可以視為無參數的查詢
，與屬性沒有分別。這是一致化存取(uniform access) 的原則，實作者可自行定奪
要以無參數查詢還是屬性實作，都不會影響到客戶端程式碼。

對於不欣賞遞迴函式的人們，我們也寫個迭代版本的 factorial

```
factorial_iterative (n: INTEGER): INTEGER
  require
    n >= 0
  local 
    i: INTEGER
  do
    from Result:=1; i:=0 until i = n loop
      i := i + 1
      Result := i * Result
    end
  end
```

撰寫風格注意事項：Eiffel 並不要求以分號作為陳述句的結尾或分隔，但是它們是被允
許的，語法上分號都是定義為可有可無的。上面的程式我們可以改用
```Result:=1 n:=1```而不使用分號。不過多個陳述句擺在同一行時，為了可讀性最好
還是加上分號。

遞迴版本的函式很容易可以驗證，因為他只是 Eiffel 語法版的數學定義；而驗證迭代
版本則需要思考一下：迴圈邊界是否正確？迭代次數是否正確？雖然這個迴圈並不複雜
，我們還是嚴謹一點地以 Eiffel 技巧來驗證一下。

關鍵想法是 Result 總是含有 ```i!``` 。迴圈從 ```Result=1``` 開始，從定義得知
這是 ```0!``` 的值。也就是說在迴圈初始時 ```Result=i!``` 是被滿足的。每次迭
代我們遞增 i 並將 ```i*Result``` 賦值給 Result，即```i*(i-1)!```。因此，若
```Result=i!``` 在迴圈初始時是合法的，在迴圈結尾時也會是合法的。

我們稱一個在迴圈開始與結束都為真的條件為迴圈不變性 (loop invariant)。

截至目前，我們可以說服自己 ```Result=i!``` 是一個迴圈不變性。

在迴圈結尾時，我們知道終止條件 i=n 為真，因此我們知道在迴圈結束時

```
i = n and Result = i!
```

等同

```
Result = n!
```

Eiffel 允許我們制訂迴圈不變性。既然我們已經有個可靠的遞迴函式 fac ，我們可以
完全倚靠 Eiffel 撰寫不變性(譯：a little bit overkill)

```
factorial_iterative ( n: INTEGER) : INTEGER
  require n >= 0
  local   i: INTEGER
  do
    from i:=0; Result:=1 invariant
      0 <= i and i <= n
      Result = fac(i)
    until i = n loop
    loop
      i       := i + 1
      Result  := i * Result
    variant
      n - i
    end
  end
```

我們增加了直觀的不變性條件聲明 i 在 0 到 n 之間迭代。你可以使用 Eiffel 的斷言
監控工具來檢查迴圈不變性，方法是在 ace-file 中添加```default assertions(all)```。

在上面的程式我們加入了一個迴圈可變式 (loop variant) 。這是個檢驗無窮迴圈的工
具，一個可變式是一個非負的整數運算式，他必須在每次迭代時至少減 1 。
這個可變式是剩下的迭代次數上界。由於 i 從 0 遞增到 n ，剩下的迭代次數為
```n-i``` 。

斷言監控模式下，Eiffel 執行期在每次迭代時，會檢驗可變式為非負並至少被減 1 。
若以上條件為假，同樣會告知可變式被違反。

# 更專注於類別

現在為止，我們只建立過根類別以及使用核心函式庫的一些類別、專注於演算法角度
撰寫一些如迴圈的控制結構。然而 Eiffel 真正的威力在於他能用來製作各式的類別
與結合這些類別。

這個章節的範例將展示不同的類別用法；第一個演示繼承的一些使用方式，第二個則是
泛型示範，第三個則用類別來呈現複數。

## 一個矩形是一種形狀

在圖形世界裡我們會處理圖形物件，像是矩形、圓形等。我們稱這些圖形物件為形狀。

形狀有一些共通點，可以被移動、顯示、疊在其他形狀之上。如果我們每次使用形狀時
都要先區分他是矩形或圓形，程式碼會變得十分雜亂。

Eiffel 允許使用者定義 SHAPE 抽象類別，擁有一些共通特徵但不真正實作這些特徵。
更加明確的類別像是 RETANGLE 則繼承 SHAPE 並實作抽象類別裡的特徵。

為了範例的簡潔性，我們為 SHAPE 定義四個抽象特徵與一個具體特徵如下。

```
deferred class SHAPE feature
  x_left:   INTEGER deferred end
  x_right:  INTEGER deferred end
  y_lower:  INTEGER deferred end
  y_upper:  INTEGER deferred end
  write_dimensions do ... end
invariant
  x_left <= x_right
  y_lower <= y_upper
end
```

四個抽象特徵讓形狀在 x, y 軸上可自由延伸，它們並沒有實作，往常的 ```do..end```
區塊被 ```deferred end``` 取代了。特徵的實作被延後到繼承了 SHAPE 的類別裡。

這四個特徵被稱為延遲特徵(deferred feature)。

SHAPE 類別不能用來建立物件 (譯：抽象類別無法實體化) ，因為這樣的物件有未定義
的特徵。一個有延遲特徵的類別本身也會被延遲。因此我們必須寫成 ```deferred class SHAPE```
而不僅僅是 ```class SHAPE``` 。擁有延遲特徵的類別必須標註 deffered 是語言定
義的規則。

你可能覺得編譯器已經知道特徵被延遲，不需要多加一個標註到類別名稱前，但是語言
這樣規定是為了清楚表明一個類別是抽象的。

同時注意一下，SHAPE 類別沒有任何建立程序，因為對不能有實體物件的抽象類別，建
立程序是沒有意義的。

雖然該類別只宣告了四個抽象特徵，但是有一些不變性已經可以確立了。謹記類別不
變性是一種類別特徵的恆常性關係。

SHAPE 類別將這個恆常性需求，套用至所有的衍生類別。意即每個衍生類別都得滿足這
個類別不變性。此外，所有衍生 (譯：類別裡的) 的特徵也繼承了同樣的類別不變性。

類別不變性是一種斷言，可在執行期監控。在建立類別與使用公開特徵的前後，類別不
變性都必須被滿足。這可以強力保證任何常式的修改不會違反這個變性。

有了四個抽象特徵 x_left, xright, y_lower, y_upper 後，我們得以撰寫 
write_dimensions 程序將形狀的 x, y 軸資訊寫入到標準輸出。write_dimensions
實作如下

```
write_dimensions
  do
    io.put_string ("shape with dimensions x = ")
    io.put_integer(x_left)
    io.put_string ("..")
    io.put_integer(x_right)
    io.put_string (" and y = ")
    io.put_integer(y_lower)
    io.put_string ("..")
    io.put_integer(x_upper)
    io.put_new_line
  end
```

如你所見，即使是延遲特徵也能被 write_dimensions 使用。這個 SHAPE 類別有時可稱
做局部實作 (partial implementation) 。他實做了 write_dimensions 但是將延遲特
徵的實作延後到衍生類別裡。

現在來定義一種形狀，矩形。矩形可以直接以左右上下的維度定義

```
class 
  RECTANGLE
inherit
  SHAPE
create
  make
feature
  x_left:   INTEGER
  x_right:  INTEGER
  y_lower:  INTEGER
  y_upper:  INTEGER
feature {NONE}
  make(x1, y1, x2, y2: INTEGER)
    -- 建立一個以 (x1, y1) 為左下角，(x2, y2) 為右上角的矩形
    require
      x1 <= x2
      y1 <= y2
    do
      x_left  := x1;  x_right := x2
      y_lower := y1;  y_upper := y2
    end
end
```

RECTANGLE 繼承延遲類別 SHAPE 並實作其中的延遲特徵。以 Eiffel 的術語來說，重新
宣告一個延遲特徵並啟用他，稱為啟用特徵 (effecting a feature) 。

RECTANGLE 選擇以屬性來宣告延遲特徵。

由於 RECTANGLE 重新宣告並啟用了所有延遲特徵，這個類別不再是一個延遲類別。
RETANGLE 類別提供了建立程序 make ，接受左下、右上座標並初始化屬性。

為了滿足父類別 SHAPE 的不變性，建立程序 make 設定了參數的前條件。

藉由在 feature 關鍵字後加上 ```{NONE}``` 的方式，我們讓 make 變成一個私有函式
，加上我們將他列為建構標頭區塊中 (譯：create ...)，make 就只有在建構過程中能
被呼叫。建構函式常會以這種方式實作，藉以避免被建構以外的程序呼叫。

不過這並非鐵則，你仍可以開放外部直接呼叫建構函式，但是最好確保在物件的生命週
期中，任意呼叫建構函式不會造成任何異常。

另一種形狀，圓形，定義成由圓心及半徑構成。因此我們需要 x_center, y_center 及
radius 屬性。由這些屬性我們可以計算出 x_left, x_right, y_lower 及 y_upper ，
在 CIRCLE 類別裡，x_left 等並非屬性，而是以查詢實作。

類別 CIRCLE 定義如下

```
class
  CIRCLE
inherit
  SHAPE
create
  make
feature
  x_center: INTEGER
  y_center: INTEGER
  radius:   INTEGER

  x_left:   INTEGER do Result := x_center - radius end
  x_right:  INTEGER do Result := x_center + radius end
  y_lower:  INTEGER do Result := y_center - radius end
  y_upper:  INTEGER do Result := y_center + radius end
feature {NONE}
  make ( x, y: INTEGER; r: INTEGER )
    -- (圓心、半徑)
    require
      r >= 0
    do
      x_center  := x
      y_center  := y
      radius    := r
    end
end
```

RECTANGLE 與 CIRCLE 類別以不同的技巧啟用父類別裡延遲特徵，兩種方式都是合法的。
完整的實作，不論是記憶體型的屬性，或計算型的函式，都能延遲到子類別再定義。

有了以上兩個類別，我們就能建立兩種圖形物件。由於兩種類別都繼承了 SHAPE 類別
，我們能將一個 RECTANGLE 型別的變數，賦值給 SHAPE 型別的變數。我們稱 
RECTANGLE 遵循 (conforms) SHAPE 。

```
local
  s: SHAPE
  r: RECTANGLE
  c: CIRCLE
do
  create r.make(0, 0, 10, 20)
  create c.make(5, 5, 30)

  s := r -- OK, RECTANGLE 遵循 SHAPE
  s.write_dimensions

  s := c -- OK, CIRCLE 遵循 SHAPE
  s.write_dimensions
end
```

範例較為簡單，你也可以宣告一個```ARRAY[SHAPE]```，將兩個不同的圖形物件，放
到陣列中。

若 SHAPE 定義了一些額外的延遲特徵如下

```
deferred class SHAPE feature
  ...
  move (x, y: INTEGER)
    -- 將物件向左移動 x，向右移動 y
    deferred end
  draw 
    -- 繪製圖形
    deferred end
  wipe_out 
    -- 清除圖形
    deffered end
end
```

並在子類別裡實作以上延遲特徵，我們就能簡單地撰寫一個移動所有圖形物件的功能

```eiffel
class GRAPHICAL_SYSTEM feature
  ...
  objects: ARRAY[SHAPE]
  move_all (x, y: INTEGER)
    -- 移動所有物件
    local
      i: INTEGER
    do
      from i:=objects.lower until i > objects.upper loop
        objects[i].wipe_out
        objects[i].move (x, y)
        objects[i].draw
        i := i + 1
      end
    end
end
```

由於 SHAPE 是一個參照類別，每個陣列元素都是參照物件。

```
                 +----+
   objects -->   |    |  --> rectangle object
                 +----+
                 |    |  --> circle object
                 +----+
                 |    |  --> rectangle object
                 +----+
                 |    |  --> triangle object
                 +----+
                 |    |  -->  ...
                 +----+
                 |    |  -->  ...
                 +----+
```

## 矩陣物件

矩陣是種矩形的數字編排法

```
       col 1      col 2     col 3

row 1:   1         10         -5
row 2:  -1          2         -7
```

上面的矩陣有兩列三行，每個元素為整數值。 ```a[1, 3]``` 則用來定位第 1 列第 3 
行的元素。

我們以 a.rows 與 a.columns 來取得列與行的總數。

兩個矩陣可以被相加減、互乘。

做矩陣加減法的兩個矩陣必須有相同維度，c = a+b 可根據以下數學式計算

```
c[i, j] = a[i, j] + b[i, j]
```

c = a*b 乘法運算的數學式為

```
c[i, k] = a[i, 1] * b[1, k] + a[i, 2] * b[2, k] + ... a[i,n] * b[n, k]

  where n = a.columns = b.rows
```

目前為止是矩陣的基本數學性質。

現在來建立一個可以 Eiffel 的 MATRIX 類別，我們並不需要為了不同的元素型別各
寫一種矩陣類別，在這裡泛型可派上用場，省略細節後的 MATRIX 類別如下

```eiffel
class
  MATRIX[G->NUMERIC creat default_create end]
create
  make
feature {NONE}
  make (r, c: INTEGER)
    -- r 列數, c 行數
    ...
    ensure
      rows = r
      columns = c
    end
feature
  rows: INTEGER ...
  columns: INTEGER ...

  is_valid_row (i: INTEGER) : BOOLEAN
    do Result := 1 <= i and i < rows end
  is_valid_column (j: INTEGER) : BOOLEAN
    do Result := 1 <= j and j <= columns end
  item alias "[]" (i, j: INTEGER) : G
    -- 在第 i 列第 j 行的元素
    require
      is_valid_row (i)
      is_valid_column (j)
    ...
    end
  put (e1: G; i,j: INTEGER)
    -- 將元素 e1 放到 [i, j] 位置
    require
      is_valid_row (i)
      is_valid_column (j)
    ...
    ensure
      item (i, j) = el
    end
  plus alias "+" (other: like Current): like Current
    require
      rows = other.rows
      columns = other.columns
    ...
    end
  minus alias "-" (other: like Current): like Current
    require
      rows = other.rows
      columns = other.columns
    ...
    end
  product alias "*" (other: like Current): like Current
    require
      columns = other.rows
    ...
    end
feature {NONE} -- 實作
    ...
end
```

這個架構裡可以看到數個 Eiffel 的觀念

### 1. 制約式泛型 (Constrained genericity)

```class MATRIX[G->NUMERIC create default_create end]``` 表明 MATRIX 是個
泛型類別，可以用來定義型別為 ```MATRIX[INTEGER]``` 或 ```MATRIX[REAL]```
的變數。藉由設定 "G 必須遵循 NUMERIC 型別" 這項制約，我們得以避免 
```MATRIX[BOOLEAN]``` 被使用。

NUMERIC 是一個核心函式庫中的抽象類別，將加減乘等運算宣告為延遲特徵。類別
INTEGER 與 REAL 繼承了 NUMERIC (即遵循 NUMERIC) 並實作數值運算元。

進一步我們希望所有 MATRIX 都能自行初始化，藉由 
```create default_create end```指定泛型 MATRIX 都需具備 default_create 這個
建構程序。預設建構式的語法為 ```create cp1, cp2, ... end``` 且是可有可無的。
可用來表明 cp1, cp2 在此泛型類別中必須是建構程序。

### 2. 運算元別名 (Operator aliases)

使用 Eiffel 的別名機制，我們可以寫出 a+b, a-b 與 a+b 。

### 3. 下標別名 (Bracket alias)

```a[i, j]``` 的定址語法相較於 ```a.item(i, j)``` 來說可讀性較高。須注意
每個類別最多只能有一個下標別名。這裡我們使用在 item 特徵上。

### 4. 錨點型別 (Anchored types)

我們可以定義 plus 特徵為

```eiffel
plus alias "+" (other: MATRIX[G]): MATRIX[G]
```

但你可以看到我們實際上如此定義

```
plus alias "+" (other: like Current): like Current
```

在 MATRIX 類別中 MATRIX[G] 與 like Current 等價，因為目前型別為 MATRIX[G]。

然而當我們定義 ```class SPECIAL_MATRIX inherit MATRIX ... end``` 時，我們希望
SPECIAL_MATRIX[G] 的運算傳回型別為 SPECIAL_MATRIX[G] 的回傳值。

在 plus 等特徵的參數與回傳值，使用錨點型別即能達到上述的效果。
```like Current``` 設定一個對應到 **目前型別** 的錨點，在 SPECIAL_MATRIX[G]
裡，目前型別即 SPECIAL_MATRIX[G] 。

錨點可以設置到 Current 或其他類別的特徵 (必須為查詢) 。

### 5. 條約式設計 (Design by Contract)

與其他範例一樣，前條件與後條件可用來說明允許的、預期的常式行為。

plus, minus 與 product 裡的前條件避免我們對兩個不相容的矩陣做運算。

斷言在 Eiffel 程式設計中佔有非常重要的地位。

第一，它們能夠說明你的意圖，使用者能透過閱讀常式的標頭註解、前條件、後條件
，對常式的功能有精確的了解，而不需要閱讀實作碼。

第二，它們能輔助程式的除錯。開發中的程式通常會開啟所有種類的斷言監控，若有
足夠多的斷言，一個錯誤通常能在接近源頭的地方被截取。這能顯著加速除錯。

第三，若一個常式本身必定滿足後條件，需要檢查的只有客戶端的呼叫方式。
這時斷言是最好的理論根據。


現在我們得找個適合的實作來儲存矩陣的元素。若我們定義

```eiffel
feature {NONE} -- 實作
  matrix: ARRAY[ARRAY[G]]
end
```

我們能以 matrix[i][j] 或 matrix.item(i).item(j) 存取 (i, j) 的矩陣元素。

make 特徵則妥善地初始化矩陣

```eiffel
feature {NONE}
  make (r, c: INTEGER)
    local
      i: INTEGER
      one_row: ARRAY[G]
      default_value: G
    do
      create matrix.make_empty(r, 1)
      from i:=1 until i > r loop
        create one_row.make_filled(default_value, 1, c)
        matrix.extend_rear(one_row)
        i := i + 1
      end
    ensure
      rows = r
      columns = c
    end
```

我們使用 matrix 的行列數作為 rows 與 columns 特徵的實作

```eiffel
rows: INTEGER
  do Result := matrix.upper end
columns: INTEGER
  do
    if rows > 0 then
      Results := matrix[1].upper
    end
  end
```

元素存取常式 item 與 put

```eiffel
item alias "[]" (i, j: INTEGER): G
  require
    is_valid_row (i)
    is_valid_column (j)
  do
    Result := matrix[i][j]
  end
put (e1: G; i, j: INTEGER)
  require
    is_valid_row (i)
    is_valid_column (j)
  do
    matrix[i][j] := e1
  ensure
    item(i, j) = e1
  end
```

以下呈現乘法運算的實作，其他則留給讀者做練習

```eiffel
product alias "*" (other: like Current): like Current
  require
    columns = other.rows
  local
    i,j,k: INTEGER
  do
    create Result.make ( rows, other.columns)
    from i:=1 until i>rows loop
      from k:=1 until k > other.columns loop
        from j := 1 until j > columns loop
          Result[i,k] := Result[i, k] + Current[i, j] * other[j, k]
          j := j + 1
        end
        k := k + 1
      end
      i := i + 1
    end
  end
```

### 習題：

- 撰寫 MATRIX 的類別不變性，確保每一列長度相同

- 實作特徵 transporsed 傳回一個旋轉過的矩陣 
(即 ```a.transposed[i, j] = a[j,  i]``` ) ，並添加後條件驗證 rows 與 columns。

## 複數

複數包含兩個實數，用以表示實部與虛部，通常 COMPLEX 類別架構會是

```
class COMPLEX feature
  ...
  real: REAL
  imag: REAL
  ...
end
```

不過，一般數值型別會使用複製語意，讓變數賦值時進行複製而非參照，因此定義成
```expanded class COMPLEX``` 較為恰當。

此外，核心函式庫的 NUMERIC ─ 數值類別的基底型別 ─ 其定義了數個數值特徵如下

```
deferred class NUMERIC feature
  zero: like Current 
    -- 加法單元
    deferred end
  one : like Current
    -- 乘法單元
    deferred end
  plus alias "+" (other: like Current): like Current
    deferred end
  minus alias "-" (other: like Current): like Current
    deferred end
  product alias "*" (other: like Current): like Current
    deferred end
  divided alias "/" (other: like Current): like Current
    require
      good_divisor : divisible (other)
    deferred end
  identity alias "+": like Current
    deferred end
  negated alias "-": like Current
    deferred end
  divisible (other: like Current): BOOLEAN
    deferred end
end
```

如 NUMERIC 這種類別也稱為行為類別 (behaviour class) ，它們定義子類應滿足的
行為。用戶得以預期所有繼承行為類別的衍生類別，將妥善實作延遲特徵。

實作 COMPLEX 類別這項任務，實際上就是啟用 NUMERIC 裡定義的延遲特徵。

```
expanded class COMPLEX inherit NUMERIC create
  make, default_create
feature {NONE}
  make (r, i: REAL) do read := r; imag := i end
feature
  read: REAL
  imag: REAL
  one : like Current do create Result.make(1.,0.) end
  zero: like Current do end

  puls alias "+" (other: like Current): like Current
    do
      create Result.make(real + other.real, imag + other.imag)
    end
  minus alias "-" (other: like Current): like Current
    do
      create Result.make(real - other.read, imag - other.imag)
    end
  product alias "*" (other: like Current): like Current
    do
      create Result.make(real * other.real - imag * other.imag,
                         imag * other.real + read * other.imag)
    end
  divided alias "/" (other: like Current): like Current
    local
      a, b, c, d: REAL
      r, i:       REAL
      n:          REAL
    do
      a := real; b:= imag;
      c := other.real; d := other.imag
      r := a*c + b*d
      i := b*c - a*d
      n := c*c + d*d
      create Result.make (r/n, i/n)
    end
  identity alias "+": like Current
    do Result := Current end
  negated alias "-": like Current
    do create Result.make(-real, -imag) end
  divisible (other: like Current): BOOLEAN
    do Result := other /~ zero end
end
```

注意，這裡有些確保浮點運算正確性的技巧。由於實數是以 IEEE 浮點數格式儲存，
所以 0 有分正負，兩個值代表同一數值但在電腦裡是不同的。布林運算式 
```0. ~ -0.``` 為假。因此，特徵 divisible 在參數 other 含有負零值時，會傳回
不正確的結果。

正確的實作會將 other 的絕對值與一個極小的實數做比較，這裡我們忽略這個問題。

值得一提的是建構程序包含了 make 與 default_create 。前者用來初始化一個複數，
後者並未在 COMPLEX 裡定義，它繼承自核心函式庫的 ANY ，不做任何事。

不做任何事的建構式乍看之下令人訝異，然而，一旦了解 COMPLEX 僅有的兩個屬性
皆為 REAL 型別，且 REAL 可以自我建構 (不明確初始化即初始化為零) 。不做任何
事就代表實部與虛部會初始化為零。

基於同樣事實，我們可以將特徵 zero 寫成

```
zero: like Current do end
```

而不是

```
zero: like Current do create Result.make(0.,0.) end
```

未明確初始化的變數 ```var``` 會在第一次被使用前，被系統如下初始化

```
create var.default_create
```

但前提是這個變數的型別能自我建構。將 default_create 指定為建構程序之一即代表
這個意義。

額外的好處是我們的 COMPLEX 類別，遵循了 NUMERIC 類別，因此得以套用到前一章節的
MATRIX 泛型，具現化為 MATRIX[COMPLEX] 型別。

# 鏈結串列 (Linked lists)

鏈結串列是基礎的資料結構，優點是能快速地在頭尾兩端插入元素，缺點是隨機存取
元素成本高昂。

資料結構的概念如下

```
  +-------+
  | List  |
  +-|---|-+
    |   |
    |   \----------------------------------------------\
    |                                                  |
    V                                                  V
  +-------+     +-------+    +-------+             +-------+
  | first |---->|       |--->|       |--->.....--->| last  |---X
  +-------+     +-------+    +-------+             +-------+
```

串列含有兩個參照，一個指涉第一節點，另一個則指涉最後結點。
第一節點內含有一個參照，指涉第二個節點，第二節點也含有參照，
指涉第三節點，以此類推。
最後一個節點則含有空參照。

當串列為空，參照到第一與最後節點的參照皆為空參照。

節點為元素的容器，含有一個元素與下一個節點的參照。

移除第一節點的動作很簡單，將原本參照到第一節點的參照，改參照第二節點即可。

現在考慮一下邊界狀況，即空串列與只有一個元素的串列，可圖是如下。

```
    empty     one element
  +-------+    +-------+
  | List  |    | List  |
  +-|---|-+    +-|---|-+
    x   x        v   v
               +-------+
               |       |
               +-------+
```

節點的設計很簡單

```
class LINKABLE[G] create put feature
    item: G
    next: ?like Current
    put (element: G)
        do item := element end
    put_next (n: ?like Current)
        do next := n end
end
```

唯一的新東西是 like Current 前的問號，型別 ?T 稱為可卸除型別 (detachable type)
Eiffel 裡所有型別預設為 "已附帶 (attached)"，變數、運算式等必須附帶到物件上。
但以串列來說，我們需要一個可以指涉到 "無" 的型別。可卸除型別即符合這個需求。

若變數 v 的型別為 ?T ，我們可令 ```v:= Void``` 或執行 
```v /= Void``` 布林運算，測試這個變數是否附帶在一個物件上。

對於已附帶型別的變數，Eiffel 編譯器會驗證他們總是附帶至實體物件，否則會視為
錯誤。

鏈結骨架的外觀如下

```
class
  LINKED_LIST[G]
feature {NONE}
  first_linkable: ?LINKABLE[G]
  last_linkable : ?LINKABLE[G]
feature
  is_emptr: BOOLEAN
    do Result := first_linkable = Void end
  first: G
    ...
  last: G
    ...
  extend_front ( element: G )
    ...
  extend_rear ( element: G )
    ...
  remove_first
    ...
invriant
  (first_linkable = Void) = (last_linkable = Void)
end
```

我們的鏈結串列中有兩個參照屬性，分別指涉第一個與最後一個節點。
它們只能都指涉到 Void 或指涉到 LINKABLE[G] 節點。這一點我們透過類別不變性
確認。

錯誤的函式 first 實作：

```
first: G
  require
    not is_empty
  do
    Result := first_linkable.item
  end
```

由於 first_linkable 是可卸除型別，可能為 Void ，因此編譯器不會接受運算式 
```first_linkable.item``` 。

然而我們已經透過 not is_empty 前條件確認 first_linkable 不可能是 Void ，因此
我們希望讓編譯器能認可上面的操作，這時我們必須添加 check 斷言

```
first: G
  require
    not is_empty
  do
    check {l:LINKABLE[G]} first_linkable end
  Result := l.item
  end 
```

運算式 ```{x:T} expr``` 是一種物件測試，檢查 expr 是否附帶到一個 T 型別的
物件上。若結果為真，該物件會被附加到區域變數 x 。

這個變數會再被宣告的區域中存活，以此例而言，l 會存活到常式結束。一個 check 的
區域變數是唯讀的。

離開了存活區域就無法使用該變數了，如：

```
...
if condition then
  ...
  check {x:T} expr end
  ...
  x.some_feature -- 合法
  ...
end
x.some_feature -- 非法
...
```

函式 last 與 first 幾乎相同。

插入元素到串列前端的程序不難，我們需建立一個新的節點用來存放新的元素，接著
將 first_linkable 指涉到原有的第一個節點。另外，這個程序也得考量串列為空的
狀況，以確保正確性。

```
extend_front (element: G)
  local
    new_cell: LINKABLE[G]
  do
    create new_cell.put (element)
    if is_empty then
      first_linkable := new_cell
      last_linkable := new_cell
    else
      new_cell.put_next (first_linkable)
      first_linkable := new_cell
    end
  ensure 
    first = element
  end
```

插入新元素到串列結尾我們再次使用物件測試。

```
extend_rear (element: G)
  local
    new_cell: LINKABLE[G]
  do
    create new_cell.put (element)
    if is_empty then
      first_linkable := new_cell
      last_linkable := new_cell
    else
      check {l:LINKABLE[G]} last_linkable end
      l.put_next (new_cell)
      last_linkable := new_cell
    end
  ensure
    last = element
  end
```

我們使用物件測試 ```{l:LINKABLE[G]} last_linkable``` 來確保 last_linkable
附帶在一個物件實體上。注意 else 的部分只有在串列非空時才會執行。

### 習題

- 實作 remove_first

- 實作 remove_last 。為何這個功能較複雜？

- 增加 ```count: INTEGER``` 屬性，紀錄串列內的元素個數。

- 實作 ```item alias "[]" (i:INTEGER): G``` 函式，傳回第 i 個元素，假設 i 從
0 開始

- 保留 LINKABLE 類別不動的狀況下，修改 LINKED_LIST 類別，讓下面的迭代可以快
速存取串列

```
from i:=0 until i=list.count loop
  list[i].do_something
  i := i+1
end
```

# 中英對照

```
條約式設計 designed by contract
類別 class
物件 object
泛型 generic type, genericity
程序 procedure
常式 routine
函式 function
查詢 query
命令 command
特徵 feature
叢集 cluster
逐字字串 verbatime string
逸出序列 escaped sequence
屬性 attribute
前條件 precondition
後條件 postcondition
不變性 invariant
恆常性條件 consistency condition
制約式 constrained
參照 reference
啟用 effect
遵循 conform
已附帶 attached
可卸除 detachable
```

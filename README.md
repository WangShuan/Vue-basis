# VUE.js

## 第一單元 - `basic` 目錄

## 1. vue 開發者工具使用方式（dev.html）

在 VS code 中安裝延伸模組 `Preview on Web Server` 通過快捷鍵 `contral+shift+L` 可以在線上網頁打開檔案

或

在 VS code 中安裝延伸模組 `Live Server` 通過右下角的 `Go Live` 也可在線上網頁打開檔案

然後在 VS code 中通過 `command+P` 搜尋網頁網址打開指定網頁的編輯頁面 

  * 這裏我們打開 `dev.html` 在該頁面最下面可以看到 `script` 標籤 
 
  * 裡面的 `var demo = new Vue({})` 就是創建一個 Vue 實例

在 `Google Chrome` 瀏覽器中安裝 `Vue.js devtools` 就能使用開發者工具 通過在頁面點擊右鍵的檢查中可以發現多了一個 `Vue`

  * 點進去 `Vue` 可以看到 `<root>` 標籤 該標籤就是 Vue 實例的 `#demo` 對象

  * 在 `<DemoGrid>` 裡面有一個 `filterKey` 是用來查詢資料的 當你在網頁中的輸入框輸入資料會同時傳遞到 `filterKey` 中 並過濾資料 該 `filterKey` 是一個方法

## 2. 自行創建 Vue 實例（instant.html）

打開 `instant.html` 的編輯畫面 然後到最下面的 `script` 標籤 創建一個變量 `app = new Vue({})`

然後在 html 創建一個 `div#app` 元素 通過在實例對象中添加 `el:'#app'` 把它綁定到 Vue 實例中

再回到 vue 實例對象中添加 `data:{text:'This is test text.'}` 創建一個 `text` 資料

然後回到 `div#app` 元素中 在內容處添加上 `{{ text }}` 

開啟瀏覽器 就能看到在該 div 中添加了 data 中的 text 那段文字內容了

  * 在 Vue 實例中雖然也可以通過 class 名稱定位到元素上 但 Vue 本身有規定一次只能綁定一個元素 所以基本上還是建議以 ID 定位元素

  * Vue 實例是可以同時創建多個的 但是不能使用巢狀建立 否則只有第一個有用 其他的都會失效

    * 巢狀代碼如下：

    ```html
    
    <div id="demo1">
      {{ text }}
      <div id="demo2">
        {{ text }}
      </div>
    </div>

    <!-- 
    結果會只輸出 demo1 和其 text 
    且 demo2 的 text 為 demo1 的 text
    -->
    
    ```

## 3. 雙向綁定資料（MVVM.html）

在 vue 實例中 `data` 物件就是一個 `model` 我們可以通過添加 `v-model` 屬性 來修改 `model` 中的資料內容

比如 vue 實例中的 data 物件裡有一個 message 當你在 input 標籤上添加屬性 `v-model="message"` 頁面上的 `{{ message }}` 就會被同步修改

除了通過雙花括號引入資料內容外 也可通過在標籤中添加 `v-text` 屬性引入資料內容

  * `{{}}` 會連雙花括號一起渲染顯示到頁面上 變成 `"    xxx      "`

  * `v-text` 只會輸出文本內容 內容是啥就傳啥 即使傳入 `html標籤` 也不會自動解析成 html

  * `v-html` 則是會將內容以 html 解析後渲染到頁面中

```html

<div id="app">
  {{message}}
  <p v-text="message"></p>
  <input type="text" v-model="message">
  <div v-html="html"></div>
</div>

<script>
  var app = new Vue({
    el: '#app',
    data: {
      message: '我是文本內容',
      html: '<h1>我是 h1 標籤</h1>'
    }
  })
</script>

```

## 4. 動態屬性指令（directive.html）

當我們要在標籤上通過 vue 來操作屬性時 無法直接使用雙花括號來添加

這時候可以使用 `v-bind` 屬性 其簡寫為冒號 使用方式如下：

```html

<div id="app">
  <!-- 完整寫法 -->
  <img v-bind:src="imgSrc" v-bind:class="className" alt="">
  <!-- 簡寫寫法 -->
  <img :src="imgSrc" :class="className" alt="">
</div>

<script>
var app = new Vue({
  el: '#app',
  data: {
    imgSrc: '',
    className: ''
  }
})
</script>

```

## 5. 動態產生多筆資料（if_for.html）

可以通過在標籤中添加 `v-for` 屬性來產生多筆資料

其中 `item` 是數據中的單個資料對象 而 `index` 則是該資料對象的索引

這個 `in list` 的 `list` 就是 data 中要被 for 循環的數據陣列

另外可以通過標籤中添加 `v-if` 獲取判斷條件後為 true 的資料內容

代碼如下：

```html

<li v-for="(item, index) in list" v-if="item.age > 18">
  {{ index +1 }} - {{item.name}} , {{item.age}} years old.
</li>

<script>
  var app = new Vue({
    el: '#app',
    data: {
      list: [
        { name: 'John',age: 18 },
        { name: 'Amy',age: 26 },
        { name: 'Lee',age: 30 }
        ]
    }
  })
</script>

```

## 6. `methods` 操作事件方式與修飾符（v_on.html + modifiers.html）

在 vue 實例對象中添加一個 `methods` 對象 裡面放入創建的函數方法

通過 `v-on:事件="函數名"` 就可以使標籤觸發該函數了 `v-on:事件` 可簡寫為小老鼠 `@事件`

在函數中若要操作 data 裡的 `xxxx 物件` 則可通過 `this.xxxx` 定位獲取該物件

  * 若要在函數中操作 data 裡的物件 須在 data 中預先定義好物件 ***它無法自己生成***

補充修飾符：

  * 在 js 中常用的 `event.preventDefault()` 在 vue 中可直接寫 `事件.prevent` 使用

  * 在 js 中 enter 觸發事件的 `keyup.code===13` 可直接寫成 `事件.enter` 使用

代碼如下：

```html

<!-- 完整寫法 -->
<input type="text" class="form-control" v-model="text" v-on:keyup.enter="reverseText">
<button class="btn btn-primary mt-1" v-on:click.prevent="reverseText">反轉字串</button>

<!-- 簡寫寫法 -->
<input type="text" class="form-control" v-model="text" @keyup.enter="reverseText">
<button class="btn btn-primary mt-1" @click.prevent="reverseText">反轉字串</button>

<script>
  var app = new Vue({
    el: '#app',
    data: {
      text: ''
    },
    methods: {
      reverseText () {
        return this.text.split('').reverse().join('')
      }
    }
  })
</script>

```

## 7. 動態切換 class（v_class.html）

在 `v-bind` 中可以傳入物件 該物件中左邊是要切換的類名 右邊是布爾值 通過點擊事件把布爾值設為相反 就能動態切換類名了

  * className 如果為帶有下底線或減號的名稱時 需使用中括號包住 否則會出錯 EX: `['btn-outline-primary']`
  
  * 多個 class 時 可在物件中使用 `,` 做分割

代碼如下：

```html

<!-- 這裏的 isTransform 是一個布爾值 -->
<div class="box" :class="{ rotate : isTransform }"></div>
<button class="btn btn-outline-primary" @click="isTransform = !isTransform">旋轉物件</button>

```

## 8. `computed` 計算屬性（computed.html）

在 vue 實例中除了 `methods` 物件外 還有一個 `computed` 物件 這兩者很相似

`methods` 與 `computed` 的差別是 `methods` 通常是觸發事件後使用的函數 必須定義一個事件才能觸發其方法

而 `computed` 則是用來返回一個值的 且它是當資料一有變動就會觸發 通常只用於將 data 中的資料計算結果回傳 呈現到畫面上

代碼如下：

```html

<div id="app">
  <input type="text" class="form-control" v-model="text" placeholder="輸入的文字會立馬反轉在下方">
  <br>
  {{ reverse }}
</div>

<script>
  var app = new Vue({
    el: '#app',
    data: {
      text: ''
    },
    computed: {
      reverse: function () {
        return this.text.split('').reverse().join('')
      }
    }
  })
</script>

```

## 9. 表單的雙向雙定

`input` 跟 `textarea` 一樣都是添加 `v-model="text"` 屬性 在畫面中通過 `{{text}}` 查看字串的變化

`checkbox` 添加 `v-model="checkbox1"` 屬性 在畫面中通過 `{{ checkbox1 }}` 查看布爾值變化

當要得到多選的值時 使用 `checkbox` 且添加 `value` 屬性 在 `data` 對應的物件添加或移除該 `value` 值 

然後在每個 `checkbox` 上添加 `v-model="checkboxArray"` 屬性 在畫面中通過 `v-for` 的 `{{ item }}` 即可查看添加的 `value` 值

要得到單選的值時 則使用 `radio` 且添加 `value` 屬性 也是添加 `v-model="singleRadio"` 屬性 在畫面中通過 `{{ singleRadio }}` 查看值的變化

`select` 下拉式選單 也是添加 `v-model="selected"` 然後在 `option` 中添加 `value` 屬性 選中時 `value` 的值就會被傳入 `selected` 中

## 10. 元件

當頁面中有一個以上相同的東西 想要同時使用 但又想分開處理資料時 就可以使用元件 元件可以有多個 且每個的資料都是獨立的

元件通過 `Vue.component()` 來創建 且要放在創建 vue 實例之前 而 `Vue.component()` 的參數1 為自定義標籤名稱 參數2 為物件 

  * 在參數2 的物件中 `data` 必須使用 `function` 來 `return` 

  * 參數2 的物件中有一個 `template` 內容就是你要渲染在元件標籤中的內容

這時在瀏覽器中開啟 vue 工具 可以發現在 `Root` 下面多了新的元件標籤 這裏的 `Root` 就是 vue 的實例 元件標籤則是創建的各個元件

## 11. 生命週期（v_lifecycle.html）

在 vue 中有一個生命週期 整個 vue.js 從開始渲染頁面到渲染完成有很多個階段狀態

* 在沒有 `keep-alive` 標籤包裹於外面時 vue 的生命週期為 `beforeCreate` > `created` > `beforeMount` > `mounted` > `updated` > `beforeDestroy` > `destroyed`

第一個狀態 `beforeCreate` 什麼都沒有 data 變數為 undefined

第二個狀態 `created` 模板 `child` 尚未渲染到頁面 但 data 變數已創建

第三個狀態 `beforeMount` 綁定 `template` 之前

第四個狀態 `mount` 綁定 `template` 之後 這裏如果有設定 `mount` 則開始執行其函數

第五個狀態 `updated` 資料更新 順便更新 `template` 中的資料內容

第六個狀態 `beforeDestroy` 移除並銷毀整個 vue 元件前

第七個狀態 `destroyed` 移除並銷毀整個 vue 元件後

* 在有 `keep-alive` 標籤包裹於外面時 vue 的生命週期第六七狀態會改變成 `activated` 與 `deactivated`

這個 `activated` 與 `deactivated` 跟 `beforeDestroy` 與 `destroyed` 的差別是：

`destroyed` 會移除並銷毀整個資料 而 `activated` 則會保留資料與 data 變數的更改內容

_____

## 第二單元 - `template` 目錄

## 1. 通過陣列方式添加/刪除樣式（v_class.html）

在要設定樣式的標籤中添加屬性 `v-bind:class="arrayClass"` 然後我們使用 `checkbox` 來動態添加類名

我們在 `check` 中添加屬性 `v-model="arrayClass"` 再給它加上 `value="class名稱"`

此時通過打勾或取消打勾 就可以往 `arrayClass` 陣列中添加移除 `value` 的類名了

## 2. `v-bind:style` 使用方式（v_class.html）

在標籤中添加 `v-bind:style=""` 可以添加樣式 使用方式有三種 

  * 一種直接傳入物件： `<div class="box" :style="{backgroundColor: '#ccc'}"></div>`
  
  * 一種直接傳入 data 變數： `<div class="box" :style="styleObject"></div>`
  
  * 一種直接傳入陣列： `<div class="box" :style="[styleObject,styleObject2,styleObject3]"></div>`

且在 vue 中通過動態添加的樣式可以不用寫 `prefix` (即處理跨瀏覽器相容性問題)

vue 會自行處理各個瀏覽器的相容問題 如： `Safari 瀏覽器的 -webkit-`、`IE 瀏覽器的 -ms-` 等

## 3. 關於 `v-for` 的補充（v_for.html）

### 3-1. 索引

在之前我們使用 `v-for` 都是用於陣列 陣列的索引就是普通索引 是數字

其實 `v-for` 也可以使用在物件上 只是物件的索引就會變成物件的名稱 而不是數字的普通索引

### 3-2. `key`

當在渲染的資料後方有 DOM 元素時 須通過 `key` 才可在變動資料時跟著操作 DOM 元素

以陣列反轉為例 我們通過 `v-for` 創建 `li` 然後每個 `li` 後方都有一個 `input` 

此時我們可在 `li` 的 `v-for` 迴圈後面加上 `v-bind:key="xxxx"` 這樣在反轉陣列的同時輸入框就會跟著變動了

  * 建議將 `key` 設為 `item.xxx` 且該 `item.xxx` 為各自不同的值

代碼如下：

```html

<ul>
  <li v-for="(item, key) in arrayData" :key="item.age">
  {{ key }} - {{ item.name }} {{ item.age }} 歲 <input type="text">
  </li>
</ul>
<button class="btn btn-outline-primary" @click="reverseArray">反轉陣列</button>

```

### 3-3. `this` 補充

在 vue 實例中 如果我們創建的 `data` 要用於 `methods` 函數時 須通過 `this.xxx` 對應到 data 的該 `xxx` 變數

假設你在 `methods` 中使用了 for 迴圈等方法 在該方法中的 `this` 就會指向不同對象 因而無法對應到實例中的 data 變數

為了解決這個問題 我們在一開始先創建一個變量 `var vm = this` 讓 data 的 `this` 可以在後面繼續對應到 vue 實例的 data 變數中

### 3-4. 補充在 vue 的陣列遇到不能運行的狀況

* 情況1 - 自行竄改一個陣列的長度

  - 在 js 中我們如果使用 `array.length = 0` 會把該陣列變成空陣列 但在 vue 中則無效(雖然長度會是0 但陣列中的資料都還在)

* 情況2 - 自行竄改陣列某索引的資料

  - 在 js 中我們如果使用 `array[0] = {...}` 會把該陣列索引0的資料更改 但在 vue 中則無效(在開發者工具中可看到數據已改變的頁面上顯示的資料不會變動)

  - 原因是在 vue 中如果要設置原本不存在的資料 須通過 `Vue.set(要修改的資料對象,修改的該對象索引,修改的內容)` 方法 資料才能被 vue 實例監控 進而同步出現在頁面中

### 3-5. 純數字的迴圈

我們可以直接通過 `v-for="item in num"` 來建立純數字的迴圈 此處的 `item` 就會從 1~num 輸出

代碼如下：

```html

<ul>
  <li v-for="item in 10">
  {{ item }}
  </li>
</ul>

<!-- 輸出結果就是 <li>1</li> ~ <li>10</li> -->

```

### 3-6. `template` 標籤

當你想同時通過 `v-for` 創建一個以上的相同標籤時就可以使用 `template` 標籤 原因是該標籤會自動隱藏 不會出現在頁面的代碼中

  * 當需要使用 vue 指令同時又不希望輸出標籤時 就能使用 `template` 標籤

這裡我們以 `v-for` 同時創建兩個 `td` 標籤為例 代碼如下：

```html

<table class="table">
  <template v-for="item in arrayData">
    <tr>
      <td>
      {{ item.name }}
      </td>
    </tr>
    <tr>
      <td>
      {{ item.age }}
      </td>
    </tr>
  </template>
</table>

```

### 3-7. 當標籤中同時帶有 `v-for` 與 `v-if`

一個標籤裡同時有 `v-for` 與 `v-if` 時 會優先執行 `v-for` 再執行 `v-if`

### 3-8. 元件與 `v-for`

目前在 2.2 版本以上的 vue 在元件中使用 `v-for` 時都 **必須** 為它加上 `:key` 

  * 在元件中可以通過直接在元件的標籤上添加 `:key`

  * 建議設置 `:key` 時最好是使用 id 這種唯一的值 或是其他 **不重複** 的值

## 4. 關於 `v-if` 補充（v_if.html）

## 4-1. `v-if` 簡寫

`v-if="變數 == true"` 可以簡寫為 `v-if="變數"`

當上方已經有一個 `v-if` 時 `v-if="變數 == false"` 就可以簡寫為 `v-else` 

這個 `v-else` 就會直接作為 `v-if="xxx"` 以外的結果

這裏的變數通過 `checkbox` 切換值為 `true`或 `false`

## 4-2. `v-else-if`

當程式碼中需要判斷的內容有連帶關係時可以使用 `v-else-if` 其功能和 js 中的 `if(){}else if(){}` 是相同的意思

舉例來說標籤分頁就很適合使用這個方式 代碼範例如下：

```html

<ul class="nav nav-tabs">
  <li class="nav-item">
    <a class="nav-link" href="#" :class="{'active':link=='a'}" @click.prevent="link='a'">標題一</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="#" :class="{'active':link=='b'}" @click.prevent="link='b'">標題二</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="#" :class="{'active':link=='c'}" @click.prevent="link='c'">標題三</a>
  </li>
</ul>
<div class="content">
  <div v-if="link=='a'">Ａ</div>
  <div v-else-if="link=='b'">Ｂ</div>
  <div v-else="link=='c'">Ｃ</div>
</div>

```

## 4-3. 高效能替換問題

當內容結構相同的兩個區塊通過 `v-if` 切換顯示的時候 vue 會使用最高效能的替換方式做切換

例如兩個 `template` 區塊中一樣都有一個 `label` 與 `input` 

此時你切換會發現在輸入框中的內容還存留在切換後的輸入框中 原因是 vue 的高效能處理會做局部替換而非重新渲染

為了避免這個問題需使用 `key` 來將兩者做區分 讓它能重新渲染

  * 當 `key` 為直接創建時 就給其添加屬性 `key` 就可以了

  * 當 `key` 為通過 `v-for` 等方式得到的數據時 則需通過 `v-bind:key` 來動態添加 `key` 屬性

代碼如下：

```html

<template v-if="loginType === 'username'">
  <label>Username</label>
  <input class="form-control" placeholder="Enter your username" key="name">
</template>
<template v-else>
  <label>Email</label>
  <input class="form-control" placeholder="Enter your email address" key="mail">
</template>
<button class="btn btn-outline-primary mt-3" @click="toggleLoginType">切換狀態</button>

```

## 4-4. `v-if` 與 `v-show`

在 vue 中有一個跟 `v-if` 非常相似的指令為 `v-show`

`v-if` 與 `v-show` 的差異為

  * `v-if` 在切換的過程中是會直接銷毀並重建元素的 適用於不頻繁的切換 因為該切換方式耗費的效能更多

  * `v-show` 在切換的過程中則是通過 css 的 `display = "none"` 隱藏另一個元素

  * 當是要頻繁切換條件時 就建議使用 `v-show` 切換 該切換方式耗費的效能會小很多 因為它僅是切換樣式而非整個移除又重新創建渲染

## 5. 計算與監聽（computed_watch.html）

### 5-1. `computed` 中的函數會在每次資料做更動時觸發 

以之前的過濾資料來說 原本使用 `methods` 的函數調用時 需要通過按按鈕或者按按鍵等事件觸發才會執行過濾

如果使用 `computed` 則可以省略按鈕或按鍵就觸發該過濾函數 因為有綁定 `v-model` 

即每次當 `v-model` 值改變的同時 `computed` 就會被執行 因此該方法建議使用於資料量較少時 否則容易影響效能

### 5-2. `mounted`

`mounted` 為畫面運行完畢後執行的 類似 `methods` 與 `computed` 都是存放函數用的 差別在於：
  
  * `methods` 須通過事件觸發
  
  * `computed` 須通過資料改變觸發
  
  * `mounted` 則為畫面運行完畢後觸發

### 5-3. `watch`

`watch` 為監聽事件 類似 `methods` 與 `computed` 都是存放函數用的 差別在於：

`watch` 通常用於計時器事件 它會監聽變數 當變數改變後自動開始執行函數

這裡以動態添加樣式旋轉 box 為例 我們點擊按鈕後將 `trigger` 設為 `true` 三秒後通過 `watch` 將其設回 `false`

代碼如下：

```html
<div class="box" :class="{'rotate': trigger }"></div>
<button class="btn btn-outline-primary" @click="trigger = true">Counter</button>

<script>
var app = new Vue({
  el: '#app',
  data: {
    trigger: false
  },
  watch: {
    trigger: function () {
      var vm = this;
      setTimeout(function () {
        vm.trigger = false
      }, 3000)
    }
  }
})
</script>

```

## 6. 表單補充（form.html）

* `select` 的 `option` 可以通過 `v-for` 生成 不過這裏的 `value` 就變成動態生成的 所以須改成 `v-bind:value`

* `select` 可以通過添加 `multiple` 屬性將 `option` 改為多選框 通過按住鼠標拖移或 `shift` 鍵等方式選擇多個選項

* `checkbox` 的勾選狀態可以通過添加 `true-value="xx"` 和 `false-value="xx"` 屬性更改 `value` 值 用以切換顯示內容

  代碼如下：

  ```html
  
  <input type="checkbox" class="form-check-input" id="sex" v-model="sex" true-value="男生" false-value="女生">
  <label class="form-check-label" for="sex">{{ sex }}</label>
  
  ```

* `v-model` 的修飾符補充：

  - `v-model.lazy`：在默認情況下 當輸入框內容變動時就會同步顯示於畫面上 這時候如果給 `v-model` 添加 `.lazy` 則會在用戶點擊輸入框以外的地方或按下 `enter` 鍵時才傳回內容

  - `v-model.number`：創建變數時通常我們會給一個空字符串 此時假設你傳入的內容是數字也會變成字符串形式的數字 為解決這個問題可在 `v-model` 添加 `.number` 將其類型轉為純文字

  - `v-model.trim`：當輸入框中有多餘的空白鍵不想被傳回時可在 `v-model` 添加 `.trim` 用以去除多餘的空格

## 7. `v-on` 補充（v_on.html）

補充常用的幾個事件修飾符：

- `stop` 阻止冒泡事件： `div>button` 點擊 `button` 默認會觸發兩個事件 且順序為 `button>div` 若改成 `v-on.stop="xxx"` 則只會觸發 `button` 事件

- `capture` 將冒泡事件順序相反： `div>button` 點擊 `button` 默認會觸發兩個事件 且順序為 `button>div` 若改成 `v-on.capture="xxx"` 則觸發順序就變為 `div>button` 

- `self` 只觸發自己的事件：觸發結果與 `stop` 一樣 點擊各自區塊觸發各自效果 不會有任何冒泡事件

- `prevent` 阻止默認事件：如 `a` 標籤點擊後的跳轉頁面會失效 或阻止 `button` 表單提交行為

- `once` 只觸發事件一次：觸發一次後事件失效 即第一次點擊後觸發事件 再次點擊則事件無效化

補充幾個常用的按鍵修飾符：

- `.enter .tab .delete .esc .space .ctrl .alt .shift .meta` 這幾個按鍵可以直接通過 `@keyup.xxx` 來取代 `keyup(keycode=xxx)`

  + `meta` 鍵為 `mac` 鍵盤的四花瓣 或 `windows` 鍵盤的 `windows` 鍵

- 若要使用組合鍵觸發事件 可通過 `@keyup.xxx.xxx` 來取代

補充滑鼠鍵修飾符：

- `.left .right .middle` 分別為滑鼠左鍵、右鍵、中間鍵 可通過 `@click.xxx` 來設定 默認 `@click` 事件是點擊滑鼠左鍵時觸發 即 `@click.left` 與 `@click` 是一樣的

_____

## 第三單元 - `components` 目錄

## 1. 元件基礎介紹（basic.html）

在 vue 中的元件有一些很好利用的特性 比如每個元件都是獨立的資料內容 且同一個元件是可以被重複使用的

在元件中 若要從元件本身傳遞資料給父元素 需通過在事件函數中設置 `this.$emit` 傳遞

而若要獲取父元素傳遞的資料 則需在元件中掛載一個 `props` 屬性

創建元件的 `html` 結構 我們原本是使用 `template: html 代碼` 

其實還可以改成 通過 `script` 標籤存放代碼給 `template` 調用

使用方式為 創建一個 `script` 標籤 將其 `type` 改為 `text/x-template` 然後給該標籤設定一個 `id` 並在該標籤中寫入 html 代碼

完成後再到元件的 `template` 中傳入剛剛給 `script` 設置的 `id`

另外要注意在 html 規定中 `table` 裡面不能存在 `table` 結構外的標籤 

為了解決這個問題 我們可以通過 給 `tr` 加上 `is="元件標籤名"` 屬性 來取代直接創建元件標籤

代碼如下：

```html

<div id="app">
  <table class="table">
    <thead>
    </thead>
    <tbody>
      <tr is="myComponent" v-for="(item, index) in data" :item="item" :key="index"></tr>
    </tbody>
  </table>
</div>

<script type="text/x-template" id="mytemplate">
  <tr>
    <td>{{ item.name }}</td>
    <td>{{ item.cash }}</td>
    <td>{{ item.icash }}</td>
  </tr>
</script>

<script>
  Vue.component('myComponent', {
    props: ['item'],
    template: '#mytemplate'
  })

  var app = new Vue({
    el: '#app',
    data: {
      data: [{
        name: '小明',
        cash: 100,
        icash: 500,
      },
      {
        name: '杰倫',
        cash: 10000,
        icash: 5000,
      },
      {
        name: '漂亮阿姨',
        cash: 500,
        icash: 500,
      },
      {
        name: '老媽',
        cash: 10000,
        icash: 100,
      }]
    }
  })
</script>

```

## 2. 元件中的 `data`（function_return.html）

在元件中因為資料都是獨立的 所以 `data` 必須使用 `function(){return ...}` 來傳遞

這樣做才能保證每個元件中的資料都是獨立運作的 且元件中的資料也與 vue 實例是分開的

## 3. `props` 基礎使用（prop_basic.html）

前面有說 在元件中若要獲取父元素傳遞的資料需通過 `props` 這個屬性

該屬性可以靜態傳遞 也可以動態傳遞 靜態傳遞方式為直接傳入字符串內容 動態則是通過 `v-bind` 傳遞 data

## 4. `props` 注意事項與解法（prop_adv.html）

* 在使用 `props` 時盡量使用單向數據流 即通過它獲取資料就好 不要試圖又修改它的資料 以免報錯

  舉例來說畫面中有一張圖片一個輸入框 其圖片網址與輸入框內容綁定了同個 `props` 資料對象 請將輸入框的 `v-model` 指向元件中新建立的 `data` 對象 而不要綁定在同個資料 否則會報錯

* 在 `props` 中如果要獲取的資料是要經過一小段時間才能取得的話會因此而報錯 因為渲染時資料尚未取得（比如使用 ajax 獲取數據等）此時可以通過在該標籤中添加 `v-if` 屬性 判斷獲取的資料已經得到了沒 再去執行該標籤的渲染 這樣就不會因為找不到資料而報錯了

* 在 js 中有個 ***物件傳參考*** 的特性使我們在使用 vue 時會經常碰到一些問題 比如你的 vue 實例中有一個 `data: {user:'John'}` 你將這個 `user` 通過 `props` 傳遞給元件使用 那當元件中的 `user` 值被做改變時 在 vue 實例中的 `user` 值同樣也會被改變 *這是 js 的特性 與 vue.js 本身無關 但要特別注意這個問題*

* 在有使用 `ajax` 的元件中通過 `checkbox` 更改 `v-show` 屬性會造成該元件整個重新銷毀再創建 導致原本的內容消失 若要保留原本的內容 可以通過之前所學到的 `keep-alive` 標籤將該元件的標籤包裹住來維持它的生命週期

## 5. `props` 的類別（prop_type.html）

如果要設定 `props` 的類別 可以通過傳入物件的方式 將資料設置 `type` 與 `defalt`

代碼如下：

```js

Vue.component('prop-type', {
  props: {
    cash: {
      type: Number,
      default: 300
    }
  },
  template: '#propType',
    data: function () {
      return {
        newCash: this.cash
      }
    }
})

```

## 6. `emit` 用法（emit.html）

當元件要將數據或事件從元件本身往父元素傳遞時 可以通過在事件中設置 `this.$emit()` 來傳遞

它可以傳入兩個參數 第一個參數是綁定在元件標籤中的 `v-on:` 事件 (原先是傳入 `click` 等事件 這裏則要改為傳入一個自定義名稱) 第二個參數可選 假設你想在觸發的事件裡加入參數就可以通過第二個參數來設置父元素事件的參數 然後將該參數寫入 vue 實例下的事件中做調用

範例代碼如下：

```html

<!-- 定義元件事件名為 event ,事件函數為 vue 實例中的 total -->
<button-counter v-on:event="total"></button-counter>

<script>
  Vue.component('buttonCounter', {
    template: `<div>
    <button @click="incrementCounter" class="btn btn-outline-primary">增加 {{ counter }} 元</button>
    <input type="number" class="form-control mt-2" v-model="counter">
  </div>`,
    data: function () {
      return {
        counter: 1
      }
    },
    methods: {
      // 這個 incrementCounter 就是元件模板中 click 事件的對應函數
      incrementCounter: function () {
      // 元件中點擊後觸發的事件為父元素中的事件 所以通過 this.$emit() 來調用
      // 參數1 為元件事件名 即元件標籤上定義的 v-on 那個 'event' 
      // 參數2 為要傳遞到父元素中的事件的參數 這裡傳入元件中的 data 下那個 counter
      this.$emit('event', Number(this.counter))
      }
    }
  });

  var app = new Vue({
    el: '#app',
    data: {
      cash: 300
    },
    methods: {
      // 這裏的 num 就是元件通過 $emit 的第二個參數傳過來的內容了
      total: function (num) {
        this.cash = this.cash + num
      }
    }
  });
</script>

```

## 7. slot 插槽替換（slot.html）

元件雖然是可以重複使用的 但有時候裡面想做一點更動替換內容就會變得很不方便

這裡可以使用插槽的功能來幫助我們修改元件的部分內容 插槽本身是一個標籤

我們通過在元件的模板內容中添加 `<slot></slot>` 然後在元件的標籤中直接添加內容來替換或增加段落

  * 如果是要往元件中新增內容則直接在該元件模板中添加 `<slot></slot>` 

  * 如果是要替換元件中的某段模板內容則在要替換的地方包裹上 `<slot></slot>` 

要替換部分元件模板內容的代碼如下：

```html

<div id="app">
  <single-slot-component>
    <p>我是要取代預設段落的 Slot。</p>
  </single-slot-component>
</div>

<script type="text/x-template" id="singleSlotComponent">
  <div class="alert alert-warning">
    <h6>我是一個元件</h6>
    <slot>我是預設段落</slot>
  </div>
</script>

<script>
  Vue.component('single-slot-component', {
    template: '#singleSlotComponent',
  });

  var app = new Vue({
    el: '#app'
  })
</script>

```

另外 `slot` 還有具名插槽 當插槽不只一個的時候 可以通過添加 `name` 屬性 來分別替換對應內容

使用方式為在元件模板內容的插槽中給其添加自定義的 `name` 屬性 然後在元件渲染標籤上給其添加屬性 `slot="自定義 name 屬性值"`

  * 當元件渲染標籤本身是不需要被渲染到頁面時 可使用 `template` 標籤替換插槽處標籤 如：要替換的是 `a` 的值 可以在 `a` 標籤中添加 `template` 標籤包裹值並給其添加插槽屬性 這樣該 `template` 不會被渲染為標籤 但該插槽屬性會讓內容確實渲染上去

## 8. 通過 `is` 動態切換顯示元件

前面說過可以使用 `div` 加上 `is="元件標籤名"` 這個屬性 來取代直接使用元件標籤名創建元件標籤

理所當然也可以通過 `v-bind:is` 來動態切換 `is` 的屬性值達到 `v-if` 的效果

這個方法通常適用於 `v-if` 的選項太多個的時候 如果使用 `v-if` 來判斷切換時代碼就會變得很冗長 但如果是使用 `:is` 動態切換的話則只需要一行代碼就可以了

範例如下：

```html

<div id="app">
  <!-- 原本使用 v-if 如果很多個就要越寫越多行 -->
  <primary-component :data="item" v-if="current == 'primary-component'"></primary-component>
  <danger-component :data="item" v-if="current == 'danger-component'"></danger-component>

  <!-- 使用 v-bind:is 只需一行代碼 -->
  <div :is="current" :data="item"></div>
</div>

<script type="text/x-template" id="primaryComponent">
  <div class="card text-white bg-primary mb-3" style="max-width: 18rem;">
  <div class="card-header">{{ data.header }}</div>
  <div class="card-body">
    <h5 class="card-title">{{ data.title }}</h5>
    <p class="card-text">{{ data.text }}</p>
  </div>
</div>
</script>

<script type="text/x-template" id="dangerComponent">
  <div class="card text-white bg-danger mb-3" style="max-width: 18rem;">
  <div class="card-header">{{ data.header }}</div>
  <div class="card-body">
    <h5 class="card-title">{{ data.title }}</h5>
    <p class="card-text">{{ data.text }}</p>
  </div>
</div>
</script>

<script>
  Vue.component('primary-component', {
    props: ['data'],
    template: '#primaryComponent',
  });
  Vue.component('danger-component', {
    props: ['data'],
    template: '#dangerComponent',
  });

  var app = new Vue({
    el: '#app',
    data: {
      item: {
        header: '這裡是 header',
        title: '這裡是 title',
        text: 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Enim perferendis illo reprehenderit ex natus earum explicabo modi voluptas cupiditate aperiam, quasi quisquam mollitia velit ut odio vitae atque incidunt minus?'
      },
      current: 'primary-component'
    }
  })
</script>

```

_____

## 第四單元 - `es6` 目錄

## 1. 聲明變數的三種方式詳解（let_const.html）

* 在 js 中原本聲明變數是使用 `var` 該方法是用來聲明全局變量 這會在給其變量賦值前就創建該變量 只是值為 `undefined` (意指該變量存在只是找不到值) 但若用 `let` 取代 `var` 聲明變量 則在賦值前該變量就完全不存在 顯示為 `not defined` (該變量不存在，報錯)

* 在函數中變量若為 `var` 定義的則作用域為整個函數都適用 但若為 `let` 定義的則作用域僅限於區塊中(除了函數的 `{}` 外的每一個各自的 `{}` 中)

  * 這點在 `for` 迴圈中有很強大的作用 因為使用 `let` 宣告變數時 每一個變數在迴圈的內部都會是獨立的作用域 即每一次迴圈就聲明一個新的變數 舉例來說：

  ```js
  
  // 原本的迴圈是這樣
  for(let i = 0; i < 2; i++){
    console.log(i)
  }

  // 但你可以當成它其實這樣
  (let i = 0) {
    console.log(i)
  }
  (let i = 1) {
    console.log(i)
  }

  ```

* `const` 是用來聲明一個永遠不會被更改的變量 當你用其創建變量時 若在代碼的後面要修改該變量值是會直接報錯的

## 2. 陣列合併（spread_operator.html）

js 中提供的陣列合併方法為 `concat` 在 `ES6` 中有更快更簡潔的方式可以合併陣列

代碼如下：

```js

let groupA = ['小明', '杰倫', '阿姨'];
let groupB = ['老媽', '老爸'];
let all = [...groupA , ...groupB]

```

`[...Array]` 方法是用來將某數組中的值 一個一個取出來 再放到新的數組中 

由此可知它除了像上述一樣用來合併數組外 還可用來複製數組、將偽數組變成真數組等

該方法還可以使用在函數的參數上 當我們在函數定義中沒有傳入參數而是在使用該函數時才傳入參數時參數會變成 `arguments` 這個變數 該變數是一個偽數組 我們可通過 `[...arguments]` 來將參數轉成一個數組並對其操作

最後補充一個 **其餘參數** 當你在函數中使用多個參數 可以通過 `fn(a, ...something){}` 來獲取 a 以外的其餘參數

代碼如下：

```js

function getMoney(a,...money){
  console.log(a,money)
}

getMoney('A',10,20,30)

// 輸出結果為 > A , [10,20,30]

```

## 3. 解構（destructuring.html）

解構可以理解為不更換順序的完全鏡射 有點類似完全複製 可用在陣列與物件的賦值與複製上

```js

let arr1 = [1,2,3]
let [a,b,c] = arr1
// console.log(a,b,c) > 1,2,3

let arr1 = [1,2,3]
let [a, ,c] = arr1
// console.log(a,c) > 1,3

let a = 1
let b = 2
let [a, b] = [b, a]
// console.log(a,b) > 2,1

let str = 'ABC'
let [a,b,c] = str
// console.log(a,b,c) > A,B,C

let user = {name: 'John', password: '12345'}
let john = {...user}
// console.log(john) > {name: 'John', password: '12345'}

let GinyuTeam = {
  Ginyu: '基紐',
  Jeice: '吉斯',
  burter: '巴特'
}
let {
  Ginyu: A
} = GinyuTeam 
// 這裡會先去 Team 中找到對應的對象 獲取它的值 
// 然後賦予給 Ginyu 
// 再把 Ginyu 變成新變數 A 
// 所以 Ginyu 不存在 而 A 的值是 Team 中的 Ginyu 的值
// console.log(A) > '基紐'
// console.log(Ginyu) > is not defined

let {
  family: ming = '小明'
} = {}
// console.log(ming) > '小明'
```

## 4. 箭頭函數與函數（arrow_function.html）

在箭頭函式中 當函數只有一行時 可以省略 `function` `{}` `return` (它會自動補上 `return`) 直接寫 `參數=>參數` 即可 (原本為 `function(參數){return 參數}`)

在箭頭函數中沒有 `arguments` 所以在箭頭函數中要獲取參數時 需使用其餘參數的方法來獲取(即 `...arg` 方法)

箭頭函數與函數指向的 `this` 是不同的 箭頭函式大多時候指向的 `this` 會是 `window` 所以在 `vue` 中統一建議不要使用箭頭函式 否則容易無法獲取到 `this`

  * 若要處理 `this` 指向的問題 可以通過創建變量 `vm = this` 來指向我們要的 `this` 對象

## 5. ES6 中變量為字串的賦值方法（template_string.html）

`｀` 這個符號可以用來取代包裹字符串的單雙引號 且可以更方便的拼接需要換行的字串

以往要拼接字串以及換行都需通過 `+` 或 `\` 符號 若使用 `｀` 則可以省略很多特殊符號 

另外它也可以通過 `${這裡可插入js代碼或一個變量}` 來撰寫 js 代碼

如下：

```js

// 假設 people 是一個物件
let str = `我叫做 ${people[0].name}`
let Html = `<ul>
    ${people.map(person=>`<li>我叫做 ${person.name}</li>`).join('')}
  </ul>`

```

## 6. ES6 中陣列的一些常用方法（array_function.html）

* `map` 與 `forEach` : 兩個方法都用來遍歷陣列，唯一差別在 `map` 會 `return` 一個新陣列， `forEach` 沒有回傳值。

* `filter` : 用來過濾的方法， `return` 回傳的是條件為 `true` 的所有值，如果多個就會回傳陣列，單個就回傳 `item` 本身。

* `find` : 用來查找符合條件的第一個對象， `return` 回傳的是條件為 `true` 的第一個值。

* `every` 與 `some` : 兩個方法都用來回傳是否符合條件(回傳布爾值) 差別為 `every` 會判斷所有物件是否都符合條件才回傳 `true` 而 `some` 則是只要有某個條件符合就回傳 `true`。

* 以上方法參數都分別為 `item` `index` `array`

* 最後補充一個 `reduce` 方法 : 可用於比較大小或加總數字，其參數分別為 `prev(前一個數字)` `item(當前數字)` `index`，該方法會回傳一個數字，當沒有前一個數字時可寫成 `reduce(fn(){},0)` ，用來給其添加前一個數字為零。
  
  * 加總方法代碼如下：

  ```js
  
  arr.reduce(function(pre,item,index){
    return pre + item)
  },0)
  
  ```

  * 比較方法代碼如下：

  ```js
  
  arr.reduce(function(pre,item,index){
    return Math.max(pre,item)
  },0)
  
  ```

_____

## 第五單元 - `api` 目錄

## 1. 繼承（extend.html）

當有兩個元件的結構是幾乎相同時可以通過在元件中添加 `extends` 屬性來繼承元件內容

使用方法為創建一個變量為 `Vue.extend({要繼承的內容})` 然後將該變量設為元件或實例的 `extends` 屬性的值 再給其中的一個元件添加上要修改的內容 

且假設兩個元件中的 `data` 有同名不同值時 可以直接在其中一個元件內部重新設定該同名的 `data` 值將原本的值取代

## 2. 過濾（filter.html）

在 vue 中還有一個類似 `methods` 的屬性為 `filter` 它專門用來處理畫面上的資料

`filter` 與 `computed` 的差別為 `filter` 通常適用於較簡單的資料處理 比如字串的拼接或是給價錢添加千分位與錢字符等

其可用於元件或實例中 使用方法為給實例或元件添加 `filters:{方法名:方法}` 屬性 另外它也可以全局註冊 全局註冊方法為 `Vue.filter('filter方法名稱',方法)` 然後在元素上使用 `|` 符號 後面插入 `filter方法名稱`

`filter` 方法會接收一個參數 該參數是要處理的資料本身 且一個資料可以同時有多個 `filter` 方法

示例：

```html

<!-- 最後輸出結果為 $10,000 -->
{{ data | currency | dollarSign }}

<script>
Vue.filter('dollarSign', function (n) {
  return `$${n}`
})

Vue.filter('currency', function (n) {
  return n.toFixed().replace(/./g, function (c, i, a) {
    return i && c !== "." && ((a.length - i) % 3 === 0) ? ',' + c : c;
  });
})

var app = new Vue({
  data: 10000,
})
</script>

```

## 3. 往 vue 中寫入資料並被 vue 監控（vue_set.html）

之前曾經提到過 我們無法通過 `data.xxx = xxx` 往 `data` 中添加資料（vue 不會讓它渲染到畫面上 且 vue 也不會實時監控操作它）要處理這個問題必需使用 `this.$set()` 方法來修改資料讓該資料寫入到 vue 實例中 且被 vue 隨時監控與操作

## 4. 混合多個元件（mixin.html）

`mixins` 類似 `extends` 差別於 `mixins` 可以只獲取小片段並混合給其他實例或元件使用

使用方法跟 `extends` 也類似 都是創建變量並將變量設為元件或實例中 `mixins` 的值 差別於 `mixins` 的值不是變量而是陣列 陣列中可放單個或多個變量 用以將不同元件的部分拉出來混合使用

## 5. 自定義事件（direcitv.html）

vue 中可以使用自定義指令 使用方法為 `Vue.directive()` 參數1是自定義指令名稱 參數2為一個物件 物件中放入函數名稱 然後在函數名稱的值裡面撰寫你要的 js 代碼

  * 函數名稱是 vue 提供的幾種鉤子函數 如下：
    
    * `bind` : 只調用一次

    * `inserted` : 元素插入父節點時調用（父節點存在但未插入元素時沒作用） 也可理解為當元素生成到畫面上時調用

    * `update` : 資料發生變化時調用

    * `componentUpdated` : 指令所在元件資料發生變化時調用

    * `unbind` : 指令與元素解除綁定時調用（僅調用一次）

  * 鉤子函數的參數 vue 也已經定義好了 如下：

    * `el` : 指令綁定的元素 即 js 中的 `element`

    * `binding` : 這是一個對象 對象中有該指令的一些屬性

    * `vnode` : vue 的節點

  * 通常鉤子函數的參數只有 `el` 是可以被操作的 其他僅供讀取而不能更改

自定義指令的使用方式為在元素上增加屬性 `v-自定義指令名稱`

## 6. 套件使用（use.html）

vue 是現在較主流的框架 也因此有人開發了一些套件給大家使用 這邊介紹一個 `vue + Bootstrap4` 的套件 套件名為 `BootstrapVue`

可以使用 `webpack` 加載 也可以直接通過 `script` 標籤引入 引入後就可以使用該套件的指令了


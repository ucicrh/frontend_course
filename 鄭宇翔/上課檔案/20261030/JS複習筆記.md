# JavaScript 複習筆記（DOM 與事件）

> 適合對象：JS 初學者。範例都是單一 HTML 檔，用瀏覽器打開就能看效果。
> 建議用法：每個概念先看對應的範例檔，再自己改一改數字或文字，觀察結果。

---

## 一、這批範例在學什麼？

| 主題 | 檔案 | 核心重點 |
|---|---|---|
| 行內事件 | `事件 - 行內.html` | 直接寫在 HTML 的 `onclick="..."` |
| 綁定事件 | `事件 - 綁定.html` | 用 JS 設定 `el.onclick = ...` |
| 事件監聽 | `事件 - 監聽.html` | 用 `addEventListener` 監聽，一次幫多個元素綁定 |
| 綁定 vs 監聽 | `事件 - 比較.html` | 同一個事件綁兩次會怎樣 |
| 事件物件 | `事件 - event.html` | `event` 參數、阻止預設行為、捕獲／冒泡 |
| 表單事件 | `事件 - 表單.html` | `focus` `blur` `input` `change` |
| 猜拳 | `猜拳.html` + `猜拳.md` | 綜合練習：選元素、綁事件、隨機、判斷、新增 li |
| 購物車卡片 | `卡片.html` | `data-*` 屬性、`dataset`、動態新增元素 |
| 載入畫面 | `載入.html` | `onload`、`onreadystatechange`、`setTimeout` |
| 視差背景 | `視差.html` | `onscroll`、捲動位置、動態修改樣式 |

---

## 二、核心概念

### 1. 取得 HTML 元素（DOM 選取）

```js
document.getElementById('on')          // 用 id，回傳「一個」元素
document.getElementsByClassName('btn') // 用 class，回傳「類陣列」（HTMLCollection）
document.querySelectorAll('.card input') // 用 CSS 選擇器，回傳 NodeList
```

- `getElementsByClassName` 的結果可以用 `for...of` 跑。
- `getElementById` 找不到會回傳 `null`，要確定 id 寫對。

### 2. 綁定事件的三種寫法

```html
<!-- (1) 行內：寫在 HTML 裡，不建議大量使用 -->
<input type="button" onclick="alert('hi')">
```

```js
// (2) 綁定：用屬性設定，同一個事件只會留最後一個
el.onclick = () => { alert('hi') }

// (3) 監聽：可以綁很多個，也可以用 removeEventListener 移除
el.addEventListener('click', () => { alert('hi') })
```

**重點：** `on屬性` 會被覆蓋，`addEventListener` 不會。
（看 `事件 - 比較.html`：按 on 按鈕只會彈出「on 2」；按 listener 會依序彈出「listener 1」「listener 2」。）

### 3. `this` 與事件物件 `event`

```html
<input type="button" value="0" onclick="this.value++">
```

- 行內寫法裡，`this` 指的是**那個 HTML 標籤本身**。
- 事件處理函式的第一個參數 `event`，裡面有事件的資訊：
  - `event.key`：按的是哪個鍵
  - `event.preventDefault()`：阻止預設行為（例如 `<a>` 點了跳轉、`<form>` 送出後重新整理）
  - `event.stopPropagation()`：停止事件往上傳

### 4. 事件傳遞：捕獲與冒泡

點到內層元素時，事件會經過外層元素，分成兩個階段：

```
捕獲（capture）：由外往內  document → 外層 div → 內層 div
冒泡（bubble）：由內往外  內層 div → 外層 div → document
```

```js
divOut.addEventListener('click', fn, true)  // true = 捕獲階段
divOut.addEventListener('click', fn, false) // false（預設）= 冒泡階段
```

- 點內層會同時觸發外層的 click。
- `event.stopPropagation()` 可以讓事件停在目前這一層。
- 對照 `事件 - event.html`，取消註解後可以看到 log 順序的變化。

### 5. 表單相關事件

| 事件 | 什麼時候觸發 |
|---|---|
| `focus` | 游標進入輸入框 |
| `blur` | 游標離開輸入框 |
| `input` | **每次打字、拖拉就觸發** |
| `change` | 離開元素「且值有變動」才觸發 |

`input` 很適合做即時檢查（例如 Email 有沒有 `@`），`change` 適合「完成後」才處理。

### 6. `data-*` 屬性與 `dataset`

```html
<input type="button" data-name="FUGENE 06" data-price="2990">
```

```js
el.dataset.name   // "FUGENE 06"
el.dataset.price  // "2990"（注意：拿到的都是字串）
```

自訂資料就用 `data-` 開頭，JS 用 `dataset` 讀。

### 7. 動態新增元素

```js
const li = document.createElement('li')  // 建立
li.innerText = '文字'                    // 設定內容
list.append(li)                          // 放進 ul 裡（加在最後）
// list.prepend(li)                      // 加在最前面
```

### 8. 隨機數與判斷

```js
Math.random()        // 0 到 1 之間（不含 1）的小數
Math.floor(Math.random() * 3)  // 0、1、2（正確的三選一寫法）
```

- 比較值用 `===`（嚴格相等，型別也要一樣）。
- 多條件用 `if / else if / else`，只要判斷其中兩個，剩下的用 `else`。

### 9. 頁面載入相關

| 寫法 | 意思 |
|---|---|
| `window.onload` | 所有資源（圖片、CSS）都載完才觸發 |
| `document.onreadystatechange` | 文件狀態改變時觸發（`loading` → `interactive` → `complete`） |
| `img.onload` | 單張圖片載完觸發 |
| `setTimeout(fn, 3000)` | 3000 毫秒（3 秒）後執行一次 |

### 10. 捲動位置

```js
window.onscroll = () => {
  const top = document.documentElement.scrollTop // 往下捲了多少 px
}
```

- `document.documentElement` 就是 `<html>`。
- `scrollTop` 是垂直捲動距離，`scrollLeft` 是水平。

### 11. 改樣式

```js
el.style.color = 'red'                 // 行內樣式用駝峰命名
el.style.outline = '5px solid green'
document.body.style.backgroundPosition = `center -${top/120}px` // 模板字串
```

- CSS 的 `background-position` 在 JS 裡要寫成 `backgroundPosition`。

### 12. 模板字串

```js
const text = `你出${player}，電腦出${computer}，${result}`
```

用反引號 `` ` `` 包起來，變數用 `${}` 插入。

---

## 三、猜拳練習的流程（綜合複習）

1. 選取三個按鈕、三個計數的 `span`、`ul`。
2. 每個按鈕綁定 `onclick`。
3. 點擊時：
   - `el.value` 拿到自己出的拳。
   - `Math.random()` 產生電腦的拳。
   - 判斷：平手 → 贏的三種情況 → 其他都是輸。
   - 更新計數（`innerText++`）。
   - `alert` 顯示結果，並新增一個 `li` 到紀錄列表。

這個流程幾乎是大部分「點擊互動網頁」的通用模板，建議自己從零寫一次。

---

## 四、容易踩的坑（課堂範例裡值得注意的地方）

1. **`onclick` 會被覆蓋**：同一個元素只要寫第二次 `onclick`，第一次就失效。多個行為請用 `addEventListener`。
2. **`innerText` 拿到的是字串**：`draw.innerText++` 雖然能跑（JS 會自動轉型），但要注意型別。若要做加減，建議先用 `Number()` 轉換。
3. **隨機寫法的偏差**：`Math.round(Math.random() * 2)` 出現 0 和 2 的機率只有 1/4，1 的機率卻是 1/2，三選一不公平。正確寫法是 `Math.floor(Math.random() * 3)`。
4. **`<form>` 送出會重新整理頁面**：要用 `event.preventDefault()` 擋掉（見 `事件 - event.html`）。
5. **`<a>` 點了會跳轉**：同樣用 `preventDefault()`。
6. **`for...of` 配合 `addEventListener` 的寫法**：`for (const el of btn)` 會幫每個元素各綁一次事件，`el` 在每次迴圈中都是獨立的，所以閉包可以正確抓到自己的元素。
7. **`const top = ...`** 在 `視差.html` 中使用，`top` 其實是瀏覽器全域變數 `window.top` 的名字，雖然在函式內是合法的，但取名最好避開，例如改成 `scrollY`。
8. **事件綁定要在事件發生前完成**：`載入.html` 的 script 放在 `</body>` 前，執行時 `<img>` 已經存在，圖片又是非同步載入，所以 `img.onload` 一定綁得上。

---

## 五、自我測驗（先自己想，再看答案）

**Q1：** `el.onclick = A` 然後又 `el.onclick = B`，點擊時會執行什麼？
**A：** 只執行 B。`onclick` 會被覆蓋。

**Q2：** `el.addEventListener('click', A)` 和 `el.addEventListener('click', B)` 呢？
**A：** A 和 B 都會執行，順序是先註冊的先執行。

**Q3：** 點擊內層元素時，外層的 click 會觸發嗎？
**A：** 會，因為事件會冒泡。要擋住就在內層用 `event.stopPropagation()`。

**Q4：** `input` 和 `change` 差在哪？
**A：** `input` 每次改值就觸發；`change` 要離開輸入框且值有變化才觸發。

**Q5：** `data-price="2990"` 用 `el.dataset.price` 拿到的是數字還是字串？
**A：** 字串。要做計算請先轉成數字。

**Q6：** 怎樣產生 1 到 6 的隨機整數？
**A：** `Math.floor(Math.random() * 6) + 1`

**Q7：** `document.getElementById('x')` 找不到元素時會回傳什麼？
**A：** `null`。之後對 `null` 做操作會報錯。

---

## 六、延伸練習建議

- 把猜拳的 `Math.round` 改成 `Math.floor`，並加上「清除紀錄」按鈕。
- 卡片頁面加上「數量」，同一商品按兩次時數量變 2，而不是新增兩筆。
- 表單頁加上密碼長度至少 8 碼的檢查，不符合時顯示紅色外框。
- 視差頁把 `scrollTop` 換成 `window.scrollY` 再比較看看。

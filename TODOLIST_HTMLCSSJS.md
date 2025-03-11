# 全体の目次

- **STEP 0. フォルダ構成を準備**
- **STEP 1. 最小限のHTMLを作る**
- **STEP 2. CSS をざっくり整える**
- **STEP 3. JavaScriptの基本準備**
- **STEP 4. 要素を取得し、ボタンを押すとイベント発火**
- **STEP 5. 入力値を取得してテーブルに追加**
- **STEP 6. 入力チェックを改善**
- **STEP 7. データを配列で管理する（表示用関数を作成）**
- **STEP 8. localStorage を使ってデータを保存**
- **STEP 9. 絞り込み（フィルター）機能を追加**
- **STEP 10. さらに工夫してみよう**



## STEP 0. フォルダ構成を準備

### 0-1. フォルダ＆ファイル作成

**フォルダ構成**  
```
my-todo/
├─ index.html
├─ style.css
└─ script.js
```

1. `my-todo` という名前のフォルダを作る。  
2. その中に `index.html`・`style.css`・`script.js` の3ファイルを空で用意する。

**このパートはコピペでOK／特に練習問題なし**  
- ディレクトリを作るだけなので、この指示通りに進めましょう。



## STEP 1. 最小限のHTMLを作る

まずはブラウザに表示するための**基本構造**を用意します。

### 1-1. `index.html`（コピペOK）
```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8" />
  <title>TODOリスト</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <h1>TODOリスト</h1>

  <div id="input-area">
    <label>TODO:
      <input id="todo-input" type="text" placeholder="やることを入力" />
    </label>
    <button id="add-btn">登録</button>
  </div>

  <table id="todo-table">
    <thead>
      <tr>
        <th>やること</th>
        <th>期日</th>
        <th>優先度</th>
        <th>完了</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

  <script src="script.js"></script>
</body>
</html>
```

### 1-2. **問題**  
1. タイトル（`<title>`）や `<h1>` のテキストを、自分が好きな表現に変更してみましょう。  
2. `<h1>` のすぐ下に、アプリの簡単な説明文などを `<p>` タグで書いてみましょう（例：「やることを一覧管理できるページです！」など）。

**ブラウザで `index.html` を開き、タイトルや見出しが変わったかどうか** を確認しましょう。



## STEP 2. CSS をざっくり整える

見た目をシンプルに調整するため、`style.css` を編集します。

### 2-1. `style.css`（コピペOK）
```css
body {
  font-family: sans-serif;
  margin: 20px;
}

h1 {
  margin-bottom: 10px;
}

#input-area {
  margin-bottom: 20px;
}

#todo-input {
  margin-left: 5px;
  width: 200px;
  padding: 5px;
}

#add-btn {
  margin-left: 10px;
  padding: 5px 10px;
}

table {
  width: 100%;
  border-collapse: collapse;
  max-width: 600px; /* お好みで */
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}
```

ブラウザをリロードして、**入力フォームやテーブルの罫線** などが整っているか確認してください。

### 2-2. **問題**  
1. `body` の背景色を薄い色に変更してみましょう（例：`background-color: #f9f9f9;`）。  
2. `th`（ヘッダセル）の背景色を変えたり、文字色を変えてみましょう。  
3. `#todo-input` の幅を変更したり、`border` を付けてみましょう。



## STEP 3. JavaScriptの基本準備

### 3-1. `script.js` にテスト用コード（コピペOK）
```js
console.log("スクリプト読み込みテスト：OK");
```

ブラウザをリロードし、**開発者ツール（F12など）** のコンソールに  
「スクリプト読み込みテスト：OK」  
と表示されるか確認します。  
表示されればOKです。

### 3-2. **問題**  
1. `console.log(...)` で他の文字列を出力してみましょう。自分の名前など、何でもかまいません。  
2. `alert("Hello World!")` を書き、ページを開いた瞬間にアラートが表示されるようにしてみてください。



## STEP 4. 要素を取得し、ボタンを押すとイベント発火

### 4-1. DOM要素の取得（コピペOK）
```js
// script.js
const todoInput = document.getElementById('todo-input'); // テキストボックス
const addBtn = document.getElementById('add-btn');       // 登録ボタン
const todoTable = document.getElementById('todo-table'); // テーブル
const tbody = todoTable.querySelector('tbody');          // tbody部分
```

### 4-2. ボタンクリックのイベントを登録（コピペOK）
```js
addBtn.addEventListener('click', () => {
  console.log("登録ボタンがクリックされました！");
});
```

ブラウザをリロードして、**ボタンをクリック** すると、コンソールに「登録ボタンがクリックされました！」と出るかを確認します。

### 4-3. **問題**  
1. `querySelectorAll` を使って、 `<label>` 要素をすべて取得してみてください。取得できたかコンソールに表示してみましょう。  
2. 新しく `<button>` をもう1つ追加し、クリックすると別のメッセージを表示するようにしてみましょう。



## STEP 5. 入力値を取得してテーブルに追加

**重要な概念**：  
- DOM要素の値を取得して変数に入れる  
- `createElement` で行やセルを作り、`appendChild` でテーブルに追加する  

ここではまず、**コピペでOKの雛形** を示します。その後に**自分で書いてみるとよい問題**を出題します。

### 5-1. HTMLフォームを拡張（コピペOK）
```html
<!-- index.html内のフォーム部分 -->
<div id="input-area">
  <label>TODO:
    <input id="todo-input" type="text" placeholder="やることを入力" />
  </label>
  <label>期日:
    <input id="date-input" type="date" />
  </label>
  <label>優先度:
    <select id="priority">
      <option>低</option>
      <option selected>普通</option>
      <option>高</option>
    </select>
  </label>
  <button id="add-btn">登録</button>
</div>
```

### 5-2. テーブルに行を追加するコード（コピペOK）
```js
// script.js（上部で要素取得がすでにある前提）
const dateInput = document.getElementById('date-input');
const prioritySelect = document.getElementById('priority');

addBtn.addEventListener('click', () => {
  const todoValue = todoInput.value.trim();
  const dateValue = dateInput.value;     // yyyy-mm-dd
  const priorityValue = prioritySelect.value;

  if (!todoValue) {
    alert("TODOを入力してください。");
    return;
  }

  // 新しい行を作る
  const newRow = document.createElement('tr');

  // 「やること」セル
  const tdTodo = document.createElement('td');
  tdTodo.textContent = todoValue;

  // 「期日」セル
  const tdDate = document.createElement('td');
  tdDate.textContent = dateValue;

  // 「優先度」セル
  const tdPriority = document.createElement('td');
  tdPriority.textContent = priorityValue;

  // 「完了」セル（チェックボックス）
  const tdCheck = document.createElement('td');
  const checkBox = document.createElement('input');
  checkBox.type = 'checkbox';
  tdCheck.appendChild(checkBox);

  // 行に各セルをappend
  newRow.appendChild(tdTodo);
  newRow.appendChild(tdDate);
  newRow.appendChild(tdPriority);
  newRow.appendChild(tdCheck);

  // テーブルに行を追加
  tbody.appendChild(newRow);

  // 入力フォームをリセット
  todoInput.value = "";
  dateInput.value = "";
  prioritySelect.value = "普通";
});
```

### 5-3. 動作確認
1. ブラウザでページをリロード。  
2. 「やること」に何か入力・期日や優先度は任意で入力→ 登録ボタン  
3. テーブルに新しい行が追加されているか確認する。

### 5-4. **問題A**: 列を追加してみる  
1. **「担当者」** を入力する欄をフォームに追加してみましょう。  
2. テーブルにも「担当者」列を作成し、登録されたときに表示されるようにしましょう。  
   - HTMLで `<th>` も増やすのを忘れずに  
   - JavaScriptで `document.createElement('td')` をもう1つ作り、`appendChild` で行に追加してください。

### 5-5. **問題B**: 行の色を変えてみる  
- 「優先度」が「高」の場合、その行だけ背景色を変えてみましょう。  
  - `newRow.style.backgroundColor = "lightpink";` など  
  - 条件分岐で `if (priorityValue === "高") { ... }`



## STEP 6. 入力チェックを改善

### 6-1. 基本的なチェック（コピペOK）
```js
if (!todoValue) {
  alert("TODOを入力してください。");
  return;
}
```

これにより「やること」が空のときにアラートを出し、登録処理を中止します。

### 6-2. **問題**: 日付チェック  
1. もし期日が未入力だったらアラートを出す  
2. 期日が現在の日付よりも過去の日付ならアラートを出す  
   - `new Date()` と `new Date(dateValue)` を比較しましょう  
   - 例） `if (new Date(dateValue) < new Date()) { ... }`



## STEP 7. データを配列で管理する（表示用関数を作成）

ここからはより発展的なやり方として、**配列 + レンダリング関数** を導入します。  
実際のアプリでは「入力時にすぐ表を作る」のではなく、**メモリ上のデータ**（配列）を更新し、それをもとに画面を再描画するやり方が一般的です。

### 7-1. `todos` 配列を作る（コピペOK）
```js
let todos = [];
```

### 7-2. テーブルを再描画する関数 `renderTable()`（コピペOK）
```js
function renderTable() {
  // いったんテーブルを空にする
  tbody.innerHTML = "";

  // 配列の要素を1つずつ行にする
  todos.forEach((todoObj) => {
    const newRow = document.createElement('tr');

    const tdTodo = document.createElement('td');
    tdTodo.textContent = todoObj.text;

    const tdDate = document.createElement('td');
    tdDate.textContent = todoObj.date;

    const tdPriority = document.createElement('td');
    tdPriority.textContent = todoObj.priority;

    const tdCheck = document.createElement('td');
    const checkBox = document.createElement('input');
    checkBox.type = 'checkbox';
    checkBox.checked = todoObj.completed; // completedの状態を反映

    // チェックが変わったとき配列内のデータを更新
    checkBox.addEventListener('change', () => {
      todoObj.completed = checkBox.checked;
      renderTable(); // 再描画すると表示に反映
    });

    tdCheck.appendChild(checkBox);

    newRow.appendChild(tdTodo);
    newRow.appendChild(tdDate);
    newRow.appendChild(tdPriority);
    newRow.appendChild(tdCheck);

    tbody.appendChild(newRow);
  });
}
```

### 7-3. 登録ボタンで配列を更新 + 再描画（コピペOK）
```js
addBtn.addEventListener('click', () => {
  const todoValue = todoInput.value.trim();
  const dateValue = dateInput.value;
  const priorityValue = prioritySelect.value;

  if (!todoValue) {
    alert("TODOを入力してください。");
    return;
  }

  // 配列に新規オブジェクトを追加
  todos.push({
    text: todoValue,
    date: dateValue,
    priority: priorityValue,
    completed: false
  });

  // テーブルを再描画
  renderTable();

  // フォームをクリア
  todoInput.value = "";
  dateInput.value = "";
  prioritySelect.value = "普通";
});
```

### 7-4. 動作確認
1. ブラウザをリロードし、いくつかTODOを追加。  
2. チェックボックスをON/OFFして完成状態を変えてみる（文字は取り消し線にならないが、`todoObj.completed` は更新されている）。  
3. `renderTable()` が再度呼ばれ、DOMが作り直されていることを確認。

### 7-5. **問題**  
1. `renderTable()` 内で、テーブルの最上部（`tbody.innerHTML = "";` の直後など）に「`○件のTODOがあります`」と表示してみてください。  
   - 例）`document.getElementById("info").textContent = todos.length + "件のTODOがあります";` という領域を使う  
2. 登録するたびに `console.log()` で配列 `todos` の内容を出力してみましょう。どんなオブジェクトが入っているかを見てみるとデバッグの勉強になります。



## STEP 8. localStorage を使ってデータを保存

### 8-1. 保存用・読み込み用関数（コピペOK）

```js
function saveTodos() {
  localStorage.setItem('todoData', JSON.stringify(todos));
}

function loadTodos() {
  const storedData = localStorage.getItem('todoData');
  if (storedData) {
    todos = JSON.parse(storedData);
  }
}
```

### 8-2. ボタンクリックやチェック時に保存（コピペOK）
```js
addBtn.addEventListener('click', () => {
  // ...todos.push({ ... });
  renderTable();
  saveTodos();
});

checkBox.addEventListener('change', () => {
  todoObj.completed = checkBox.checked;
  renderTable();
  saveTodos();
});
```

### 8-3. ページロード時に復元（コピペOK）
```js
window.addEventListener('DOMContentLoaded', () => {
  loadTodos();
  renderTable();
});
```

### 8-4. 動作確認
1. 何件かTODOを登録し、チェックをいくつか入れる。  
2. ページをリロードして、同じ状態が復元されているかを確認。

### 8-5. **問題**  
1. localStorage のキー名を `myTodoList` に変更してみましょう。問題なく動作するか確認。  
2. 「担当者」や他の項目などを増やした際も、同様に `saveTodos()` で正しく保存されるかチェックしてみてください。



## STEP 9. 絞り込み（フィルター）機能を追加

「すべて」「未完了」「完了済み」の3つのボタンで表示を切り替える例を示します。

### 9-1. フィルターボタンをHTMLに追加（コピペOK）
```html
<!-- index.html内、テーブルの上あたりに追加 -->
<button id="filter-all">すべて</button>
<button id="filter-active">未完了</button>
<button id="filter-completed">完了</button>
```

### 9-2. JavaScriptでフィルター状態を管理（コピペOK）
```js
let currentFilter = 'all';

const filterAllBtn = document.getElementById('filter-all');
const filterActiveBtn = document.getElementById('filter-active');
const filterCompletedBtn = document.getElementById('filter-completed');

filterAllBtn.addEventListener('click', () => {
  currentFilter = 'all';
  renderTable();
});
filterActiveBtn.addEventListener('click', () => {
  currentFilter = 'active';
  renderTable();
});
filterCompletedBtn.addEventListener('click', () => {
  currentFilter = 'completed';
  renderTable();
});

function renderTable() {
  tbody.innerHTML = "";

  // フィルタリング
  let displayTodos = [];
  if (currentFilter === 'all') {
    displayTodos = todos;
  } else if (currentFilter === 'active') {
    displayTodos = todos.filter(t => !t.completed);
  } else {
    // completed
    displayTodos = todos.filter(t => t.completed);
  }

  // ここからは同じループ
  displayTodos.forEach((todoObj) => {
    // ... <tr>生成 etc ...
  });
}
```

### 9-3. 動作確認
- TODOをいくつか登録し、いくつかにチェックを付ける。  
- 「すべて」「未完了」「完了」をクリックして表示が変わるかを確認。  
- フィルターボタンを押しても `todos` はそのままで、**画面に表示する配列だけを切り替えている** ことがポイント。

### 9-4. **問題**  
- 「優先度が高いものだけ表示」ボタンを新たに作ってみましょう。  
  - 例）`filterHighBtn` を追加し、`todos.filter(t => t.priority === "高")` などを使う



## STEP 10. さらに工夫してみよう

ここからは、**より実用的な機能**を追加するための**自由課題**です。学習したDOM操作やイベント処理、配列操作を組み合わせて、自分なりに考えて実装してみましょう。

1. **削除機能**  
   - 各行の右端に「削除」ボタンを付ける  
   - クリックしたら `todos` から該当オブジェクトを削除（`splice()` など）して `renderTable()` し、`saveTodos()`  
2. **編集モード**  
   - 行をダブルクリックするとテキストボックスを表示し、編集後にエンターキーで確定して `todos` を更新する  
3. **ソート機能**  
   - 「期日順」「優先度順」で並び替えボタンを追加  
   - `sort()` メソッドで `todos` の順序を変えてから `renderTable()` する  
4. **CSSやUIの充実**  
   - ボタンをおしゃれに、ホバー時に色を変える  
   - モバイルでも見やすいレイアウトにする  



# まとめ

ここまでのステップを順番に進めると、**シンプルながら実用的なTODOリスト** が完成します。  

- **STEP 0～4** でHTML/CSSの基礎と要素取得・イベント処理を学び、  
- **STEP 5～7** で入力値をテーブルに追加し、配列管理・レンダリングという基本構造を理解、  
- **STEP 8** で `localStorage` を用いた永続化、  
- **STEP 9** で絞り込み表示、  
- **STEP 10** で追加の工夫と拡張へ。

さらに、自分のアイデアを盛り込みながら機能を実装していくと、自然とJavaScriptのスキルが上がっていきます。  
「**コピペしてOKな部分**」と「**自分で手を動かして書いてみる部分（問題）**」をメリハリつけて学習し、**ミスを減らしながら理解を定着** させてください。
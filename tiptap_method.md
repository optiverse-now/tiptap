

@page.tsxで使用されているTiptapのメソッドを抽出し、解説させていただきます。

### 1. エディタの状態チェック系メソッド

#### `isActive()`
```typescript
// 見出しのチェック
editor.isActive('heading', { level: 1 })
editor.isActive('heading', { level: 2 })
editor.isActive('heading', { level: 3 })

// その他の要素のチェック
editor.isActive('bold')
editor.isActive('italic')
editor.isActive('underline')
editor.isActive('bulletList')
editor.isActive('orderedList')
editor.isActive('link')
editor.isActive({ textAlign: 'left' })
editor.isActive({ textAlign: 'center' })
editor.isActive({ textAlign: 'right' })
```
- 現在のカーソル位置の状態をチェック
- 戻り値：boolean
- 使用例：ツールバーのアクティブ状態の制御に使用

### 2. コマンド実行系メソッド

#### `chain()`と`focus()`
```typescript
editor.chain().focus()  // メソッドチェーンの開始
```
- `chain()`: 複数のコマンドを連鎖的に実行するための開始点
- `focus()`: エディタにフォーカスを設定

#### テキストスタイル設定
```typescript
// 見出しと段落
editor.chain().focus().setHeading({ level: 1 | 2 | 3 }).run()
editor.chain().focus().setParagraph().run()

// 文字装飾
editor.chain().focus().toggleBold().run()
editor.chain().focus().toggleItalic().run()
editor.chain().focus().toggleUnderline().run()

// テキストの配置
editor.chain().focus().setTextAlign('left').run()
editor.chain().focus().setTextAlign('center').run()
editor.chain().focus().setTextAlign('right').run()

// リスト
editor.chain().focus().toggleBulletList().run()
editor.chain().focus().toggleOrderedList().run()

// リンクと画像
editor.chain().focus().setLink({ href: url }).run()
editor.chain().focus().setImage({ src: url }).run()

// 編集履歴
editor.chain().focus().undo().run()
editor.chain().focus().redo().run()
```

### 3. メソッドの詳細解説

#### テキストスタイル系
- `setHeading()`: 指定したレベルの見出しを設定
- `setParagraph()`: 通常の段落スタイルを設定
- `toggleBold()`: 太字の切り替え
- `toggleItalic()`: イタリックの切り替え
- `toggleUnderline()`: 下線の切り替え

#### 配置系
- `setTextAlign()`: テキストの配置を設定
  - 引数：'left', 'center', 'right'

#### リスト系
- `toggleBulletList()`: 箇条書きリストの切り替え
- `toggleOrderedList()`: 番号付きリストの切り替え

#### リンク・画像系
- `setLink()`: リンクを設定
  - 引数：`{ href: string }`
- `setImage()`: 画像を挿入
  - 引数：`{ src: string }`

#### 編集履歴系
- `undo()`: 操作を元に戻す
- `redo()`: 操作をやり直す

### 4. 共通の特徴

1. **メソッドチェーン**
   - `chain()`で開始
   - `focus()`でエディタにフォーカス
   - `run()`で変更を適用

2. **トグル動作**
   - `toggle`系のメソッドは、現在の状態を反転
   - 例：太字の場合は解除、非太字の場合は太字化

3. **状態管理との連携**
   - `isActive()`と組み合わせて使用
   - UIの状態とエディタの状態を同期

これらのメソッドを組み合わせることで、リッチテキストエディタとしての様々な機能を実現しています。



### エディタの内容を取得するメソッド

```typescript
// HTMLとして取得
editor.getHTML()
// 例: "<h1>タイトル</h1><p>本文です</p>"

// JSONとして取得
editor.getJSON()
// 例: { type: 'doc', content: [{ type: 'heading', ... }] }

// プレーンテキストとして取得
editor.getText()
// 例: "タイトル\n本文です"
```


`chain().focus()`は主に**エディタの内容を変更する時**に使用します：
```typescript
// 内容を変更する場合（chain().focus()が必要）
editor.chain().focus().setHeading({ level: 1 }).run()
editor.chain().focus().setBold().run()

// 内容を取得する場合（chain().focus()は不要）
const content = editor.getHTML()
const text = editor.getText()

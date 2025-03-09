---
title: "Web で使うテクニック演習 の解説"
---
[ゼミ](/../../index.md)/[webアプリの基礎①](/../index.md)/[JavaScript について学ぶ](./index.md)/Web で使うテクニック演習 の解説

## 解説
#### `index.html`
```html
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8" />
    <link rel="stylesheet" href="style.css" />
</head>

<body>
    <header>
        <div class="appbar">選択中のTab番号:<span class="tab_number">未選択</span></div>
    </header>
    <main>
        <div class="tabs_container"></div>
    </main>

    <footer>© 2021 DeMiA inc.</footer>

    <script src="./index.js"></script>
</body>

</html>

```


#### `style.css`
```css
/* タブカードのスタイル */
.tabs_container {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    padding: 100px 20px 20px 20px;
    justify-content: center;
    max-width: 100%;
    overflow-x: hidden;
}

.tab_card {
    /* width: 120px; */
    width: calc(100% / 6);
    height: 80px;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #ccc;
    font-size: 1.2rem;
    cursor: pointer;
    border-radius: 10px;
    transition: background-color 0.3s;
}

.tab_card:hover {
    background-color: #bbb;
}

.tab_card.active {
    background-color: #28c04a;
    color: white;
    font-weight: bold;
}

.appbar {
    background-color: #333;
    color: white;
    padding: 1em;
    text-align: center;
    font-size: 1.2rem;
    position: fixed;
    width: 100%;
    top: 0;
    left: 0;
    box-sizing: border-box;
}

/* フッターのスタイル */
footer {
    text-align: center;
    padding: 1em;
    background-color: #f8f8f8;
    width: 100%;
    box-sizing: border-box;
}

/* 全体のレイアウト調整 */
body {
    font-family: Arial, sans-serif;
    width: 100%;
    margin: 0;
    padding: 0;
    overflow-x: hidden;
}
```


### 解答例

#### `index.js`
```javascript
'use strict';

const tabsContainer = document.querySelector('.tabs_container');
const tabNumberDisplay = document.querySelector('.tab_number');

/**
 *
 * @param {number} tabNum
 * @param {number} currentMaxTabNum
 * @returns 更新した`currentMaxTabNum`
 */
function createTabs(tabNum, currentMaxTabNum) {
    let count = 1;
    while (count <= tabNum) {
        const currentNum = count + currentMaxTabNum;
        const tab = document.createElement('div');
        tab.classList.add('tab_card');
        tab.textContent = `Tab ${currentNum}`;
        tab.addEventListener('click', () => {
            // 他の全てからactiveを除去
            document
                .querySelectorAll('.tab_card')
                .forEach((t) => t.classList.remove('active'));
            tab.classList.add('active');
            tabNumberDisplay.textContent = currentNum;
        });

        tabsContainer.appendChild(tab);
        count++;
    }
    return (currentMaxTabNum += tabNum);
}

let currentMaxTabNum = 0;
createTabs(100, currentMaxTabNum);

let isRequestPending = false;
window.addEventListener('scroll', () => {
    if (!isRequestPending) {
        window.requestAnimationFrame(() => {
            const el = document.scrollingElement;
            if (el.scrollHeight - el.scrollTop <= el.clientHeight) {
                currentMaxTabNum = createTabs(100, currentMaxTabNum);
            }
            isRequestPending = false;
        });
        isRequestPending = true;
    }
});

```
---
title: "基礎文法演習の解説"
---
[ゼミ](/../../index.md)/[webアプリの基礎①](/../index.md)/[JavaScript について学ぶ](./index.md)/基礎文法演習の解説

## 演習 1 - n 進数への変換
### コード例
```javascript
let num = window.prompt("割られる数を半角数字で入力してください");
const baseNum = window.prompt("割る数を半角数字で入力してください");

let count = 0;
const answer = [];

while (num > 0) {
    answer.unshift(num % baseNum);
    num = Math.floor(num / baseNum);

    count += 1;
}

console.log("answer:", answer);
console.log("count:", count);
```


## 演習 2 - 投票の集計
### コード例
```javascript
answerList.forEach((answer) => {
    const answerId = answer.id;
    const answerPlace = answer.place;
    studentList.forEach((student) => {
        if (student.id === answerId) {
            student.place = answerPlace;
        }
    });
});

console.log(studentList);

```


## 演習 3 - 中間テストの集計

### コード例

```javascript
scoreList.forEach((score) => {
    const studentId = score.studentId;
    const scoreDay = score.day;
    studentList.forEach((student) => {
        if (student.id === studentId) {
            if (scoreDay === 1) {
                student.english = score.english;
                student.math = score.math;
                student.science = score.science;
            } else if (scoreDay === 2) {
                student.japanese = score.japanese;
                student.history = score.history;
                student.informatics = score.informatics;
            }
        }
    });
});

console.log(studentList);
```

## 演習 4 - 無限級数の収束値

### 問題 1

#### コード例

```javascript
// 100回たし合わせる関数
function sumCalculator1() {
    let sum = 0;
    let add = 1;
    while (add <= 100) {
        sum += add;
        add++;
    }
    return sum;
}

console.log("問題1(100回):", sumCalculator1());

// n回足し合わせる関数
function sumCalculator1n(n) {
    let sum = 0;
    let add = 1;
    while (add <= n) {
        sum += add;
        add++;
    }
    return sum;
}
console.log("問題1(n回):", sumCalculator1n(100));
```

### 問題 2

#### コード例

```javascript
// 100回足し合わせる関数
function sumCalculator2() {
    let sum = 0;
    let count = 1;
    while (count <= 100) {
        sum += 1 / (2 * count - 1);
        count++;
    }
    return sum;
}

console.log("問題2(100回):", sumCalculator2(100));

// n回たし合わせる関数
function sumCalculator2n(n) {
    let sum = 0;
    let count = 1;
    while (count <= n) {
        sum += 1 / (2 * count - 1);
        count++;
    }
    return sum;
}
console.log("問題2(n回):", sumCalculator2n(100));

```

### 問題 3

#### コード例

```javascript
// 100回たし合わせる関数
function sumCalculator3() {
    let sum = 0;
    let count = 1;
    while (count <= 100) {
        if (count % 2 === 0) {
            sum -= 4 / (2 * count - 1);
        } else {
            sum += 4 / (2 * count - 1);
        }
        count++;
    }
    return sum;
}
console.log("問題3(100回):", sumCalculator3());

// n回たし合わせる関数
function sumCalculator3n() {
    let sum = 0;
    let count = 1;
    while (count <= 100) {
        if (count % 2 === 0) {
            sum -= 4 / (2 * count - 1);
        } else {
            sum += 4 / (2 * count - 1);
        }
        count++;
    }
    return sum;
}
console.log("問題3(n回):", sumCalculator3n(100));

// 複数パターンたし合わせる関数
function sumOutput3() {
    const roopCounts = [
        1, 2, 3, 4, 5, 10, 20, 50, 100, 200, 500, 1000, 5000, 10000,
    ];
    roopCounts.forEach((roopCount) => {
        const result = sumCalculator3n(roopCount);
        console.log(`${roopCount}回足した時の和は ${result}です。`);
    });
}

sumOutput3();
```

### 問題 4

#### コード例

```javascript
// 100回たし合わせる関数
function sumCalculator4() {
    let sum = 0;
    let count = 1;
    while (count <= 100) {
        if (count % 2 === 0) {
            sum -= 1 / count;
        } else {
            sum += 1 / count;
        }
        count++;
    }
    return sum;
}
console.log("問題4(100回):", sumCalculator4());

// n回足し合わせる関数
function sumCalculator4n(n) {
    let sum = 0;
    let count = 1;
    while (count <= n) {
        if (count % 2 === 0) {
            sum -= 1 / count;
        } else {
            sum += 1 / count;
        }
        count++;
    }
    return sum;
}
console.log("問題4(n回):", sumCalculator4n(100));

// 複数パターンたし合わせる関数
function sumOutput4() {
    const roopCounts = [3, 6, 9, 30, 60, 90, 300, 600, 900, 3000, 9000, 15000];
    roopCounts.forEach((roopCount) => {
        const result = sumCalculator4n(roopCount);
        console.log(`${roopCount}回足した時の和は ${result}です。`);
    });
}

sumOutput4();
```
### 問題5

#### コード例
```javascript
// 100回たし合わせる関数
function sumCalculator5() {
    let sum = 0;
    let count = 1;
    while (count <= 100) {
        sum += 1 / (4 * count - 3) + 1 / (4 * count - 1) - 1 / (2 * count);
        count++;
    }
    return sum;
}
console.log("問題5(100回):", sumCalculator5());

// n回足し合わせる関数
function sumCalculator5n(n) {
    let sum = 0;
    let count = 1;
    while (count <= n) {
        sum += 1 / (4 * count - 3) + 1 / (4 * count - 1) - 1 / (2 * count);
        count++;
    }
    return sum;
}
console.log("問題5(n回):", sumCalculator5n(100));

// 複数パターンたし合わせる関数
function sumOutput5() {
    const roopCounts = [3, 6, 9, 30, 60, 90, 300, 600, 900, 3000, 9000, 15000];
    roopCounts.forEach((roopCount) => {
        const result = sumCalculator5n(roopCount);
        console.log(`${roopCount}回足した時の和は ${result}です。`);
    });
}

sumOutput5();
```
### 問題6

#### コード例
```javascript
// 100回たし合わせる関数
function sumCalculator6() {
    let sum = 0;
    let count = 1;
    while (count <= 100) {
        sum = (sum + 2) ** (1 / 2);
        count++;
    }
    return sum;
}
console.log("問題5(100回):", sumCalculator6());

// n回足し合わせる関数
function sumCalculator6n() {
    let sum = 0;
    let count = 1;
    while (count <= 100) {
        sum = (sum + 2) ** (1 / 2);
        count++;
    }
    return sum;
}
console.log("問題5(n):", sumCalculator6n(100));

// 複数パターンたし合わせる関数
function sumOutput6() {
    const roopCounts = [3, 6, 9, 30, 60, 90, 300, 600, 900, 3000, 9000, 15000];
    roopCounts.forEach((roopCount) => {
        const result = sumCalculator6n(roopCount);
        console.log(`${roopCount}回足した時の和は ${result}です。`);
    });
}
sumOutput6();

```

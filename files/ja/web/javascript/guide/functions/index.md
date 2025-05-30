---
title: 関数
slug: Web/JavaScript/Guide/Functions
l10n:
  sourceCommit: f2dc3d5367203c860cf1a71ce0e972f018523849
---

{{jsSidebar("JavaScript Guide")}} {{PreviousNext("Web/JavaScript/Guide/Loops_and_iteration", "Web/JavaScript/Guide/Expressions_and_operators")}}

関数は、 JavaScript の基本的な構成要素のひとつです。 JavaScript の関数は、プロシージャに似ています。タスクを実行したり値を計算したりする一連の文ですが、プロシージャが関数として認められるためには、何らかの入力を受け取り、入力と出力の間に何らかの明白な関係がある出力を返す必要があります。関数を使用するには、呼び出したいスコープのどこかで関数を定義する必要があります。

より詳しくは [JavaScript の関数に関する完全なリファレンスについての章](/ja/docs/Web/JavaScript/Reference/Functions)をご覧ください。

## 関数の定義

### 関数宣言

**関数定義** (**関数宣言**や**関数定義文** とも呼ばれます) は [`function`](/ja/docs/Web/JavaScript/Reference/Statements/function) キーワードと、それに続く以下の内容で構成されます。

- 関数の名前。
- 関数への引数のリスト。丸括弧でくくり、カンマで区切ります。
- 関数を定義する JavaScript の文。波括弧 `{ /* … */ }` でくくります。

例えば、次のコードは `square` という名前の関数を定義します。

```js
function square(number) {
  return number * number;
}
```

関数 `square` は `number` という名前の引数を 1 つとります。この関数は、引数 (すなわち `number`) の 2 乗を返すように指示する 1 つの文で構成されています。[`return`](/ja/docs/Web/JavaScript/Reference/Statements/return) 文は、関数が返す値、すなわち `number * number` を指定します。

引数は基本的に関数に**値渡し**されます。そのため、関数本体のコードで関数に渡された引数に完全に新しい値を代入しても、**その変更はグローバルまたはその関数を呼び出したコードには反映されません**。

オブジェクトを引数として渡すと、関数がオブジェクトのプロパティを変更した場合、次の例にみられるように、その変更は関数の外側でも反映されます。

```js
function myFunc(theObject) {
  theObject.make = "Toyota";
}

const myCar = {
  make: "Honda",
  model: "Accord",
  year: 1998,
};

console.log(myCar.make); // "Honda"
myFunc(myCar);
console.log(myCar.make); // "Toyota"
```

配列を引数として渡すと、関数が配列の値を変更した場合、次の例にあるように、その変更は関数の外でも反映されます。

```js
function myFunc(theArr) {
  theArr[0] = 30;
}

const arr = [45];

console.log(arr[0]); // 45
myFunc(arr);
console.log(arr[0]); // 30
```

関数の宣言と式は入れ子にすることができ、これにより「スコープチェーン」が形成されます。例を示します。

```js
function addSquares(a, b) {
  function square(x) {
    return x * x;
  }
  return square(a) + square(b);
}
```

詳しくは、[関数スコープとクロージャ](#関数スコープとクロージャ)を参照してください。

### 関数式

ここまでの関数宣言はすべて構文的な文でしたが、関数は[関数式](/ja/docs/Web/JavaScript/Reference/Operators/function)によって作成することもできます。

このような関数は**無名** (anonymous) にすることができます。名前をつけなくてもよいのです。例えば、関数 `square` は次のように定義することができます。

```js
const square = function (number) {
  return number * number;
};

console.log(square(4)); // 16
```

ただし、関数式には名前を指定することもできます。名前を指定することで、関数が自分自身を参照することができ、また、デバッガーのスタックトレースで関数を特定しやすくなります。

```js
const factorial = function fac(n) {
  return n < 2 ? 1 : n * fac(n - 1);
};

console.log(factorial(3)); // 6
```

関数式は、ある関数を別の関数の引数として渡す場合に便利です。次の例では、最初の引数に関数、2 番目の引数に配列を受け取る `map` 関数を定義しています。そして、関数式で定義した関数で呼び出します。

```js
function map(f, a) {
  const result = new Array(a.length);
  for (let i = 0; i < a.length; i++) {
    result[i] = f(a[i]);
  }
  return result;
}

const numbers = [0, 1, 2, 5, 10];
const cubedNumbers = map(function (x) {
  return x * x * x;
}, numbers);
console.log(cubedNumbers); // [0, 1, 8, 125, 1000]
```

JavaScript では、条件に基づいて関数を定義することもできます。例えば次の関数の定義は、 `myFunc` という関数を、変数 `num` が `0` に等しい場合のみ定義します。

```js
let myFunc;
if (num === 0) {
  myFunc = function (theObject) {
    theObject.make = "Toyota";
  };
}
```

これまで説明してきた関数の定義に加えて、{{jsxref("Function")}} コンストラクターを、{{jsxref("Global_Objects/eval", "eval()")}} のような文字列からの関数作成に用いることができます。

**メソッド**は、オブジェクトのプロパティである関数のことです。オブジェクトとメソッドについて詳しくは、「[オブジェクトを利用する](/ja/docs/Web/JavaScript/Guide/Working_with_objects)」の章をご覧ください。

## 関数の呼び出し

関数を*定義*しても、その関数が*実行*されるわけではありません。関数の定義とは、ただ単に関数に名前をつけ、その関数が呼び出されたときに何をするかを指定することです。

関数の**呼び出し**は、指定した引数を用いて実際に指定された動作を実行します。例えば、 `square` という関数を定義した場合、次のようにして呼び出すことができます。

```js
square(5);
```

この文は `5` という引数とともに関数を呼び出します。関数は自身の文を実行し、 `25` という値を返します。

関数を呼び出すときは*スコープ内*になければいけませんが、次の例のように、関数の宣言を[巻き上げる](#関数の巻き上げ)（呼び出しより後に置く）ことができます。関数宣言のスコープは、自身が宣言された関数内（あるいは最上位で宣言されたのであればプログラム全体）になります。

関数の引数は、文字列や数値に限られてはいません。オブジェクト全体を関数に渡すこともできます。`showProps()` 関数 ([オブジェクトの利用](/ja/docs/Web/JavaScript/Guide/Working_with_objects#オブジェクトとプロパティ)の章で定義) は、オブジェクトを引数にとる関数の例です。

関数はその関数自身を呼び出すこともできます。例えば、ここに階乗を計算する関数を示します。

```js
function factorial(n) {
  if (n === 0 || n === 1) {
    return 1;
  }
  return n * factorial(n - 1);
}
```

1 から 5 までの階乗の計算は、次のようになります。

```js
console.log(factorial(1)); // 1
console.log(factorial(2)); // 2
console.log(factorial(3)); // 6
console.log(factorial(4)); // 24
console.log(factorial(5)); // 120
```

関数を呼び出す方法は他にもあります。関数を動的に呼び出す必要があったり、関数の引数の数が変化したり、関数呼び出しのコンテキストを実行時に決定された特定のオブジェクトに設定する必要があったりする場合がよくあります。

*関数はそれ自体がオブジェクト*です。 — そして、それらのオブジェクトはメソッドを持っています。 ({{jsxref("Function")}} オブジェクトを参照してください。) [`call()`](/ja/docs/Web/JavaScript/Reference/Global_Objects/Function/call) and [`apply()`](/ja/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) メソッドを使って、この目的を達成することができます。

### 関数の巻き上げ

以下の例があったとします。

```js
console.log(square(5)); // 25

function square(n) {
  return n * n;
}
```

関数 `square()` が宣言される前に呼び出されていますが、このコードはエラーなく実行されます。これは、JavaScript インタープリターが関数宣言全体を現在のスコープの先頭に巻き上げるためです。したがって、上記のコードは次のコードと同等です。

```js
// すべての関数宣言は、事実上、スコープの最上位にある
function square(n) {
  return n * n;
}

console.log(square(5)); // 25
```

関数巻き上げは関数宣言でのみ作業し、関数式では動作しません。次のコードは動作しません。

```js example-bad
console.log(square(5)); // ReferenceError: Cannot access 'square' before initialization
const square = function (n) {
  return n * n;
};
```

### 再帰

関数は自身を参照し、呼び出すことができます。関数が自身を参照する方法は 3 種類あります。関数式または宣言の名前、あるいは関数オブジェクトを参照するスコープ内の変数によって参照できます。例えば、以下のような関数定義を考えてみましょう。

```js
const foo = function bar() {
  // ここには文が来る
};
```

関数本体内では、関数自体を `bar` または `foo` として参照し、`bar()` または `foo()` を使用して呼び出すことができます。

自身を呼び出す関数のことを**再帰関数**と言います。いくつかの点で、再帰はループに似ています。どちらも同じコードを何度も実行しますし、（無限ループを防ぐため、というより無限再帰を防ぐために）条件が必要です。

例えば、以下のループを考えてみてください。

```js
let x = 0;
// "x < 10" がループの条件
while (x < 10) {
  // 何らかの処理を行う
  x++;
}
```

再帰関数宣言に変換することができ、その関数を呼び出すことで続きます。

```js
function loop(x) {
  // "x >= 10" が終了条件 ("!(x < 10)" と同等)
  if (x >= 10) {
    return;
  }
  // 何らかの処理を行う
  loop(x + 1); // 再帰呼び出し
}
loop(0);
```

一方で、単純な反復ループでは行えないアルゴリズムもあります。例えば、ツリー構造 (例えば [DOM](/ja/docs/Web/API/Document_Object_Model)) のすべてのノードを取得する処理は、再帰を使うとより簡単です。

```js
function walkTree(node) {
  if (node === null) {
    return;
  }
  // ノードに対し処理を行う
  for (let i = 0; i < node.childNodes.length; i++) {
    walkTree(node.childNodes[i]);
  }
}
```

関数 `loop` と比較すると、再帰呼び出しごとにさらに多数の再帰呼び出しを行います。

どんな再帰アルゴリズムも再帰でないものに書き換えることが可能ですが、ロジックがはるかに複雑になることが多く、そのためにはスタックを使用する必要があります。

実際、再帰自体もスタックを使用しています。関数スタックです。このスタックのような動作は、次の例で見ることができます。

```js
function foo(i) {
  if (i < 0) {
    return;
  }
  console.log(`begin: ${i}`);
  foo(i - 1);
  console.log(`end: ${i}`);
}
foo(3);

// 出力:
// begin: 3
// begin: 2
// begin: 1
// begin: 0
// end: 0
// end: 1
// end: 2
// end: 3
```

### 即時実行関数式 (IIFE)

[即時実行関数式 (IIFE)](/ja/docs/Glossary/IIFE) は、式として定義された関数を直接呼び出すコードパターンです。次のように見ていきます。

```js
(function () {
  // 何かする
})();

const value = (function () {
  // 何かする
  return someValue;
})();
```

関数を変数に保存する代わりに、関数はすぐに呼び出されます。これは、関数の本体を記述するのとほぼ同じですが、いくつかの固有の利点があります。

- これにより、変数の追加の[スコープ](#関数スコープとクロージャ)が作成され、変数を有用な場所に限定するのに役立ちます。
- これで、一連の文ではなく、式になります。これにより、変数を初期化するときに複雑な計算ロジックを書くことができます。

詳しくは、用語集の項目の [IIFE](/ja/docs/Glossary/IIFE) を参照してください。

## 関数スコープとクロージャ

関数の内部で宣言された変数は、関数の外部からアクセスすることができません。これは、変数が関数のスコープ内でのみ定義されているためです。その一方、関数は自身が定義されたスコープ内で定義されているすべての変数や関数にアクセスできます。
言い換えると、グローバルスコープで定義された関数は、グローバルスコープで定義されたすべての変数にアクセスできます。ある関数の内部で宣言された関数は、自身の親となる関数内で定義されたすべての変数や、その関数がアクセス権を持つ他の変数にもアクセスできます。

関数は変数の[スコープ](/ja/docs/Glossary/Scope)を形成します。つまり、関数内で定義された変数は、その関数の外部からはアクセスできません。関数のスコープは、それより上のすべてのスコープから継承されます。例えば、グローバルスコープで定義された関数は、グローバルスコープで定義されたすべての変数にアクセスできます。別の関数内で定義された関数も、その親関数で定義されたすべての変数、および親関数がアクセスできるその他の変数にアクセスできます。一方、親関数（およびその他の親スコープ）は、内部関数内で定義された変数や関数にアクセスできません。これにより、内部関数内の変数が一種のカプセル化されます。

```js
// 以下の変数はグローバルスコープで定義
const num1 = 20;
const num2 = 3;
const name = "Chamakh";

// この関数はグローバルスコープで定義
function multiply() {
  return num1 * num2;
}

console.log(multiply()); // 60

// 入れ子になっている関数の例
function getScore() {
  const num1 = 2;
  const num2 = 3;

  function add() {
    return `${name} scored ${num1 + num2}`;
  }

  return add();
}

console.log(getScore()); // "Chamakh scored 5"
```

### クロージャ

関数本体を「クロージャ」とも呼びます。クロージャは、いくつかの変数を参照するソースコードの一部 （最も一般的には関数） で、これらの変数が宣言されたスコープが終了しても、その変数を「記憶」します。

クロージャは通常、親スコープの寿命を超えて変数を記憶していることを示すために、入れ子になった関数で表現されます。しかし、実際には、入れ子になった関数は必要ありません。技術的には、 JavaScript のすべての関数はクロージャを形成しています。ただし、何らかの情報を取得しないものもあり、クロージャは関数である必要もありません。「有用な」クロージャの重要な要素は、次のとおりです。

- いくつかの変数や関数を定義する親スコープ。これは明確な有効期間を持つ必要があります。つまり、ある時点で実行が完了する必要があります。グローバルスコープ以外のすべてのスコープは、この要件を満たします。これには、ブロック、関数、モジュールなどが含まれます。
- 親スコープ内で定義され、親スコープで定義された変数や関数を参照する内部スコープ。
- 内側のスコープは、親スコープの有効期間を超えて存続します。例えば、親スコープの外で定義された変数に保存されるか、親スコープから返されます （親スコープが関数である場合）。
- その後、親スコープの外で関数を呼び出しても、親スコープの実行が完了していても、親スコープで定義された変数や関数にアクセスすることができます。

次の例は、クロージャの典型的な例です。

```js
// 外側の関数は変数 "name" を定義
const pet = function (name) {
  const getName = function () {
    // 内側の関数は外側の関数の変数 "name" にアクセス可能
    return name;
  };
  return getName; // 内側の関数を返すことで、外側の関数に公開する
};
const myPet = pet("Vivie");

console.log(myPet()); // "Vivie"
```

上記のコードより複雑なコードにすることもできます。外側の関数の内部にある変数を操作するメソッドを含む、オブジェクトを返すことができます。

```js
const createPet = function (name) {
  let sex;

  const pet = {
    // setName(newName) is equivalent to setName: function (newName)
    // in this context
    setName(newName) {
      name = newName;
    },

    getName() {
      return name;
    },

    getSex() {
      return sex;
    },

    setSex(newSex) {
      if (
        typeof newSex === "string" &&
        (newSex.toLowerCase() === "male" || newSex.toLowerCase() === "female")
      ) {
        sex = newSex;
      }
    },
  };

  return pet;
};

const pet = createPet("Vivie");
console.log(pet.getName()); // Vivie

pet.setName("Oliver");
pet.setSex("male");
console.log(pet.getSex()); // male
console.log(pet.getName()); // Oliver
```

上記の例で、外側の関数の変数 `name` は内側の関数からアクセスでき、また内側の関数を通さずに内側の変数へアクセスする他の方法はありません。内側の関数の内部変数は、内側の関数の安全な保存領域として振る舞います。それらは内側の関数と連動するデータを、「永続的」かつ「安全に」保持します。関数は変数を割り当てる必要さえなく、また名前を持つ必要もありません。

```js
const getCode = (function () {
  const apiCode = "0]Eal(eh&2"; // 外側の関数が変更できないようにしたいコード

  return function () {
    return apiCode;
  };
})();

console.log(getCode()); // "0]Eal(eh&2"
```

上記のコードでは、 [IIFE](#即時実行関数式_iife) パターンを使用しています。この IIFE スコープ内には、2 つの値、つまり変数 `apiCode` と、返され、変数 `getCode` に代入される無名関数が存在します。 `apiCode` は、返される無名関数のスコープ内にはありますが、プログラムの他の部分には属していないため、 `getCode` 関数以外では `apiCode` の値を読み取る方法はありません。

### 多重入れ子関数

関数は下記のように、多重に入れ子にすることができます。

- 関数 (`A`) の中に関数 (`B`) があり、関数 (`B`) の中に関数 (`C`) があるとします。
- 関数 `B` と `C` はクロージャとなるので、 `B` は `A` にアクセスでき、 `C` は `B` にアクセスできます。
- さらに、 `C` は `A` にアクセス可能な `B` にアクセスできるので、 `C` は `A` にもアクセスできます。

このようにして、クロージャは多重スコープを導入できます。つまり関数のスコープが再帰的に包含されているのです。これを「スコープチェーン」と呼びます。次の例を見てみましょう。

```js
function A(x) {
  function B(y) {
    function C(z) {
      console.log(x + y + z);
    }
    C(3);
  }
  B(2);
}
A(1); // 6 がログに出力される (1 + 2 + 3)
```

この例では、関数 `C` は関数 `B` の引数 `y` と関数 `A` の引数 `x` にアクセスしています。なぜこれが可能かというと、

1. 関数 `B` は関数 `A` に含まれたクロージャとなっています (すなわち、 `B` は `A` の引数と変数にアクセスできます)。
2. 関数 `C` は関数 `B` に含まれたクロージャとなっています。
3. `C` のクロージャには `B` が含まれ、 `B` のクロージャには `A` が含まれます。つまり、 `C` のクロージャには `A` も含まれます。これは、 `C` が `B` と `A` の引数や変数にアクセスできるということを意味しています。言い換えれば、 `C` は `B` と `A` のスコープを順番に連結しているということです。

その一方で、逆は成り立ちません。 `A` は `C` にアクセスできません。なぜなら `A` は、 `C` を変数の一つとして持っている `B` の引数や変数にはアクセスできないからです。このように `C` は `B` の外に対してのみ非公開となっています。

### 名前の衝突

クロージャ中のスコープに同じ名前の 2 つの引数や変数がある場合、**名前の衝突**が生じます。より内側のスコープが優先されるので、最も内側にあるスコープが最優先に、最も外側のスコープが最も低い優先度となります。これがスコープチェーンです。チェーンの最初は最も内側のスコープ、そして最後は最も外側のスコープとなります。次の例を見てみましょう。

```js
function outside() {
  const x = 5;
  function inside(x) {
    return x * 2;
  }
  return inside;
}

console.log(outside()(10)); // 10 ではなく 20 を返す
```

`return x * 2` の文では、`inside` の引数 `x` と `outside` の変数 `x` の間で名前の競合が発生します。この場合のスコープチェーンは、`inside` => `outside` => グローバルオブジェクトとなります。したがって、`inside`の`x`が`outside`の`x`よりも優先され、`10` (`outside`の`x`)ではなく、`20` (`inside`の`x`)が返されます。

## arguments オブジェクトの使用

関数の引数 (argument) は、配列風オブジェクトで管理されます。関数内では、次のようにして渡された引数を指定することができます。

```js
arguments[i];
```

ここで `i` は引数の順序を表す数で、 `0` から始まります。関数に渡された第 1 引数は `arguments[0]` となります。引数の総数は `arguments.length` で表されます。

`arguments` オブジェクトを使用すると、宣言時の引数の数よりも多くの引数を用いて関数を呼び出すことができます。これによって関数に渡す引数の数が前もってわからない場合にしばしば役立ちます。 `arguments.length` を使用することで、実際に関数に渡された引数の数を特定することができます。そして、 `arguments` オブジェクトを使用して各引数にアクセスできます。

例えば、複数の文字列を連結する関数を考えてみましょう。この関数の唯一の正式な引数は，連結する項目を区切る文字を指定する文字列です。この関数は次のように定義されています。

```js
function myConcat(separator) {
  let result = ""; // リストを初期化する
  // 引数について繰り返し
  for (let i = 1; i < arguments.length; i++) {
    result += arguments[i] + separator;
  }
  return result;
}
```

この関数に引数をいくつも渡すことができます。そして、各引数を文字列の「リスト」に連結します。

```js
console.log(myConcat(", ", "red", "orange", "blue"));
// "red, orange, blue, "

console.log(myConcat("; ", "elephant", "giraffe", "lion", "cheetah"));
// "elephant; giraffe; lion; cheetah; "

console.log(myConcat(". ", "sage", "basil", "oregano", "pepper", "parsley"));
// "sage. basil. oregano. pepper. parsley. "
```

> [!NOTE]
> 変数 `arguments` は「配列風の変数」であり、配列ではありません。添字が数値であることと `length` プロパティがあることで、配列風となってはいます。しかし、配列操作のメソッドのすべてを持っているわけではありません。

さらなる情報については、JavaScript リファレンスの {{jsxref("Function")}} オブジェクトをご覧ください。

## 関数の引数

特殊な種類の引数の構文が 2 つあります。それが*デフォルト引数*と*残余引数*です。

### デフォルト引数

JavaScript では、関数の引数は既定で `undefined` となります。しかし、別の既定値が設定されていれば便利だという状況もあるでしょう。デフォルト引数がここで役に立ちます。

以前、既定値を設定する一般的な方法は、関数の本体で引数の値をテストし、`undefined` だった場合にある値を割り当てる、というものでした。

以下の例では、 `b` の値が与えられなかった場合、 `a*b` を評価する際にこの値は `undefined` となるため、 `multiply` を呼び出すと `NaN` が返されます。しかしながら、この例の 2 行目でこの問題を回避しています。

```js
function multiply(a, b) {
  b = typeof b !== "undefined" ? b : 1;
  return a * b;
}

console.log(multiply(5)); // 5
```

*デフォルト引数*を使えば、関数本体での引数チェックはもう必要ありません。これからは、関数の最初で単純に `b` に `1` を代入することができます。

```js
function multiply(a, b = 1) {
  return a * b;
}

console.log(multiply(5)); // 5
```

詳細については、リファレンスの[デフォルト引数](/ja/docs/Web/JavaScript/Reference/Functions/Default_parameters)をご覧ください。

### 残余引数

[残余引数](/ja/docs/Web/JavaScript/Reference/Functions/rest_parameters)の構文によって、不特定多数の引数を配列のように表すことができます。

次の例では、2 つ目から最後までの引数をまとめるのに*残余引数*を使っています。そして最初の引数を使って乗算します。

```js
function multiply(multiplier, ...theArgs) {
  return theArgs.map((x) => multiplier * x);
}

const arr = multiply(2, 1, 2, 3);
console.log(arr); // [2, 4, 6]
```

## アロー関数

[アロー関数式](/ja/docs/Web/JavaScript/Reference/Functions/Arrow_functions)（将来の JavaScript で想定される `->` 構文と区別するために**ファットアロー関数**とも呼ばれる）は関数式と比較してより短い構文を持ち、[`this`](/ja/docs/Web/JavaScript/Reference/Operators/this)、[`arguments`](/ja/docs/Web/JavaScript/Reference/Functions/arguments)、[`super`](/ja/docs/Web/JavaScript/Reference/Operators/super)、[`new.target`](/ja/docs/Web/JavaScript/Reference/Operators/new.target) の値を持ちません。アロー関数は常に無名関数です。

アロー関数の導入には 2 つの要素が絡んでいます。それは*短縮形の関数*と `this` との*結びつけがない*ことです。

### 短縮形の関数

関数パターンによっては、短縮形の関数がうってつけです。比較してみましょう。

```js
const a = ["Hydrogen", "Helium", "Lithium", "Beryllium"];

const a2 = a.map(function (s) {
  return s.length;
});

console.log(a2); // [8, 6, 7, 9]

const a3 = a.map((s) => s.length);

console.log(a3); // [8, 6, 7, 9]
```

### 独自の this を持たない

アロー関数の導入以前は、すべての新しい関数には独自の [this](/ja/docs/Web/JavaScript/Reference/Operators/this) 値が定義されていました (コンストラクターの場合は新しいオブジェクトに、 [strict モード](/ja/docs/Web/JavaScript/Reference/Strict_mode) の関数呼び出しの場合は undefined に、関数が「オブジェクトのメソッド」として呼び出された場合はその基底オブジェクトに、といったように) 。これはオブジェクト指向プログラミングにとっては厄介です。

```js
function Person() {
  // この Person() コンストラクターは自身を `this` と定義します。
  this.age = 0;

  setInterval(function growUp() {
    // strict モードでない場合、この growUp() 関数は
    // Person() コンストラクターによる定義とは異なり、
    // グローバルオブジェクトを `this` として定義します。
    this.age++;
  }, 1000);
}

const p = new Person();
```

ECMAScript 3/5 では、`this` の値をアクセス可能な別の値に割り当てることでこの問題を解決します。

```js
function Person() {
  // `self` の代わりに `that` を選ぶ人もいます。
  // どちらか一方を選び、そちらだけを使うようにしましょう。
  const self = this;
  self.age = 0;

  setInterval(function growUp() {
    // このコールバックは、その値が期待通りのオブジェクトを指す
    // 変数 `self` を参照しています。
    self.age++;
  }, 1000);
}
```

代わりに、[束縛関数](/ja/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)を使って変数を束縛すれば `growUp()` 関数に適切な `this` を渡すことができます。

アロー関数は自身の `this` を持ちません、つまり関数を取り囲む実行コンテキストの `this` の値が使われます。このため、下記のコードでは、`setInterval` に渡される関数内の `this` は、それを取り囲む関数の `this` と同じ値を持ちます。

```js
function Person() {
  this.age = 0;

  setInterval(() => {
    this.age++; // `this` は的確に person オブジェクトを参照する
  }, 1000);
}

const p = new Person();
```

{{PreviousNext("Web/JavaScript/Guide/Loops_and_iteration", "Web/JavaScript/Guide/Expressions_and_operators")}}

---
title: extension.isAllowedIncognitoAccess()
slug: Mozilla/Add-ons/WebExtensions/API/extension/isAllowedIncognitoAccess
l10n:
  sourceCommit: b8a0743ca8b1e1b1b1a95cc93a4413c020f11262
---

{{AddonSidebar}}

检查插件是否允许访问在“隐私浏览”模式下打开的标签页。

这是一个返回 [`Promise`](/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise) 的异步函数。

## 语法

```js-nolint
let isAllowed = browser.extension.isAllowedIncognitoAccess()
```

### 参数

无。

### 返回值

[`Promise`](/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)，将会兑现布尔值：如果插件被允许访问隐私标签页，则为 `true`，否则为 `false`。

## 示例

```js
function logIsAllowed(answer) {
  console.log(`是否被允许：${answer}`);
}

let isAllowed = browser.extension.isAllowedIncognitoAccess();
isAllowed.then(logIsAllowed);
```

{{WebExtExamples}}

## 浏览器兼容性

{{Compat}}

> [!NOTE]
> 此 API 基于 Chromium 的 [`chrome.extension`](https://developer.chrome.google.cn/docs/extensions/reference/api/extension#method-isAllowedIncognitoAccess) API。该文档衍生自 Chromium 代码中的 [`extension.json`](https://chromium.googlesource.com/chromium/src/+/master/chrome/common/extensions/api/extension.json)。

<!--
// Copyright 2015 The Chromium Authors. All rights reserved.
//
// Redistribution and use in source and binary forms, with or without
// modification, are permitted provided that the following conditions are
// met:
//
//    * Redistributions of source code must retain the above copyright
// notice, this list of conditions and the following disclaimer.
//    * Redistributions in binary form must reproduce the above
// copyright notice, this list of conditions and the following disclaimer
// in the documentation and/or other materials provided with the
// distribution.
//    * Neither the name of Google Inc. nor the names of its
// contributors may be used to endorse or promote products derived from
// this software without specific prior written permission.
//
// THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
// "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
// LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
// A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
// OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
// SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
// LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
// DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
// THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
// (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
// OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
-->

# 从零开始学习 jest

Jest 是 JavaScript 测试运行程序，即用于创建，运行和构建测试的 JavaScript 库。Jest 是作为 NPM 软件包分发的，您可以将其安装在任何 JavaScript 项目中。 Jest 是目前最受欢迎的测试运行程序之一，也是 Create React App 的默认选择。

## 官网

- [jest](https://jestjs.io/docs/getting-started)

## 开始

```bash
mkdir jest & cd jest 
npm init -y
yarn add -D jset
```

修改 package.json

```json
 "scripts": {
    "test": "jest"
  },
```

创建测试目录

```bash
mkdir __tests__ && cd __tests__
touch filterByTerm.spec.js
```

filterByTerm.spec.js 的内容吗

```js
describe("Filter function", () => {
  test("it should filter by a search term (link)", () => {
    const input = [
      { id: 1, url: "https://www.url1.dev" },
      { id: 2, url: "https://www.url2.dev" },
      { id: 3, url: "https://www.link3.dev" }
    ];
 
    const output = [{ id: 3, url: "https://www.link3.dev" }];
 
    expect(filterByTerm(input, "link")).toEqual(output);
 
  });
});

```

npm run test 

你会看到，测试失败了：

```bash

FAIL  __tests__/filterByTerm.spec.js
  Filter function
    ✕ it should filter by a search term (2ms)
 
  ● Filter function › it should filter by a search term (link)
 
    ReferenceError: filterByTerm is not defined
 
       9 |     const output = [{ id: 3, url: "https://www.link3.dev" }];
      10 | 
    > 11 |     expect(filterByTerm(input, "link")).toEqual(output);
         |     ^
      12 |   });
      13 | });
      14 |

```

因为我们没有创建  filterByTerm 方法

## 修复上面的问题 

```js
function filterByTerm(inputArr, searchTerm) {
  return inputArr.filter(function(arrayElement) {
    return arrayElement.url.match(searchTerm);
  });
}
 
describe("Filter function", () => {
  test("it should filter by a search term (link)", () => {
    const input = [
      { id: 1, url: "https://www.url1.dev" },
      { id: 2, url: "https://www.url2.dev" },
      { id: 3, url: "https://www.link3.dev" }
    ];
 
    const output = [{ id: 3, url: "https://www.link3.dev" }];
 
    expect(filterByTerm(input, "link")).toEqual(output);
  });
});

```

再次 npm run test  就能看到测试通过了

```bash
PASS  __tests__/filterByTerm.spec.js
  Filter function
    ✓ it should filter by a search term (link) (4ms)
 
Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        0.836s, estimated 1s
```

## 代码覆盖率

什么是代码覆盖率？在谈论它之前，让我们快速调整我们的代码。在项目根目录 src 中创建一个新文件夹，并创建一个名为 filterByTerm.js 的文件，我们将在其中放置和导出函数：

```
mkdir src && cd _$
touch filterByTerm.js
```


```js
function filterByTerm(inputArr, searchTerm) {
  if (!searchTerm) throw Error("searchTerm cannot be empty");
  if (!inputArr.length) throw Error("inputArr cannot be empty"); // new line
  const regex = new RegExp(searchTerm, "i");
  return inputArr.filter(function(arrayElement) {
    return arrayElement.url.match(regex);
  });
}
 
module.exports = filterByTerm;
```

在进行覆盖测试之前，请确保将 filterByTerm 导入 tests / filterByTerm.spec.js：

```js
const filterByTerm = require("../src/filterByTerm");
```

保存文件并进行覆盖测试：

```bash
npm test -- --coverage
```
你会看到下面的输出：

```bash

 PASS  __tests__/filterByTerm.spec.js
  Filter function
    ✓ it should filter by a search term (link) (3ms)
    ✓ it should filter by a search term (uRl) (1ms)
    ✓ it should throw when searchTerm is empty string (2ms)
 
-----------------|----------|----------|----------|----------|-------------------|
File             |  % Stmts | % Branch |  % Funcs |  % Lines | Uncovered Line #s |
-----------------|----------|----------|----------|----------|-------------------|
All files        |     87.5 |       75 |      100 |      100 |                   |
 filterByTerm.js |     87.5 |       75 |      100 |      100 |                 3 |
-----------------|----------|----------|----------|----------|-------------------|
Test Suites: 1 passed, 1 total
Tests:       3 passed, 3 total

```

如果要保持代码覆盖率始终处于开启状态，请在 package.json 中配置 Jest，如下所示：

```json

"scripts": {
    "test": "jest"
  },
  "jest": {
    "collectCoverage": true
  }

```

或者 

```json
 "scripts": {
    "test": "jest --coverage"
  },
```

如果您是一个有视觉素养的人，那么也可以使用一种 HTML 报告来覆盖代码，这就像配置 Jest 一样简单：

```json
"scripts": {
    "test": "jest"
  },
  "jest": {
    "collectCoverage": true,
    "coverageReporters": ["html"]
  },

```

现在，每次运行 npm test 时，您都可以在项目文件夹中访问一个名为 coverage 的新文件夹：jest / jest / coverage /。在该文件夹中，您会发现一堆文件，其中/coverage/index.html 是代码覆盖率的完整 HTML 摘要：

![](https://www.valentinog.com/blog/static/103027a1ecf34032c19f225c2d27a3d7/50517/jest-html-code-coverage-report.png)


如果单击函数名称，你还将看到确切的未经测试的代码行：

![](https://www.valentinog.com/blog/static/b3717c8169510b4cb7ae44f92a927289/dd507/jest-html-code-coverage-report-single-file.png)

## 参考资料 

- [Jest Tutorial for Beginners: Getting Started With JavaScript Testing
](https://www.valentinog.com/blog/jest/)
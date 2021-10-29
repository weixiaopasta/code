#### 安装jest遇到的问题

https://zhuanlan.zhihu.com/p/396293224


#### jest安装与VSCode配置

我们在项目目录下执行npm install --save-dev jest ，即可完成安装。在安装完成之后，需要对 package.json 进行配置，在 scripts 配置中新增 "test": jest ，这样我们可以通过 npm run test 执行测试。

为了让VSCode 可以对 Jest 进行代码提示和检查。我们需要额外配置：

```
执行 npm install -d "@types/jest" 安装类型检查和代码提示。
在 .eslintrc.json 中的 env 配置中增加 "jest": true 的配置。
```


### 同步方法的测试

我们假定有以下的测试代码：
```
// sum.js
function sum(a, b) {
  const na = a || 0;
  const nb = b || 0;
  return na + nb;
}
module.exports = { sum };
```
测试代码如下：
```
// sum.test.js
const { sum } = require('./sum');

test('common test', () => {
  expect(sum(1, 2)).toBe(3);
});
```

#### test 方法
接受两个参数，第一个参数是对测试的说明，第二个参数是一个方法，测试代码写在这个方法中。

##### expect 方法
其中关键的测试断言是 expect 方法。expect 执行参数中指定的方法，返回一个 Expect 对象，我们使用该对象与预期结果进行比较。


##### Matcher
在Jest 中把Expect 对象中用于比较的属性成为Matcher 。

常见的Matcher 有：
```
toBe：用于对象的比较
toEqual/toBeGreaterThan/toBeLessThan……：用于数字的比较
toMatch：用于字符串的比较（toMatch 方法的参数可以是正则表达式/reg/）
toContain：用于List 和Map 判断是否包含指定元素
toThrow：用于判断是否抛出异常
```

### 异步方法的测试

异步方法有两种常见的模式，一种是使用callback 方法，另外一种是使用Promise 。

#### Callback方式的异步测试

我们假定以下异步方法：
```
// fetchDataCallback.js
function fetchData(callback) {
  const data = 'hello world!';
  setTimeout(callback(data), 1000);
}

module.exports = { fetchData };

```
我们可以在 callback 方法中进行测试。但是要注意Jest默认在执行完 test 中代码就退出。因此下面的代码是起不到测试作用的。
```
// 错误的示例
test('the data is hello world', () => {
  function callback(data) {
    expect(data).toBe('peanut butter');
  }

  fetchData(callback);
}); 
```
为了让Jest 可以等待异步方法执行完成，我们可以在调用test 方法的时候传入一个done 方法，Jest 会等待 done 方法执行完成才退出。如果 done 方法没有被调用（比如超时），Jest 会返回测试失败。下面是正确的测试方法：
```
const { fetchData } from 'fetchDataCallback');

test('the data is hello world', done => {
  function callback(data) {
    expect(data).toBe('hello world');
    done();
  }

  fetchData(callback);
});
```

#### Promise方式的异步测试

Promise 方式的异步测试更加简单，我们可以在then方法里面进行判断，也可以使用ES6中的async/await方法编写同步风格的测试代码。


我们假定以下的异步方法：
```
function fetchDataWithPromise() {
  return new Promise((resolve) => {
    setTimeout(() => resolve('hello promise'), 1000);
  });
}
```
我们可以通过下面的代码进行测试：
```
// 在then()方法中进行测试
test('promise test', () => {
  return fetchDataWithPromise().then((data) => {
    expect(data).toBe('hello promise');
  });
});

// 通过async/await进行测试
test('promise test 2', async () => {
  const data = await fetchDataWithPromise();
  expect(data).toBe('hello promise');
});

```

### Mock

Jest中我们常用jest.fn来定义一个mock方法，用spyOn来mock类。

假定我们有一个deleteUserData方法，这是一个危险的方法，它会删除用户数据，如果删除成功会返回0。在测试中，我们可以用mock方法来替代该方法。
```
function deleteUserData(userName) {
  // 删除用户数据
  return 0;
}

// 需要测试方法
function accountAction(userName, actionFunc) {
  const ret = actionFunc(userName);
  if (ret === 0) {
    // 执行剩余业务逻辑
    return 0;
  } else {
    return -1;
  }
}

// 测试代码
test('accountAction test', () => {
  const deleteMock = jest.fn();
  deleteMock.mockReturnValue(0);

  const userName = 'germlin';
  expect(accountAction(userName, deleteMock)).toEqual(0);
});
```
上面的代码通过mockReturnValue指定了返回值。除此之外，我们还可以用假的逻辑来替换真实的逻辑。
```
function deleteUserData(nameList) {
  // 返回删除的用户数量
}

function accountAction(nameList, actionFunction) {
  const ret = actionFunction(nameList);
  if (ret === nameList.length) {
    // 执行剩余业务逻辑
    return 0;
  } else {
    return -1;
  }
}

// 测试代码
test('accountAction test 2', () => {
  const deleteMock = jest.fn();
  deleteMock.mockImplementation((nameList) => nameList.length);

  const nameList = ['germlin', 'yuellin'];
  expect(accountAction(nameList, deleteMock)).toEqual(0);
});
```
jest.fn可以用来mock方法，如果需要对一个类中的某些方法进行mock，我们可以使用jest.spyOn(类名，方法名).mockImplementation方法。

```
// video.js，需要测试的类和方法
const video = {
  play() {
    // 返回播放是否成功（true/false)
  },
};

module.exports = video;

// video.test.js 测试代码
test('video test', () => {
    const videoSpy = jest.spyOn(video, 'play').mockImplementation(() => true);
    expect(video.play).toBeTruthy;
    videoSpy.mockRestore();
})

```


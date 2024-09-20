### Jest

#### ES6 Class Mock 的 四种方式

https://jestjs.io/docs/es6-class-mocks

**sound-player.js**

```javascript
export default class SoundPlayer {
  constructor() {
    this.foo = 'bar';
  }

  playSoundFile(fileName) {
    console.log('Playing sound file ' + fileName);
  }
}
```

**sound-player-consumer.js**

```javascript
import SoundPlayer from './sound-player';

export default class SoundPlayerConsumer {
  constructor() {
    this.soundPlayer = new SoundPlayer();
  }

  playSomethingCool() {
    const coolSoundFileName = 'song.mp3';
    this.soundPlayer.playSoundFile(coolSoundFileName);
  }
}
```

我们现在来模拟**SoundPlayer**类

##### Automatic mock

```js
import SoundPlayer from './sound-player';
import SoundPlayerConsumer from './sound-player-consumer';
// Automatic mock
jest.mock('./sound-player'); // SoundPlayer is now a mock constructor

beforeEach(() => {
  // Clear all instances and calls to constructor and all methods:
  SoundPlayer.mockClear();
});
```

##### Manual mock

**_mocks__/sound-player.js**

```js
// Import this named export into your test file:
export const mockPlaySoundFile = jest.fn();
const mock = jest.fn().mockImplementation(() => {
  return {playSoundFile: mockPlaySoundFile};
});

export default mock;
```

**sound-player-consumer.test.js**

```js
import SoundPlayer, {mockPlaySoundFile} from './sound-player';
import SoundPlayerConsumer from './sound-player-consumer';
jest.mock('./sound-player'); // SoundPlayer is now a mock constructor

beforeEach(() => {
  // Clear all instances and calls to constructor and all methods:
  SoundPlayer.mockClear();
  mockPlaySoundFile.mockClear();
});

it('We can check if the consumer called the class constructor', () => {
  const soundPlayerConsumer = new SoundPlayerConsumer();
  expect(SoundPlayer).toHaveBeenCalledTimes(1);
});

it('We can check if the consumer called a method on the class instance', () => {
  const soundPlayerConsumer = new SoundPlayerConsumer();
  const coolSoundFileName = 'song.mp3';
  soundPlayerConsumer.playSomethingCool();
  expect(mockPlaySoundFile).toHaveBeenCalledWith(coolSoundFileName);
});
```

##### jest.mock(path[, moduleFactory])

The module factory must be a function that returns a function - a higher-order function (HOF—高阶函数).

```js
import SoundPlayer from './sound-player';
const mockPlaySoundFile = jest.fn();
jest.mock('./sound-player', () => {
  return jest.fn().mockImplementation(() => {
    return {playSoundFile: mockPlaySoundFile};
  });
});
```

`jest.mock()` 会被提升到作用域顶层，因此声明的变量 `fakePlaySoundFile` 不能在 `moduleFactory` 中使用（否则会报错：使用之前未定义）。但以 `mock`开头的变量除外（jest会做额外处理）。

```js
// Note: this will fail
import SoundPlayer from './sound-player';
const fakePlaySoundFile = jest.fn();
jest.mock('./sound-player', () => {
  return jest.fn().mockImplementation(() => {
    return {playSoundFile: fakePlaySoundFile};
  });
});
```

**不能返回箭头函数**，ES5 doesn't have arrow functions nor classes

```js
jest.mock('./sound-player', () => {
  return function () {
    return {playSoundFile: () => {}};
  };
});
```

**Note: Arrow functions won't work**

```js
jest.mock('./sound-player', () => {
  return () => {
    // Does not work; arrow functions can't be called with new
    return {playSoundFile: () => {}};
  };
});
```

**Mocking non-default class exports**

mock非default exports的class

```js
import {SoundPlayer} from './sound-player';
jest.mock('./sound-player', () => {
  // Works and lets you check for constructor calls:
  return {
    SoundPlayer: jest.fn().mockImplementation(() => {
      return {playSoundFile: () => {}};
    }),
  };
});

```

##### mockImplementation()和mockImplementationOnce()

```js
import SoundPlayer from './sound-player';
import SoundPlayerConsumer from './sound-player-consumer';

jest.mock('./sound-player');

describe('When SoundPlayer throws an error', () => {
  beforeAll(() => {
    SoundPlayer.mockImplementation(() => {
      return {
        playSoundFile: () => {
          throw new Error('Test error');
        },
      };
    });
  });

  it('Should throw an error when calling playSomethingCool', () => {
    const soundPlayerConsumer = new SoundPlayerConsumer();
    expect(() => soundPlayerConsumer.playSomethingCool()).toThrow();
  });
});
```

##### Summary

- `Automatic mock` 会使用 mock constructor 替代 ES6 class，使用 mock function（always return `undefined`）替代类的方法，因此我们可以调用类的方法（已被替换掉）。Method calls are saved in `theAutomaticMock.mock.instances[index].methodName.mock.calls`
- `Manual mock` 和 `jest.mock(path[, moduleFactory])` 都用到了 jest.fn + mockImplementation，需要手动模拟类的属性和方法

- 后三者都用到`mockImplementation`

##### Complete example

`jest.mock(path[, moduleFactory])` 方法的完整例子

**sound-player-consumer.test.js**

```js
import SoundPlayer from './sound-player';
import SoundPlayerConsumer from './sound-player-consumer';

const mockPlaySoundFile = jest.fn();
jest.mock('./sound-player', () => {
  return jest.fn().mockImplementation(() => {
    return {playSoundFile: mockPlaySoundFile};
  });
});

beforeEach(() => {
  SoundPlayer.mockClear();
  mockPlaySoundFile.mockClear();
});

it('The consumer should be able to call new() on SoundPlayer', () => {
  const soundPlayerConsumer = new SoundPlayerConsumer();
  // Ensure constructor created the object:
  expect(soundPlayerConsumer).toBeTruthy();
});

it('We can check if the consumer called the class constructor', () => {
  const soundPlayerConsumer = new SoundPlayerConsumer();
  expect(SoundPlayer).toHaveBeenCalledTimes(1);
});

it('We can check if the consumer called a method on the class instance', () => {
  const soundPlayerConsumer = new SoundPlayerConsumer();
  const coolSoundFileName = 'song.mp3';
  soundPlayerConsumer.playSomethingCool();
  expect(mockPlaySoundFile.mock.calls[0][0]).toEqual(coolSoundFileName);
});
```

#### 绕过模块模拟

**jest.requireActual**

Jest允许您在测试用例中模拟整个模块。这对于您测试你的代码是否正常调用某个模块的函数是很有用的。 然而，您有时可能想要在您的 *测试文件*中只使用部分模拟的模块， 在这种情况下，你想要访问原生的实现，而不是模拟的版本。

考虑为这个 `createUser`函数编写一个测试用例:

**createUser.js**

```javascript
import fetch from 'node-fetch';

export const createUser = async () => {
  const response = await fetch('http://website.com/users', {method: 'POST'});
  const userId = await response.text();
  return userId;
};
```

您的测试将要模拟 `fetch` 函数，这样我们就可以确保它在没有实发出际网络请求的情况下被调用。 但是，您还需要使用一个`Response`(包装在 `Promise`中) 来模拟`fetch`的返回值，因为我们的函数使用它来获取已经创建用户的ID。 因此，您最初可以尝试编写这样的测试用例:

```javascript
jest.mock('node-fetch');

import fetch, {Response} from 'node-fetch';
import {createUser} from './createUser';

test('createUser calls fetch with the right args and returns the user id', async () => {
  fetch.mockReturnValue(Promise.resolve(new Response('4')));

  const userId = await createUser();

  expect(fetch).toHaveBeenCalledTimes(1);
  expect(fetch).toHaveBeenCalledWith('http://website.com/users', {
    method: 'POST',
  });
  expect(userId).toBe('4');
});
```

但是，如果运行该测试，您将发现`createUser`函数将失败，并抛出错误: `TypeError: response.text is not a function`。 这是因为您从 `node-fetch`导入的 `Response` 类已经被模拟了(由测试文件顶部的`jest.mock`调用) ，所以它不再具备原有的行为。

为了解决这样的问题，Jest 提供了 `jest.requireActual` 辅助器。 要使上述测试用例工作，请对测试文件中导入的内容做如下更改:

```javascript
// BEFORE
jest.mock('node-fetch');
import fetch, {Response} from 'node-fetch';
```

```javascript
// AFTER
jest.mock('node-fetch');
import fetch from 'node-fetch';
const {Response} = jest.requireActual('node-fetch');
```

这允许您的测试文件从`node-fetch`导入真实的 `Response` 对象，而不是一个模拟的版本。 意味着测试用例现在将正确通过。

#### Mocking methods which are not implemented in JSDOM

如果JSDOM（web标准的JavaScript实现，用于Node.js）的一些方法在test中没有执行，例如：`window.matchMedia()`，Jest returns `TypeError: window.matchMedia is not a function` and doesn't properly execute the test.

解决：模拟 `window.matchMedia()`

In this case, mocking `matchMedia` in the test file should solve the issue:

```js
Object.defineProperty(window, 'matchMedia', {
  writable: true,
  value: jest.fn().mockImplementation(query => ({
    matches: false,
    media: query,
    onchange: null,
    addListener: jest.fn(), // deprecated
    removeListener: jest.fn(), // deprecated
    addEventListener: jest.fn(),
    removeEventListener: jest.fn(),
    dispatchEvent: jest.fn(),
  })),
});
```

如果上述代码段直接运行在要测试的文件中，jest依然会返回相同的错误。此时应该将上述代码抽取到另外的文件（matchMedia.mock.js），且必须被引入在要测试的模块（file-to-test）之前。

```js
import './matchMedia.mock'; // Must be imported before the tested file
import {myMethod} from './file-to-test';

describe('myMethod()', () => {
  // Test the method here...
});
```

#### 配置

https://juejin.cn/post/7003595612977365028#heading-54

##### setupFilesAfterEnv [array]

在测试框架被安装在环境之后，每个测试文件执行之前，执行所配置的路径的文件。

```js
module.exports = {
  setupFilesAfterEnv: ['./jest.setup.js','<rootDir>/setup.js'],
};
```

##### moduleNameMapper [object<string, string | array<string>>]

**.***

- `.`  ：匹配除换行符之外的任何单字符
- `*`  ：匹配前面的子表达式零次或多次

**$1**  ：$1是第一个小括号里的内容，$2是第二个小括号里面的内容，依此类推

```js
{
  "moduleNameMapper": {
   	"^@/(.*)$": "<rootDir>/src/$1",
    "components/(.*)$": "<rootDir>/src/components/$1",
    "module_name_(.*)": "<rootDir>/substituted_module_$1.js",
    // 支持数组
    "assets/(.*)": [
      "<rootDir>/images/$1",
      "<rootDir>/photos/$1",
      "<rootDir>/recipes/$1"
    ]
  }
}
```

#### 覆盖率指标

%stmts是语句覆盖率（statement coverage）：是不是每个语句都执行了？

%Branch分支覆盖率（branch coverage）：是不是每个if代码块都执行了？

%Funcs函数覆盖率（function coverage）：是不是每个函数都调用了？

%Lines行覆盖率（line coverage）：是不是每一行都执行了？

### React Test

https://www.jianshu.com/p/1b7ba84e5a73

https://www.jianshu.com/p/b271e1395167

#### Core API

##### Queries

| search type （单个元素） | search type （多个元素） | function （查询单个元素）                                    |                     适用场景                      |
| :----------------------: | :----------------------: | ------------------------------------------------------------ | :-----------------------------------------------: |
|          getBy           |         getAllBy         | getByText getByRole getByLabelText getByPlaceholderText getByAltText getByDisplayValue | 由于只返回元素或error，适用于确定该元素存在的情况 |
|         queryBy          |        queryAllBy        | queryByText queryByRole queryByLabelText queryByPlaceholderText queryByAltText queryByDisplayValue |                用于元素可能不存在                 |
|          findBy          |        findAllBy         | findByText findByRole findByLabelText findByPlaceholderText findByAltText findByDisplayValue |                   用于异步元素                    |

参数可以是字符串或正则，如果参数是字符串，则默认是精确匹配（exact match）。

```xml
LabelText: getByLabelText: <label for="search" />
PlaceholderText: getByPlaceholderText: <input placeholder="Search" />
AltText: getByAltText: <img alt="profile" />
DisplayValue: getByDisplayValue: <input value="JavaScript" />
TestId: getByTestId: <div data-testid='search'></div>
```

###### getBy 和 queryBy 的区别

区别是：`getByText` 找不到元素时，会直接抛异常，test case 会随之中断报错；而 `queryByText` 在这种情况下是返回 `null`，所以 `queryBy` 常用于断言某个元素不存在于 Document 中。这种测试挺常见的，比如传个参数到组件里让它隐藏掉某个元素。

###### getByRole

The `getByRole` function is usually used to retrieve elements by [aria-label attributes](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_aria-label_attribute). However, there are also [implicit roles on HTML elements](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles) -- like button for a button element. （某些元素已经隐式地含有role）

A neat feature of `getByRole` is that [it suggests roles if you provide a role that's not available](https://twitter.com/rwieruch/status/1260912349978013696).（如果你设置的role无效，这里有些建议）

The neat thing about `getByRole`: it shows all the selectable roles if you provide a role that isn't available in the rendered component's HTML:（如果你设置的role无效，将会展示可选的role供你设置）

```jsx
// App.jsx
...
 <input
   id="search"
   type="text"
   value={value}
   onChange={onChange}
 />
...
```

这里 `<input>` 标签的 role 是 `textbox`（不知道 role 是啥？没关系，`getByRole(瞎写一个)`，控制台会告诉你的）

```jsx
import React from 'react';
import { render, screen } from '@testing-library/react';

import App from './App';

describe('App', () => {
  test('renders App component', () => {
    render(<App />);

    screen.getByRole('');
  });
});
```

This means that the previous test outputs the following to the command line after running it:

```text
Unable to find an accessible element with the role ""

Here are the accessible roles:

document:

Name "":
<body />

--------------------------------------------------
textbox:

Name "Search:":
<input
  id="search"
  type="text"
  value=""
/>

--------------------------------------------------
```

```js
expect(screen.getByRole('textbox')).toBeInTheDocument();
```

So quite often it isn't necessary to assign aria roles to HTML elements explicitly for the sake of testing, because the DOM already has implicit（隐式） roles attached to HTML elements. 

###### findBy 函数

是异步函数；用在一些需要异步渲染的元素测试上

**异步元素**

```jsx
function getUser() {
  return Promise.resolve({ id: '1', name: 'Robin' });
}

function App() {
  const [search, setSearch] = React.useState('');
  const [user, setUser] = React.useState(null);

  React.useEffect(() => {
    const loadUser = async () => {
      const user = await getUser();
      setUser(user);
    };

    loadUser();
  }, []);

  function handleChange(event) {
    setSearch(event.target.value);
  }

  return (
    <div>
      {user ? <p>Signed in as {user.name}</p> : null}

      <Search value={search} onChange={handleChange}>
        Search:
      </Search>

      <p>Searches for {search ? search : '...'}</p>
    </div>
  );
}
```

```js
import React from 'react';
import { render, screen } from '@testing-library/react';

import App from './App';

describe('App', () => {
  test('renders App component', async () => {
    render(<App />);

    expect(screen.queryByText(/Signed in as/)).toBeNull();

    screen.debug();

    expect(await screen.findByText(/Signed in as/)).toBeInTheDocument();

    screen.debug();
  });
});
```

###### 应该使用哪个查询

- getByRole

> 它可以用于查询处于`accessibility tree`中的所有元素，并且可以通过`name`选项过滤返回的元素。**它应该是你查询的首选项**。大多数情况下，它都会带着`name`选项一起使用，就像这样：`getByRole('button', {name: /submit/i})`。这里是Roles的列表可供参考[Roles list](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FAccessibility%2FARIA%2FARIA_Techniques%23Roles)

- getByLabelText

> 这是查询表单元素的首选项

- getByPlaceholderText

> 这是查询表单元素的一个代替方案

- getByText

> 它对表单没有用，但这是找到大多数非交互式元素（例如div和spans）的第一方法

- getByDisplayValue

- getByAltText

> 如果你查询的元素支持`alt`text（例如`img` `area` `input`），那么可以使用它来进行查询

- getByTitle

> 通过title属性查询，但是注意，一般通过屏幕查看页面的用户是无法直接看到titile的

- getByTestId

> 它通常用于查询一些用户看不到听不到的内容，这些内容无法通过Role或者Text匹配到。



#### @testing-library/jest-dom

扩展断言

```js
//引入
import '@testing-library/jest-dom';
```

```js
//Custom matchers
toBeDisabled
toBeEnabled
toBeEmptyDOMElement
toBeInTheDocument
toBeInvalid
toBeRequired
toBeValid
toBeVisible
toContainElement
toContainHTML
toHaveAccessibleDescription
toHaveAccessibleName
toHaveAttribute
toHaveClass
toHaveFocus
toHaveFormValues
toHaveStyle
toHaveTextContent
toHaveValue
toHaveDisplayValue
toBeChecked
toBePartiallyChecked
toHaveErrorMessage
```

#### @test-library/user-wvent

React Testing Library（以下简称**RTL**）

它为 **RTL** 提供了一整套用户操作集: 除了`type`，还包括如下几种，大家有空可以试一下，都非常直观。

- click(element, eventInit, options)
- dblClick(element, eventInit, options)
- type(element, text, [options])
- upload(element, file, [{ clickInit, changeInit }])
- clear(element)
- selectOptions(element, values)
- deselectOptions(element, values)
- tab({shift, focusTrap})
- hover(element)
- unhover(element)
- paste(element, text, eventInit, options)
- specialChars

#### msw

模拟请求接口

#### reacr-test-render

https://zh-hans.reactjs.org/docs/test-renderer.html

##### TestRenderer

- [`TestRenderer.create()`](https://zh-hans.reactjs.org/docs/test-renderer.html#testrenderercreate)
- [`TestRenderer.act()`](https://zh-hans.reactjs.org/docs/test-renderer.html#testrendereract)

##### TestRenderer instance

- [`testRenderer.toJSON()`](https://zh-hans.reactjs.org/docs/test-renderer.html#testrenderertojson)
- [`testRenderer.toTree()`](https://zh-hans.reactjs.org/docs/test-renderer.html#testrenderertotree)
- [`testRenderer.update()`](https://zh-hans.reactjs.org/docs/test-renderer.html#testrendererupdate)
- [`testRenderer.unmount()`](https://zh-hans.reactjs.org/docs/test-renderer.html#testrendererunmount)
- [`testRenderer.getInstance()`](https://zh-hans.reactjs.org/docs/test-renderer.html#testrenderergetinstance)
- [`testRenderer.root`](https://zh-hans.reactjs.org/docs/test-renderer.html#testrendererroot)

##### TestInstance

- [`testInstance.find()`](https://zh-hans.reactjs.org/docs/test-renderer.html#testinstancefind)
- [`testInstance.findByType()`](https://zh-hans.reactjs.org/docs/test-renderer.html#testinstancefindbytype)
- [`testInstance.findByProps()`](https://zh-hans.reactjs.org/docs/test-renderer.html#testinstancefindbyprops)
- [`testInstance.findAll()`](https://zh-hans.reactjs.org/docs/test-renderer.html#testinstancefindall)
- [`testInstance.findAllByType()`](https://zh-hans.reactjs.org/docs/test-renderer.html#testinstancefindallbytype)
- [`testInstance.findAllByProps()`](https://zh-hans.reactjs.org/docs/test-renderer.html#testinstancefindallbyprops)
- [`testInstance.instance`](https://zh-hans.reactjs.org/docs/test-renderer.html#testinstanceinstance)
- [`testInstance.type`](https://zh-hans.reactjs.org/docs/test-renderer.html#testinstancetype)
- [`testInstance.props`](https://zh-hans.reactjs.org/docs/test-renderer.html#testinstanceprops)
- [`testInstance.parent`](https://zh-hans.reactjs.org/docs/test-renderer.html#testinstanceparent)
- [`testInstance.children`](https://zh-hans.reactjs.org/docs/test-renderer.html#testinstancechildren)

#### @testing-library/react

基础教程：https://www.robinwieruch.de/react-testing-library/

##### API

###### cleanup

Unmounts React trees that were mounted with [render](http://localhost:3000/docs/react-testing-library/api#render).

如果测试框架支持全局属性`afterEach`(like mocha, Jest, and Jasmine)，则会自动执行，不需额外手动引入。

###### act

 对react-dom/test-utils中的[act](https://links.jianshu.com/go?to=https%3A%2F%2Freactjs.org%2Fdocs%2Ftest-utils.html%23act)函数进行了一层包装。act的作用是在你进行断言之前，保证所有由组件渲染、用户交互以及数据获取产生的**更新**全部在dom实现。

```jsx
act(() => {
  // render components
});
act(() => {
  // fireEvent
});
// make assertions
```

#### 模拟ref

1. You should use `jest.mock()` and `jest.requireActual()` to partial mock `react` module. It means you only need to mock `useRef` hook, keep others as original.
2. Use `jest.mocked` to make your TS types correctly.
3. Use `Object.defineProperty()` to define the setter and getter methods of `deviceRef`. We will add the spy to the `addEventListener` method of the current element in the setter.
4. Use `jest.resetAllMocks()` to reset partial mocked `react` module to original version after testing.

```js
import React, { useState, useRef, useEffect } from 'react';

interface Props {}

export enum EVENT_TYPE {
  MOUSEOVER = 'MOUSEOVER',
  MOUSEOUT = 'MOUSEOUT',
}

export const DeviceModule = (props: Props) => {
  const [isTooltipVisible, changeTooltipVisibility] = useState(false);
  const deviceRef = useRef<HTMLDivElement>(null);
  useEffect(() => {
    if (deviceRef && deviceRef.current) {
      deviceRef.current.addEventListener(EVENT_TYPE.MOUSEOVER, () => changeTooltipVisibility(true));
      deviceRef.current.addEventListener(EVENT_TYPE.MOUSEOUT, () => changeTooltipVisibility(false));
    }
    return () => {
      if (deviceRef && deviceRef.current) {
        deviceRef.current.removeEventListener(EVENT_TYPE.MOUSEOVER, () => changeTooltipVisibility(true));
        deviceRef.current.removeEventListener(EVENT_TYPE.MOUSEOUT, () => changeTooltipVisibility(false));
      }
    };
  });

  return <div ref={deviceRef}>my device module</div>;
};
```

```js
import React, { useRef } from 'react';
import { mount } from 'enzyme';
import { DeviceModule, EVENT_TYPE } from './';

jest.mock('react', () => {
  const originReact = jest.requireActual('react');
  return {
    ...originReact,
    useRef: jest.fn(),
  };
});

const mUseRef = jest.cked(useRef);

describe('66561050', () => {
  afterAll(() => {
    jest.resetAllMocks();
  });
  it('should add event listener for device ref and do cleanup work when component unmount', () => {
    const mRef = { current: {} };
    let addEventListenerSpy!: jest.SpyInstance;
    let removeEventListenerSpy!: jest.SpyInstance;
    Object.defineProperty(mRef, 'current', {
      get() {
        return this._current;
      },
      set(current) {
        if (current) {
          addEventListenerSpy = jest.spyOn(current, 'addEventListener');
          removeEventListenerSpy = jest.spyOn(current, 'removeEventListener');
        }

        this._current = current;
      },
    });
    mUseRef.mockReturnValueOnce(mRef);
    const wrapper = mount(<DeviceModule />);
    expect(addEventListenerSpy).toBeCalledWith(EVENT_TYPE.MOUSEOVER, expect.any(Function));
    expect(addEventListenerSpy).toBeCalledWith(EVENT_TYPE.MOUSEOUT, expect.any(Function));
    wrapper.unmount();
    expect(removeEventListenerSpy).toBeCalledWith(EVENT_TYPE.MOUSEOVER, expect.any(Function));
    expect(removeEventListenerSpy).toBeCalledWith(EVENT_TYPE.MOUSEOUT, expect.any(Function));
  });
```


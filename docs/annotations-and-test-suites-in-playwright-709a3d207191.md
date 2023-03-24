# 剧作家中的注释和测试套件

> 原文：<https://medium.com/nerd-for-tech/annotations-and-test-suites-in-playwright-709a3d207191?source=collection_archive---------4----------------------->

# 释文

我们可以将注释应用到单个测试或者一组测试中。注释可以是有条件的，这意味着它们只在条件为真时使用。注释可能会受到测试夹具的影响。同一个测试上的多个注释，也许在不同的设置中，是可能的。

剧作家测试提供了处理失败、剥落、跳过、聚焦和标签测试的测试注释:

test.skip(title，testFunction):它将测试标记为不相关。剧作家测试不做这样的测试。当测试与特定设置无关时，使用此注释。

```
test.skip('skip this test', *async* ({ page }) => {
  // This test is not run
});
```

test . fail([条件，描述]):测试被标记为失败。剧作家测试将执行这个测试，以确认它失败。如果测试没有失败，剧作家测试将会抱怨。

```
test('not yet ready', *async* ({ page }) => {
  test.fail();
  // ...
});
```

test.fixme(title，testFunction):测试被标记为失败。与失败注释相反，剧作家测试不会执行这个测试。当测试运行缓慢或崩溃时，使用 fixme。

```
test.fixme('test to be fixed', *async* ({ page }) => {
  // ...
});
```

test.slow([condition，description]):它将测试归类为迟缓，并将测试超时增加两倍。

```
test('slow test', *async* ({ page }) => {
  test.slow();
  // ...
});
```

test.only(title，testFunction):可以缩小各种测试的范围。当有焦点测试时，只执行焦点测试。

```
test.only('focus this test', *async* ({ page }) => {
  // Run only focused tests in the entire project.
});
```

test.skip(condition，description):根据具体情况，您可能能够绕过某些测试。

```
test.skip('skip this test', *async* ({ page }) => {
  // This test is not run
});test('skip this test', *async* ({ page, browserName }) => {
  test.skip(browserName === 'firefox', 'Still working on it');
  //it will skip if the condition true
});
```

例如，您可以通过传递一个回调函数在 Chromium 中运行一组测试。

```
test.describe('chromium only', () => {
  test.skip(({ browserName }) => browserName !== 'chromium', 'Chromium only!'); test.beforeAll(*async* () => {
    // This hook is only run in Chromium.
  }); test('test 1', *async* ({ page }) => {
    // This test is only run in Chromium.
  }); test('test 2', *async* ({ page }) => {
    // This test is only run in Chromium.
  });
});
```

为了避免运行`beforeEach`钩子，您可以在钩子本身中放置注释。

```
test.beforeEach(*async* ({ page, isMobile }) => {
  test.fixme(isMobile, 'Settings page does not work in mobile yet'); *await* page.goto('http://localhost:3000/settings');
});test('user profile', *async* ({ page }) => {
  *await* page.click('text=My Profile');
  // ...
});
```

# 分组和组织测试

有些功能，比如测试套件，对剧作家来说是不可用的。但是，您可以对测试进行分组，在挂钩到组之前/之后给它们一个逻辑名称或范围。

```
*const* { test, expect } = require('@playwright/test');

test.describe('two tests', () => {
  test('one', *async* ({ page }) => {
    // ...
  });

  test('two', *async* ({ page }) => {
    // ...
  });
});
```

您可能希望将您的测试指定为[@快速](http://twitter.com/fast)或[@慢速](http://twitter.com/slow)，并使用适当的标签执行测试。为此，我们建议使用— grep 和— grep-invert 命令行标志:

```
*const* { test, expect } = require('@playwright/test');

test('Test login page @fast @regression', *async* ({ page }) => {
  // ...
});

test('Test full report @slow', *async* ({ page }) => {
  // ...
});
```

然后，您将只能运行该测试:

```
npx playwright test --grep @fast
```

它防止我们在不同的文件或另一个文件夹结构中保存相同的代码。
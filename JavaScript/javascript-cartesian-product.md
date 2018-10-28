# 使用 JavaScript 实现笛卡尔积计算

## 笛卡尔积

由两个或多个集合中的元素，相互组合的出来的另一个大的集合

例如扑克牌，实际上是由两个集合中的元素组成的

- [A, 2, 3, 4, 5, 6, 7, ,8 ,9, 10, J, Q, K] 一共 13 个
- [♠, ♥, ♦, ♣] 一共 4 个

由上面两个元素两两组合，得出了

- [♠A, ♠2, ♠3, ♠4, ...]
- [♥A, ♥2, ♥3, ♥4, ...]
- [♦A, ♦2, ♦3, ♦4, ...]
- [♣A, ♣2, ♣3, ♣4, ...]

一共有 13 • 4 = 52 个元素

## 实际运用

在商用领域上，使用到笛卡尔积的，就是根据商品规格，计算出商品的组合，例如，给出下列的规格

```js
const data = {
  color: ['黑色', '白色', '金色'], // 颜色
  storage: ['64', '128', '256', '512'], // 存储空间
  size: ['5.5', '6.5', '7.5'], // 屏幕大小
  brand: ['Apple', 'Huawei', 'Mi', 'vivo'], // 品牌
};
```

我们需要得出如下的数据结构

```js
const result = [
  {
    color: '',
    storage: '',
    size: '',
    brand: '',
  },
  {
    color: '',
    storage: '',
    size: '',
    brand: '',
  },
  // ...
];
```

## 实现

首先，先计算两个集合的情况

```js
const d = {
  color: ['黑色', '白色', '金色'],
  storage: ['64', '128', '256', '512'],
};

function cartesianTwo(dataSet) {
  const entries = Object.entries(dataSet);
  
  // entries:
  // [
  //   ['color', ['黑色', '白色', '金色']]
  //   ['storage', ['64', '128', '256', '512' ]]
  // ]

  const a = entries[0]; // ['color', ['黑色', '白色', '金色']]
  const b = entries[1]; // ['storage', ['64', '128', '256', '512' ]]

  const aKey = a[0]; // 'color'
  const aValue = a[1]; // 'storage'

  const bKey = b[0]; // ['黑色', '白色', '金色']
  const bValue = b[1]; // ['64', '128', '256', '512' ]

  // 下面就是核心的算法
  const products = [];

  Object.keys(aValue).forEach((aIndex) => {
    Object.keys(bValue).forEach((bIndex) => {
      // 使用两层循环，根据 a, b 的值不断生成新的组合
      products.push({
        [aKey]: aValue[aIndex],
        [bKey]: bValue[bIndex],
      });
    });
  });

  return products;
}
```

通过上面的算法，可以计算出两个组合的笛卡尔积，但如果有更多的集合呢？

模拟一下手动计算笛卡尔积的过程，可以发现，实际上每次的计算都是两个集合之间的计算

- 一个新的集合 + 一个空的集合（占位）
    - 第一轮计算，但人工手动计算的时候，往往在潜意识已经忽略了
- 一个新的集合 + 前几个集合的笛卡尔积的集合

因此，将会有一个计算两个集合的笛卡尔积的函数，命名为 `cartesianProduct`, 同时，将会有两一个函数 `cartesian` 用作计算某一整个数据集的笛卡尔积的入口

👇 函数 `cartesian`

```js
function cartesian(input) {
  const entries = Object.entries(input || {});

  let products = [];
  entries.forEach((entry) => {
    products = cartesianProduct(entry, products);
  });

  return products;
}
```

- 使用 `Object.entries()` 来将数据集化为一个数组，以便遍历
- 在循环体中，我们不断取出当前遍历的元素，即新集合；同时将已经计算好的笛卡尔积的数组传入，与新集合一同计算
- `products` 每次都要指向新的数组，因为要从 `cartesianProduct` 返回的都是一个新的数组

👇 函数 `cartesianProduct`

```js
function cartesianProduct(a, currentProducts) {

  const aKey = a[0];
  const aValue = a[1];
  const _currentProducts = currentProducts || [];

  // 1.
  if (Object.keys(_currentProducts).length <= 0) {
    return aValue.map(value => ({
      [aKey]: value,
    }));
  }

  const products = [];
  
  Object.keys(aValue).forEach((key) => {
    _currentProducts.forEach((product) => {
      // 2.
      products.push({
        ...product,
        [aKey]: aValue[key],
      });
    })
  });

  return products;
}
```

1. 我们添增一个判断，如果 `currentProducts` 为空，则表明当前是第一轮计算，直接返回集合转换一下数据结构后返回
    - 转换数据结构，即：本来数据集是以数组形式，元素以字符串形式存在的，但我们最后要的结果是以数组形式，元素以对象形式存在的
2. 在核心算法这里，我们改变了一下推入数组中的元素的编码方式
    - 需要新建一个对象，并将当前内循环，即已经计算好笛卡尔积的部分，复制到新对象，随后将新的属性添加到新对象中
    - 这里必须是使用需要新的对象，否则不断往后计算的笛卡尔积时，新属性得到的，始终是新集合中最后一个元素的值，因为 `currentProducts` 中的值会不断地用到，如果像下面一样一直像内循环中的 `product` 中直接添加属性，只会导致不断修改同一个对象

    ```js
    // ❌ 错误
    Object.keys(aValue).forEach((key) => {
      _currentProducts.forEach((product) => {
        product[aKey] = aValue[key];
        products.push(product);
      })
    });
    ```



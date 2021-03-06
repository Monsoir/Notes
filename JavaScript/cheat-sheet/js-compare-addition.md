# JavaScript 中如何正确比较 0.1 + 0.2 =?= 0.3

为什么在 JavaScript 中 0.1 + 0.2 === 0.3 为 `false` ？

简单来说，一句概括：由于 JavaScript 采用的浮点数存储方法，并不能很好的存储像 0.1, 0.2 此类的小数，会出现偏差的情况。

所以，连计算的因子存储过程中都有误差，那么计算的结果为正确的可能性就很小了

---

但如果计算结果在一定的误差范围内，我们就可以看作是相等的，因此，要判断两个结果的差值，即

```js
Math.abs(0.1 + 0.2 - 0.3) <= Number.EPSILON
```


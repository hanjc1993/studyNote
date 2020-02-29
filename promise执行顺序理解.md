```js
new Promise((res, rej) => {
  res(1);
  rej(2);
  res(3);
  console.log("aa");
})
  .then(res => {
    console.log("bb");
    console.log("res1", res);
    throw 5;
  })
  .catch(err => {
    console.log("rej1", err);
    return 4;
  })
  .then(res => {
    console.log("res2", res);
  });

// 'aa' 'bb' 'res1,1' 'rej1,5' 'res2,4'
```

### promise 执行顺序

1. promise 里面的 res 或者 rej 只能执行其中一个，写再多都只执行第一个
2. promise 里面的函数内容，在 res 后者 rej 触发后仍然会执行
3. promise 里面的内容执行完成之后，才开始回调
4. 不论是.catch 还是.then 仅对上一级负责
5. throw 可以触发 catch 里的回调，return 可以触发.then 里的回调
6. 向下传递的时候，如果临近的下一级没有被触发，则继续往下传递，直到触发或报错或终止。
   - 比如.then 下面依次是.catch 和.then，在 return 的时候，会跳过.catch 直接触发下面的.then

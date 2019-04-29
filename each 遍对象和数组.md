## 
```
function each(obj, callback) {
  let length, i = 0;
  for (let item in obj) {
    // 回调函数 退出的时候， 退出循环
    // 在回调函数内， this 指向， 指向传入的 对象元素
    if (callback.call(obj[item], item, obj[item]) === false) {
      break;
    } 
  }
  return obj;
}
console.time('each');
...// 计算某段代码的运行时间
console.timeEnd('each');
```

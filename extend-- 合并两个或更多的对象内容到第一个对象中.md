## 实现功能：
- extend(target [, object1] [,objectN])
- 后面的参数，都是传入的对象，内容都会复制到目标对象中，后面的会覆盖前面的。
## 第一版：
```
function extend(...args) {
  let copy;
  let target = args.shift();
  args.forEach(option => {
    if (option !== null) {
      for (name in option) {
        copy = option[name];
        if (copy !== undefined) target[name] = copy;
      }
    }
  })
  return target;
}
```
## 第二版， 可以选择进行深拷贝
```
function extend (deep, target, ...options) {
  if (typeof deep !== 'boolean') {
    options.unshift(target);
    target = deep || {};
    deep = false;
  }
  if (typeof target !== 'object') target = {};
  let src, copy, clone, copyIsArray;
  options.forEach(item  => {
    if (item !== null) {
      for (let name in item) {
        src = target[name];
        copy = item[name];
        // 解决循环调用。
        if (target === copy) {
          continue;
        }
        // 加入类型判断，
        if (deep && copy && (isPlainObject(copy) || copyIsArray = Array.isArray(copy))) {
          if (copyIsArray) {
            copyIsArray = false;
            clone = src && Array.isArray(src) ? src : [];
          } else clone = src && isPlainObject(src) ? src : {};
          target[name] = extend(deep, src, copy);
        } else if (copy !== undefined) target[name] = copy;
      }
    }
  })
  return target;
}
// 为了处理， 目标属性值 与 待复制的 对象的属性值类型不一致的情况， 加入类型判断。
```

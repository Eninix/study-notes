***目录:***

[TOC]

---

# 1. 获取元素

```typescript
document.querySelector(selector) //获取第一个对象
document.querySelectorAll(selector) //获取所有对象

/* 获取页面中 id为title的DOM元素
document.querySelector('#title)
*/
```

# 2. 类型断言

```typescript
var img1 = document.querySelector('#image') as HTMLImageElement
//通过类型断言可以访问到元素特有的属性 例如:img标签的src属性
img1.src = './2.jpg'
console.log(img1)
```


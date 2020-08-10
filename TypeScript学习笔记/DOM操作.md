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

# 3. 读取和设置文本内容

```typescript
let h = document.querySelector('#title') as HTMLHeadingElement

console.log(h.innerText)

h.innerText = '李白的静夜思'

h.innerText += '李白'
```

# 4. 操作样式(style)

```typescript
let p = document.querySelector('#txt') as HTMLParagraphElement

p.style.fontSize = '200px'
p.style.color = 'red'

p.style.display = 'none'
p.style.display = 'block'

```

# 5.操作样式(class)

```typescript
let p = document.querySelector('#txt') as HTMLParagraphElement

//添加类
p.classList.add('a', 'w')

//移除类
p.classList.remove('a', 'w')

//判断类名是否存在
let has: boolean = p.classList.contains('bbq')
```


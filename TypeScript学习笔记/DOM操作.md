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

# 6. 操作事件(addEventListener)

```typescript
let btn = document.querySelector('#btn') as HTMLButtonElement

//添加事件
//点击事件
btn.addEventListener('click',() => {
    console.log('你还真的点下去了!')
})
//鼠标移入事件
btn.addEventListener('mouseenter', () => { 
    console.log('你放上去了...哈哈!')
})
```

# 7. 操作事件(事件对象)

```typescript
var n = 30

let btn = document.querySelector('#btn') as HTMLButtonElement

btn.addEventListener('click', (event) => {

    // console.log('event.type为', event.type)
    // console.log('event.target为', event.target)
    // console.log('event为', event)

    let target = event.target as HTMLButtonElement
    n += 1
    target.style.fontSize = n + 'px'
    console.log('啊啊啊~我变成了', n, 'px~~')

})
```

# 8. 移除事件

```typescript
var n = 30

let btn = document.querySelector('#btn') as HTMLButtonElement

function bb(event)
{
    let target = event.target as HTMLButtonElement
    n += 1
    target.style.fontSize = n + 'px'
    console.log('啊啊啊~我变成了', n, 'px~~')

}

btn.addEventListener('click', bb, {once: true}) //若事件只需触发一次,可以添加once: true

btn.removeEventListener('click', bb) //移除事件
```


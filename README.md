<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    @import url('https://fonts.googleapis.com/css?family=Roboto+Mono:100');
html, body {
  font-family: 'Roboto Mono', monospace;
  background: #212121;
  height: 100%;
}
.container {
  height: 100%;
  width: 100%;
  justify-content: center;
  align-items: center;
  display: flex;
}
.text {
  font-weight: 100;
  font-size: 28px;
  color: #FAFAFA;
}
.dud {
  color: #757575;
}
  </style>
</head>
<body>
  <div class="container">
    <div class="text"></div>
  </div>
  <script>
    // ——————————————————————————————————————————————————
// TextScramble
// ——————————————————————————————————————————————————

class TextScramble {
  constructor(el) {
    this.el = el
    this.chars = '!<>-_\\/[]{}—=+*^?#________'
    this.update = this.update.bind(this)
  }
  setText(newText) {
    const oldText = this.el.innerText
    const length = Math.max(oldText.length, newText.length)
    const promise = new Promise((resolve) => this.resolve = resolve)
    this.queue = []
    for (let i = 0; i < length; i++) {
      const from = oldText[i] || ''
      const to = newText[i] || ''
      const start = Math.floor(Math.random() * 40)
      const end = start + Math.floor(Math.random() * 40)
      this.queue.push({ from, to, start, end })
    }
    cancelAnimationFrame(this.frameRequest)
    this.frame = 0
    this.update()
    return promise
  }
  update() {
    let output = ''
    let complete = 0
    for (let i = 0, n = this.queue.length; i < n; i++) {
      let { from, to, start, end, char } = this.queue[i]
      if (this.frame >= end) {
        complete++
        output += to
      } else if (this.frame >= start) {
        if (!char || Math.random() < 0.28) {
          char = this.randomChar()
          this.queue[i].char = char
        }
        output += `<span class="dud">${char}</span>`
      } else {
        output += from
      }
    }
    this.el.innerHTML = output
    if (complete === this.queue.length) {
      this.resolve()
    } else {
      this.frameRequest = requestAnimationFrame(this.update)
      this.frame++
    }
  }
  randomChar() {
    return this.chars[Math.floor(Math.random() * this.chars.length)]
  }
}

// ——————————————————————————————————————————————————
// Example
// ——————————————————————————————————————————————————

const phrases = [
  'Hello.',' Welcome to the official website of',
  'Jesse Hollingsworth.'
]

const el = document.querySelector('.text')
const fx = new TextScramble(el)

let counter = 0
const next = () => {
  fx.setText(phrases[counter]).then(() => {
    setTimeout(next, 4500)
  })
  counter = (counter + 1) % phrases.length
}

next()
  </script>
</body>
</html>

<div id="badges">
  <a href="https://www.elihartnett.com">
    <img src="https://img.shields.io/badge/Website-blue?style=flat-square&logo=data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABoAAAAPCAQAAABQWBdtAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAAmJLR0QA/4ePzL8AAAAJcEhZcwAALiMAAC4jAXilP3YAAAAHdElNRQfmCAQXEwW7N45BAAACZElEQVQozz2Ry0uUYRyFn/ebmzNexvnMCjW1vIwpWhEKs1AJzTYZWkSFkJAgBhFFROKqnbQJapGrQIMgjQpBxo1CUKEkdJOazEWaTqXpqFHNNDPf+2uhdP6B5zznKAAUpbTSTAUu2+8Df1vXM9efyUvmsSzG6CPGWU7g4h23mAEAB0cYZYNfzPCaBU/8kAzpkH6kO3W+qBWu46OYfqJEuUvOJqWJV1i85yo1lNPIHZYrZFiSsiZPpFaMZc7jwE8QYYNLOKGUIMIUh7FtgvHSTaRaJkXEkueyX5imDmhmDmGCKhvnaEPxgKd4MTEx8RChYqnQRS0p5BDlWVZSeMoCZezDS9jOUTKwaHHV7yGPFBy4SFFWHlKgEoCd4yzZxhrmKne+CI3TSjoNihVMBGVXWZRSjp9CcskTLzblxACEGJ8S44M3R8IlXMYkbMcNfHDO7N5bkJed5jISJLGTqjyoLUWFJuL4fkodw8CN4LOzhoewvcefVl6RX7mjOCs303S7Vx1fMnAVKQ8QZ4gb+utsfBYf1bhJwGMs5qgzSFctzo7M9vyuqiuB02eKp1tlUUREQlIjRLlIOidZRXgLbYSx6N88bStOupyr3fJHRLQMilf4TADFNWL85TZk00ecn9zGjwMDG9vpVLON8kZERCLSIcpigAxyCSLM0gRQxkNiRJmglwv0GMPbfpzSU2KJlu/SK6YQohGDDlaw6MO3WaeEG4SMqJn0x+oTnfqeLOqY/qaDul2bmi904SbAJMI09fzfNYODqQ2B6pqikuxdrlQd31idn4yMykcSSUa5j0UHTcAIA8T+Ab8OAaIcdNIyAAAAJXRFWHRkYXRlOmNyZWF0ZQAyMDIyLTA4LTA0VDIzOjE5OjA1KzAwOjAwzFuNvgAAACV0RVh0ZGF0ZTptb2RpZnkAMjAyMi0wOC0wNFQyMzoxOTowNSswMDowML0GNQIAAAAASUVORK5CYII=&logoColor=white" alt="Website Badge"/>
</div>

![](https://komarev.com/ghpvc/?username=elichartnett&style=flat-square)

[![GitHub Streak](http://github-readme-streak-stats.herokuapp.com?user=elichartnett&hide_border=true)](https://git.io/streak-stats)

⚠️Take public repos with a grain of salt and use with caution. They are learner projects, not production apps!⚠️

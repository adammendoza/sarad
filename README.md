sarad
======================

Write a program only by a simple click

### Demo

Left click to generate a code and right click to erase a code.

![screenshot](http://abagames.sakura.ne.jp/16/sarad/screenshot11.gif)

* [p5.js demo](http://abagames.sakura.ne.jp/16/sarad/app/index.html)

### How it works

Sarad is a [word salad](https://en.wikipedia.org/wiki/Word_salad) friendly programming language.
Its interpreter never output any error for any generated program.
Sarad code can be automatically generated by an [LSTM](https://en.wikipedia.org/wiki/Long_short-term_memory) model 
with [RecurrentJS](http://cs.stanford.edu/people/karpathy/recurrentjs/).

1. Select input sentences for training from 
[processing sample codes](https://github.com/processing/processing-docs/tree/master/content/examples).

  ```
  int whichBar = mouseX / barWidth;
  if (whichBar != lastBar) {
    int barX = whichBar * barWidth;
    fill(barX, 100, mouseY);
    rect(barX, 0, barWidth, height);
    lastBar = whichBar;
  }
  ```

2. Convert them to sarad codes with the [pde2sarad](https://github.com/abagames/pde2sarad).
Digits('0'-'9') are converted to 'D' and general variable names are changed to 'V0'-'V4'.

  ```
  =V0 / mouseX V1
  if != V0 V2
    =V3 * V0 V1
    fill/3 V3 DDD mouseY
    rect/4 V3 0 V1 height
    =V2 V0
  ```

  Sarad is also a [stack-oriented programming language](https://en.wikipedia.org/wiki/Stack-oriented_programming_language)
  written in a [polish notation](https://en.wikipedia.org/wiki/Polish_notation).

3. The model is trained with the [sarad-learner](https://github.com/abagames/sarad-learner).

4. Generate a code with the tained LSTM model.

  ```
  if V0
    stroke/1 DDD
  else
    stroke/1 0
  line/4 - mouseX DD mouseX + mouseX DD mouseY
  D DDD 0
  vertex/3 - width DD - mouseY 1 0
  ```

5. Assign digits to 'D', convert the operator into infix notation and eliminate dead code.

  ```
  if(V0)
    stroke(656)
  else
    stroke(0)
  line((mouseX - 51), mouseX, (mouseX + 75), mouseY)
  vertex((width - 16), (mouseY - 1), 0)
  ```

### Acknowledgement

#### Libraries

[RecurrentJS](https://github.com/karpathy/recurrentjs) /
[p5.js](http://p5js.org/) /
[lz-string](http://pieroxy.net/blog/pages/lz-string/index.html) /
[lodash](https://lodash.com/)

License
----------
MIT

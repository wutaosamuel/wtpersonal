# Angular


## Contents

1. [组件中双向数据流动(@Input & @Output)](组件中双向数据流动)

## 组件中双向数据流动

### 有时候, 数据无法传递. 丢弃

- @Input value 和 @Output valueChange 无法准确检测 value 值已经改变. 在多次继承下尤其明显.
- 会尝试使用传值@ViewChild 或者@ViewChildren

母子组件的双向数据流动会使用@Input 和@Output 来修饰变量. 更多的细节参考[官方导向](https://angular.io/guide/inputs-outputs)

```ts
// Import Input, Output and EventEmitter
import { Input, Output, EventEmitter } from "@angular/core";
```


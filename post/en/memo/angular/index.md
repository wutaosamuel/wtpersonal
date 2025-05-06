# Angular


## Contents

1. [Bidirectional data flow between components(@Input & @Output)](bidirectional-data-flow-between-components)

## Bidirectional data flow between components

### Sometimes, not work very well. ABANDON

- @Input value and @Output valueChange cannot detect value change when child component valueChange emit a value, especially, Multiple inheritance.
- next, try to use @ViewChild or @ViewChildren

Bidirectional data flow between parent component and child component.
It uses @Input and @Output decorates the variables.
The more Input and Output details are at [official angular guide](https://angular.io/guide/inputs-outputs)

```ts
// Import Input, Output and EventEmitter
import { Input, Output, EventEmitter } from "@angular/core";
```


## Expressioner

This is a simple expression grammer builder. You can use this tool to define your expression grammer.

In other word, you define how to work of expression like `a + b`, `a | b` or other any kind in a simple way.

What is expression? See https://en.wikipedia.org/wiki/Expression_(computer_science)?

## Install

`npm install expressioner`

## Example

- import library

```
var expressioner = require("expressioner");
```

- initial expression grammar

```
let arithmetic = expressioner({
    "+": {
        priority: 10,
        opNum: 2,
        execute: function(a, b) {
            return Number(a) + Number(b);
        }
    },
    "-": {
        priority: 10,
        opNum: 2,
        execute: function(a, b) {
            return Number(a) - Number(b);
        }
    },
    "*": {
        priority: 20,
        opNum: 2,
        execute: function(a, b) {
            return Number(a) * Number(b);
        }
    },
    "++": {
        priority: 40,
        opNum: 1,
        execute: function(a) {
            return Number(a) + 1;
        }
    },
    "(": {
        type: "start"
    },
    ")": {
        type: "close",
        match: "("
    }
});
```

- write expression by your grammer and run it.

```
console.log( arithmetic(" 3 + (2 * 4)++").value );
// result is 12.
```

## Usage

### var arithmetic = expressioner( operationMap )
  
  In the example, the big map passed to expressioner is our operationMap. It's used to define our expression grammer. 

  Key of operationMap is keyword of operation symbol, Value of operationMap is a config of operation symbol. The attributes of config include :

- type

  `"start" | "end" | undefined`

  There are some types for operation symbol. For example, in math, `+` is different from `(` and `)`. Blanket in math stands for block type which includes a start symbol and end symbol. The type attribute is used to define block concept in expression. So type need start and end value.

- match

  `string`
  
  When we define "end" type of operation symbol, we need to know it's who's end. By using type and match, we can define a pair of block by this:

```
   "{": {
        type: "start"
    },
    "}": {
        type: "close",
        match: "{" // match "{" symbol
    }
```

- needAfterSpace

  `true | false`, default is `false`
  
  We can declare our operation symbol should have space after it. When it's true means: `a+ b` is legal and `a+b` is not legal.

- needBeforeSpace

  `true | false`, default is `false`

  We can declare our operation symbol should have space before it. When it's true means: `a +b` is legal and `a+b` is not legal.

- priority
  
  `number`

  It's used to define priority of operation symbol. For example, if `*` has a higher priority than `+`, expression `a + b * c`, should run like this:

```
var t1 = b * c;
var t2 = a + t1;
```

- opNum
  
  `1 || 2`
  
  Unary or Binocular Operators.

- execute
 
  `function`
  
  `execute` is important, it decides how operation should work. It's a function, accept the operation values. If it's binocular operators, it accept two inputs, if it's unary, accept one.

### var result = arithmetic( expresionSentence ).value

After you define your expression grammer, you can use it to calculate something.

`arithmetic` is the function you defined before by passing operationMap to expressioner.

`expresionSentence` is a string, like `a + b * (c - d)`

`result` is the value of expression. For example, in math the value of `3 + 4` is 7.

## License

MIT
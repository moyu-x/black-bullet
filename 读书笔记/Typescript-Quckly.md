## Chapter 1

In programming languages with static typing, a type must be assigned to a variablebefore you can use it.

在`tsconfig.json`中设置`wachth: true`可以监控文件变化自动编译

## Chapter 2

`--strictNullCheck`可以帮助检查未进行初始化的变量

If you need to declare a variable that can hold values of more than onetype, don’t use the type any; use a union such as letpadding:string|number.Another choice is to declare two separate variables: letpaddingStr: string;letpaddingNum:number;

## Chapter 3

The Typescript compiler converts classes into JavaScript constructor functions ifES5 is specified as the compilation target syntax. If ES6 or later is specified asthe target, TypeScript classes will be compiled into JavaScript classes.

Under the hood, JavaScript supports prototypal object-based inheri-tance, where one object can be assigned to another object as its prototype—this happens at runtime. Once TypeScript code that uses inheritance is com-piled, the generated JavaScript code uses the syntax of prototypal inheritance.

TypeScript is asuperset of JavaScript, which doesn’t support the private keyword, so thekeywords private and protected (as well as public) are removed duringcode compilation. The resulting JavaScript won’t include these keywords, soyou can consider them just a convenience during development.

## Chapter 3

We can say that an interface enforces a specific contract.
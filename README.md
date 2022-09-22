# answer for type-challenges

find the questions in [type-challenges][challenges].

* [x] [warm-up (1)](#warm-up (1))
* [x] [easy (13)](#easy (13))
* [ ] [medium (68)](#medium (68))
* [ ] [hard (39)](#hard (39))

## warm-up (1)

[13・Hello World][13]
```ts
type HelloWorld = string
```

## easy (13)

[4・Pick][4]
```ts
type MyPick<T, K extends keyof T> = {
  [P in K]: T[P]
}
```

[7・Readonly][7]
```ts
type MyReadonly<T> = {
  readonly [P in keyof T]: T[P]
}
```

[11・Tuple to Object][11]
```ts
type TupleToObject<T extends ReadonlyArray<string | number>> = {
  [P in T[number]]: P
}
```

[14・First of Array][14]
```ts
type First<T extends any[]> = T extends [] ? never : T[0]
```

[18・Length of Tuple][18]
```ts
type Length<T extends readonly any[]> = T['length']
```

[43・Exclude][43]
```ts
type MyExclude<T, U> = T extends U ? never : T
```

[189・Awaited][189]
```ts
type MyAwaited<T extends Promise<unknown>> = T extends Promise<infer U>
  ? U extends Promise<unknown>
    ? MyAwaited<U>
    : U
  : never

```
[268・If][268]
```ts
type If<C extends boolean, T, F> = C extends true ? T : F
```

[533・Concat][533]
```ts
type Concat<T extends any[], U extends any[]> = [...T, ...U]
```

[898・Includes][898]
```ts
type Includes<T extends readonly any[], U> = T extends [infer A, ...infer reset] 
  ? Equal<A, U> extends true
    ? true
    : Includes<reset, U>
  : false
```

[3057・Push][3057]
```ts
type Push<T extends any[], U> = [...T, U]
```

[3060・Unshift][3060]
```ts
type Unshift<T extends any[], U> = [U, ...T]
```

[3312・Parameters][3312]
```ts
type MyParameters<T extends (...args: any[]) => any> = T extends (...args: infer P) => any ? P : never
```

## medium (68)

## hard (39)


[challenges]: https://github.com/type-challenges/type-challenges
[4]: https://github.com/type-challenges/type-challenges/blob/main/questions/00004-easy-pick/README.md
[7]: https://github.com/type-challenges/type-challenges/blob/main/questions/00007-easy-readonly/README.md
[11]: https://github.com/type-challenges/type-challenges/blob/main/questions/00011-easy-tuple-to-object/README.md
[13]: https://github.com/type-challenges/type-challenges/blob/main/questions/00013-warm-hello-world/README.md
[14]: https://github.com/type-challenges/type-challenges/blob/main/questions/00014-easy-first/README.md
[18]: https://github.com/type-challenges/type-challenges/blob/main/questions/00018-easy-tuple-length/README.md
[43]: https://github.com/type-challenges/type-challenges/blob/main/questions/00043-easy-exclude/README.md
[189]: https://github.com/type-challenges/type-challenges/blob/main/questions/00189-easy-awaited/README.md
[268]: https://github.com/type-challenges/type-challenges/blob/main/questions/00268-easy-if/README.md
[533]: https://github.com/type-challenges/type-challenges/blob/main/questions/00533-easy-concat/README.md
[898]: https://github.com/type-challenges/type-challenges/blob/main/questions/00898-easy-includes/README.md
[3057]: https://github.com/type-challenges/type-challenges/blob/main/questions/03057-easy-push/README.md
[3060]: https://github.com/type-challenges/type-challenges/blob/main/questions/03060-easy-unshift/README.md
[3312]: https://github.com/type-challenges/type-challenges/blob/main/questions/03312-easy-parameters/README.md`
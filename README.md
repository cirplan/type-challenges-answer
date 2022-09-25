# answer for type-challenges

find the questions in [type-challenges][challenges].

* [x] [warm-up (1)](#warm-up-1)
* [x] [easy (13)](#easy-13)
* [ ] [medium (68)](#medium-68)
* [ ] [hard (39)](#hard-39)

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
[2・Get Return Type][2]
```ts
type MyReturnType<T> = T extends (...args: any[]) => infer R ? R : never
```

[3・Omit][3]
```ts
type MyOmit<T, K extends keyof T> = {
  [P in Exclude<keyof T, K>]: T[P]
}
// or use as
type MyOmit<T, K extends keyof T> = {
  [P in keyof T as P extends K ? never : P]: T[P]
}
```

[8・Readonly 2][8]
```ts
type Diff<A, B> = A extends B ? never : A;

type MyReadonly2<T, K extends keyof T = keyof T> = {
  readonly [S in K]: T[S]
} & {
  [S in Diff<keyof T, K>]: T[S]
}

```

[9・Deep Readonly][9]
```ts
type DeepReadonly<T> = {
  readonly [P in keyof T]: T[P] extends {}
    ? T[P] extends Function
      ? T[P]
      : DeepReadonly<T[P]>
    : T[P]
}
```

[10・Tuple to Union][10]
```ts
type TupleToUnion<T extends any[]> = T[number]
// or 
type TupleToUnion<T extends any[]> = T extends Array<infer R> ? R : never
```

[12・Chainable Options][12]
```ts
type Chainable<T = {}> = {
  option<K extends string, V>(
    key: K extends keyof T 
      ? Equal<V, T[K]> extends true
        ? never 
        : K 
      : K,
    value: V): Chainable<{ [S in keyof T as S extends K ? never : S]: T[S] } & { [S in K]: V }>
  get(): T
}
```

[15・Last of Array][15]
```ts
type Last<T extends any[]> = T extends [...infer S, infer R] ? R : never
```

[16・Pop][16]
```ts
type Pop<T extends any[]> = T extends [...infer S, infer R] ? S : never;
```

[20・Promise.all][20]
```ts
declare function PromiseAll<T extends any[]>(values: readonly [...T]): 
  Promise<{ [K in keyof T]: T[K] extends Promise<infer R> ? R : T[K] }>

```

[62・Type Lookup][62]
```ts
type LookUp<U, T extends string> =  U extends { type: T } ? U : never
```

[106・Trim Left][106]
```ts
type Space = ' ' | '\n' | '\t'
type TrimLeft<S extends string> = S extends `${Space}${infer R}` ? TrimLeft<R> : S;
```

[108・Trim][108]
```ts
type Space = ' ' | '\n' | '\t'
type Trim<S extends string> = S extends `${Space}${infer R}` 
  ? Trim<R> 
  : S extends `${infer R}${Space}`
    ? Trim<R> 
    : S
```

[110・Capitalize][110]
```ts
type MyCapitalize<S extends string> = S extends `${infer F}${infer R}` ? `${Uppercase<F>}${R}` : S
```

[116・Replace][116]
```ts
type Replace<S extends string, From extends string, To extends string> = S extends `${infer S}${From extends '' ? never : From}${infer E}` 
  ? `${S}${To}${E}` 
  : S;
```

[119・ReplaceAll][119]
```ts
type ReplaceAll<S extends string, From extends string, To extends string> = S extends `${infer S}${From extends '' ? never : From}${infer E}` 
  ? `${ReplaceAll<S, From, To>}${To}${ReplaceAll<E, From, To>}`
  : S;
```

[191・Append Argument][191]
```ts
type AppendArgument<Fn, A> = Fn extends (...args: infer R) => infer T ? (...args: [...R, A]) => T : never
```

[296・Permutation][296]
```ts
type Permutation<T, K = T> = 
  [T] extends [never] 
    ? []
    : K extends K
      ? [K, ...Permutation<Exclude<T, K>>]
      : never
```

[298・Length of String][298]
```ts
type LengthOfString<S extends string, T extends string[] = []> = S extends `${infer F}${infer R}` 
  ? LengthOfString<R, [...T, F]>
  : T["length"]
```

[459・Flatten][459]
```ts
type Flatten<T extends unknown[], R extends unknown[] = []> = T extends [infer F, ...infer E] 
  ? F extends unknown[]
    ? Flatten<E, [...R, ...Flatten<F>]>
    : Flatten<E, [...R, F]>
  : R
```

[527・Append to object][527]
```ts
type AppendToObject<T, U extends string, V> = {
  [K in (keyof T | U)]: K extends keyof T ? T[K] : V
}
```

[529・Absolute][529]
```ts
type Absolute<T extends number | string | bigint> = `${T}` extends `-${infer R}`
  ? R
  : `${T}`
```

[531・String to Union][531]
```ts
type StringToUnion<T extends string> = T extends '' 
  ? never 
  : T extends `${infer A}${infer R}`
    ? A | StringToUnion<R>
    : T
```

[599・Merge][599]
```ts
type Merge<F, S> = {
  [K in keyof F | keyof S]: K extends keyof S 
    ? S[K] 
    : K extends keyof F 
      ? F[K]
      : never
}
```

[612・KebabCase][]
```ts
```
[645・Diff][]
```ts
```
[949・AnyOf][]
```ts
```
[1042・IsNever][]
```ts
```
[1097・IsUnion][]
```ts
```
[1130・ReplaceKeys][]
```ts
```
[1367・Remove Index Signature][]
```ts
```
[1978・Percentage Parser][]
```ts
```
[2070・Drop Char][]
```ts
```
[2257・MinusOne][]
```ts
```
[2595・PickByType][]
```ts
```
[2688・StartsWith][]
```ts
```
[2693・EndsWith][]
```ts
```
[2757・PartialByKeys][]
```ts
```
[2759・RequiredByKeys][]
```ts
```
[2793・Mutable][]
```ts
```
[2852・OmitByType][]
```ts
```
[2946・ObjectEntries][]
```ts
```
[3062・Shift][]
```ts
```
[3188・Tuple to Nested Object][]
```ts
```
[3192・Reverse][]
```ts
```
[3196・Flip Arguments][]
```ts
```
[3243・FlattenDepth][]
```ts
```
[3326・BEM style string][]
```ts
```
[3376・InorderTraversal][]
```ts
```
[4179・Flip][]
```ts
```
[4182・Fibonacci Sequence][]
```ts
```
[4260・AllCombinations][]
```ts
```
[4425・Greater Than][]
```ts
```
[4471・Zip][]
```ts
```
[4484・IsTuple][]
```ts
```
[4499・Chunk][]
```ts
```
[4518・Fill][]
```ts
```
[4803・Trim Right][]
```ts
```
[5117・Without][]
```ts
```
[5140・Trunc][]
```ts
```
[5153・IndexOf][]
```ts
```
[5310・Join][]
```ts
```
[5317・LastIndexOf][]
```ts
```
[5360・Unique][]
```ts
```
[5821・MapTypes][]
```ts
```
[7544・Construct Tuple][]
```ts
```
[8640・Number Range][]
```ts
```
[8767・Combination][]
```ts
```
[8987・Subsequence][]
```ts
```


## hard (39)


[challenges]: https://github.com/type-challenges/type-challenges
[2]: https://github.com/type-challenges/type-challenges/blob/main/questions/00002-medium-return-type/README.md
[3]: https://github.com/type-challenges/type-challenges/blob/main/questions/00003-medium-omit/README.md
[4]: https://github.com/type-challenges/type-challenges/blob/main/questions/00004-easy-pick/README.md
[7]: https://github.com/type-challenges/type-challenges/blob/main/questions/00007-easy-readonly/README.md
[8]: https://github.com/type-challenges/type-challenges/blob/main/questions/00008-medium-readonly-2/README.md
[9]: https://github.com/type-challenges/type-challenges/blob/main/questions/00009-medium-deep-readonly/README.md
[10]: https://github.com/type-challenges/type-challenges/blob/main/questions/00010-medium-tuple-to-union/README.md
[11]: https://github.com/type-challenges/type-challenges/blob/main/questions/00011-easy-tuple-to-object/README.md
[12]: https://github.com/type-challenges/type-challenges/blob/main/questions/00012-medium-chainable-options/README.md
[13]: https://github.com/type-challenges/type-challenges/blob/main/questions/00013-warm-hello-world/README.md
[14]: https://github.com/type-challenges/type-challenges/blob/main/questions/00014-easy-first/README.md
[15]: https://github.com/type-challenges/type-challenges/blob/main/questions/00015-medium-last/README.md
[16]: https://github.com/type-challenges/type-challenges/blob/main/questions/00016-medium-pop/README.md
[18]: https://github.com/type-challenges/type-challenges/blob/main/questions/00018-easy-tuple-length/README.md
[20]: https://github.com/type-challenges/type-challenges/blob/main/questions/00020-medium-promise-all/README.md
[43]: https://github.com/type-challenges/type-challenges/blob/main/questions/00043-easy-exclude/README.md
[62]: https://github.com/type-challenges/type-challenges/blob/main/questions/00062-medium-type-lookup/README.md
[106]: https://github.com/type-challenges/type-challenges/blob/main/questions/00106-medium-trimleft/README.md
[108]: https://github.com/type-challenges/type-challenges/blob/main/questions/00108-medium-trim/README.md
[110]: https://github.com/type-challenges/type-challenges/blob/main/questions/00110-medium-capitalize/README.md
[116]: https://github.com/type-challenges/type-challenges/blob/main/questions/00116-medium-replace/README.md
[119]: https://github.com/type-challenges/type-challenges/blob/main/questions/00119-medium-replaceall/README.md
[189]: https://github.com/type-challenges/type-challenges/blob/main/questions/00189-easy-awaited/README.md
[191]: https://github.com/type-challenges/type-challenges/blob/main/questions/00191-medium-append-argument/README.md
[268]: https://github.com/type-challenges/type-challenges/blob/main/questions/00268-easy-if/README.md
[296]: https://github.com/type-challenges/type-challenges/blob/main/questions/00296-medium-permutation/README.md
[298]: https://github.com/type-challenges/type-challenges/blob/main/questions/00298-medium-length-of-string/README.md
[459]: https://github.com/type-challenges/type-challenges/blob/main/questions/00459-medium-flatten/README.md
[527]: https://github.com/type-challenges/type-challenges/blob/main/questions/00527-medium-append-to-object/README.md
[529]: https://github.com/type-challenges/type-challenges/blob/main/questions/00529-medium-absolute/README.md
[531]: https://github.com/type-challenges/type-challenges/blob/main/questions/00531-medium-string-to-union/README.md
[533]: https://github.com/type-challenges/type-challenges/blob/main/questions/00533-easy-concat/README.md
[599]: https://github.com/type-challenges/type-challenges/blob/main/questions/00599-medium-merge/README.md
[898]: https://github.com/type-challenges/type-challenges/blob/main/questions/00898-easy-includes/README.md
[3057]: https://github.com/type-challenges/type-challenges/blob/main/questions/03057-easy-push/README.md
[3060]: https://github.com/type-challenges/type-challenges/blob/main/questions/03060-easy-unshift/README.md
[3312]: https://github.com/type-challenges/type-challenges/blob/main/questions/03312-easy-parameters/README.md`

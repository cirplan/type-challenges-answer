# answer for type-challenges

find the questions in [type-challenges][challenges].

* [x] [warm-up (1)](#warm-up-1)
* [x] [easy (13)](#easy-13)
* [x] [medium (68)](#medium-68)
* [ ] [hard (39)](#hard-39)
* [ ] [extreme (14)](#extreme-14)

## warm-up (1)

[13・Hello World][13]
```ts
type HelloWorld = string
```

## easy (13)

[4・Pick][4]

思路：`keyof` 获取对象的 `keys`，`in` 进行遍历

```ts
type MyPick<T, K extends keyof T> = {
  [P in K]: T[P]
}
```

[7・Readonly][7]

思路：`readonly` 关键字的使用

```ts
type MyReadonly<T> = {
  readonly [P in keyof T]: T[P]
}
```

[11・Tuple to Object][11]

思路：`number` 作为元组的下标，`ReadonlyArray` 的使用

```ts
type TupleToObject<T extends ReadonlyArray<string | number>> = {
  [P in T[number]]: P
}
```

[14・First of Array][14]

思路：`[]` 空数组

```ts
type First<T extends any[]> = T extends [] ? never : T[0]
```

[18・Length of Tuple][18]

思路：`length` 属性

```ts
type Length<T extends readonly any[]> = T['length']
```

[43・Exclude][43]

思路：联合会自动展开

```ts
type MyExclude<T, U> = T extends U ? never : T
```

[189・Awaited][189]

思路：`infer` 和 `PromiseLike` 的使用，注意判断递归

```ts
type MyAwaited<T extends PromiseLike<any>> = T extends PromiseLike<infer U> 
  ? U extends PromiseLike<unknown> 
    ? MyAwaited<U>
    : U
  : never
```
[268・If][268]

思路：`boolean` 的使用

```ts
type If<C extends boolean, T, F> = C extends true ? T : F
```

[533・Concat][533]

思路：`...` 的使用

```ts
type Concat<T extends any[], U extends any[]> = [...T, ...U]
```

[898・Includes][898]

思路：判断两个变量是否相等得用 `Equal`，其实现是：

```ts
type Equal<X, Y> = (<T>() => T extends X 
  ? 1 
  : 2) extends <T>() => T extends Y 
    ? 1 
    : 2 
      ? true 
      : false
```

```ts
type Includes<T extends readonly any[], U> = T extends [infer A, ...infer reset] 
  ? Equal<A, U> extends true
    ? true
    : Includes<reset, U>
  : false
```

[3057・Push][3057]

思路：`...` 的使用

```ts
type Push<T extends any[], U> = [...T, U]
```

[3060・Unshift][3060]

思路：`...` 的使用

```ts
type Unshift<T extends any[], U> = [U, ...T]
```

[3312・Parameters][3312]

思路：`infer` 的使用

```ts
type MyParameters<T extends (...args: any[]) => any> = T extends (...args: infer P) => any ? P : never
```

## medium (68)
[2・Get Return Type][2]

思路：`infer` 的使用，同时注意函数参数的有无

```ts
type MyReturnType<T> = T extends (...args: any[]) => infer R ? R : never
```

[3・Omit][3]

思路：`as` 的使用。或者 `Exclude`。

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

思路：`&` 的使用，还要注意默认值

```ts
type Diff<A, B> = A extends B ? never : A;

type MyReadonly2<T, K extends keyof T = keyof T> = {
  readonly [S in K]: T[S]
} & {
  [S in Diff<keyof T, K>]: T[S]
}

// or
type MyReadonly2<T, K extends keyof T = keyof T> = {
  readonly [P in K] : T[P]
} & {
  [P in Exclude<keyof T, K>]: T[P]
}
```

[9・Deep Readonly][9]

思路：`object` 表示非基础类型；基础类型是可以赋值给 `Object` 和 `{}`，但在编译时，`{}` 是不包含 `Object` 属性的，自然也不会进行校验。`Object` 则会进行对应校验，相比更加严格。具体可以看 [Object vs object vs {}](https://cirplan.me/post/2022/10-11-ts-types-challenges/#object-vs-object-vs-)

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

思路：`T[number]`

```ts
type TupleToUnion<T extends any[]> = T[number]
// or 
type TupleToUnion<T extends any[]> = T extends Array<infer R> ? R : never
```

[12・Chainable Options][12]

思路：设置默认对象，注意 key 不能重复

```ts
type Chainable<T = {}> = {
  option<K extends string, V>(key: K extends keyof T ? never : K, value: V): Chainable<{ [S in keyof T as S extends K ? never : S]: T[S] } & { [S in K]: V }>
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

[612・KebabCase][612]
```ts
type KebabCase<S extends string> = S extends `${infer R}${infer P}`
  ? P extends Uncapitalize<P>
    ? `${Lowercase<R>}${KebabCase<P>}`
    : `${Lowercase<R>}-${KebabCase<P>}`
  : S  
```

[645・Diff][645]
```ts
type Diff<O, O1> = Omit<O & O1, keyof O & keyof O1>
```

[949・AnyOf][949]
```ts
type Empty = '' | 0 | false | { [key: string]: never } | [] ;

type AnyOf<T extends readonly any[]> = T extends [infer A, ...infer R]
  ? A extends Empty 
    ? AnyOf<R>
    : true
  : false
```

[1042・IsNever][1042]
```ts
type IsNever<T> = [T] extends [never] ? true : false
```

[1097・IsUnion][1097]
```ts
type IsUnion<T, C = T> = [T] extends [never] 
  ? false
  : T extends T 
    ? [C] extends [T]
      ? false
      : true
    : false
```

[1130・ReplaceKeys][1130]
```ts
type ReplaceKeys<U, T, Y> = {
  [K in keyof U]: K extends T 
    ? K extends keyof Y
      ? Y[K]
      : never
    : U[K]
}
```

[1367・Remove Index Signature][1367]
```ts
type RemoveIndexSignature<T> = {
  [K in keyof T as string extends K 
    ? never 
    : number extends K
      ? never
      : symbol extends K
        ? never
        : K
  ]: T[K]
}
```

[1978・Percentage Parser][1978]
```ts
type PercentageParser<A extends string> = A extends `${infer S extends '-' | '+'}${infer N}` 
  ? N extends `${infer N}%`
    ? [S, N, '%']
    : [S, N, '']
  : A extends `${infer N}%`
    ? ['', N, '%']
    : ['', A, '']
```

[2070・Drop Char][2070]
```ts
type DropChar<S, C> = S extends `${infer A}${infer R}`
  ? Equal<A, C> extends true
    ? DropChar<R, C>
    : `${A}${DropChar<R, C>}`
  : S
```

[2257・MinusOne][2257]
```ts
type Dict = {
  '0': [];
  '1': [0];
  '2': [0, 0];
  '3': [0, 0, 0];
  '4': [0, 0, 0, 0];
  '5': [0, 0, 0, 0, 0];
  '6': [0, 0, 0, 0, 0, 0];
  '7': [0, 0, 0, 0, 0, 0, 0];
  '8': [0, 0, 0, 0, 0, 0, 0, 0];
  '9': [0, 0, 0, 0, 0, 0, 0, 0, 0];
}

type TenTimes<A extends 0[]> = [
  ...A, ...A, ...A, ...A, ...A,
  ...A, ...A, ...A, ...A, ...A
]

type ToArray<
  N extends string,
  Result extends 0[] = []
> = N extends `${infer F extends keyof Dict}${infer R}`
  ? ToArray<R, [...TenTimes<Result>, ...Dict[F]]>
  : Result

type MinusOne<
  T extends number
> = ToArray<`${T}`> extends [infer F, ...infer R]
      ? R['length']
      : never
```

[2595・PickByType][2595]
```ts
type PickByType<T, U> = {
  [K in keyof T as T[K] extends U ? K : never]: T[K]
}
```

[2688・StartsWith][2688]
```ts
type StartsWith<T extends string, U extends string> = T extends `${U}${infer R}`
  ? true
  : false
```

[2693・EndsWith][2693]
```ts
type EndsWith<T extends string, U extends string> = T extends `${infer R}${U}`
  ? true
  : false
```

[2757・PartialByKeys][2757]
```ts
type PartialByKeys<T, K = keyof T> = 
  { [P in keyof T as P extends K ? never : P]: T[P] } &
  { [P in keyof T as P extends K ? P : never]?: T[P] } extends infer O ? (
    { [P in keyof O]: O[P] }
  ) : never
```

[2759・RequiredByKeys][2759]
```ts
type RequiredByKeys<T, K = keyof T> = { [P in keyof T as P extends K ? never : P]: T[P] } &
  { [P in keyof T as P extends K ? P : never]-?: T[P] } extends infer O ? (
    { [P in keyof O]: O[P] }
  ) : never
```
[2793・Mutable][2793]
```ts
type Mutable<T extends object> = {
  -readonly [K in keyof T]: T[K]
}
```

[2852・OmitByType][2852]
```ts
type OmitByType<T, U> = {
  [K in keyof T as T[K] extends U ? never : K]: T[K]
}
```

[2946・ObjectEntries][2946]
```ts
type ObjectEntries<T> = {
  [K in keyof T]-?: [K, T[K] extends undefined ? undefined : Exclude<T[K], undefined>]
}[keyof T]
```

[3062・Shift][3062]
```ts
type Shift<T extends any[]> = T extends [infer A, ...infer R] 
  ? R
  : T
```

[3188・Tuple to Nested Object][3188]
```ts
type TupleToNestedObject<T extends any[], U, R = U> = T extends [...infer A, infer L] 
  ? TupleToNestedObject<A, U, {[K in L & string]: R}>
  : R
```

[3192・Reverse][3192]
```ts
type Reverse<T> = T extends [...infer F, infer L]
  ? [L, ...Reverse<F>]
  : []
```

[3196・Flip Arguments][3196]
```ts
type Reverse<T> = T extends [...infer F, infer L]
  ? [L, ...Reverse<F>]
  : []

type FlipArguments<T extends Function> = T extends (...arg: infer A) => infer R 
  ? (...arg: Reverse<A>) => R
  : never
```

[3243・FlattenDepth][3242]
```ts
type FlattenDepth<T extends any[], C extends number = 1, U extends any[] = []> = U['length'] extends C
  ? T
  : T extends [infer A, ...infer R]
    ? A extends any[]
      ? [...FlattenDepth<A, C, [0, ...U]>, ...FlattenDepth<R, C, U>]
      : [A, ...FlattenDepth<R, C, U>]
    : T
```

[3326・BEM style string][3326]
```ts
type BEM<B extends string, E extends string[], M extends string[]> = `${B}${E extends [] ? '' : `__${E[number]}`}${M extends [] ? '' : `--${M[number]}`}`
```

[3376・InorderTraversal][3376]
```ts
type InorderTraversal<T extends TreeNode | null> = [T] extends [TreeNode]
  ? [
    ...InorderTraversal<T['left']>,
    T['val'],
    ...InorderTraversal<T['right']>
  ]
  : []
```

[4179・Flip][4179]
```ts
type Flip<T extends Record<string, string | number | boolean>> = {
  [K in keyof T as `${T[K]}`]: K
}
```

[4182・Fibonacci Sequence][4182]
```ts
type Fibonacci<T extends number, C extends any[] = [1], P extends any[] = [], V extends any[] = [1]> = C['length'] extends T
  ? V['length']
  : Fibonacci<T, [...C, 1], V, [...P, ...V]>
```

[4260・AllCombinations][4260]
```ts
type StringToUnion<S> = S extends `${infer A}${infer R}` ? A | StringToUnion<R> : S;
type AllCombinations<S extends string, T extends string = StringToUnion<S>, U extends string = T> = S extends `${infer A}${infer R}`
  ? U extends U
    ? `${U}${AllCombinations<R, U extends '' ? T : Exclude<T, U>>}`
    : never
  : ''
```

[4425・Greater Than][4425]
```ts
type Length<T extends any[]> = T['length'];
type GreaterThan<T extends number, U extends number, N extends any[] = []> = Length<N> extends T
  ? false
  : Length<N> extends U
    ? true
    : GreaterThan<T, U, [...N, 1]>
```

[4471・Zip][4471]
```ts
type Zip<T extends any[], P extends any[]> = [T, P] extends [[infer TA, ...infer TR], [infer PA, ...infer PR]]
  ? [[TA, PA], ...Zip<TR, PR>]
  : []
```

[4484・IsTuple][4484]
```ts
type IsTuple<T> = [T] extends [never]
  ? false
  : T extends readonly [any?]
    ? true
    : false
```

[4499・Chunk][4499]
```ts
type Chunk<T extends any[], N, C extends any[] = []> = C['length'] extends N
  ? [C, ...Chunk<T, N>]
  : T extends [infer A, ...infer B]
    ? Chunk<B, N, [...C, A]>
    : C extends []
      ? C
      : [C]
```

[4518・Fill][4518]
```ts
type Fill<
  T extends unknown[],
  N,
  Start extends number = 0,
  End extends number = T['length'],
  U extends any[] = [],
  Flag extends boolean = false
> = Start extends End
    ? T
    : T extends [infer A, ...infer L]
      ? U['length'] extends Start
        ? Fill<L, N, Start, End, [...U, N], true>
        : U['length'] extends End
          ? Fill<L, N, Start, End, [...U, A], false>
          : Flag extends true
            ? Fill<L, N, Start, End, [...U, N], true>
            : Fill<L, N, Start, End, [...U, A], false>
      : U
```

[4803・Trim Right][4803]
```ts
type Empty = ' ' | '\n' | '\t';
type TrimRight<S extends string> = S extends `${infer A}${Empty}` ? TrimRight<A> : S
```

[5117・Without][5117]
```ts
type ToUnion<T> = T extends any[] ? T[number] : T;
type Without<T extends any[], U extends number | any[]> = T extends [infer A, ...infer R]
  ? A extends ToUnion<U>
    ? Without<R, U>
    : [A, ... Without<R, U>]
  : []
```

[5140・Trunc][5140]
```ts
type Trunc<T extends number | string> = `${T}` extends `${infer A}.${infer R}`
  ? A
  : `${T}`
```

[5153・IndexOf][5153]
```ts
type IndexOf<T extends any[], U, R extends any[] = []> = T extends [infer A, ...infer B]
  ? Equal<A, U> extends true
    ? R['length']
    : IndexOf<B, U, [...R, 0]>
  : -1
```

[5310・Join][5310]
```ts
type Join<T extends any[], U extends string> = T extends [infer A extends string, ...infer R]
  ? R extends []
    ? `${A}`
    : `${A}${U}${Join<R, U>}`
  : ''
```

[5317・LastIndexOf][5317]
```ts
type LastIndexOf<T extends any[], U> = T extends [...infer A, infer L]
  ? Equal<U, L> extends true
    ? A['length']
    : LastIndexOf<A, U>
  : -1
```

[5360・Unique][5360]
```ts
type SelfEqual<U, T extends any[]> = T extends [infer A, ...infer B]
  ? Equal<U, A> extends true
    ? true
    : SelfEqual<U, B>
  : false
type Unique<T extends any[], U extends any[] = []> = T extends [infer A, ...infer B]
  ? SelfEqual<A, U> extends true
    ? Unique<B, U>
    : Unique<B, [...U, A]>
  : U
```

[5821・MapTypes][5821]
```ts
type MapTypes<T, R extends {
  mapFrom: any,
  mapTo: any
}> = {
  [K in keyof T]: T[K] extends R['mapFrom'] 
      ? R extends { mapFrom: T[K], mapTo: any }
        ? R['mapTo']
        : never
    : T[K]
}
```

[7544・Construct Tuple][7544]
```ts
type ConstructTuple<L extends number, U extends any[] = []> = U['length'] extends L
  ? U
  : ConstructTuple<L, [...U, unknown]>
```

[8640・Number Range][8640]
```ts
type AddOne<T, U extends any[] = []> = U['length'] extends T
  ? [...U, 1]['length']
  : AddOne<T, [...U, 1]>
type NumberRange<L extends number, H extends number, U extends any[] = []> = L extends H
  ? [...U, L][number]
  : NumberRange<AddOne<L>, H, [...U, L]>
```

[8767・Combination][8767]
```ts
type Combination<T extends string[], U = T[number], D = U> = D extends string
  ? D | `${D} ${Combination<[], Exclude<U, D>>}`
  : never
```

[8987・Subsequence][8978]
```ts
type Subsequence<T extends any[]> = T extends [infer A, ...infer L]
  ? [...([A] | []), ...Subsequence<L>]
  : []
```


## hard (39)

[6・Simple Vue][6]
```ts
type Options<Data, Computed, Methods> = {
  data?: () => Data;
  computed?: Computed & ThisType<Data & {
    [K in keyof Computed]: Computed[K] extends (...args: any) => infer R ? R : never
  }>;
  methods?: Methods & ThisType<Data & Methods & {
    [K in keyof Computed]: Computed[K] extends (...args: any) => infer R ? R : never;
  }>
}

declare function SimpleVue<Data, Computed, Methods>(options: Options<Data, Computed, Methods>): any
```

[17・Currying 1][17]
```ts
type CurryingRet<Fn> = Fn extends (first: infer H, ...args: infer Args) => infer Ret 
  ? Args extends []
    ? (h : H) => Ret
    : (h: H) => CurryingRet<(...args: Args) => Ret>
  : never
  
declare function Currying<Fn>(fn: Fn): Fn extends () => infer Ret ? () => Ret :CurryingRet<Fn>    ? (h : H) => Ret
```


[55・Union to Intersection][55]

[思路](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-8.html#type-inference-in-conditional-types)
```ts
type UnionToIntersection<U> = (U extends any ? (arg: U) => any : never) extends ((arg: infer I) => void) ? I : never
```

<!-- 
[57・Get Required][]
```ts

```

[59・Get Optional][]
```ts

```

[89・Required Keys][]
```ts

```

[90・Optional Keys][]
```ts

```

[112・Capitalize Words][]
```ts

```

[114・CamelCase][]
```ts

```

[147・C-printf Parser][]
```ts

```

[213・Vue Basic Props][]
```ts

```

[223・IsAny][]
```ts

```

[270・Typed Get][]
```ts

```

[300・String to Number][]
```ts

```

[399・Tuple Filter][]
```ts

```

[472・Tuple to Enum Object][]
```ts

```

[545・printf][]
```ts

```

[553・Deep object to unique][]
```ts

```

[651・Length of String 2][]
```ts

```

[730・Union to Tuple][]
```ts

```

[847・String Join][]
```ts

```

[956・DeepPick][]
```ts

```

[1290・Pinia][]
```ts

```

[1383・Camelize][]
```ts

```

[2059・Drop String][]
```ts

```

[2822・Split][]
```ts

```

[2828・ClassPublicKeys][]
```ts

```

[2857・IsRequiredKey][]
```ts

```

[2949・ObjectFromEntries][]
```ts

```

[4037・IsPalindrome][]
```ts

```

[5181・Mutable Keys][]
```ts

```

[5423・Intersection][]
```ts

```

[6141・Binary to Decimal][]
```ts

```

[7258・Object Key Paths][]
```ts

```

[8804・Two Sum][]
```ts

```

[9155・ValidDate][]
```ts

```

[9160・Assign][]
```ts

```

[9775・Capitalize Nest Object Keys][]
```ts

```

[14188・Run-length encoding][]
```ts

``` -->


## extreme (14)


[challenges]: https://github.com/type-challenges/type-challenges
[2]: https://github.com/type-challenges/type-challenges/blob/main/questions/00002-medium-return-type/README.md
[3]: https://github.com/type-challenges/type-challenges/blob/main/questions/00003-medium-omit/README.md
[4]: https://github.com/type-challenges/type-challenges/blob/main/questions/00004-easy-pick/README.md
[6]: https://github.com/type-challenges/type-challenges/blob/main/questions/00006-hard-simple-vue/README.md
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
[17]: https://github.com/type-challenges/type-challenges/blob/main/questions/00017-hard-currying-1/README.md
[18]: https://github.com/type-challenges/type-challenges/blob/main/questions/00018-easy-tuple-length/README.md
[20]: https://github.com/type-challenges/type-challenges/blob/main/questions/00020-medium-promise-all/README.md
[43]: https://github.com/type-challenges/type-challenges/blob/main/questions/00043-easy-exclude/README.md
[55]: https://github.com/type-challenges/type-challenges/blob/main/questions/00055-hard-union-to-intersection/README.md
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
[612]: https://github.com/type-challenges/type-challenges/blob/main/questions/00612-medium-kebabcase/README.md
[645]: https://github.com/type-challenges/type-challenges/blob/main/questions/00645-medium-diff/README.md
[898]: https://github.com/type-challenges/type-challenges/blob/main/questions/00898-easy-includes/README.md
[949]: https://github.com/type-challenges/type-challenges/blob/main/questions/00949-medium-anyof/README.md
[1042]: https://github.com/type-challenges/type-challenges/blob/main/questions/01042-medium-isnever/README.md
[1097]: https://github.com/type-challenges/type-challenges/blob/main/questions/01097-medium-isunion/README.md
[1130]: https://github.com/type-challenges/type-challenges/blob/main/questions/01130-medium-replacekeys/README.md
[1367]: https://github.com/type-challenges/type-challenges/blob/main/questions/01367-medium-remove-index-signature/README.md
[1978]: https://github.com/type-challenges/type-challenges/blob/main/questions/01978-medium-percentage-parser/README.md
[2070]: https://github.com/type-challenges/type-challenges/blob/main/questions/02070-medium-drop-char/README.md
[2257]: https://github.com/type-challenges/type-challenges/blob/main/questions/02257-medium-minusone/README.md
[2595]: https://github.com/type-challenges/type-challenges/blob/main/questions/02595-medium-pickbytype/README.md
[2688]: https://github.com/type-challenges/type-challenges/blob/main/questions/02688-medium-startswith/README.md
[2693]: https://github.com/type-challenges/type-challenges/blob/main/questions/02693-medium-endswith/README.md
[2757]: https://github.com/type-challenges/type-challenges/blob/main/questions/02757-medium-partialbykeys/README.md
[2759]: https://github.com/type-challenges/type-challenges/blob/main/questions/02759-medium-requiredbykeys/README.md
[2793]: https://github.com/type-challenges/type-challenges/blob/main/questions/02793-medium-mutable/README.md
[2852]: https://github.com/type-challenges/type-challenges/blob/main/questions/02852-medium-omitbytype/README.md
[2946]: https://github.com/type-challenges/type-challenges/blob/main/questions/02946-medium-objectentries/README.md
[3057]: https://github.com/type-challenges/type-challenges/blob/main/questions/03057-easy-push/README.md
[3060]: https://github.com/type-challenges/type-challenges/blob/main/questions/03060-easy-unshift/README.md
[3062]: https://github.com/type-challenges/type-challenges/blob/main/questions/03062-medium-shift/README.md
[3188]: https://github.com/type-challenges/type-challenges/blob/main/questions/03188-medium-tuple-to-nested-object/README.md
[3192]: https://github.com/type-challenges/type-challenges/blob/main/questions/03192-medium-reverse/README.md
[3196]: https://github.com/type-challenges/type-challenges/blob/main/questions/03196-medium-flip-arguments/README.md
[3243]: https://github.com/type-challenges/type-challenges/blob/main/questions/03243-medium-flattendepth/README.md
[3312]: https://github.com/type-challenges/type-challenges/blob/main/questions/03312-easy-parameters/README.md`
[3326]: https://github.com/type-challenges/type-challenges/blob/main/questions/03326-medium-bem-style-string/README.md
[3376]: https://github.com/type-challenges/type-challenges/blob/main/questions/03376-medium-inordertraversal/README.md
[4179]: https://github.com/type-challenges/type-challenges/blob/main/questions/04179-medium-flip/README.md
[4182]: https://github.com/type-challenges/type-challenges/blob/main/questions/04182-medium-fibonacci-sequence/README.md
[4260]: https://github.com/type-challenges/type-challenges/blob/main/questions/04260-medium-nomiwase/README.md
[4425]: https://github.com/type-challenges/type-challenges/blob/main/questions/04425-medium-greater-than/README.md
[4471]: https://github.com/type-challenges/type-challenges/blob/main/questions/04471-medium-zip/README.md
[4484]: https://github.com/type-challenges/type-challenges/blob/main/questions/04484-medium-istuple/README.md
[4499]: https://github.com/type-challenges/type-challenges/blob/main/questions/04499-medium-chunk/README.md
[4518]: https://github.com/type-challenges/type-challenges/blob/main/questions/04518-medium-fill/README.md
[4803]: https://github.com/type-challenges/type-challenges/blob/main/questions/04803-medium-trim-right/README.md
[5117]: https://github.com/type-challenges/type-challenges/blob/main/questions/05117-medium-without/README.md
[5140]: https://github.com/type-challenges/type-challenges/blob/main/questions/05140-medium-trunc/README.md
[5153]: https://github.com/type-challenges/type-challenges/blob/main/questions/05153-medium-indexof/README.md
[5310]: https://github.com/type-challenges/type-challenges/blob/main/questions/05310-medium-join/README.md
[5317]: https://github.com/type-challenges/type-challenges/blob/main/questions/05317-medium-lastindexof/README.md
[5360]: https://github.com/type-challenges/type-challenges/blob/main/questions/05360-medium-unique/README.md
[5821]: https://github.com/type-challenges/type-challenges/blob/main/questions/05821-medium-maptypes/README.md
[7544]: https://github.com/type-challenges/type-challenges/blob/main/questions/07544-medium-construct-tuple/README.md
[8640]: https://github.com/type-challenges/type-challenges/blob/main/questions/08640-medium-number-range/README.md
[8767]: https://github.com/type-challenges/type-challenges/blob/main/questions/08767-medium-combination/README.md
[8978]: https://github.com/type-challenges/type-challenges/blob/main/questions/08987-medium-subsequence/README.md

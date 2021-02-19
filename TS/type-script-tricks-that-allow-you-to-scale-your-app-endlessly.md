[TypeScript tricks that allow you to scale your app endlessly](https://medium.com/its-tinkoff/typescript-tricks-that-allow-you-to-scale-your-app-endlessly-95a0ff3d160d)

### any
There is simply no case where you cannot describe a type instead of using “any”. 
If you have such a situation, it can be a problem in architecture, legacy or something else.
Use generics, unknown or overloads and do not worry about unexpected problems with data structures. 
Such problems are hard and expensive to debug.
### strict
If you start a new project, enable “strict” and be happy.
> TypeScript has a “strict” mode that is unfortunately disabled by default.

### [readonly](https://basarat.gitbook.io/typescript/type-system/readonly)
The next important rule for us as developers in TypeScript is using readonly everywhere.
It is a bad practice to mutate structures while working with them. For example, Angular does not like this way: there are some issues with ChangeDetection and updates of your view when you mutate data.
But you can prevent all attempts to mutate data easily. Just make a habit of writing readonly word.
```typescript
export interface Thing { // before
    data: string;
}
export interface Thing { // after
    readonly data: string;
}
const safeArrayOtherWay: readonly number[] = [1, 2, 3];
const verySafeTuple: readonly [number, number, number] = [1, 2, 3]; // awesome
const safeMap: ReadonlyMap<string, number> = new Map<string, number>();
const safeSet: ReadonlySet<number> = new Set<number>();
```

### as const
It is a more strict instrument than “readonly” types because it seals instance to the most precise type and you can be sure that nobody and nothing will change it.
Using as const you also get exact types in the tips of your IDE.

### Narrow down your types
```typescript
interface Data {
  readonly greeting: string;
}
/**
 * Here is well typed sample
 *
 * An arrow function answers a question "is value a Data type"
 * It narrows down a type and "wellTypedSource$" has the right type
 */
const wellTypedSource$ = data$$.pipe(
    filter((value): value is Data => !!value)
)

```
You can narrow down types in several ways:
1. typeof operator from JavaScript to check primitive types
2. instanceof operator from JavaScript to check inherited entities
3. is T declaration from TypeScript allows checking complex types and interfaces. 
But you should be very careful using this feature because you make the correct type check your responsibility, not TypeScript's.
```typescript
// instanceof narrowing
abstract class AbstractButton {
    click(): void { }
}
class Button extends AbstractButton {
    click(): void { }
}
class IconButton extends AbstractButton {
    icon = 'src/icon';
    click(): void { }
}
function getButtonIcon(button: AbstractButton): string | null {
    /**
     * after "instanceof" TS knows that a variable has "icon"
     * field
     */
    return button instanceof IconButton ? button.icon : null;
}
```

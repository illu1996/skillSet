### Life Cycle 순서

[생성자]
1. constructor 
[Hooks] -> '인터페이스의 형태로 제공'
2. `ngOnChange`
3. `ngOnInit`
4. `ngDoCheck`  
    5-1. `ngAfterContentinit`  
    5-2. `ngAfterContentChecked`  
    5-3. `ngAfterViewInit`  
    5-4. `ngAfterViewChecked`  
6. `ngOnDestroy`

#### 1. constructor 
생성자로, 컴포넌트가 JS로 컴파일 될때 class로 변경되므로 constructor에 의해 호출되며 생성됩니다.


##### `[Hooks에 들어가기에 앞서]`  
생명주기 Hooks는 인터페이스 형태로 제공된다.
```Js
interface OnInit {
    ngOnInit():void
}
```
위와 같은 코드의 인터페이스로 제공되기 때문에 추상 메서드(Hook들)을 구현하여 사용해야 한다.
예를들어 OnInit Hook을 사용하고 싶다면 OnInit을 상속받아 ngOnInit 추상메서드를 구현해서 사용한다.

모든 생명주기 훅을 구현할 필요없이 필요한 생명주기 훅만 구현해도 좋습니다.

```Js
export class AppComponent implements OnInit {
	name = 'Lee';

	constructor() { }

	// 생명주기 OnInit 단계에 실행할 처리를 구현한다.
	ngOnInit() {...}
}
```
#### 2. ngOnChanges
부모에서 자식으로 건네주는 props `@Input()`으로 바인딩한 데이터 값이 초기화 되거나 변경되었을때 실행됩니다.
그러므로 건네주는 props가 있을 경우 호출할 필요가 없습니다.
```JS
class MyComponent implements OnChanges {
    @Input() props1:number;
    @Input() props2:string;

    ngOnChanges(changes: SimpleChanges) {
        //changes가 객체
        console.log(changes.currentValue) // currentValue 가 바뀐 값
        console.log(changes.previousValue) // preVal 이 이전 값 
    }
}
```
props를 건네주는 `@Input()` 이 존재할 경우 `OnInit` 전에 최소 한번 호출되며, 변경 시 반복호출이 된다.  
즉, 기본 자료형의 값이 재할당 되거나 객체 참조 변경되면 실행됩니다.

`ngOnchange` 는 `Ref` 변경시에만 반응하는 구조이다.

#### 3. ngOnInit
`ngOnChange` 이후, 모든 프로퍼티의 초기화(변수 선언)이 완료된 시점에 한번만 실행된다.
`TypeScript` 를 통해 `constructor`에서 프로퍼티를 초기화 하는 것이 일반적이긴 하나, DB를 사용하거나 복잡한 작업을 하는 경우 `ngOnInit`을 이용하는 것이 좋습니다.
`JS` 컴파일 과정에서 `undefined`로 초기화 되는 경우가 있기 때문입니다.

#### 4. ngDoCheck
3번의 `ngOnInit` 이후, 컴포넌트 or 디렉티브의 모든 상태 변화가 발생하면 호출됩니다.  
Angular 자체에서 변화감지를 못하거나 감지할 수 없는 변경사항을 추적하기 위해 좋습니다.  
이를 위하여 `KeyValueDiffers`와 `IterableDiffers`를 사용합니다.  

단, `ngDoCheck` 는 모든 상태변화시 매번 호출되기에 성능에 악영향을 미치기에 조심해야 합니다.

#### 5-1. ngAfterContentInit
`ngContent` directive를 사용하여 컴포넌트 뷰에 외부 콘텐츠를 **프로젝션 한 이후에 호출됩니다.

```
** 콘텐츠 프로젝션 : 부모 컴포넌트가 자식컴포넌트에게 일부 템플릿을 전달하는 기능(@ContentChild, @ContentChildren)
```
첫번째 ngDoCheck 호출 한 후 한번만 실행되며, `Component 전용 Hook 메서드`입니다.

#### 5-2. ngAfterContentChecked
앞서 말한 콘텐츠 프로젝션에 의해 부모 컴포넌트가 전달한 부모 컴포넌트의 템플릿 조각을 Check 한 후 호출됩니다. 5-1과 마찬가지로 `Component 전용 Hook 메서드`입니다.

#### 5-3. ngAfterViewInit
컴포넌트의 뷰와 자식 컴포넌트 뷰를 초기화 한 이후 1번만 호출되며, `ngAfterContentChecked` 이후 호출됩니다. `Component 전용 Hook 메서드`입니다.

#### 5-3. ngAfterViewChecked
컴포넌트의 뷰와 자식 컴포넌트 뷰를 Check 한 이후 1번만 호출되며, `ngAfterViewInit` 이후 호출됩니다. `Component 전용 Hook 메서드`입니다.

#### 6. ngOnDestroy
`Component`와 `Directive`가 소멸 전 호출되며, 메모리 누수를 방지하기 위한 코드를 정의하는 것이 좋습니다.




#### Life Cycle 순서

[생성자]
1. constructor 
[Hooks] -> '인터페이스의 형태로 제공'
2. ngOnChange
3. ngOnInit
4. ngDoCheck
    5-1. ngAfterContentinit
    5-2. ngAfterContentChecked
    5-3.ngAfterViewInit
    5-4. ngAfterViewChecked
6. ngOnDestroy

###### 1.constructor 
생성자로, 컴포넌트가 형성될 때 실행되며 변수의 초기화 등이 가능하다.

###### [Hooks에 들어가기에 앞서]
생명주기 Hooks는 인터페이스 형태로 제공된다.
'''
interface OnInit {
    ngOnInit():void
}
'''
위와 같은 코드의 인터페이스로 제공되기 때문에 추상 메서드(Hook들)을 구현하여 사용해야 한다.
예를들어 OnInit Hook을 사용하고 싶다면 OnInit을 상속받아 ngOnInit 추상메서드를 구현해서 사용한다.

'''
export class AppComponent implements OnInit {
	name = 'Lee';

	constructor() { }

	// 생명주기 OnInit 단계에 실행할 처리를 구현한다.
	ngOnInit() {...}
}
'''
###### 2.ngOnChanges


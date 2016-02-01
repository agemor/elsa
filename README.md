# 오르카 프로그래밍 언어


오르카는 표현성과 간결성을 염두에 두고 디자인된 프로그래밍 언어이다. 이 언어를 구상하고, 첫 번째 구현은 2014년 초에 Haxe로 이루어졌다. 오르카를 설계하면서 문법 요소는  *C#*과 *JScript, C++, Python*의 영향을 받았다. 이 언어들의 문법 중 참고할 만한 부분은 나름대로 재해석하고, 새로운 표현 방법을 구상하기도 하면서 쉽게 프로그램을 작성할 수 있는 언어를 제작하는데 노력을 많이 기울였다.


## 빌드하기

[Haxe](http://www.haxe.org) 를 사용하여 `build-compiler.hxml`과 `build-vm.hxml`로 오르카 컴파일러와 가상 머신의 *C++*버전 실행 파일을 빌드할 수 있다. 

이미 빌드된 버전은 [여기](http://hyunjun.org/orca/release.zip)에서 다운로드 받을 수 있다.

## 실행하기

`.orca` 확장자를 가진 파일에 오르카 코드를 적고 `orcac.exe` 파일로 컴파일한다. 아래 코드는 `test.orca` 파일을 컴파일한다.

```
> orcac test.orca
```
컴파일에 성공했다면 어셈블리 코드가 들어 있는 `test.orx` 실행 파일이 생성된다. `.orx`파일을 실행시켜 보려면 `orca.exe` 파일을 사용한다.
```
> orca test.orx
```
`.orx` 파일은 압축되어 있지 않으므로 일반 텍스트 에디터로 열어서 내부 어셈블리 구조를 읽을 수 있다.

## 예제 코드

`/examples` 폴더에 다양한 구현이 존재한다. 특히 `evaluator.orca` 예제는 여러 기능을 종합적으로 구현하고 있으므로 참고 바람.


## 1. 입출력 방법

오르카는 기본적으로 콘솔 창으로부터 입력을 받고 콘솔 창에 출력을 한다. 메모장에 다음과 같이 적어 놓고 빌드하여 실행시켜 보자.

```
print ( "Hello world!" );
```

아마 검은색 콘솔에 Hello world!가 찍힌 후 프로그램이 종료될 것이다. 출력은 이와 같이 print 전역 함수를 통해 이루어진다. 출력할 수 있는 데이터형에는 제한이 없으므로 따로 문자열로 캐스팅 해 줄 필요가 없다.

사용자의 입력을 받기 위해서는 read함수를 사용하면 된다. 아래 코드를 보고 따라해 보자.

```
 var name : string;print ( "좋아하는 동물을 입력해 주세요." );
 name = read ( );
 print ( "당신은 "+ name + "을(를) 좋아하시는군요." ); 
```

 
아직 다루지 않은 개념인 변수나 연산자를 사용한 예제이긴 하지만 이해하는 데 큰 문제는 없을 것이다. 여기 나온 것처럼 사용자의 입력을 받을 때는` read` 전역 함수를 사용한다. `read`함수는 사용자가 무언가를 입력하고 엔터를 눌렀을 때, 사용자가 입력한 문자열을 반환한다. 별다른 매개 변수가 필요하지 않으므로 그냥 `read()` 형태로 사용하면 된다.

## 2. 주석의 사용

프로그램을 작성하다 보면 프로그램의 곳곳에 메모를 해야 할 필요가 생긴다. 이 부분에 문제가 있으니 나중에 한 번 검토해 보라든지, 이 부분은 이러이러한 특성 때문에 이렇게 작성했다던지 등의 여러 이유로 프로그램에 주석을 달아야 한다. 하지만 무턱대고 그냥 달면 컴파일러는 주석 역시 프로그램의 일부분으로 생각해 버리기 때문에 에러를 발생시킨다. 따라서 주석을 표현하는 형식을 따로 지정해 두어 이런 문제를 방지하였다. 오르카에서는 두 가지 주석 작성 방법을 지원한다. 주석의 길이에 따라 한 줄 주석을 쓰거나 여러 줄 주석을 쓸 수 있다.

한 줄 주석은 슬래시 문자`/`를 두 번 쳐서 표현한다.

```
// 한 줄 주석은 이렇게 사용합니다.
// 이제 코드에 설명을 곁들일 수 있습니다. 
```


한 줄 이상을 주석으로 하고 싶을 때는 주석 열기 명령`/*` 와 주석 닫기 명령`*/`을 쌍으로 사용하여 표현한다.

```
/* 이렇게 사용하면 한 줄 주석처럼 쓸 수 있습니다. */
/* 긴 내용을 주석으로 달 경우 여러 줄 주석이 편합니다. */
```

주석은 컴파일 시 컴파일러에 의해 무시되기 때문에 코드의 실행에 어떤 지장도 주지 않는다.

## 3. 타입과 데이터

타입은 프로그래밍 언어에서 데이터와 연산의 의미를 결정한다. 오르카에서 정의하는 기본 타입에는 실수형과 문자열형, 그리고 함수의 반환 값이 존재하지 않음을 나타내는` void `타입이 있다. 데이터는 타입을 기반으로 해서 어떤 의미나 값을 갖는 정보를 말한다. 데이터를 정의할 수 있는 기본 타입인 실수형과 문자열형 데이터는 아래와 같이 표현한다.

```
7343
-27
3.141592
"안녕하세요"
"11231"
"뭐 먹을래? '짜장면.'"
```

실수형의 경우 우리가 평상시에 실수를 표현하는 방법으로 쓰면 된다. 마이너스 부호를 사용하여 음수를 표현하거나, 소수점을 찍어 소수를 표현할 수 있다. 문자열의 경우 큰 따옴표`"`를 사용하여 표현한다. 따옴표 안에 있기만 하면 어떤 문장이라도 모두 문자열로 인식된다. 문자열 내에 따옴표를 쓰려면 역슬래시 기호`\`를 따옴표 뒤에 붙여 사용하면 된다.

### 캐스팅

실수형은 실수형끼리만 연산할 수 있고, 문자열형은 문자열형끼리만 연산할 수 있다. 하지만 특별한 경우, 문자열형 데이터에 실수형 데이터를 더해야 한다거나, 실수형 데이터를 문자열형처럼 취급해야 하는 상황이 올 수 있다. 예를 들면 아래의 상황이 있다.

```
print ( "몇 달러를 원으로 바꾸시겠습니까?" );
var dollar : string = read ( );// 에러! 문자열에 실수를 곱할 수 없음.
var won : number = dollar * 1024; // 에러! 문자열과 실수를 더할 수 없음.
print ( "총 " + won +"원이 환전되었습니다." );
```
위의 코드에서는 두 번의 에러가 발생한다. 첫 번째 에러는 세 번째 줄에서 발생하는데, 문자열에 숫자를 곱하려고 하였기 때문에 발생한다. 즉, 사용자가 입력한 문자열 값을 저장하는 dollar 변수는 문자열형이고, 그 다음 줄에 나오는 won 변수는 실수형이기 때문에 에러가 발생하는 것이다. 두 번째 에러는 마지막 줄에서 일어난다. "총", "원이 환전되었습니다."와 같은 문자열 데이터와 won이라는 실수형 데이터를 더하려고 시도했기 때문에 에러가 발생한다.

이런 문제를 해결하기 위해서는 이질적인 두 데이터 타입 간 형 변환을 해 주는 기능이 필요하다. 이를 캐스팅*casting*이라 한다. 오르카에서는 모든 데이터가 다른 형식의 데이터로 캐스팅되지는 않고, 제한적으로만 캐스팅된다. 기본 데이터형인 실수형과 문자열형에서의 캐스팅 가능 여부는 아래 표로 쉽게 알 수 있다.

| 캐스팅 방법 | 캐스팅 가능 여부 |
| --- | --- |
| 문자열 -> 실수 | 실수를 이루는 문자로만 구성된 문자열 | 가능 |
| 그 외 | 불가능 |
| 실수 -> 문자열 | 가능 |

캐스팅은 `as`연산자로 할 수 있다. 캐스팅 대상이 되는 데이터를 `as` 앞에 쓰고, 캐스팅할 데이터 타입을 뒤에 써 주면 된다. 이런 방법으로 캐스팅을 추가하여 위의 코드를 옳게 바꾸면 아래와 같다.

```
print ( "몇 달러를 원으로 바꾸시겠습니까?" );
var dollar : string = read ( );
var won : number = dollar as number * 1024;
print ( "총 " + won as string + "원이 환전되었습니다" );
```

## 4. 변수

변수는 데이터를 담는 그릇이라고 할 수 있다. 일반적으로 데이터는 혼자 쓰이지 않고 변수라는 그릇에 담겨 사용된다. 변수는 '변하는 수'라는 단어 그 자체에 담긴 의미처럼 내용물을 쉽게 바꿀 수 있다. 또한 식별자(*identifier*)라는 이름으로 변수에 의미를 부여할 수 있기 때문에 프로그래밍에서 데이터를 다루는 매우 중요한 개념이다. 변수를 새로 만드는 것을 변수를 선언하거나 정의한다고 하는데, 오르카에서 변수를 선언할 때는` var`키워드를 사용하여 아래와 같이 한다.

```
var 식별자 : 타입
```

변수는 선언될 때, 앞으로 변수가 담게 될 데이터의 타입을 명시해 준다. 만약 타입이 명시되지 않았을 경우 컴파일러는 이 변수가 어떤 타입의 데이터를 담고 있는지 모르게 된다. 즉 컴파일러는 컴파일 시 타입 오류 체크가 어려워져 잠재적 에러를 잡아내지 못하게 된다. 또한 가상 머신에서도 모든 데이터 타입을 포괄할 수 있는 메모리 유닛을 기본 저장 단위로 사용해야 하므로 시스템 자원의 소모가 커지는 문제가 발생한다. 이런 문제를 방지하기 위해 변수 선언 시에는 데이터 타입을 기재해 주어야 한다.

식별자는 컴파일러가 다른 의미로 오인할 수 있는 특수문자나 공백 문자를 포함하면 안 된다. 식별자가 허용하는 문자 범위는 기본적인 언어 문자(영어, 한글 등)와 숫자이고 예외적으로 언더바`_`를 허용한다.

변수에는 선언과 동시에, 또는 선언 후 =기호를 이용하여 값을 대입할 수 있다.

```
var myAge : number = 18;
var myName : string;
myName = "Hyunjun Kim";
```

## 5. 배열

배열은 데이터의 선형적 집합으로, 다수의 데이터를 일렬로 세워 놓고 각각에 번호를 매겨 관리한다. 오르카에서 배열은 크기와 포함할 수 있는 데이터 타입이 정해지지 않은 일종의 다이나믹 리스트 *dynamic list*라 할 수 있다. 가상 머신의 메모리 관리 시스템 중 컨테이너 개념의 유연성을 토대로 설계되었기 때문에 원소의 수정과 추가가 매우 자유롭다는 특징이 있다. 하지만 컴파일 타임 시 타입 체크를 할 수 없기 때문에 배열을 다이나믹 리스트로 활용할 경우 확실한 안전성을 보장하지 못한다. 배열은 변수의 형태로 정의하는데, 데이터 타입에 배열을 의미하는 타입인 `array`를 기입해 주면 된다.

```
var 배열_식별자 : array
```

배열의 경우 데이터를 대괄호`[]`를 사용하여 표현한다. 대괄호 쌍 사이에 원소들을 콤마`,`로 구분하여 써 준다.

```
var fibonacci : array = [1, 1, 2, 3, 5, 8, 13, 21];
var animals : array = ["곰", "참새", "도마뱀", "가오리", "나비"];

// 배열을 다이나믹 리스트로 활용할 경우
var stuff : array = [1, "곰", 2, "참새", 13, ["도마뱀", "가오리", 100]];
```

위 코드의 세 번째 배열은 실수형, 문자열형, 배열형의 다양한 데이터형을 원소로 포함하고 있는 것을 볼 수 있다. 이런 특성을 이용하여 부분적 다차원 배열을 구성하는 것도 가능하다.

배열의 원소를 읽어 올 때 역시 대괄호 연산자를 사용한다. 배열의 식별자 뒤에 읽어 올 원소의 인덱스를 포함한 대괄호 쌍을 표현해 준다. 배열의 인덱스는 0부터 시작하기 때문에 첫 번째 원소를 참조하기 위해서는 0을 인덱스로 써 주면 된다. (아래 코드는 위의 코드와 이어진다고 하자.)

```
// 2를 출력한다.
print ( fibonacci [2] );

// '가오리'를 출력한다.
print ( stuff [5] [1] );

// 121을 출력한다. (캐스팅 필요)
print ( stuff [5] [2] as number + fibonacci [7] as number );
```

위의 코드에서 볼 수 있는 것처럼 배열 내부의 배열에 접근할 때는 단순하게 대괄호 인덱스 표현을 두 번 연달아 써 주면 된다. 또 배열은 기본적으로 데이터 타입이 명시되어 있지 않기 때문에 특정 연산을 위해서는 해당 연산자로 캐스팅 해 주어야 한다.

### 배열 API

오르카의 `include`명령을 통해 외부의 코드를 컴파일 시 같이 빌드되도록 포함시킬 수 있는데, 배열 관련 api가 들어 있는`array.orca`를 코드 첫머리에서 `include` 해 줄 경우 다양한 배열 활용 함수를 사용할 수 있다. 배열을 스택처럼 활용할 수 있게 해 주는 `push/pop` 함수부터, 배열의 길이(마지막 유효 원소가 들어 있는 위치)를 알려 주는 `length`함수까지 다양한 기능을 지원한다.

## 6. 데이터 구조체

데이터 구조체는 오르카에 객체 지향 프로그래밍 *OOP*개념을 접목시켜 보려는 시도에서 탄생하였다. 구조체 안에 맴버 변수와 함수를 가질 수 있으며 프로그래머가 직접 데이터 타입을 정의할 때 사용된다. 대다수 객체 지향 언어에 등장하는 클래스*class*와 일정 부분 비슷한 개념이나 상속 *inheritance*은 지원하지 않는다. 데이터 구조체는 프로그램의 어떤 동작 요소를 한 덩어리로 추상화할 수 있게 하여 복잡한 프로그램을 이해하기 쉬운 여러 요소들로 나누어 프로그래밍할 수 있도록 해 준다.

데이터 구조체는 `define` 키워드와 중괄호를 통해 정의한다. 중괄호 내부에는 구조체의 구성 요소가 되는 여러 변수들과 함수가 올 수 있다.

```
define 구조체_이름 {...}
```

구조체의 맴버 함수는 뒤에서 다루도록 하고, 맴버 변수만 포함시켜 보자.

```
define Person {
    var age : number = 30;
    var gender : number = 1;
    var name : string = "Gildong Hong";
    var hobby : string = "playing the piano";
}
```

여기에서는 `Person`이란 이름의 데이터 구조체를 선언하고 내부에 각종 속성을 정의하였다. 하지만 이 자체로는 프로그램에서 어떠한 의미도 갖지 않는다. 구조체가 활용되지 않았기 때문이다. 구조체는 말 그대로 '데이터의 구조'이기 때문에 구조체가 의미를 갖기 위해서는 정의된 구조체의 구조를 따르는 데이터가 생성되어 사용되어야 한다. 그리고 이런 데이터를 생성하는 것을 인스턴스 생성이라 한다. 인스턴스 생성은 `new` 키워드를 통해 이루어진다.

```
new 구조체_이름
```

이렇게 생성된 인스턴스를 변수에 담아 두고 활용해 보자. 인스턴스를 담는 변수는 데이터 타입으로 인스턴스가 속한 구조체를 쓰면 된다. (아래의 코드는 위의 코드와 이어진다고 하자.)

```
var mother : Person = new Person;
var father : Person = new Person;
var baby : Person = new Person;
```

`mother, father, baby` 라는 변수에 각각 `Person`의 인스턴스가 담겼다. 즉 이 세 변수는 아까 정의한 `Person` 구조체와 동일한 구조를 가진 데이터를 담고 있는 것이다. `Person` 구조체에는 네 가지 속성이 있었으므로, 이 세 변수도 네 가지 속성을 가지게 된다.

인스턴스의 속성에 접근하기 위해서는 점 연산자`.`를 사용한다. 점 뒤에는 접근할 속성의 이름을 쓴다. 아래와 같이 구조체 인스턴스 속성에 접근할 수 있다.

```
// Gildong Hong 출력
print ( mother.name );

// 속성 수정
mother.name = "Katrina";

// Katrina 출력
print ( mother.name );
```

처음 `mother.name`을 출력했을 때, 속성 정의 시에 기본값으로 대입했던 `Gildong Hong`이 출력된다. 속성 역시 하나의 변수이기 때문에 여타 변수들과 동일하게 값을 대입하거나 계산에 사용할 수 있다.

### 참조

인스턴스는 숫자나 문자열과 다르게 다른 변수나 배열에 대입되었을 경우, 실수형이나 문자열 데이터처럼 데이터 자체가 복사되지 않고 원본의 위치를 알려 주는 데이터인 참조 정보 *reference data* 가 담긴다. 즉, `new` 키워드로 생성된 인스턴스 데이터는 다른 변수에 대입한다고 해서 데이터가 복제되지 않는다. 대신 일종의 바로 가기 형태로 원본에 접근할 수 있는 데이터를 저장하게 된다.

```
var robin : Person = new Person;
robin.name = "Robin";

var friend : Person = robin;
friend.name = "Jackson";

// Jackson 출력
print( robin.name );
```

`friend`변수에 `robin`변수가 대입될 때 `robin`변수가 갖고 있던 인스턴스의 참조 정보가 `friend`변수로 저장된다. 따라서 `friend`와 `robin` 변수는 같은 인스턴스를 참조하고 있는 변수가 된다. 따라서 하나를 수정하면 다른 하나에 영향을 미친다.

## 7. 연산자

연산자는 데이터를 정해진 규칙에 따라 다른 데이터로 가공할 때 쓰인다. 특수한 연산자의 경우 값을 읽어오거나 쓸 때 사용된다. 연산자는 기본적으로 *대입, 산술, 비트, 논리* 연산자의 네 가지 종류로 구분할 수 있다. 경우에 따라서는 특수한 역할을 수행하는 특수 연산자가 쓰이기도 한다.

### 1. 대입 연산자

대입 연산자는 `=`기호를 사용하여 우변에 있는 데이터를 좌변의 변수에 대입할 때 사용된다. 수학적 기호의 의미와는 조금 다른데, 수학에서는 좌, 우변이 바뀌여도 의미가 변하지 않지만 오르카에서는 좌, 우변이 바뀌면 의미가 달라지거나 좌변이 데이터일 경우 에러가 발생하게 된다.

```
변수 = 데이터
```

대입 연산자는 아래에서 다룰 산술 연산자와 결합하여 사용할 수 있는데, 좌변과 우변에 대하여 산술 연산을 한 후, 대입을 하라는 의미이다.

### 1. 산술 연산자

산술 연산자는 기본적인 사칙 연산 및 모듈러 *modulo* 연산을 지원한다. 수학에서 연산을 할 때와 마찬가지로 식을 널리 알려진 사칙연산 기호를 써서 표현하면 된다. 단 나머지 연산은 슬래시 기호`/`로 표현하며 모듈러 연산은 퍼센트 기호`%`로 표현한다. 연산자의 우선 순위가 존재하기 때문에 나눗셈, 곱셈은 덧셈보다 먼저 계산되므로 따로 괄호를 사용하여 우선 순위를 표현해 주지 않아도 된다.

### 2. 비트 연산자

비트 연산자는 비트 수준에서 데이터를 다룰 수 있게 해 준다. 비트 논리 연산자로 AND`&`, OR`|`, XOR`^`, NOT`~` 연산자가 존재한다. 오르카에는 이진형을 다루는 데이터 타입이 따로 없기 때문에 비트 연산의 결과는 실수형으로 처리한다. 비트 쉬프트 연산자 `<<, <<<, >>, >>>` 의 경우에도 마찬가지로 실수형으로 처리된다. 비트 연산자도 대입 연산 축약형으로 쓸 수 있다. 비트 연산자 뒤에 대입 기호`=`를 써 주면 된다.

### 3. 논리 연산자

논리 연산자는 연산 결과가 항상 `0`또는 `1`로 출력된다. 여기서 `0`은 거짓 `false`이고 `1`은 참 `true`이다. 두 수의 크기 비교나 값의 동일성 체크를 할 때 쓰이는 연산자로 조건문, 반복문 등에서 실행 조건을 결정해 주는 조건식을 구성하는 핵심 요소이다.

### 4. 특수 연산자

프로그램을 작성하다 보면 어떤 값에 `1`을 증가시키거나 감소시켜야 할 상황이 많이 발생한다. 이 명령은 기본적인 산술 연산자와 대입 연산자만으로도 충분히 해결할 수 있지만, 증감 연산자를 도입하여 더 간결하게 표현할 수 있도록 하였다. 증감 연산자는 어떤 값에서 `1`을 증가시키거나 감소시키는 역할을 하는데, 증가시킬 경우 `++`기호를 사용하고 감소시킬 경우 `--`기호를 사용한다. 증감 연산자에도 두 가지 종류가 있는데, 접미사*Suffix*형 증감 연산자와 접두사 *Prefix*형 증감 연산자가 바로 그것이다. 이 두 연산자의 차이는 아래 표로 정리해 두었다. 변수 `V`의 값은 `0`이라고 하자.

| 증감 연산자 | 동작 |
| --- | --- |
| 1 증가 | 접미사형 `print(V++)` | `V`의 값을 처리한 후 1을 더한다. (0출력) |
| 접두사형 `print(++V)` | 1을 더한 후 `V`의 값을 처리한다. (1출력) |
| 1 감소 | 접미사형 `print(V--)` | `V`의 값을 처리한 후 1을 뺀다. (0출력) |
| 접두사형 `print(--V)` | 1을 뺀 후 `V`의 값을 처리한다. (-1출력) |

증감 연산자 이외에도 배열에서 원소를 읽는 데 필요한 중괄호 연산자나, 인스턴스의 속성을 참조하는데 쓰이는 점 연산자는 위에서 설명하였으므로 제외하겠다.

### 5. 연산자의 우선순위

사칙 연산에서 곱셈이 덧셈보다 우선 순위가 앞섰던 것처럼 지금까지 나왔던 여러 연산자에도 우선 순위가 존재한다. 이런 우선 순위가 존재하기 때문에 코드를 작성할 때 괄호를 대거로 사용하지 않아도 컴파일러는 프로그래머가 의도한 결과대로 컴파일 해 준다. (오르카의 연산자 우선 순위는 C++의 연산자 우선순위를 바탕으로 정하였다.)

| 우선 순위 | 연산자 | 우선 순위 | 연산자 |
| --- | --- | --- | --- |
| 1 | `.` (인스턴스 속성 참조) | 9 | `<`, `<=`, `>`, `>=` |
| 2 | `[]` | 10 | `==`, `!=` |
| 3 | `as` (캐스팅) | 11 | `&` |
| 4 | `++`, `--` (접미사형) | 12 | `^` |
| 5 | `++`, `--` (접두사형), `~`, `!`, `-` (부호) | 13 | `||` |
| 6 | `\*`, `/`, `%` | 14 | `&&` |
| 7 | `+`, `-` | 15 | `||` |
| 8 | `<<`, `>>` | 16 | `=`, 각종 축약형 대입 연산자 |

### 6. 자동 캐스팅

문자열형 데이터에서의 `+`연산의 의미와 실수형 데이터에서의 `+`연산의 의미는 다르다. 전자는 두 문자열을 이어 주는 역할을 하고, 후자는 좌변과 우변의 수를 더하라는 의미이다. 프로그램을 작성하다 보면 문자열 데이터와 실수 데이터를 연결해서 써야 하는 경우가 자주 쓰이는데 (이전에 한번 등장했던 환율 프로그램에서도 그랬다.) 문자열끼리 이어 주는 `+`연산은 실수에는 적용될 수 없으므로 실수형을 문자열형으로 캐스팅 한 후 `+` 연산을 해야 한다. 이런 번거로운 캐스팅 작업을 줄여 주기 위해 오르카 컴파일러는 문자열과 실수가 더해진 형태의 식에서는 실수를 자동으로 문자열로 캐스팅한다. 즉 아래의 첫 번째 코드는 두 번째 코드와 같이 캐스팅하지 않아도 자동 캐스팅된다.

```
print("1 더하기 1은 " + ( 1 + 1 ) + "입니다.");
print("1 더하기 1은 " + ( 1 + 1 ) as string + "입니다.");
```

## 8. 조건문

프로그램의 흐름 제어에서 조건문은 중요한 역할을 한다. 조건문은 이름 그대로 특정 조건을 만족할 때만 실행되는 명령문 모음이다. 조건문은 조건식과 조건식을 만족할 경우 실행될 코드 블록으로 이루어져 있다. 조건식은 계산 결과가 참 `1`또는 거짓 `0`이 나오는 연산식인데, 산술 연산자를 이용하여 구성할 수도 있지만 보통 논리 연산자를 사용하여 구성한다. 조건문은 `if`키워드를 사용하여 구현한다.

```
if ( 조건식 ) {
    조건식의 계산 값이 참(=1)일 경우 실행되는 명령문
}
```

조건식을 다양한 논리 연산자를 사용하여 구성해 보자. 실수 데이터로 조건식을 구성할 경우 대소 관계를 이용한 비교식을 작성할 수 있다.

```
var age : number = 17;
if ( age < 19 ) {
    print( "알림: 만 19세 미만은 입장하실 수 없습니다." );
} 
```

위의 식에서 < 연산자는 우변이 좌변보다 클 경우 참 `1`을 나타내고 그렇지 않을 경우 거짓 `0`을 나타낸다. 변수 `age`에 담긴 값은 17로, 19보다 작으므로 조건문이 실행되어 알림 메시지가 나타나게 된다. 문자열형 데이터로 조건식을 구성할 경우에는 같다(==), 같지 않다(!=) 연산자를 사용할 수 있다.

```
var id : string = "admin";
if ( id == "admin" ) {    
    print( "관리자 계정에 로그인하였습니다." );
}
```

위의 코드에서는 `==` 연산자를 사용하여 변수 `id`의 값이 문자열 데이터 `"admin"`와 일치할 경우 조건문을 실행한다. 만약 위의 코드에서 보안성을 강화하여 아이디가 `admin`이고, 비밀번호도 일치할 경우에만 관리자 계정 접속 메시지를 띄우려면 어떻게 해야 할까? 가장 간단한 방법으로는 `if`문을 두 번 중첩해서 사용하는 방법이 있다.

```
if ( id == "admin" ) {
    if ( password == "root" ) {    
        print( "관리자 계정에 로그인하였습니다." );
    }
}
```

하지만 이렇게 if문을 두 번 중첩하지 않고도 `if`문 하나로 동일한 결과를 얻을 수 있는 방법이 있다. 논리 `and` 연산자를 사용하여 두 논리문이 모두 참 `1`일 때만 1을 나타내도록 하면 된다. 혹은 산술 연산자를 사용하여 논리 `and`를 대체할 수도 있다.

```
// 첫 번째 방법 (일반적인 방법)
if ( id == "admin" && password == "root" ) {
    print( "관리자 계정에 로그인하였습니다." );
}

// 두 번째 방법 (사실 방법은 무수히 많다.)
if ( ( id == "admin" ) + ( password == "root" ) >= 2 ){
    print( "관리자 계정에 로그인하였습니다." );
}
```

만약 로그인에 실패하였을 경우에도 메시지를 띄우려면 어떻게 해야 할까? 지금까지 알고 있는 지식만을 사용하여 로그인 성공 여부 체크에 쓰였던 논리식의 역을 구하여 새로운 조건문을 만들어도 된다. 하지만 이런 번거로운 작업을 하지 않아도 조건문이 거짓일 경우 실행되는 코드 블록을 지정해 줄 수 있다. `if`문과 한 세트로 쓰이는 종속 명령문인 `else` 문을 사용하면 된다.

```
if ( id == "admin" && password == "root" ) {
    print( "관리자 계정에 로그인하였습니다." );
} else {    
    print( "로그인에 실패했습니다." );
}
```

`else`문은 `if`문 뒤에 사용되어 `if`문의 조건식이 거짓`0`일 경우에 실행된다. 어찌 보면 당연한 이야기지만 `else`문은 `if`문이 거짓일 때만 실행되므로 `if`문 당 하나만 쓸 수 있다. 여기서 한 발짝 더 나가서, 로그인에 실패했을 경우 아이디가 틀렸는지 비밀번호가 틀렸는지 알려 주려면 어떻게 해야 할까? `else` 문 내부에 여러 `if`문을 작성하는 방법으로 이 문제를 해결할 수 있다. 하지만 `else`문 내부에 굳이 `if`문을 작성하지 않아도 `else-if`문을 사용하면 보다 간편하게 코드를 작성할 수 있다. `else if` 문을 지정하는 키워드는 그대로 `else if` 를 사용한다.

```
if ( id == "admin" && password == "root" ) {
    print( "관리자 계정에 로그인하였습니다." );
} else if ( id == "admin" ) {
    print( "로그인에 실패했습니다. (비밀번호가 잘못되었습니다.)" );
} else if ( password == "root" ) {    
    print( "로그인에 실패했습니다. (아이디가 잘못되었습니다.)" );
} else {    
    print( "로그인에 실패했습니다. (아이디와 비밀번호가 모두 잘못되었습니다.)" );
}
```

`else if`문은, ... 첫 조건이 거짓일 경우 다음 조건으로 넘어가고, 그 조건도 거짓일 경우 다음 조건으로 넘어가고 .. 의 식으로 실행되므로 `if`문의 연장형으로 생각하면 된다.

```
IF | ELSE IF | ELSE IF | ELSE IF | ... | ELSE IF | ELSE
```

## 9. 반복문

반복문은 위에서 다뤘던 `if`문과 비슷한 형태로 구성되어 있다. 조건문에서는 특정 조건을 만족하면 조건문 블록에 있는 코드를 한 번 실행하고 다음 단계로 넘어가지만, 반복문에서는 특정 조건을 만족하는 이상 반복문 블록에 있는 코드를 계속 반복하여 실행한다. 따라서 반복문의 실행을 그만 두게 하기 위해서는 중간에 실행 조건을 만족하지 않도록 해 주는 작업이 필요하다. 만약 이런 작업이 올바르게 되지 않았을 경우 반복문 블록은 무한히 반복 실행되는 무한 루프 *infinite loop*상태에 빠져 프로그램이 정상적으로 동작하지 못하도록 한다.

기본적인 반복문은 `while`키워드를 사용해서 만든다. `while`문은 주어진 조건이 참일 경우 `while` 블록을 반복해서 실행한다. 이 실행은 주어진 조건이 거짓이 될 때까지 계속된다.

```
 while ( 조건식 ) {    
    조건식이 참(=1)일 때 실행되는 명령
 }
```


`while`문을 이용해서 간단한 반복문을 만들어 보자. 아래의 오르카 코드는 구구단 표를 출력하는 프로그램이다.

```
var i : number = 1;
while ( i ++ < 9 ) {   
    var j : number = 1;
    while ( j ++ < 9 ) {
       print ( i + "X" + "j" + "=" + ( i \* j ) );
    }
}
```

위 코드에서는 `while`문의 조건식으로 `i ++ < 10` 라는 다소 생소한 식을 사용하였다. 이 식에서 `++`연산자(접미형 증가 연산자)는 `<`연산자보다 우선순위가 앞서므로 먼저 계산되는데 접미형 증가 연산자는 앞 변수의 값을 결과값으로 가진 후, 앞 변수의 값을 `1`증가시키므로 런타임에서 실제 조건식은` 1 < 9, 2 < 9, ..., 9 < 9` 가 된다. 결국 반복문은 거짓인 조건식인 `9 < 9` 가 등장하기 전까지 8번 반복하게 된다. 종속된 반복문 역시 같은 방법으로 8번 반복하므로 호출되는 `print`함수의 횟수는 결과적으로 64번이 된다. 위의 코드를 빌드하여 실행시켜 보면 2X2=4 부터 9X9=81까지 구구단 표가 출력된다.

대부분의 반복문에서 조건식은 위와 같은 형태로, 한 변수의 값을 계속 증가시키거나 감소시켜서 일정 수에 이를 때까지 참이도록 구성하는 것이 일반적이다. `while`문 중에 이런 형태를 띄는 경우에는 증감형 반복에 특화된 반복문으로 대체할 수 있다. `for`문이 바로 그것이다. `for`문은 `while`문보다 훨씬 구성이 단순하고 표현이 직관적이기 때문에 대부분의 반복문을 작성하는 데 편리하게 이용될 수 있다. `for` 문은 증감 변수와 반복 범위로 표현한다.

```
for ( 반복 변수 in 시작 ... 끝 ) {
   반복될 내용
}
```

`for` 문을 이용한 간단한 예제를 보자. 아래는 `maximum` 보다 작은 수 범위에서 모든 소수를 찾는 프로그램의 오르카 코드이다.

```
var maximum:number = 100000;
for ( i in 3 ... maximum ) {
    var flag : number = 1;
    for ( j in 2 ... sqrt( i ) ) {
        if( ( i % j ) == 0 ) {
            flag = 0;
        }
    }
    if( flag ) {
        print( i );
    }
}
```

위 프로그램의 소수를 찾는 로직은 복잡하지 않다. `maximum`이하 3이상의 모든 수에 대해, 해당하는 수의 제곱근보다 작은 2 이상의 수로 모두 나누어 본 후 나누어지는 수가 있으면 소수가 아니라고 표시를 해 두는 것이다. 위 코드에서 `for`문은 `while`문으로 작성하였다면 자칫 복잡해 질 수 있는 조건식을 직관적으로 표현하고 있다.

### 제어문

위 코드에서 한 가지 생소한 점은 나머지가 0인지 체크하고 있는 `if`문의 내부에 `break`라는 이름의 명령이 쓰였다는 것이다. 이 명령은 소수를 체크하던 수가 특정 수로 나누어 떨어졌을 때 실행되었는데, 소수가 아님을 `flag` 변수에 표시하고 난 뒤에는 더 이상 다른 수로 소수 검사를 할 필요가 없으므로 반복을 그만두라는 의미로 사용된 것이다. 이처럼 반복문의 내부에서 반복문의 실행을 제어하는 명령을 제어문이라 한다. 제어문에는 반복을 중단시키는 `break` 문과 곧장 반복문의 처음으로 돌아가게 하는 `continue`문의 두 가지 종류가 있다.

반복문 내에서 `break`문이 호출되면 `break`를 포함하고 있는 반복문은 반복을 중단하고 반복문 코드 블록을 빠져나온다. 반복문이 여러 번 중첩된 상태에서 `break`문이 호출될 경우에는 `break`문을 가장 근접하게 포함하고 있는 반복문만 반복이 중지된다. 이런 `break` 문의 특징을 이용해서 `while`반복문을 작성할 때 내부에 `break`를 가지는 `if`문으로 조건식을 대신할 수도 있다.

```
// 조건식을 이렇게 쓸 수도 있다.
while( 1 ) {
    if ( 조건식 ) {
        break;
    }
}
```

`continue`문 역시 `break`문처럼 등장 위치에서 반복문 코드 블록의 실행을 중지한다. 대신 `continue`문은 실행을 중지한 후 곧장 반복문의 조건 체크 부분으로 되돌아간다. 일반적으로 반복문은 프로그래밍에서 다수의 데이터를 가공하기 위해 사용되는데, 반복 도중 특정 순서에서 데이터가 이미 가공되었다던가 하는 경우로 계속 반복 블록을 실행하는 것이 시간 낭비라고 여겨질 때 `continue`문이 사용된다. `continue`문의 쓰임을 알 수 있는 간단한 예제를 확인하자.

```
for ( n in 1 ... 10 ) {
    if ( n % 2 == 0 ) {
        continue;
    }
    print( n );
}
```

위의 코드는 `n`이 2의 배수일 경우 현재 실행을 그만 두고 다음 실행으로 넘어가기 때문에 `print` 명령이 실행되지 않는다. 따라서 출력 화면에는 홀수만 표시가 된다.

## 10. 함수

수학에서 함수는 정의역에 존재하는 원소와 치역에 존재하는 원소의 대응을 의미한다. 이는 오르카에서의 함수 개념과 크게 다르지 않다. 정의역 원소의 개념으로 입력을 받으면 일반적으로 그에 상응하는 치역 원소 개념의 결과값을 출력해 주기 때문이다. 하지만 오르카에서 함수는 수학적인 의미보다는 어떤 한 정보 처리 과정에 이름을 붙인다는 개념으로 이해하는 것이 정확하다. 입력이나 출력이 없는 함수도 존재할뿐더러, 입력이 같으면 항상 출력도 같은 함수만 존재하지는 않기 때문이다. 함수는 데이터 구조체를 선언할 때와 마찬가지로 `define`키워드를 사용하여 정의한다. 함수는 기본적으로 입력과 출력을 받는 형태이기 때문에 함수가 데이터를 입력받을 매개 변수와, 함수 내부의 처리가 끝난 후 반환될 데이터 타입을 지정해 주어야 한다.

```
define 함수명 ( 매개 변수 ) -> 출력되는 데이터 타입 {
    실행할 내용
}
```

함수 내부에서 값을 반환하기 위해서는 `return`명령을 사용한다. 함수는 값을 반환함과 동시에 종료되므로, `return`명령으로 값을 반환하였을 경우 남은 과정이 있더라도 무시되고 함수는 종료되게 된다. `return`명령은 아래와 같은 형식으로 사용한다.

```
return 반환할 데이터
```

아래는 위에서 다룬 함수 정의와 값 반환의 개념을 이용하여 입력과 출력이 있는 함수를 작성한 예제 프로그램이다. 이 프로그램은 입력으로 받은 수 `n`의 `n!` (펙토리얼)값을 계산한다.

```
// n!값을 계산한다.
define factorial ( n : number ) -> number {
    if ( n < 1 ) {
        return 1;
    } else {
        return n * factorial ( n – 1 );
    }
}

// 3628800 출력
print ( factorial ( 10 ) );
```

이 프로그램은 반복문 없이 재귀 호출을 사용하여 함수만으로 로직을 구현하였다. 펙토리얼 연산의 점화식 `a(n)=n\*a(n-1)`을 생각해 보면 위 함수의 의미를 쉽게 파악할 수 있을 것이다.

함수의 매개 변수가 두 개 이상일 경우에는 콤마 `,`를 사용하여 매개 변수를 구분하여 적는다.

```
// 두 수의 평균을 구한다.
define average ( n1 : number, n2 : number ) -> number {
    return ( n1 + n2 ) / 2;
}

// 200 출력
print ( average ( average ( average ( 100, 300 ), 300 ), 150 ) );
```

함수 중에는 매개 변수를 필요로 하지 않는 함수들도 있다. 보통 이런 함수들은 출력이 항상 일정하거나, 연산에 필요한 값들을 전역 변수를 통해 얻기 때문에 매개 변수를 필요로 하지 않는다. 함수에 매개 변수가 없을 경우 그냥 공란으로 비워 두면 된다.

```
define getPI ( ) -> number {
    return 3.141592;
}
```

한편, 함수 중에서는 값을 반환하지 않는 함수도 존재한다. 이 함수들은 (위에서 살펴 본 사례들 과 비슷하게) 계산 결과를 전역 변수에 저장하기 때문에 값을 반환할 필요가 없는 경우나 값을 도출해 내는 것이 함수의 목적이 아닌 경우에 값을 반환하지 않는다. 함수가 값을 반환하지 않는다는 것을 표현하는 방법에는 두 가지가 있다. 첫 번째는 `void`타입을 반환하는 방법이다. `void`타입은 '타입 없음'을 나타내는 특수 타입으로, 함수 정의에서 반환 타입이 존재하지 않음을 나타낼 때만 쓰인다. 더 간단한 두 번째 방법은, 아예 함수의 반환을 나타내는 `->`기호를 생략해 버리는 것이다. 아래의 함수는 후자에 해당한다.

```
// 이 함수의 관심은 오로지 Hello World!를 출력하는 것이다.
define printHelloWorld ( ) {
    print ( "Hello World!" );
}

printHelloWorld ( );
```

### 데이터 구조체 맴버 함수

데이터 구조체 내부에 함수를 정의하면 관련된 데이터를 데이터 구조 내에서 일관적으로 처리할 수 있기 때문에 형식상, 구조상 더 의미가 뚜렷한 코드를 작성하는 데 도움을 준다. 또한 실제 구현 내용을 구조체에 정의하고 외부에는 접근 방법만 제시해 주기 때문에 캡슐화 *encapsulation* 의 방법을 제공한다. 사실 엄밀히 말해 구조체 맴버 함수는 구조체 내부에서 정의되지 않는다. 구조체 밖에서 구조체의 프로토타입 *prototype*에 속성을 추가하는 방식으로 추가되는데, 이와 같은 방식을 채택한 이유는 컴파일 시 구조체 스캐닝 *scanning*작업이 간편해져 컴파일이 조금 빨라지는 이유도 있지만 필요에 따라 외부에서 함수를 추가하거나 수정할 수 있게 해 주기 때문이다.

구조체 맴버 함수는 구조체의 식별자를 함수 정의 앞에 추가하는 방식으로 정의된다. 참고로 구조체 맴버 함수는 반드시 구조체가 정의된 스코프와 동일한 스코프에서 정의되어야 한다. (스코프에 대해서는 다음 장에서 다룬다.)

```
define 구조체명 . 함수명 ( 매개 변수 ) -> 출력되는 데이터 타입 {    
    실행할 내용
}
```
우선 2차원 좌표를 관리하는 구조체 `Point`를 만들어 보자.

```
define Point {
    var x : number = 0;
    var y : number = 0;
}

// 예제로 사용할 인스턴스 생성
var startPoint : Point = new Point;
var destinationPoint : Point = new Point;
```
위에서 다룬 맴버 함수 정의 방법을 이용하여 이 구조체에 다른 점과의 거리를 구하는 함수를 추가해 보자. 이 함수는 다른 `Point`인스턴스를 입력으로 받아 두 점 사이의 거리를 출력한다.

```
define Point . getDistance ( p:Point ) -> number {    
    return sqrt ( pow ( this.x – p.x ) + pow ( this.y – p.y ) );
}

// startPoint와 destinationPoint 사이의 거리를 출력한다.
print ( startPoint.getDistance ( destinationPoint ) );
```

위의 `getDistance`함수 정의에서 확인할 수 있듯이, 구조체 맴버 함수가 자신의 속성에 접근할 때는 `this`키워드를 사용하여 접근한다.

## 11. 스코프

스코프란 간단히 말하면 특정 명령이 영향을 미칠 수 있는 코드 내 범위를 의미한다. 스코프는 함수나 변수가 선언되는 위치에 따라 사용될 수 있는 위치 범위가 달라지기 때문에 생긴다. 오르카에서 스코프 개념을 도입한 이유는 코드 블록 내에서 정의된 변수나 함수는 그 코드 블록을 정의한 위치에서 영향력을 발휘하지 못하도록 하기 위해서이다. 다음과 같은 두 가지 상황을 생각해 보면 스코프 도입의 필요성을 느낄 수 있다.

1. 안전 상 문제: 반복문이나 조건문 블록에서 정의된 변수나 함수는 말 그대로 조건부로 정의되므로, 블록 외부에서는 변수나 함수가 정의되었다는 확신을 가질 수 없다. 이 경우 조건이 충족되지 않아 정의되지 않았는데 외부에서 이 데이터를 사용할 경우 런타임 에러가 발생한다.

2. 자원 관리의 편리함: 변수는 프로그램이 종료될 때까지 필요한 데이터를 담고 있는 경우보다 어느 구간 내에서만 임시적으로 필요한 경우가 더 많다. 이 경우 메모리 자원을 효율적으로 사용하기 위해서는 더 이상 필요하지 않을 변수는 반환해 주어야 하는데, 변수의 수가 많을 경우 매우 번거로운 작업이 된다. 하지만 스코프를 도입하여 컴파일러가 특정 범위를 벗어난 변수를 자동으로 반환해 주게 하면 이런 작업을 하지 않아도 된다.

스코프의 범위는 선언된 함수나 변수가 존재하는 블록에 한정된다. 최상위에 선언된 함수나 변수는 프로그램 전체에서 유효 범위를 갖는다.

```
var a : number = 0;

if ( 1 ) {
    // 여기서 변수 a는 유효하다.    
    var b : number = 0;
}

// 여기서 변수 b는 유효하지 않다.
```

만약 상위 블록에 존재하는 변수와 같은 이름의 변수가 하위 블록에 선언되었을 경우에는 하위 블록의 스코프에서는 상위 블록에서 선언된 변수는 무시된다.

```
var a : number = 10;

if ( 1 ) {
    var a : number = 100;
    
    // 100을 출력한다.
    print ( a );
} 
```

# 어셈블리 코드

오르카로 작성된 프로그램은 컴파일러를 거쳐, 가상 머신이 실행할 수 있는 형태인 오르카 어셈블리 코드로 번역된다. 이 어셈블리 코드는 가상 머신이 소프트웨어적으로 빠르고 간편하게 처리할 수 있도록 설계되었다. 어셈블리 코드는 메모리 읽고 쓰기, 데이터 연산과 같은 원시적인 정보 처리를 하거나 어셈블리 리스트의 읽는 위치를 수정하여 프로그램의 흐름을 제어한다. 기본적으로 오르카의 어셈블리 코드는 스택 처리에 기반을 두고 있는데, 어떤 명령이 실행되면 그 명령을 수행하는 데 필요한 데이터를 스택에서 `POP`하여 사용한 후, 결과 값을 스택에 다시 PUSH 해 준다. 이러한 과정을 통해 후위표기법으로 작성된 각종 연산식과 함수 호출 명령을 별다른 파싱 과정 없이 빠르게 연산할 수 있으며 간편하게 표현할 수 있게 된다. 현재 오르카 어셈블리 코드는 총 21가지가 있다. 이 21가지의 코드는 *흐름 제어, 스택 제어, 데이터 입출력, 연산*의 네 종류로 분류할 수 있다.

## 1. 흐름 제어

오르카로 만들어진 프로그램은 가상 머신 내에서 어셈블리 코드로 된 리스트로 재구성된다. 가상 머신은 이 코드를 순차적으로 실행하면서 프로그램을 진행한다. 흐름 제어 코드는 일정 조건에 따라 가상 머신의 리스트 읽는 위치(리딩 포인트)를 수정함으로써 다양한 조건 실행이나 반복을 할 수 있게 해 준다. 이렇게 리딩 포인트를 수정하는 명령을 점프 명령이라 하고, 두 가지 종류가 있다.

| 명령 이름 | 동작 | 어셈블리 코드 |
| --- | --- | --- |
| 포인트 점프 *Point Jump* | 스택 `POP -> L` (점프할 포인트)현재 읽는 위치를 `L`로 설정 | `JMP` |
| 조건부 포인트 점프 *Conditional Point Jump* | 스택 `POP -> C` (논리 데이터)스택 `POP -> L` (점프할 포인트)`C < 1` 일 경우 (조건이 거짓일 때)현재 읽는 위치를 `L`로 설정 | `JMF` |

포인트 점프 명령의 경우 무조건적으로 정해진 포인트로 점프하게 된다. 이 명령은 함수에서 `return`명령이 나온 뒤 처음 함수가 호출된 위치로 이동하거나, `if-else`문에서 `if`문의 끝에서 `else`문의 끝으로 이동할 때 처럼 어떤 작업이 끝나고 특정한 위치로 복귀하거나 어느 부분의 실행을 생략하고 넘길 때 사용된다. 반면 조건부 포인트 점프의 경우 특정 조건이 만족될 시에만 점프를 하게 된다. 런타임 시 조건에 따라 실행 양상이 달라지는 조건문이나 반복문의 처리에서 쓰인다.

점프 명령 시 대부분의 경우 점프할 위치가 고정되어 있어 컴파일 시 점프할 포인트를 명시적으로 정해 줄 수 있다. 하지만 함수 호출과 같은 상황에서는 점프할 포인트가 고정되어 있지 않다. 함수는 프로그램 내 다양한 위치에서 여러번 호출될 수 있기 때문이다. 따라서 함수 실행이 끝난 후, 원래 실행하던 포인트로 복귀하기 위해서는 마지막으로 함수가 호출된 지점을 가상 머신이 기억하고 있어야 한다. 가상 머신에게 현재 포인트를 나중에 읽어올 수 있도록 스택에 `PUSH`하라는 명령을 포인트 기억이라 한다.

| 명령 이름 | 동작 | 어셈블리 코드 |
| --- | --- | --- |
| 포인트 기억 *Current Point Push* | 포인트 스택 `PUSH <- 현재 프로그램의 실행 포인트` | `PSC` |
| 포인트 병합 *Point Stack Merge* | 스택 `PUSH <- 포인트 스택 POP` | `CSC` |

포인트 기억 명령은 현재 포인트를 바로 메인 스택에 `PUSH`하지 않고, 포인터 스택에 따로 `PUSH` 한다. 그 이유는 메인 스택에 예외가 발생하면 `POP`되야 할 요소들이 스택에서 그냥 남아 있게 되는데, 남아있는 요소들이 `PUSH`된 포인트보다 스택에서 위에 있을 경우 포인트 대신 남아있는 요소가 점프 포인트로 사용되는 에러가 생길 수 있기 때문이다. 그래서 포인트만 관리하는 스택을 따로 만들고, 점프 명령을 내리기 전 메인 스택에 포인트 스택에서 `POP`한 포인트를 `PUSH`해서 사용한다. (아래 참고: 함수 호출 시 사용되는 흐름 제어 명령의 실행 순서)

```
함수 호출 | 포인트 기억(PSC) | 함수 실행 | 포인트 병합(CSC) | 포인트 점프(JMP)
```

오르카 가상 머신은 구조의 단순화와 최소한의 메모리 사용를 위해 런타임 시 모든 지역 변수를 일종의 전역 변수로 취급한다. 이러한 특징 때문에 오르카 가상 머신은 재귀 함수를 처리할 수 없는데, 이 문제를 해결하기 위해 도입된 개념이 메모리에 Level 속성을 주는 방법이다.

| 명령 이름 | 동작 | 어셈블리 코드 |
| --- | --- | --- |
| 스코프 열기 *Scope Opening* | 메모리 레벨을 한 단계 증가시킨다. | `OSC` |
| 스코프 닫기 *Scope Closing* | 메모리 레벨을 한 단계 감소시킨다. | `CSC` |

이 방법을 통해 동일하지 않은 스코프에 있는 지역 변수를 구분할 수 있다. 이 어셈블리 명령은 함수 스코프가 열릴 때와 닫힐 때 실행되어야 하므로 각각 스코프 열기와 닫기 명령으로 이름을 정하게 되었다. (이에 관한 내용은 *<가상 머신>*에서 자세히 다룬다).

```
하위 스코프에 들어감 | 스코프 열기(OSC) | 상위 스코프로 복귀 | 스코프 닫기(CSC)
```

프로그램의 끝이나 강제 종료 명령 위치에 컴파일러는 종료(`END`)명령을 넣는다. 종료 명령이 나오면 가상 머신은 어셈블리 리스트를 실행하던 메인 루프를 정지시키고 모든 메모리 자원을 반납한 뒤 대기(`IDLE`)상태로 돌아간다.

| 명령 이름 | 동작 | 어셈블리 코드 |
| --- | --- | --- |
| 종료 *End* | 메인 루프를 정지한다.모든 메모리 자원을 반납한다. | `END` |

## 2. 스택 제어

스택 제어는 스택 푸시(`PSH`)와 스택 팝(`POP`)의 두 명령으로 구성된다. 여기서 스택은 모든 연산 과정이 이루어지는 가상 머신의 메인 스택을 의미한다. 모든 중간 계산 값들은 메인 스택에 `PUSH`되었다가, 다시 `POP`되어 연산 과정을 거친 후 다시 `PUSH`되는 과정을 반복한다. `PUSH` 명령에는 세 가지 종류가 있어, 어느 방법으로 데이터를 스택에 추가할 것인지 결정할 수 있다.

리터럴 *Literal* `PUSH`의 경우 어셈블리 명령의 매개 변수로 실제 데이터를 받는다. 리터럴 푸시로 스택에 `"Orca"`라는 데이터를 추가하고자 한다면 `PSH Orca`와 같이 사용할 수 있다. 컴파일러는 이미 알려졌거나 코드에서 명시된 데이터가 사용될 때 리터럴 푸시를 사용하여 처리한다. 레지스터 *Register* `PUSH`는 매개 변수로 레지스터 주소를 받는데, 레지스터에서 데이터를 취득하여 스택에 `PUSH`하는 명령이다. 레지스터 푸시는 리터럴 푸시만큼 흔하게 사용되지는 않지만 특정 값을 스택에서 `POP`한 후 다시 `PUSH`할 때 사용되며, 스택에서 특정 원소의 순서를 바꿀 때 중요한 역할을 한다. 메모리 *Memory* `PUSH`는 매개 변수로 메모리 주소를 받는다. 역시 레지스터 푸시와 유사하게 메모리에서 데이터를 취득하여 스택에 `PUSH`하는 명령이다. 메모리 푸시는 메모리에서 값을 읽어 올 때 쓰인다.

스택 `POP`명령은 스택의 가장 위에 있는 데이터를 꺼내어 매개 변수로 받은 레지스터에 저장한다. 일반적으로 다른 어셈블리 명령 내에 포함되어 있는 경우가 많으므로 독립적으로 쓰이는 경우보다는 레지스터 푸시 명령과 함께 스택의 원소 순서 조작을 위해 쓰인다.

| 명령 이름 | 동작 | 어셈블리 코드 |
| --- | --- | --- |
| 스택 리터럴 푸시 *Stack Literal Push* | 스택 `PUSH <- 리터럴 값` | `PSH [리터럴 값]` |
| 스택 레지스터 푸시 *Stack Register Push* | 스택 `PUSH <- 레지스터의 데이터` | `PSR [레지스터 주소]` |
| 스택 메모리 푸시 *Stack Memory Push* | 스택 `PUSH <- 메모리의 데이터` | `PSM [메모리 주소]` |
| 스택 팝 *Stack Pop* | 스택 `POP -> 레지스터에 저장` | `POP [레지스터 주소]` |

## 3. 데이터 입출력

가상 머신의 메모리 자원을 관리하는 명령어로, *메모리 할당, 읽기, 쓰기* 명령으로 구성되어 있다. 오르카 가상 머신의 메모리(물리적인 메모리를 소프트웨어로 구현)는 다차원 배열의 자료구조로 구성되어 있다. 메모리에서 한 단위의 데이터가 담길 수 있는 공간을 데이터 셀 *data cell* 이라 하며, 가상 머신은 각각의 데이터 셀을 구분할 수 있도록 고유의 주소를 부여한다. 데이터 셀은 그 자체가 다른 데이터 셀들을 포함하는 모체가 될 수 있는데, 이런 데이터 셀을 컨테이너 *Container* 라고 한다. 배열이나 오브젝트와 같은 여러 데이터를 포함하고 있는 경우 컨테이너 셀을 사용하여 데이터를 메모리에 저장한다.

데이터 셀을 사용하기 위해서는 데이터 셀에 주소가 부여되어야 한다. 그리고 데이터 셀에 주소를 부여하는 프로세스를 메모리 할당이라고 한다. 메모리를 할당하는 방법에는 두 가지가 있다. 특정 메모리 주소를 정해 주고 그 주소에 데이터 셀을 부여하는 방법과(정적 메모리 할당), 데이터 셀에 임의의 주소를 부여하는 방법이다(동적 메모리 할당).

정적 메모리 할당의 경우 컴파일러가 상수나 프로그램 내의 전역 변수를 처리할 때 사용한다. 메모리 주소를 일정하게 정해 두기 때문에, 컴파일 시 메모리 데이터를 별다른 처리 없이 바로 읽어올 수 있다는 특징이 있다. 하지만 메모리 반환이 불가능하여 프로그램 종료시까지 계속 메모리 자원을 차지하기 때문에 전역 상수나 변수 처리 등에 대해서만 제한적으로 사용된다.

그 외의 모든 데이터는 동적 메모리 할당을 통해 데이터 셀과 대응된다. 메모리를 동적 할당할 경우, 필요할 때 메모리를 할당 받아 쓰다가 더 이상 데이터가 필요하지 않으면 시스템에 반환하면 된다. 따라서 메모리를 비교적 효율적으로 운용할 수 있다는 장점이 있다. 하지만 메모리 주소가 고정되어 있지 않기 때문에 메모리 주소를 메모리 읽기, 쓰기 작업이 있을 때마다 레지스터와 스택을 매개로 전달해야 하므로 정적으로 할당된 메모리의 읽기, 쓰기 속도보다는 느리다. 동적 메모리 할당 명령이 내려지면 가상 머신은 빈 데이터 셀을 찾아 임의의 주소를 부여한 뒤, 그 주소를 스택에 `PUSH`하여 나중에 참조할 수 있도록 한다.

| 명령 이름 | 동작 | 어셈블리 코드 |
| --- | --- | --- |
| 정적 메모리 할당(데이터 셀) | 빈 데이터 셀에 주어진 주소를 부여한다. |`SAL [메모리 주소]` |
| 정적 메모리 할당(컨테이너) | 빈 컨테이너에 주어진 주소를 부여한다. | `SAA [메모리 주소]` |
| 동적 메모리 할당(데이터 셀) | 빈 데이터 셀에 임의로 주소를 부여한다.스택 `PUSH <- 부여한 주소` | `DAL` |
| 동적 메모리 할당(컨테이너) | 빈 컨테이너에 임의로 주소를 부여한다.스택 `PUSH <- 부여한 주소` | `DAA` |

메모리 할당 명령은 데이터 셀을 할당할 것인지, 컨테이너를 할당할 것인지에 따라 분리하여 사용한다. 일반적인 변수의 경우 데이터 셀을 할당하고, 오브젝트나 배열은 컨테이너를 할당한다.

데이터 셀(또는 컨테이너)와 연결된 유효한 메모리 주소를 취득하고 나면 갖고 있는 주소를 통해 그 메모리에 언제든지 엑세스할 수 있게 된다. 데이터 셀에 엑세스하기 위해서는 위에서 다룬 스택 제어 명령 중 스택 메모리 푸시(`PSM`) 명령을 사용하면 된다. 하지만 컨테이너의 경우 내부에 다수의 데이터 셀을 포함하고 있어 스택 메모리 푸시 명령을 사용하면 스택에 원하지 않은 데이터 뭉치가 들어가게 된다. 따라서 컨테이너 내부의 특정 데이터 셀에 접근하게 해 주는 명령이 필요한데, 이 명령을 배열 읽기(`RDA`)라고 한다. 컨테이너는 기본적으로 배열 구조이기 때문에 엑세스하려는 데이터 셀의 인덱스를 알면 그 데이터에 접근할 수 있다.

| 명령 이름 | 동작 | 어셈블리 코드 |
| --- | --- | --- |
| 배열 읽기 *Read Array(data cell container)* | 스택 `POP -> 컨테이너(배열)` <br> 참조스택 `POP -> 데이터 셀 인덱스` <br> 스택 `PUSH <- 컨테이너에서 읽은 데이터` | `RDA` |

배열 읽기(`RDA`)명령은 첫 번째 스택에서 컨테이너의 주소를 받는 것이 아니라 컨테이너 참조를 바로 받는다. 컨테이너의 주소를 받게 되면 그 컨테이너의 하위 항목만 접근할 수 있지만 컨테이너 참조를 받으면 여러 겹의 컨테이너로 된(컨테이너에 컨테이너가 포함되어 있는 구조) 복잡한 자료 구조도 동일한 명령으로 접근할 수 있기 때문이다.

메모리의 데이터를 조작할 때는 메모리 쓰기(`STO`)를 사용한다. 이 명령은 메모리의 주소와 담길 데이터를 스택을 통해 받아 메모리를 수정한다. 컨테이너의 경우도 메모리 쓰기 명령(`STO`)으로 데이터를 저장할 수 있지만, 내부 원소 중 하나를 조작하고자 할 때는 배열 쓰기 명령(`STA`)를 사용한다.

| 명령 이름 | 동작 | 어셈블리 코드 |
| --- | --- | --- |
| 메모리 쓰기 *Store Data* | 스택 `POP -> 메모리 주소` <br> 스택 `POP -> 메모리에 쓸 값` <br> 메모리에 값을 쓴다. | STO |
| 배열 쓰기 *Store Array Data* | 스택 `POP -> 컨테이너(배열) 참조` 스택 `POP -> 데이터 셀 인덱스` <br>스택 `POP -> 하위 데이터 셀에 쓸 값` <br> 주어진 인덱스에 해당하는 컨테이너의 하위 데이터 셀에 값을 쓴다. | STA |

동적 메모리 할당의 경우, 더 이상 메모리 공간이 필요하지 않을 경우 사용하고 있던 주소를 반환함으로써 메모리 공간을 확보할 수 있다. 메모리 반환(FRE)명령은 동적 할당된 메모리 주소를 가용 메모리 풀에 반환하고, 데이터 셀을 초기화함으로써 시스템 자원을 환원한다.

| 명령 이름 | 동작 | 어셈블리 코드 |
| --- | --- | --- |
| 메모리 반환 *Free Memory* | 스택 `POP -> 메모리 주소` <br>가용 메모리 주소 풀에 추가한다.<br>데이터 셀을 초기화한다. | `FRE` |

컴파일러는 지역 변수가 만료되는 스코프의 종료점에서, 또는 함수 실행 후 종료된 스코프 내의모든 지역 변수에 대해 메모리 반환을 시도한다.

## 4. 연산

연산은 가상 머신 내에서 데이터를 실질적으로 활용하는 부분이다. 기본적인 산술 연산부터 외부 함수를 이용한 연산까지 광범위한 역할을 수행한다. 연산 명령은 기본적인 내장 연산을 위한 `OPR`명령과 확장성을 고려해 만들어진 외부 명령 호출인 `IVK` 명령이 있다. `OPR`명령은 산술, 비트, 논리 연산을 처리하게 되며 오르카에서 지원하는 연산자와 일대일 대응된다. `IVK`명령은 언어 시스템 자체에서 지원하지 않는 여러 수학 함수 등의 처리에 사용된다. 연산의 종류는 내장 연산 코드 *operation code*, 외장 연산 코드*invoke code*라는 이름으로 매개 변수를 통해 전달 받는다.

| 명령 이름 | 동작 | 어셈블리 코드 |
| --- | --- | --- |
| 내장 연산 *Operation* | 스택 `POP -> 피연산자1`<br>스택 `POP -> 피연산자2`<br>...<br>스택 `POP -> 피연산자n`<br>연산 코드가 요구하는 피연산자 갯수만큼 `POP`한 후 연산 코드가 요구하는 대로 연산한다.<br>스택 `PUSH <- 연산된 값` | `OPR [연산 코드]` |
| 외장 연산 Invoke | 스택 `POP -> 피연산자1`<br>스택 `POP -> 피연산자2`<br>...<br>스택 `POP -> 피연산자n`<br>연산 코드가 요구하는 피연산자 갯수만큼 `POP`한 후 외부 함수에 넘겨 연산한다.<br>스택 `PUSH <- 연산된 값` | `IVK [연산 코드]` |

내장 연산 코드는 16진수로 된 두 자리 코드로, `0x00`부터 `0x32`까지 총 51가지가 있다. 외장 연산 코드는 호출되는 외부 라이브러리의 경로와 메서드명으로 구성된다. 수학 관련 함수가 대부분을 차지하며 콘솔 입/출력 관련 함수와 디버깅 함수로 구성되어 있다.

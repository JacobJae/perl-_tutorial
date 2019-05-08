**2시간 반만에 펄 익히기**
===================
저자: Sam Hughes, 번역: 김진(Kim Jin)
펄(Perl)은 동적 타입의 고수준, (인터프리트되는) 스크립트 언어로 PHP와 파이썬(Python)과 많이 비교되곤 한다. 펄의 구문은 예전의 셀 스크립트 (shell script)로부터 많이 따왔다. 그래서 구글로 검색하기 어려운 헷갈리기 쉬운 기호들을 많이 사용하는 것으로도 유명하다. 셀 스크립트로부터 기원한 까닭에 펄은 다른 스크립트나 프로그램을 엮어주는 글루 코드(glue code)를 만드는데 아주 좋다. 텍스트 데이터를 처리하거나 생성하는데 펄은 안성맞춤이다. 펄은 널리 사용되고 있으며 인기도 많으며, 이식성도 좋고 지원도 잘되고 있다. 펄에는 "그 일을 하는데엔 한가지 이상의 방법이 있다(There's More Than One Way To Do It:TMTOWTDI)"라는 설계 철학이 있다. 이는 파이썬의 "그 일을 하는데 명백한 - 그리고 가급적이면 오직 - 하나의 방법이 있어야 한다(There should be one - and preferably only one - obvious way to do it)"와 대비된다.

펄에는 안좋은 점들도 있지만 이를 벌충할만큼 아주 훌륭한 특징들도 있다. 이런 점은 이제까지 있었던 모든 다른 프로그래밍 언어와 같다.

나는 펄을 전파하기 위해서가 아니라 유익한 정보를 전하려 이 글을 썼으며 나와 비슷할 걸로 생각하는 다음과 같은 처지에 있는 사람을 대상으로 했다.

- 너무 기술적이고 극단적인 경우에 대한 설명을 많이 하고 있는 http://perl.org에 있는 펄 공식 문서를 싫어하며
- "원리와 예제"로 새로운 언어를 빨리 익히고 싶어하며
- 래리 월(Larry Wall)이 요점만 말하기를 원하며
- 이미 일반적인 프로그래밍을 할 줄 알며
- 일을 마치는데 필요한 이상으로 펄을 자세히 파고들고 싶지는 않은 사람들

그리고 가급적 짧게 쓰려고 했지만 필요한 내용은 빼놓지 않았다.

미리 알려둘 내용
--------

StackEdit stores your docum
- 이 문서에서 선언하듯 말하는 데에서는 다음과 같이 부연해야 할 것이다: "이것은, 엄격하게 이야기하자면, 맞는 말이 아니다. 상황은 실제로 훨씬 복잡하다". 심각한 문제점을 발견하면 나에게 알려라. 하지만 나는 초보자들을 위해 단순하게 설명한 것이다.

- 글 전체적으로 예제에서 출력하기 위해 print 문을 사용할 때에는 명시적으로 줄넘김 처리를 하지 않았다. 출력할 실제 문자열에 더 집중하기 위해서이다. 그래서 많은 예제들이 실행해보면 "alotofwordsallsmusheduptogetherononeline" 식으로 출력될 것이다. 이것들은 무시하면 된다.

***Hello world***
펄 스크립트(script)는 확장자가 .pl인 일반적인 텍스트 파일이다.

다음은 예제로 살펴볼 helloworld.pl 스크립트이다.
```
use strict;
use warnings;

print "Hello world";
```
펄 스크립트는 펄 인터프리터(perl 또는 perl.exe)로 실행한다.
```
perl helloworld.pl [arg0 [arg1 [arg2 ...]]]
```
먼저 알려둘 게 있다. 펄의 구문은 굉장히 자유로와서 어떻게 동작할지 종잡을 수 없는 모호한 명령을 쓰는 것도 가능하다. 하지만 모두들 이런 경우는 피하고 싶어할 것이므로 어떤 게 그런 건지 설명할 필요는 없을 것 같다. 그래서 이런 상황을 만들지 않기 위해서 `use strict; use warnings;` 구문을 펄 스크립트 또는 모듈 제일 처음에 둔다. 이런 `use foo;` 형태의 명령문을 프래그마(pragma)라고 하는데 이는 `perl.exe`에 보내는 표지로서, 프로그램이 시작하기 전 구문 검사를 할 때 실행되는 것들이다. 구문 검사가 끝나고 프로그램이 시작되면 인터프리터가 이 명령들을 마주쳐도 아무런 동작을 하지 않는다.

'#' 기호로 줄을 시작하면 그 줄 끝까지가 주석이다. 펄에 블럭 단위 주석 구문은 없다.

변수(Variable)
-------
펄에는 스칼라, 배열, 해시의 세 가지 종류의 변수가 있다. 각각을 고유의 시길(sigil) - 앞에 붙이는 특수 문자 - 로 구분한다: 각각 $, @, %이다. 변수는 my 구문으로 선언하며 감싸고 있는 블럭이 끝나거나 그렇지 않으면 파일 끝까지 그 효력 범위(scope)가 지속된다.

**스칼라 변수**

스칼라 변수에는 다음의 값들을 대입할 수 있다.

- undef (파이썬의 None, PHP의 null)
- 숫자 (펄에서는 정수값과 실수값을 구별하지 않는다)
- 문자열
- 다른 변수에 대한 참조(reference)
```
my $undef = undef;
print $undef; # 경고(warning)가 발생하며 빈 문자열("")을 출력한다.

# 묵시적인 undef
my $undef2;
print $undef2; # 위와 같은 경고 출력

my $num = 4040.5;
print $num; # "4040.5"

my $string = "world";
print $string; # "world"
```
(참조는 조금 있다가 다루겠다.)

문자열 결합은 . 연산자로 한다(PHP와 같다):
```
print "Hello ".$string; # "Hello world"
```
불리언(Boolean)
------
펄에는 불리언 데이터형이 없다. 만약 스칼라가 if 문 안에 있다면 다음과 같은 경우에만 불리언 "false" 값이 된다:

- undef
- 숫자 0
- 문자열 ""
- 문자열 "0"

펄 문서를 보다보면 함수들이 "참(true)"과 "거짓(false)" 값을 반환한다고 하는 것이 많이 있다. 실제로는 어떤 함수가 "참"을 반환하다라고 하는 것은 보통 1을 반환하는 것이고 "거짓"을 반환한다고 하는 것은 보통 빈 문자열 ""을 반환하는 것이다.

약한 타입(Weak typing)
------
**스칼라에 대입된 값이 "숫자"인지 "문자열"인지 확인하는 것은 불가능하다.** 정확하게 말하자면 그럴 필요가 없어야 한다. 왜냐하면 사용하는 연산자에 따라 스칼라를 숫자로 취급할지 또는 문자열로 취급할지가 결정되기 때문이다. 문자열로 사용하면 문자열처럼 동작하고 숫자로 사용하면 숫자로 동작하는 것이다(그것이 불가능하면 경고가 발생한다):
```
my $str1 = "4G";
my $str2 = "4H";

print $str1 .  $str2; # "4G4H"
print $str1 +  $str2; # "8" 경고가 2개 발생한다.
print $str1 eq $str2; # "" (빈 문자열. 즉 false)
print $str1 == $str2; # "1" 경고가 2개 발생

# 고전적인 실수 사례
print "yes" == "no"; # "1" 즉 참. 경고가 2개 발생. 숫자로 사용되었기에 두 개 모두 0이 된다
```
즉 스칼라를 숫자로 취급할지 문자열로 취급할지 정해서 각각 경우에 맞게 정확히 연산자를 사용해야 한다. 그래서 스칼라를 숫자로 취급하여 비교하는 연산자와 문자열로 취급하여 비교하는 연산자가 각각 있다:
```
# 숫자로 취급하여 비교하는 연산자:  <,  >, <=, >=, ==, !=, <=>, +, *
# 문자열로 취급하여 비교하는 연산자:  lt, gt, le, ge, eq, ne, cmp, ., x
```
배열 변수
------
0부터 시작하는 정수 인덱스를 가진 스칼라들의 리스트를 배열 변수라 한다. 파이썬의 list, PHP의 array에 해당한다. 배열은 스칼라 리스트를 괄호로 감싸서 선언한다:
```
my @array = (
    "print",
    "these",
    "strings",
    "out",
    "for",
    "me", # 마지막에 쉼표가 있어도 문제없다
);
```
배열에서 각각의 원소값에 접근하려면 달러 기호($)를 사용해야 한다. 얻는 값이 배열이 아니고 스칼라이기 때문에 이렇게 하는 것이다:
```
print $array[0]; # "print"
print $array[1]; # "these"
print $array[2]; # "strings"
print $array[3]; # "out"
print $array[4]; # "for"
print $array[5]; # "me"
print $array[6]; # 경고 발생, undef 반환, ""가 출력됨
```
배열의 마지막 원소부터 앞쪽으로 세면서 인덱스를 지정하려면 음수를 사용한다:
```
print $array[-1]; # "me"
print $array[-2]; # "for"
print $array[-3]; # "out"
print $array[-4]; # "strings"
print $array[-5]; # "these"
print $array[-6]; # "print"
print $array[-7]; # 경고 발생, undef 반환, ""가 출력됨
```
스칼라` $var`와 배열`@var`, 그리고 그 원소 `$var[0]`는 서로 구별되기에 
동시에 사용해도 된다. 하지만 코드를 읽기엔 혼란할 수 있으므로 혼용해서 쓰지 않는 게 좋다.

배열의 길이를 구하려면 다음과 같이 한다:
```
print "This array has ".(scalar @array)."elements"; # "This array has 6 elements"
print "The last populated index is ".$#array;       # "The last populated index is 5"
```
펄 스크립트가 실행될 때의 명령행 인자들은 내장 배열 변수(built-in array variable)인 `@ARGV`에 저장된다.

변수들은 문자열에 끼워질(interpolated) 수 있다:
```
print "Hello $string"; # "Hello world"
print "@array";        # "print these strings out for me"
```
**주의할 점**: 이메일 주소를 `"jeff@gmail.com"`과 같은 식으로 문자열로 만들면 펄은 배열 변수 @gmail이 문자열에 끼워졌다고 생각하고 문자열을 해석한다. 그래서 이 변수를 찾을 수 없으면, 실행 중 에러(runtime error)를 발생시킨다. 여기서 배열 변수로 해석하지 않도록 하려면 백슬래시()로 시길을 탈출(escape)시키거나, 큰 따옴표를 쓰지 말고 작은 따옴표(')를 사용하면 된다.
```
print "Hello, \$string"; # "Hello $string"
print 'Hello, $string'; # "Hello $string"
print "\@array"; # "@array"
print '@array'; # "@array
```
해시 변수
------
문자열로 인덱스된 스칼라 목록을 해시 변수라 한다. 파이썬에서는 dictionary라 하고 PHP에서는 array라 하는 것이다.
```
my %scientists = (
    "Newton" => "Isaac",
    "Einstein" => "Albert",
    "Darwin" => "Charles",
);
```
배열을 선언하는 방식과 비슷한 것을 알 수 있다. 실제로 이중 화살표 기호 => 는 쉼표 구분자의 별칭에 지나지 않기 때문에 "뚱뚱한 쉼표(fat comma)"라고도 부른다. 
해시는 짝수 개의 원소가 있는 리스트로 선언하며, 짝수번째 원소(0, 2, ...번째)는 문자열로 취급한다.

배열과 마찬가지로, 얻는 값이 스칼라이기 때문에, 해시의 원소에 접근할 때는 달러 기호($)를 사용한다.
```
print $scientists{"Newton"}; # "Isaac"
print $scientists{"Einstein"}; # "Albert"
print $scientists{"Darwin"}; # "Charles"
print $scientists{"Dyson"}; # 경고를 발생하며, undef 를 반환한다. 출력은 ""이다.
```
여기서는 중괄호({, })를 사용한다. 배열의 경우와 같이 스칼라 변수 `$var`와 스칼라 원소 `$var{"foo"}`를 가진 해시 변수 `%var`에 이름 충돌은 없다.

해시는 바로 배열로 변환할 수 있다. 이 경우 배열은 해시의 키값과 원소값이 번갈아 나열되서 해시의 두 배 크기가 된다(반대로 배열을 해시로 변환하는 것도 바로 된다):
```
my @scientists = %scientists;
```
주의할 점은 배열과는 달리 해시는 키값에 따른 순서가 없다는 것이다. 어떤 순서든 더 효율적인 방식으로 순서를 정한다. 그래서 키 - 값의 쌍은 유지되지만 순서는 조정되는 현상을 볼 수 있다:
```
print "@scientists"; # "Einstein Albert Darwin Charles Newton Isaac" 식으로 출력된다
```
마지막으로, 배열로부터 값을 얻으려면 대괄호([,])를 사용하고 해시로부터 값을 얻으려면 중괄호({,})를 사용한다는 점을 기억해 두자. 대괄호는 숫자 연산자로, 중괄호는 문자열 연산자로 작동한다. 인덱스 값으로 숫자를 쓰는지, 문자열을 쓰는지는 아무 의미가 없다:
```
my $data = "orange";
my @data = ("purple");
my %data = ("0" => "blue");

print $data; # "orange"
print $data[0]; # "purple"
print $data["0"]; # "purple"
print $data{0}; # "blue"
print $data{"0"}; # "blue"
```
리스트(List)
------
펄에서 리스트는 배열이나 해시와는 다른 존재이다. 하지만 이미 리스트가 나오긴 했다.
```
(
    "print",
    "these",
    "strings",
    "out",
    "for",
    "me",
)

(
    "Newton" => "Isaac",
    "Einstein" => "Albert",
    "Darwin" => "Charles",
)
```
**리스트는 변수가 아니다.** 리스트는 배열이나 해시 변수에 대입되기 위해 잠깐 생성되는 값이다. 그래서 배열을 선언하는 구문과 해시를 선언하는 구문이 같은 것이다. "리스트"와 "배열"이라는 단어를 서로 바꿔서 써도 상관없는 경우는 많다. 하지만 그런 경우만큼 또 미묘하게 다르고 완전히 헷갈리게 동작하는 경우도 많다.

자 어쨌든, `=>`는`,`의 다른 형태일 뿐이라는 점을 기억하며 다음 예를 보자
```
("one", 1, "three", 3, "five", 5)
("one" => 1, "three" => 3, "five" => 5)
```
`=>`를 사용해서 어떤 리스트는 배열 선언이며 또다른 리스트는 해시 선언이라는 힌트를 줄 수 있다. 그러나 그것 자체로는 둘 중 어떤 것도 선언하지 않는다. 그것들은 그저 리스트일 뿐이다. 더구나 다를 바 없는 같은 리스트이다.

게다가 이런 선언인 경우는:
```
()
```
아예 힌트조차 없다. 이 리스트는 빈 배열을 선언할 때에도 빈 해시를 선언할 때에도 사용할 수 있으며 perl 인터프리터가 어느 쪽인지 알 수 있는 방법은 없다. 이런 특이한 측면을 이해한다면, 리스트는 중첩할 수 없다고 할 수밖에 없는 이유도 이해할 수 있을 것이다. 다음을 시도해 보면:
```
my @array = (
    "apples",
    "bananas",
    (
        "inner",
        "list",
        "several",
        "entries",
    ),
    "cherries",
);
```
펄은 `("inner", "list", "several", "entries")`를 배열로 봐야할지 해시로 봐야할지 결정할 수 없다. 그래서 펄은 어느쪽도 아니고 **리스트를 하나의 긴 리스트로 펼쳐버린다**:
```
print $array[0]; # "apples"
print $array[1]; # "bananas"
print $array[2]; # "inner"
print $array[3]; # "list"
print $array[4]; # "several"
print $array[5]; # "entries"
print $arrya[6]; # "cherries"
```
이건 뚱뚱한 쉼표를 써도 마찬가지이다:
```
my %hash = (
    "beer" => "good",
    "bananas" => (
        "green" => "wait",
        "yellow" => "eat",
    ),
);

# 위의 경우 경고(warning)가 발생한다. 해시가 7개의 요소로 선언되기 때문이다

print $hash{"beer"}; # "good"
print $hash{"bananas"}; # "green"
print $hash{"wait"}; # "yellow"
print $hash{"eat"}; # undef, 그래서 ""가 출력되고 경고가 발생한다
```
물론, 이 점을 이용하여 여러 배열을 간단하게 합칠 수 있다:
```
my @bones = ("humerus", ("jaw, "skull"), "tibia");
my @fingers = ("thumb", "index", "middle", "ring", "little");
my @parts = (@bones, @fingers, ("foot", "toes"), "eyeball", "knuckle");
print @parts;
```
More on this shortly.

문맥(Context)
------
펄의 가장 독특한 특징은 코드가 문맥에 영향을 받는다(context-sensitive)는 점이다. **모든 펄 표현식은 결과로 스칼라를 생성할지 리스트를 생성할지에 따라 스칼라 문맥 또는 리스트 문맥 중 하나에서 평가된다.** 많은 펄 표현식과 내장 함수(built-in functions)가 평가되는 문맥에 따라 아주 다르게 동작한다.

`$scalar =`와 같은 스칼라 대입식의 경우 표현식은 스칼라 문맥에 있게 된다. 이런 경우, `"Mendeleev"`와 같은 표현식은 같은 스칼라 값 `"Mendeleev"`로 평가된다:
```
my $scalar = "Mendeleev";
```
`@array =` 또는 `%hash =`와 같은 배열 또는 해시 대입문은 표현식을 리스트 문맥에서 평가한다. 리스트 문맥에서 리스트 값을 평가하면 리스트가 반환되어 배열이나 해시로 들어가게 된다:
```
my @array = ("Alpha", "Beta", "Gamma", "Pie");
my %hash = ("Alpha" => "Beta", "Gamma" => "Pie");
```
여기까진 별거없다.

리스트 문맥에서 스칼라 표현식이 평가되면 1개의 요소를 가진 리스트로 평가된다:
```
my @array = "Mendeleev"; # my @array = ("Mendeleev"); 와 같은 결과
```
리스트 표현식이 스칼라 문맥에서 평가되면 리스트의 마지막 요소가 반환된다.
```
my $scalar = ("Alpha", "Beta", "Gamma", "Pie"); # $scalar의 값은 "Pie"
```
배열식(배열은 리스트와 다른 것이라는 점을 기억하면)은 스칼라 문맥에서 배열의 길이로 평가된다.
```
my @array = ("Alpha", "Beta", "Gamma", "Pie");
my $scalar = @array; # $scalar의 값은 4이다
```
print 내장 함수는 인자 모두를 리스트 문맥에서 평가한다. 실제로 `print`는 제한없는 수의 인자 리스트를 받아 각각 차례로 출력한다. 그래서 배열도 직접 출력하는데 이용할 수 있는 것이다:
```
my @array = ("Alpha", "Beta", "Goo");
my $scalar = "-X-";
print @array; # "AlphaBetaGoo"
print $scalar, @array, 98; # "-X-AlphaBetaGoo98"
```
scalar 내장 함수를 이용하면 어떠한 표현식이라도 스칼라 문맥에서 평가되게 할 수 있다. 배열의 길이를 구할 때 `scalar` 함수를 쓰는 것은 이를 이용한 결과이다.

서브루틴이 스칼라 문맥에서 실행되면 스칼라 값을 반환하고 리스트 문맥에서 실행되면 리스트 값을 반환하도록 작성하는데 얽매일 필요는 없다. 위에서 보았듯이 펄이 적절하게 값을 처리해준다.

참조(reference)와 중첩된 자료구조
------
리스트가 리스트를 원소로 포함할 수 없듯이 **배열과 해시는 다른 배열과 해시를 원소로 포함할 수 없다.** 오로지 스칼라만을 포함할 수 있다. 다음을 시도해 보면:
```
my @outer = ("Sun", "Mercury", "Venus", undef, "Mars");
my @inner = ("Earth", "Moon");

$outer[3] = @inner;

print $outer[3]; # "2"
```
`$outer[3]`은 스칼라이기 때문에 스칼라 값이 필요하다. `@inner`와 같은 배열값을 대입하려고 하면 `@inner`는 스칼라 문맥에서 평가된다. 이건 `scalar @inner`, 즉 `@inner `배열의 길이를 대입하는 것과 마찬가지가 되어 2가 대입된다.

하지만 스칼라 변수는 배열과 해시를 포함하여 다른 변수의 *참조(reference)* 를 저장할 수도 있다. 이를 이용하여 펄에서 복잡한 자료구조를 만들어 낼 수 있다.
참조는 역슬래시를 이용하여 생성한다.
```
my $colour = "Indigo";
my $scalarRef = \$colour;
```
참조 변수는 참조 변수가 참조하는 변수를 원래 사용하는 식에서 변수의 이름이 있는 부분을 중괄호로 감싸고 이름이 있는 부분을 그 참조 변수로 바꾸는 형태로 사용한다.
```
print $colour; # "Indigo"
print $scalarRef; # e.g. "SCALAR(0x182c180)"
print ${ $scalarRef }; # "Indigo"
```
모호한 점이 없이 결과가 명확하다면 중괄호를 생략할 수 있다:
```
print $$scalarRef; # "Indigo"
```
배열이나 해시에 대한 참조라면 중괄호를 이용하거나, 또는 좀더 많이 쓰는 형태인 화살표 연산자 ->를 이용하여 값을 얻올 수 있다:
```
my @colours = ("Red", "Orange", "Yellow", "Green", "Blue");
my $arrayRef = \@colours;

print $colours[0]; # 배열 직접 접근
print ${ $arrayRef }[0]; # 참조를 이용하여 배열에 접근
print $arrayRef->[0]; # 위와 똑같음

my %atomicWeights = ("Hydrogen" => 1.008, "Helium" => 4.003, "Manganese" => 54.94);
my $hashRef = \%atomicWeights;

print $atomicWeights{"Helium"}; # 해시 직접 접근
print ${ $hashRef }{"Helium"}; # 참조를 이용하여 해시에 접근
print $hashRef->{"Helium"}; # 위와 똑같음. 많이 사용되는 형태
```
**자료구조 선언**

네 가지 예를 들어보겠다. 하지만 실제로는 마지막 것이 가장 유용하다.
```
my %owner1 = (
    "name" => "Santa Claus",
    "DOB" => "1882-12-25",
);

my $owner1Ref = \%owner1;

my %owner2 = (
    "name" => "Mickey Mouse",
    "DOB" => "1928-11-18",
);

my $owner2Ref = \%owner2;

my @owners = ( $owner1Ref, $owner2Ref );

my $ownersRef = \@owners;

my %account = (
    "number" => "12345678",
    "opened" => "2000-01-01",
    "owners" => $ownersRef,
);
```
위의 것은 너무 노가다가 심하다. 다음과 같이 줄여보자:
```
my %owner1 = (
    "name" => "Santa Claus",
    "DOB" => "1882-12-25",
);

my %owner2 = (
    "name" => "Mickey Mouse",
    "DOB" => "1928-11-18",
);

my @owners = ( \%owner1, \%owner2 );

my %account = (
    "number" => "12345678",
    "opened" => "2000-01-01",
    "owners" => \@owners,
);
```
다른 기호를 사용하여 *익명* 배열과 해시를 선언할 수 있다. 대괄호를 사용하여 익명 배열을, 그리고 중괄호를 사용하여 익명 해시를 선언한다. 반환하는 값은 각각의 익명 자료구조의 참조이다. 자 다음은 `%accounts`와 완전히 똑같은 결과다:
```
# 중괄호는 익명 해시
my $owner1Ref = {
    "name" => "Santa Claus",
    "DOB" => "1882-12-25",
};

my $owner2Ref = {
    "name" => "Mickey Mouse",
    "DOB" => "1928-11-18",
};

# 대괄호는 익명 배열
my $ownersRef = [ $owner1Ref, $owner2Ref ];

my %account = (
    "number" => "12345678",
    "opened" => "2000-01-01",
    "owners" => $ownersRef,
);
```
더 줄여보자(그리고 이 방식이 복잡한 자료구조를 한번에 선언할 때 실제로 사용하는 방식이다):
```
my %account = (
    "number" => "31415926",
    "opened" => "3000-01-01",
    "owners" => [
        {
            "name" => "Philip Fry",
            "DOB" => "1974-08-06",
        },
        {
            "name" => "Hubert Farnsworth",
            "DOB" => "2841-04-09",
        },
    ],
);
```
**자료구조에서 값을 얻는 법**

위의 예에서 아직 `%account` 변수는 유효하고 나머지 변수들은 무효화되어 없어졌다고 해보자. 앞서의 각각의 경우를 반대로 되짚어가면 정보를 출력할 수 있다. 마찬가지로 네 가지 예를 보이겠는데, 역시 마지막 경우가 가장 유용하다.
```
my $ownersRef = $account{"owners"};
my @owners    = @{ $ownersRef };
my $owner1Ref = $owners[0];
my %owner1    = %{ $owner1Ref };
my $owner2Ref = $owners[1];
my %owner2    = %{ $owner2Ref };
print "Account #", $account{"number"}, "\n";
print "Opened on ", $account{"opened"}, "\n";
print "Joint owners:\n";
print "\t", $owner1{"name"}, " (born ", $owner1{"DOB"}, ")\n";
print "\t", $owner2{"name"}, " (born ", $owner2{"DOB"}, ")\n";
```
줄여보면:
```
my @owners = @{ $account{"owners"} };
my %owner1 = %{ $owners[0] };
my %owner2 = %{ $owners[1] };
print "Account #", $account{"number"}, "\n";
print "Opened on ", $account{"opened"}, "\n";
print "Joint owners:\n";
print "\t", $owner1{"name"}, " (born ", $owner1{"DOB"}, ")\n";
print "\t", $owner2{"name"}, " (born ", $owner2{"DOB"}, ")\n";
```
참조와 -> 연산자를 사용하면:
```
my $ownersRef = $account{"owners"};
my $owner1Ref = $ownersRef->[0];
my $owner2Ref = $ownersRef->[1];
print "Account #", $account{"number"}, "\n";
print "Opened on ", $account{"opened"}, "\n";
print "Joint owners:\n";
print "\t", $owner1Ref->{"name"}, " (born ", $owner1Ref->{"DOB"}, ")\n";
print "\t", $owner2Ref->{"name"}, " (born ", $owner2Ref->{"DOB"}, ")\n";
```
그리고 중간에 거치는 단계를 모두 생략하면
```
print "Account #", $account{"number"}, "\n";
print "Opened on ", $account{"opened"}, "\n";
print "Joint owners:\n";
print "\t", $account{"owners"}->[0]->{"name"}, " (born ", $account{"owners"}->[0]->{"DOB"}, ")\n";
print "\t", $account{"owners"}->[1]->{"name"}, " (born ", $account{"owners"}->[1]->{"DOB"}, ")\n";
```
**배열의 참조는 잘못 사용하면 낭패를 볼 수 있다.**

다음 배열은 다섯개의 원소를 가진다:
```
my @array1 = (1, 2, 3, 4, 5);
print @array1; # "12345"
```
그러나 다음 배열은 오직 하나의 원소만 가진다. 다섯개의 원소를 가진 익명 배열의 참조가 그 원소이다.
```
my @array2 = [1, 2, 3, 4, 5];
print @array2; # e.g. "ARRAY(0x182c180)"
```
다음 스칼라 변수는 다섯개의 원소를 가진 익명 배열의 참조이다.
```
my $array3Ref = [1, 2, 3, 4, 5];
print $array3Ref;      # e.g. "ARRAY(0x22710c0)"
print @{ $array3Ref }; # "12345"
print @$array3Ref;     # "12345"
```
조건문
------
**`if` ... `elsif` ... `else` ...**

`elsif`의 철자가 특별한 거 외엔 별다를 거 없다.
```
my $word = "antidisestablishmentarianism";
my $strlen = [length](http://perldoc.perl.org/functions/length.html) $word;

if($strlen >= 15) {
    print "'", $word, "' is a very long word";
} elsif(10 <= $strlen && $strlen < 15) {
    print "'", $word, "' is a medium-length word";
} else {
    print "'", $word, "' is a a short word";
}
```
명령문을 짧게 쓸 때 많이 쓰는 "명령문 if 조건" 형식도 있다.
```
print "'", $word, "' is actually enormous" if $strlen >= 20;
```
**`unless` ... `else` ...**
```
my $temperature = 20;

unless($temperature > 30) {
    print $temperature, " degrees Celsius is not very hot";
} else {
    print $temperature, " degrees Celsius is actually pretty hot";
}
```
`unless` 문을 쓰면 헷갈려지기 쉽기 때문에 대체로 사용하지 않는 게 낫다. "`unless` [`... else`]" 문은 조건절을 반대로 하거나 블럭을 뒤바꾸는 식으로 간단하게 "`if` [`... else`]" 문으로 바꿀 수 있다. 다행히도 `elsunless`와 같은 예약어(keyword)는 없다.

그러나 다음과 같이 읽기 쉽게 쓸 수 있는 경우에는 사용해도 좋다:
```
print "Oh no it's too cold" unless $temperature > 15;
```
**삼항 연산자**

삼항 연산자 `? :`를 사용해서 간단한 `if`문을 한 문장으로 압축할 수 있다. 고전적인 사용예는 다음과 같이 단/복수형을 구분하여 출력하는 코드이다:
```
my $gain = 48;
print "You gained ", $gain, " ", ($gain == 1 ? "experience point" : "experience points"), "!";
```
잠깐 옆길로 새면, 위와 같이 단/복수형을 구분하여 출력하고자 할 때는 아래의 예처럼 교묘하게 구현하는 건 별로 좋지 않다. 가능하면 단/복수형 단어를 온전히 이용하는 게 좋다. 만약 이 코드에서 "tooth"와 "teeth"를 다른 단어로 바꿔야 한다면 검색으로 찾기는 쉽지 않을 것이다:
```
my $lost = 1;
print "You lost ", $lost, " t", ($lost == 1 ? "oo" : "ee"), "th!";
```
삼항 연산자는 중첩할 수 있다:
```
my $eggs = 5;
print "You have ", $eggs == 0 ? "no eggs" :
                   $eggs == 1 ? "an egg"  :
                   "some eggs";
```
`if` 문은 조건문을 스칼라 맥락에서 평가한다. 예를 들어 `if (@array)` 는 `@array` 변수가 1개 이상의 원소를 가질 때 참이 된다. 원소가 무엇이든 - 심지어 `undef`나 false가 되는 다른 모든 값이라도 - 상관하지 않는다.

반복문
------
반복을 하는 데에도 한가지 이상의 방법이 있다(**There's More Than One Way To Do It**)

통상적인 `while` 반복문이 있다:
```
my $i = 0;
while($i < scalar @array) {
    print $i, ": ", $array[$i];
    $i++;
}
```
`until` 예약어도 있다.
```
my $i = 0;
until($i >= scalar @array) {
    print $i, ": ", $array[$i];
    $i++;
}
```
`do`를 이용한 반복문은 위의 경우와 거의 같다(`@array` 변수가 빈 배열이라면 경고가 발생한다):
```
my $i = 0;
do {
    print $i, ": ", $array[$i];
    $i++;
} while ($i < scalar @array);
```
그리고
```
my $i = 0;
do {
    print $i, ": ", $array[$i];
    $i++;
} until ($i >= scalar @array);
```
C언어 스타일의 for 반복문도 가능하다. my를 for 문 안에 두어 $i 변수의 범위를 반복문 안으로만 제한한 점도 눈여겨 볼 필요가 있다.
```
for(my $i = 0; $i < scalar @array; $i++) {
    print $i, ": ", $array[$i];
}
# $i 변수는 여기서 더이상 존재하지 않는다.
```
위와 같은 종류의 `for` 문은 구닥다리 스타일로 여겨져서 가능하면 사용하지 않는 게 좋다. 간단한 리스트 순회는 훨씬 멋지게 할 수 있다. **PHP**와는 다르게 `for`와 `foreach` 예약어는 동의어이다. 좀더 가독성이 좋은 것으로 둘 중 아무거나 사용하면 된다:
```
foreach my $string ( @array ) {
    print $string;
}
```
배열의 인덱스가 필요하면 범위 연산자 ..를 사용하여 익명 정수 리스트를 생성할 수 있다:
```
foreach my $i ( 0 .. $#array ) {
    print $i, ": ", $array[$i];
}
```
해시를 순회할 수는 없지만 해시 키를 순회할 수는 있다. keys라는 내장 함수를 쓰면 해시의 모든 키를 원소로 하는 배열을 얻을 수 있다. 여기에 배열에서와 같이 `foreach` 를 쓰면 된다:
```
foreach my $key (keys %scientists) {
    print $key, ": ", $scientists{$key};
}
```
해시는 내부에 어떤 순서도 없으므로 키도 임의의 순서로 나열된다. `sort`라는 내장 함수를 쓰면 먼저 키 배열을 알파벳 순서로 정렬할 수 있다:
```
foreach my $key (sort keys %scientists) {
    print $key, ": ", $scientists{$key};
}
```
반복자(iterator)를 지정하지 않으면 펄은 기본 반복자 `$_`를 사용한다. `$_`는 첫번째로 나오는 내장 변수로 내장 변수 중 가장 흔하게 마주치게 될 것이다:
```
foreach ( @array ) {
    print $_;
}
```
반복문에 명령어가 하나만 있는 경우 기본 반복자를 잘 이용하여 아주 짧게 반복문을 만들 수 있다:
```
print $_ foreach @array;
```
**반복문 제어**

`next`와 `last`를 사용하여 반복문의 진행을 제어할 수 있다. 다른 많은 언어에서 각각 `continue`와 `break`에 해당하는 것이다. 또 부가적으로 이름표(label)를 반복문에 붙일 수 있다. 이름표는 모두 대문자로 쓰는 것이 관례이다. 반복문에 이름표를 붙여서 `next`와 `last`에 따른 진행을 이름표로 향하게 할 수 있다. 다음 예는 100 미만의 소수를 찾는 예이다:
```
CANDIDATE: for my $candidate ( 2 .. 100 ) {
    for my $divisor ( 2 .. sqrt $candidate ) {
        next CANDIDATE if $candidate % $divisor == 0;
    }
    print $candidate." is prime\n";
}
```
배열 함수
------
**배열 자체 변경**

이제부터 `@stack`이라는 변수를 사용하여 용례를 보이겠다.
```
my @stack = ("Fred", "Eileen", "Denise", "Charlie");
print @stack; # "FredEileenDeniseCharlie"
```
pop은 배열에서 마지막 원소를 제거하고 이를 반환한다. 배열의 마지막은 스택(ᅟstack)의 맨 위로 보면 된다:
```
print pop @stack; # "Charlie"
print @stack; # "FredEileenDenise"
```
push는 배열의 마지막에 새로 원소를 추가한다.
```
push @stack, "Bob", "Alice";
print @stack; # "FredEileenDeniseBobAlice"
```
shift는 배열에서 첫번째 원소를 제거하고 이를 반환한다:
```
print shift @stack; # "Fred"
print @stack; # "EileenDeniseBobAlice"
```
unshift는 배열의 맨 앞에 새 원소를 추가한다:
```
unshift @stack, "Hank", "Grace";
print @stack; # "HankGraceEileenDeniseBobAlice"
```
`pop, push, shift, unshift`는 모두 splice의 특별한 경우이다. `splice`는 배열의 일부를 제거하고 다른 배열의 일부로 교체하며, 제거된 부분을 반환한다:
```
print splice(@stack, 1, 4, "<<<", ">>>"); # GraceEileenDeniseBob"
print @stack; # "Hank<<<>>>Alice"
```
**기존 배열로부터 새로운 배열을 생성하기**

배열을 이용하여 새로운 배열 또는 스칼라 등등을 생성하는 함수로 다음과 같은 것들이 있다.

join 함수로 여러 문자열을 하나로 합친다:
```
my @elements = ("Antimony", "Arsenic", "Aluminum", "Selenium");
print @elements; # "AntimonyArsenicAluminumSelenium"
print "@elements"; # "Antimony Arsenic Aluminum Selenium"
print join(", ", @elements); # "Antimony, Arsenic, Aluminum, Selenium"
```
reverse 함수는 리스트 문맥에서는 배열을 뒤집은 리스트를 반환하며, 스칼라 문맥에서는 전체 리스트를 하나의 문자열로 합친 후 그것을 뒤집은 문자열을 반환한다.
```
print reverse("Hello", "World"); # "WorldHello"
print reverse("HelloWorld"); # "HelloWorld"
print scalar reverse("HelloWorld"); # "dlroWolleH"
print scalar reverse("Hello", "World"); # "dlroWolleH"
```
map 함수는 배열을 입력으로 받아 배열의 모든 원소 $_에 어떤 조작을 가하여 그 결과들로 새로운 배열을 생성한다. 어떻게 조작할지는 중괄호 안에 하나의 표현식으로 지정한다:
```
my @capitals = ("Baton Rouge", "Indianapolis", "Columbus", "Montgomery", "Helena", "Denber", "Boise");

print join, ", ", map { uc $_ } @capitals;
# "BATON ROUGE, INDIANAPOLIS, COLUMBUS, MONTGOMERY, HELENA, DENVER, BOISE"
```
grep 함수는 배열을 받아 필터링하여 결과 배열을 출력한다. map과 구문이 비슷하다. 배열 입력의 각 원소들을 $_로 받아서 첫번째 인자 블럭에서 실행한다. 결과가 참이면 그 원소 스칼라는 출력 배열로 들어가며 거짓이면 들어가지 않는다.
```
print join ", ", grep { length $_ == 6 } @capitals;
# "Helena, Denver"
```
분명히 출력된 배열의 크기는 성공적인 매칭 숫자이다. 그래서 grep을 이용하여 한 배열이 어떤 원소를 포함하고 있는지를 검사할 수 있다:
```
print scalar grep { $_ eq 'Columbus' } @capitals; # "1"
```
`grep`과 `map`을 이용하여 강력한 기능인 리스트 조건제시문(list comprehensions)을 구성할 수 있다.

sort 함수는 간단하게 사용하면 문자(알파벳) 순서로 배열을 정렬한다.
```
my @elevations = (19, 1, 2, 100, 3, 89, 100, 1056);

print join ", ", sort @elevations;
# "1, 100, 100, 1056, 19, 2, 3, 98"
```
하지만 `grep`이나 `map`과 마찬가지로 코드를 지정할 수 있다. 정렬은 항상 두 원소의 비교를 계속 반복하여 진행한다. 코드 블럭은 `$a`와 `$b`를 입력으로 받아 `$a`가 `$b`보다 작으면 -1을, 같으면 0을, 크면 1을 반환한다.

`cmp` 연산자는 문자열에 대해 위의 비교를 수행한다:
```
print join ", ", sort { $a cmp $b } @elevations;
# "1, 100, 100, 1056, 19, 2, 3, 98"
```
"우주선(spaceship) 연산자"라고도 부르는, <=> 연산자는 숫자에 대해 위의 비교를 수행한다:
```
print join ", ", sort { $a <=> $b } @elevations;
# "1, 2, 3, 19, 98, 100, 100, 1056"
```
`$a`와 `$b`는 항상 스칼라이다. 하지만 직접 비교하기는 어려운 복잡한 개체의 참조일 수도 있다. 복잡한 과정을 거쳐서 비교를 해야 한다면, 별도의 서브루틴을 만든 후 그 이름을 인자로 이용할 수도 있다:
```
sub comparator {
    # 긴 코드 ...
    # return -1 또는 0, 1
}

print join ", ", sort comparator @elevations;
```
`grep`과 `map`은 이런 식으로 쓸 수는 없다.

서브루틴과 블럭이 명시적으로 `$a`, `$b` 변수를 받지 않는 점에 유의한다. `$_`처럼 `$a`와 `$b`는 전역 변수로 비교를 할 때마다 비교할 쌍의 두 값이 채워진다.

내장 함수들
------
지금까지 여러개의 내장 함수들을 봐왔다: `print`, `sort`, `map`, `grep`, `keys`, `scalar`. 내장 함수는 펄의 강점 중 하나다. 내장 함수들은

 - 풍부하며
- 매우 유용하며
- 문서화가 잘 되어 있고
- 구문에 변화가 많다. 그래서 문서를 꼭 확인해야 한다.
- 정규식을 인자로 받는 경우도 있고
- 코드 블럭을 인자로 받는 경우도 있으며
- 인자들 사이에 쉼표가 필요없는 경우도 있으며
- 임의의 숫자의 쉼표로 구분된 인자를 받을 수 있는 경우도 있고 아닌 경우도 있으며
- 필요한 인자보다 더 적은 수의 인자가 들어온 경우 적절한 인자로 채우는 경우도 있다
- 애매모호한 경우가 아니라면 대체로 인자를 괄호로 묶을 필요는 없다

내장 함수들에 대해 조언하자면 **어떤 것들이 존재하는지 아는 것**이 가장 중요하다는 점이다. 나중에 참조해 볼 수 있도록 문서를 훑어 봐두는 게 좋다. 너무 저수준이거나, 또는 이전에 자주 해봤기에 흔한 경우라 생각되는 작업을 수행한다면 내장 함수에 그 답이 있을 가능성이 있다.

***사용자 정의 서브루틴***
서브루틴은 sub 예약어로 선언한다. 내장 함수들과는 다르게 사용자 정의 서브루틴은 항상 스칼라 리스트를 입력으로 받는다. 입력으로 받는 리스트는 물론 원소가 하나만일 수도 있고 아예 없을 수도 있다. 하나의 스칼라는 원소 하나짜리 리스트로 받는다. N개 요소의 해시는 2N 원소의 리스트로 받는다.

괄호는 옵션이지만, 서브루틴은 비록 인자가 없을지라도 괄호를 이용해서 호출하는 게 좋다. 서브루틴 호출이라는 것을 명확히 할 수 있기 때문이다.

서브루틴 안에서는 인자는 내장 배열 변수 `@_`로 접근할 수 있다.
```
sub hyphenate {
    # 배열의 첫째 원소를 추출하고 나머지는 무시한다
    my $word = shift @_;

    # An overly clever list comprehension
    $word = join "-", map { substr $word, $_, 1 } (0 .. (length $word) - 1);
    return $word;
}

print hyphenate("exterminate"); # "e-x-t-e-r-m-i-n-a-t-e"
```
**인자들을 풀어내기(unpacking arguments)**

`@_`에서 인자를 꺼내려면 여러 방법을 쓸 수 있지만 그중에서도 더 좋은 방법은 있다.

아래 예제의`left_pad`는 주어진 채움 문자로 필요한 길이만큼 문자열을 채운다. (여기서 x 함수는 문자열을 주어진 숫자만큼 복사해서 합친다.) (주의점: 간단하게 하기 위해서 예제의 서브루틴들은 기본적인 에러 검사를 뺐다. 이를테면, 채움 문자는 한개여야 한다든지, 지정한 길이는 대상 문자열보다 길어야 한다든지, 필요한 인수가 모두 들어와야 한다든지 하는 것들이다.)

`left_pad`는 보통 다음과 같은 식으로 호출한다:
```
print left_pad("hello", 10, "+"); # "+++++hello"
```
1. 어떤 사람들은 인자를 받지 않고 @_를 그대로 사용한다. 이런 방법은 지저분해서 권장하지 않는 방식이다.
```
sub left_pad {
    my $newString = ($_[2] x ($_[1] - length $_[0])) . $_[0];
    return $newString;
}
```
2. 인덱스로 접근해서 값을 꺼내는 방식은 조금은 낫다:
```
sub left_pad {
    my $oldString = $_[0];
    my $width     = $_[1];
    my $padChar   = $_[2];
    my $newString = ($padChar x ($width - length $oldString)) . $oldString;
    return $newString;
}
```
3. 인자가 4개 이내면 `shift`를 이용하여 @_에서 값을 꺼내는 방법이 추천된다.
```
sub left_pad {
    my $oldString = shift @_;
    my $width     = shift @_;
    my $padChar   = shift @_;
    my $newString = ($padChar x ($width - length $oldString)) . $oldString;
    return $newString;
}
```
`shift` 함수에 아무런 배열이 주어지지 않으면 묵시적으로 @_ 를 대상으로 작업을 한다. 아래와 같은 방식은 아주 자주 나온다:
```
sub left_pad {
    my $oldString = shift;
    my $width     = shift;
    my $padChar   = shift;
    my $newString = ($padChar x ($width - length $oldString)) . $oldString;
    return $newString;
}
```
4개를 넘어가게 되면 어디서 무엇을 대입하였는지 추적하기가 힘들어진다.

4. 스칼라 대입문을 이용하여 한꺼번에 @_를 풀 수 있다. 이것도 4개까지는 괜찮다.
```
sub left_pad {
    my ($oldString, $width, $padChar) = @_;
    my $newString = ($padChar x ($width - length $oldString)) . $oldString;
    return $newString;
}
```
5. 인자가 아주 많거나, 또는 인자들 중 몇몇 인자들은 부가적인 경우 가장 좋은 방법은 서브루틴을 해시 형태의 인자로 호출하게 하는 것이다. 이럴 경우에는 @_를 해시로 변환한다. 예제의 서브루틴 호출은 아래와 같은 식으로 바뀐다:
```
print left_pad("oldString" => "pod", "width" => 10, "padChar" => "+");
```
그리고 서브루틴은 아래와 같은 식이다:
```
sub left_pad {
    my %args = @_;
    my $newString = ($args{"padChar"} x ($args{"width"} - length $args{"oldString"})) . $args{"oldString"};
    return $newString;
}
```
**결과 반환하기**

펄의 다른 표현식과 마찬가지로 서브루틴 호출도 문맥에 따라 다르게 동작하게 할 수 있다. `wantarray`(사실 `wantlist`로 이름짓는 게 더 나았으련만) 함수를 이용하여 서브루틴이 평가되는 문맥을 감지해서 문맥에 적당한 결과값을 반환하게 할 수 있다:
```
sub contextualSubroutine {
    # 호출하는 측이 리스트를 요구하므로 리스트를 반환
    return ("Everest", "K2", "Etna") if wantarray;

    # 호출하는 측이 스칼라를 요구. 스칼라를 반환
    return 3;
}

my @array = contextualSubroutine();
print @array; # "EverestK2Etna"

my $scalar = contextualSubroutine();
print $scalar; # "3"
```
시스템 콜
------
이미 알고 있는 사람도 있겠지만, 윈도우즈나 리눅스 시스템에서는(그리고 아마도 거의 모든 다른 시스템에서 그럴꺼라 생각하는데), 모든 프로세스가 끝날 때 16비트의 상태 워드(status word)를 내놓는다. 상위 8비트는 0~255사이의 값을 갖는 리턴코드(return code)를 이루는데 보통 0은 성공적으로 실행했음을 나타내고 그외의 값들은 여러가지 실패 상황을 나타낸다. 나머지 8비트는 자주 사용되지 않는데 - 그것은 코어 덤프(core dump)나 시그널(signal) death와 같은 실패 상황(mode of failure)을 반영한다.

펄 스크립트에서 exit 함수를 사용하면 원하는 리턴 코드로 스크립트를 끝낼 수 있다.

펄에는 한번의 호출로 자식 프로세스를 생성하고 그 프로세스가 끝날 때까지 현재 스크립트가 기다리게 하는 방법이 여러가지 있다. 어떤 방법을 쓰든 자식 프로세스가 끝났으면 그 상태 워드는 내장 스칼라 변수인 $?에 저장된다. 리턴 코드는 이 16비트 변수의 상위 8비트를 `$? >> 8` 같은 방식으로 뽑아내서 얻을 수 있다.

system 함수는 다른 프로그램을 주어진 인자로 실행시킨다. 이 함수의 반환값은 `$?`에 저장된 값과 같다.
```
my $rc = system "perl", "anotherscript.pl", "foo", "bar", "baz";
$rc >>= 8;
print $rc; # "37"
```
역따옴표 ````를 쓰면 명령행에서 내리는 명령어를 실행시키고 그 명령의 표준 출력 결과를 받을 수 있다. 스칼라 문맥에서는 결과가 하나의 문자열로 반환되고, 리스트 문맥에서는 결과가 줄단위로 나뉜 문자열 배열이 반환된다.
```
my $text = `perl anotherscript.pl foo bar baz`;
print $text; # "foobarbaz"
```
여기서 anotherscript.pl은 다음과 같다.
```
use strict;
use warnings;

print @ARGV;
exit 37;
```
파일과 파일 핸들
------
스칼라 변수에는 숫자/문자열/참조/undef 외에 파일 핸들(file handle)도 대입할 수 있다. 파일 핸들이란 특정 파일의 특정 위치를 참조하고 있는 것이라 할 수 있다.

open 함수를 이용하여 스칼라 변수를 파일 핸들로 만들 수 있다. `open` 함수를 호출할 때는 반드시 모드를 지정해야 한다. `<` 모드로 파일을 읽기 위해 연다:
```
my $f = "text.txt";
my $result = open my $fh, "<", $f;

if(!$result) {
    die "Couldn't open '".$f."' for reading because: ".$!;
}
```
호출이 성공하면 `open`은 참값을 반환한다. 만약 실패하면 false 값을 반환하고 에러 메시지는 내장 변수 `$!`에 담겨진다. 위 예제에서 보는 바와 같이 `open` 함수 호출이 성공했는지 확인하는 게 좋다. 확인 과정이 좀 지루하기 때문에 많이들 다음과 같이 줄여쓴다.
```
open(my $fh, "<", $f) || die "Couldn't open '".$f."' for reading because: ".$!;
```
여기에선 `open`을 호출할 때 괄호를 사용해야만 한다.

파일 핸들로부터 한줄을 읽으려면 readline 내장 함수를 이용한다. `readline`은 줄끝 문자를 포함한 완전한 한줄의 문자열을 반환하며 파일 끝에 이르면 `undef`를 반환한다.
```
while(1) {
    my $line = readline $fh;
    last unless defined $line;
    # process the line...
}
```
맨 끝에 붙은 줄끝 문자를 잘라내려면 chomp 함수를 사용한다.
```
chomp $line;
```
`chomp`는 변수를 직접 변경한다. `$line = chomp $line` 식으로 사용하면 아마 다른 결과를 얻게 될 것이다.

eof를 사용하여 파일 끝인지 검사할 수 있다.
```
while(!eof $fh) {
    my $line = readline $fh;
    # process $line...
}
```
그러나 `while (my $line = readline $fh)`와 같은 식으로 쓰려면 조심해야 한다. 왜냐하면 `$line` 이 "0" 이라면 반복문은 미처 파일 끝에 다다르기도 전에 끝나버리고 말게 될 것이기 때문이다. 요런 방식으로 사용하기 위해 펄에는 `<>` 연산자가 있다. 이 연산자는 `readline` 함수를 좀더 안전하게 사용할 수 있도록 감싼 것이다. 아래와 같은 코드는 매우 자주 사용되며 완전히 안전한 코드이다:
```
while(my $line = <$fh>) {
    # process $line...
}
```
그리고 더욱 줄이면
```
while(<$fh>) {
    # process $_...
}
```
파일에 쓰려면 다른 모드로 열어야 한다. `>` 모드는 그 파일에 쓰기 위해 연다는 것을 알린다. (`>` 를 쓰면 이미 있는 파일 내용은 지워진다. 이미 존재하는 파일에 내용을 추가하기 위해서라면 `>>` 모드를 이용해야 한다.) 그리고 `print` 함수의 0번째 인자로 그 파일 핸들을 지정하면 된다.
```
open(my $fh2, ">", $f) || die "Couldn't open '".$f."' for writing because: ".$!;
print $fh2 "The eagles have left the nest";
```
`$fh2` 파일 핸들과 그 다음 인자 사이에 쉼표는 없다.

파일 핸들은 변수의 효력 범위가 지나서 변수가 소멸될 때 닫힌다. 직접 닫을 수도 있다:
```
close $fh2;
close $fh;
```
전역 상수로 세개의 파일 핸들이 있다. `STDIN`, `STDOUT`, `STDERR`가 그것이다. 이들은 스크립트가 시작될 때 자동적으로 열린다. 사용자의 입력을 한줄 읽으려면:
```
my $line = <STDIN>;
```
사용자가 엔터키를 누를 때까지 기다리려면:
```
<STDIN>;
```
`<>`과 같이 파일 핸들을 지정하지 않고 사용하면 `STDIN`으로부터 읽거나, 또는 펄 스크립트에 인자로 주어진 파일로부터 읽는다.

이미 보았듯이 `print`는 파일 핸들이 지정되지 않으면 `STDOUT`에 출력한다.

**파일 검사**

`-e` 내장 함수는 인자로 주어진 이름의 파일이 있는지 검사한다.

```
print "what" unless -e "/usr/bin/perl";
```
`-d`는 디렉터리인지 검사한다. `-f`는 일반 파일인지 검사한다.

이들은 `-X` 형태의 여러 함수들 중 하나이다. 여기서 X는 소문자 또는 대문자 문자이다. 이러한 함수들을 통들어 파일 검사 함수라 한다. 특징적으로 이들은 '-' 문자로 시작한다. 구글 검색에서 - 문자는 검색 결과에서 제외할 패턴을 의미한다. 그래서 파일 검사 함수는 구글 검색이 힘들다. "perl file test"와 같은 식으로 검색해야 결과가 나올 것이다.

정규식
------
정규식은 펄 외에도 다른 많은 언어와 툴에 등장한다. 펄의 핵심 정규식 구문은 다른 것들과 기본적으로 같지만 완전한 정규식 기능은 무서울 정도로 복잡하고 이해하기 어렵다. 가능한 한 이런 복잡한 구문은 피하라고 말해주고 싶다.

`=~ m//` 을 이용하여 매치 연산을 할 수 있다. 스칼라 문맥에서 `=~ m//`은 성공하면 참을 실패하면 거짓을 반환한다.
```
my $string = "Hello world";
if($string =~ m/(\w+)\s+(\w+)/) {
    print "success";
}
```
괄호는 하부 매치를 수행한다. 매치가 성공하면 하부 매치는 `$1, $2, $3, ...` 등의 내장 변수에 들어간다:
```
print $1; # "Hello"
print $2; # "world"
```
리스트 문맥에서는 `=~ m//`는 `$1, $2, ...` 등등을 리스트로 반환한다.
```
my $string = "colourless green ideas sleep furiously";
my @matches = $string =~ m/(\w+)\s+((\w+)\s+(\w+))\s+(\w+)\s+(\w+)/;

print join ", ", map { "'".$_."'" } @matches;
# prints "'colourless', 'green ideas', 'green', 'ideas', 'sleep', 'furiously'"
```
바꾸기 연산은 `=~ s///`를 이용하여 수행한다.
```
my $string = "Good morning world";
$string =~ s/world/Vietnam/;
print $string; # "Good morning Vietnam"
```
`$string`의 내용이 어떻게 바뀌었는지 주의해라. `=~ ᅟs///`의 왼쪽에 스칼라 변수가 있어야 한다. 문자열 리터럴이 있으면 에러가 발생한다.

`/g` 플래그는 "그룹 매치"를 의미한다.

스칼라 문맥에서는 `=~ m//g` 연산은 이전 매치된 다음 부분부터 매치를 수행하여 성공하면 참을, 실패하면 거짓을 반환한다. 똑같이 `$1` 등등으로 값을 얻을 수 있다. 예를 들어보면:
```
my $string = "a tonne of feathers or a tonne of bricks";
while($string =~ m/(\w+)/g) {
  print "'".$1."'\n";
}
```
리스트 문맥에서는 =~ m//g는 한꺼번에 모든 일치된 결과를 반환한다
```
my @matches = $string =~ m/(\w+)/g;
print join ", ", map { "'".$_."'" } @matches;
```
`=~ s///g`는 전체를 뒤져 찾아/바꾸기를 수행하고 일치된 숫자를 반환한다. 다음은 모든 모음 문자를 "r"로 바꾸는 예이다.
```
# /g 없이 한번 실행.
$string =~ s/[aeiou]/r/;
print $string; # "r tonne of feathers or a tonne of bricks"

# 다시 또 한번 실행.
$string =~ s/[aeiou]/r/;
print $string; # "r trnne of feathers or a tonne of bricks"

# /g를 이용하여 나머지 모두를 바꾼다.
$string =~ s/[aeiou]/r/g;
print $string, "\n"; # "r trnnr rf frrthrrs rr r trnnr rf brrcks"
```
`/i` 플래그는 매치와 치환을 대소문자 구별없이 수행한다.

`/x` 플래그를 쓰면 정규식에 공백 문자(와 줄넘김)와 주석을 포함할 수 있다.
```
"Hello world" =~ m/
  (\w+) # one or more word characters
  [ ]   # single literal space, stored inside a character class
  world # literal "world"
/x;

# returns true
````
모듈과 패키지
------
펄에서 모듈과 패키지는 다른 것이다.

**모듈**

모듈이란 `.pm` 확장자를 가진 파일로 다른 펄 파일(스크립트이거나 모듈)에 포함되는 파일을 말한다. 모듈은 `.pl` 펄 스크립트와 완전히 똑같은 구문을 가진 텍스트 파일이다. 예를 들어 `C:\foo\bar\baz\Demo\StringUtils.pm` 또는 `/foo/bar/baz/Demo/StringUtils.pm` 위치에 있는 다음 파일일 수 있다:
```
use strict;
use warnings;

sub zombify {
    my $word = shift @_;
    $word =~ s/[aeiou]/r/g;
    return $word;
}

return 1;
```
모듈은 로드될 때 위에서부터 아래로 실행해가기 때문에 맨 마지막에 성공적으로 로드되었음을 알리기 위해 true 값을 반환해야 한다.

펄 인터프리터가 모듈을 찾을 수 있도록 모듈을 포함하고 있는 디렉터리는 `PERL5LIB` 환경변수에 들어가 있어야 한다. 모듈을 포함하고 있는 디렉터리여야지, 모듈 파일의 패스(Path)나 모듈의 디렉터리여서는 안된다:
```
set PERL5LIB=C:\foo\bar\baz;%PERL5LIB%
```
또는

export PERL5LIB=/foo/bar/baz:$PERL5LIB
펄 모듈을 만들었고, perl이 그것을 어디서 찾을 수 있는지 안다면 require 함수를 이용하여 펄 스크립트 안에서 모듈을 찾아 실행시킬 수 있다. 예를 들어 require Demo::StringUtils 를 호출하였다면 펄 인터프리터는 PERL5LIB 변수에 들어있는 디렉터리를 차례로 검사하여 Demo/StringUtils.pm 파일이 있는지를 찾아낸다. 모듈이 실행되면 그 안에 정의된 서브루틴은 메인 스크립트에서 바로 사용할 수 있게 된다. 예를 들어 메인 스크립트가 main.pl이고 내용이 다음과 같다고 하면:

use strict;
use warnings;

require Demo::StringUtils;

print zombify("i want brains"); # "r wrnt brrrns"
여기서 ::을 디렉터리 구분자로 사용했음을 주의하라.

여기서 문제가 생긴다. 만약 main.pl 에 require 문이 여러 개 있고, 또 각각의 모듈에도 또 require가 있다면 원래의 zombify() 서브루틴이 어디에 있는지 찾아내가기 점점 어려워질 것이다. 이 문제는 패키지를 사용해서 해결한다.

**패키지**

*패키지*는 서브루틴이 정의될 수 있는 네임스페이스이다. 어떤 서브루틴이든 항상 현재의 패키지 안에서 묵시적으로 선언된다. 스크립트가 시작될 때는 `main` 패키지 안이다. 패키지를 바꾸는 것은 package 내장 함수를 이용하면 된다:
```
use strict;
use warnings;

sub subroutine {
    print "universe";
}

package Food::Potatoes;

# 이름 충돌이 없다:
sub subroutine {
    print "kingedward";
}
```
여기서 `::`을 이름공간 구분자로 사용한 것에 주의해라.

서브루틴을 호출할 때마다 현재 패키지 안에 있는 서브루틴을 묵시적으로 호출하는 것이다. 그런데 패키지를 명시할 수도 있다. 만약 위의 스크립트에 이어서 아래와 같이 진행하면:
```
subroutine();                 # "kingedward"
main::subroutine();           # "universe"
Food::Potatoes::subroutine(); # "kingedward"
```
그래서 위 예제의 논리적인 귀결은 `C:\foo\bar\baz\Demo\StringUtils.pm` 또는 `/foo/bar/baz/Demo/StringUtils.pm` 위치에 있는 파일은 아래와 같이 하고:
```
use strict;
use warnings;

package Demo::StringUtils;

sub zombify {
    my $word = shift @_;
    $word =~ s/[aeiou]/r/g;
    return $word;
}

return 1;
```
`main.pl` 파일은 아래와 같이 하는 것이다:
```
use strict;
use warnings;

require Demo::StringUtils;

print Demo::StringUtils::zombify("i want brains"); # "r wrnt brrrns"
```
이제 주의해서 다음 사항을 살펴보자.

패키지와 모듈은 펄 프로그래밍 언어에서는 완전히 서로 관련없는 기능이다. 그런데 둘 다 더블 콜론을 구분자로 이용하는 점이 사람들을 헷갈리게 만드는 것이다. 하나의 스크립트 또는 모듈 안에서 여러 번 패키지를 바꾸는 것이 가능하다. 마찬가지로 다른 여러 파일의 여러 다른 위치에서 같은 패키지 선언을 사용하는 것도 가능하다. `require Foo::Bar`를 호출하는 것은 `package Foo::Bar` 선언이 있는 파일을 찾거나 로드하는 것도 아니다. 더더욱 `Foo::Bar` 네임스페이스에 있는 서브루틴들을 로드할 필요도 없다. `require Foo::Bar`는 단지 `Foo/Bar.pm` 파일을 로드할 뿐이다. 그리고 그 파일에는 어떤 패키지 선언도 있을 수 있다. `package Baz::Qux` 가 있어도 되고 그 밖에 어떤 이상한 게 있어도 된다.

마찬가지로 `Baz::Qux::processThis()` 라는 서브루틴을 호출할 때 이 서브루틴은 `Baz/Qux.pm` 파일에서 선언되어 있을 필요가 없다. 이 서브루틴은 그야말로 아무데서나 선언할 수 있다.

이 두가지 개념을 분리해 놓은 점이 펄의 가장 바보같은 특징 중 하나이다. 그리고 이 둘을 서로 분리된 개념으로 다룰 때 항상 혼돈스럽고 사람들을 열받게 만드는 코드를 낳았다. 다행스럽게도 대부분의 펄 프로그래머들은 다음 두 법칙을 준수한다:

1. **펄 스크립트(.pl 파일)에는 `package` 선언을 두지 않는다.**
2. **펄 모듈(.pm 파일)에는 이름과 위치가 일치하는 하나의 `package` 선언만 둔다. 즉 `Demo/StringUtils.pm` 모듈은 반드시 `package Demo::StringUtils`로 시작한다.**

그래서, 실제로는 믿을만한 서드 파티가 만든 "패키지"와 "모듈"은 서로 호환해서 참조할 수 있다. 하지만 이것을 당연시하면 안된다. 어떤 미친 인간이 만든 코드를 볼 날도 있을 수 있기 때문이다.

객체 지향 펄
------
펄은 위대한 객체 지향 언어는 아니다. 펄의 객체 지향 기능들은 중간에 이식된 것들이다. 그래서 다음과 같은 특징이 있다:

- 객체(object)는 단순한 참조이다. 다만 참조하고 있는 것이 어떤 클래스(class)에 속하는지 알 수 있다는 점이 일반 참조와 구별되는 점이다. 참조하고 있는 개체가 어느 클래스에 속할지 정하기 위해 bless를 사용한다. 참조가 참조하고 있는 것이 어떤 클래스인지 알아내기 위해 ref를 사용한다.

- 메서드(method)는 객체(또는 클래스 메서드인 경우 패키지 이름)를 첫번째 인자로 갖는 서브루틴이다. 객체의 메서드는 `$obj->method();` 식으로 호출한다. 클래스 메서드는 `Package::Name->method()` 식이다.

- 클래스(class)는 메서드를 포함하게 된 패키지이다.

예제를 보면 확실히 알 수 있다. 예제 모듈 `Animal.pm` 은 다음과 같은 `Animal` 클래스를 포함하고 있다.
```
use strict;
use warnings;

package Animal;

sub eat {
    # First argument is always the object to act upon.
    my $self = shift @_;

    foreach my $food ( @_ ) {
        if($self->can_eat($food)) {
            print "Eating ", $food;
        } else {
            print "Can't eat ", $food;
        }
    }
}

# Animal은 무엇이든지 먹을(eat) 수 있다고 하자.
sub can_eat {
    return 1;
}

return 1;
```
그리고 이 클래스는 다음과 같이 사용할 수 있다:
```
require Animal;

my $animal = {
    "legs"   => 4,
    "colour" => "brown",
};                       # $animal 은 그냥 해시 참조
print ref $animal;       # "HASH"
bless $animal, "Animal"; # 이제 "Animal" 클래스의 객체가 되었다
print ref $animal;       # "Animal"
```
주의: 문자 그대로 어떤 참조든지 아무 클래스로도 *bless* 될 수 있다. (1) 참조된 객체가 이 클래스의 인스턴스로 실제 사용될 수 있고 (2) 해당 클래스가 존재하고 로드되게 하는 것은 당신에게 달려있다.

이 객체는 여전히 원래의 해시처럼 사용할 수 있다:
```
print "Animal has ", $animal->{"legs"}, " leg(s)";
```
하지만 이제 -> 연산자를 사용하여 객체의 메서드를 호출할 수 있다.
```
$animal->eat("insects", "curry", "eucalyptus");
```
이 메서드 호출은 `Animal::eat($animal, "insects", "curry", "eucalyptus")`와 같다.

**생성자(constructor)**
생성자는 새 객체를 반환하는 클래스 메서드이다. 원하는대로 선언하면 된다. 이름도 어떤 이름이든 사용할 수 있다. 클래스 메서드는 첫번째 인자가 객체가 아니고 클래스 이름이다. 예제의 경우는 "Animal"이다:
```
use strict;
use warnings;

package Animal;

sub new {
    my $class = shift @_;
    return bless { "legs" => 4, "colour" => "brown" }, $class;
}

# ...etc.
```
그러면 다음과 같이 사용할 수 있다.
```
my $animal = Animal->new();
```
**상속(inheritance)**

부모 클래스로부터 상속하려면 `use parent` 구문을 사용한다. `Animal` 클래스를 상속하여 `Koala.pm`에 위치한 `Koala` 클래스를 만드는 경우를 보자:
```
use strict;
use warnings;

package Koala;

# Animal을 상속
use parent ("Animal");

# 메서드 오버라이드
sub can_eat {
    my $self = shift @_; # 이 변수를 사용하진 않지만 받아 놓는다
    my $food = shift @_;
    return $food eq "eucalyptus";
}

return 1;
```
사용하는 코드는 다음과 같을 수 있다:
```
use strict;
use warnings;

require Koala;

my $koala = Koala->new();
```
여기 마지막 메서드 호출에서 `Koala::eat($koala, "insects", "curry", "eucalyptus")`를 호출하려 한다. 하지만 `Koala` 패키지에는 `eat()` 서브루틴이 없다. 그러나 `Koala`는 `Animal`을 부모 클래스로 가졌기 때문에 펄 인터프리터는 대신 `Animal::eat($koala, "insects", "curry", "eucalyptus")` 를 호출하고, 이는 작동한다. `Koala.pm`에 의해 `Animal` 클래스가 어떻게 자동적으로 로드되는지도 주의해라.

`use parent`는 부모 클래스의 리스트를 받을 수 있으므로, 펄에서는 장점과 더불어 골치아픈 점도 함께 가지고 있는 다중 상속을 할 수 있다.

BEGIN 블럭
------
BEGIN 블럭은 `perl`이 이 블럭의 파싱이 끝나면 바로 실행된다. 이것은 심지어 파일의 나머지 부분의 파싱이 채끝나지 않았더라도 실행된다. 실행 시간에는 무시된다.
```
use strict;
use warnings;

print "This gets printed second";

BEGIN {
    print "This gets printed first";
}

print "This gets printed third";
```
`BEGIN` 블럭은 항상 제일 먼저 실행된다. `BEGIN` 블럭이 여러개 있다면(그러지 말기를 바라지만), 컴파일러가 위부터 아래로 만나는 순서대로 실행된다. `BEGIN` 블럭은 스크립트 중간에 위치하더라도 심지어 가장 밑에 있더라도 항상 제일 먼저 실행된다(이것도 하지 말라). **코드의 자연스러운 순서를 복잡하게 만들지 마라. `BEGIN` 블럭은 맨 처음에 둬라!**

`BEGIN` 블럭은 이 블럭의 파싱이 끝나는 즉시 실행된다. 이것이 끝나고 `BEGIN` 블럭 끝부터 파싱이 재개된다. 전체 파일 또는 모듈이 한번 파싱되고 나서야 `BEGIN` 블럭 바깥의 코드가 실행된다.
```
use strict;
use warnings;

print "This 'print' statement gets parsed successfully but never executed";

BEGIN {
    print "This gets printed first";
}

print "This, also, is parsed successfully but never executed";

...because e4h8v3oitv8h4o8gch3o84c3 there is a huge parsing error down here.
```
컴파일 과정 중에 실행되는 것이므로, 조건문 안에 BEGIN 블럭을 두었다하더라도 역시 가장 먼저 실행된다. 심지어 조건이 거짓이라도 실행된다. 조건은 평가되지도 않고 실행되며 실제로 조건은 전혀 실행되지 않을 수도 있다.
```
if(0) {
    BEGIN {
        print "This will definitely get printed";
    }
    print "Even though this won't";
}
```
**조건문 안에 `BEGIN` 블럭을 두지 마라!** 컴파일 과정 중에 조건에 따라 무엇인가를 하고 싶다면 `BEGIN` 블럭 안에 조건문을 두어라:
```
BEGIN {
    if($condition) {
        # etc.
    }
}
```
use
------
이제 패키지, 모듈, 클래스 메서드와 `BEGIN` 블럭을 이해했으므로, 굉장히 자주 보게 되는 use 함수를 살펴보자.

다음의 세 문장은:
```
use Caterpillar ("crawl", "pupate");
use Caterpillar ();
use Caterpillar;
```
각각 아래와 동일하다:
```
BEGIN {
    require Caterpillar;
    Caterpillar->import("crawl", "pupate");
}
BEGIN {
    require Caterpillar;
}
BEGIN {
    require Caterpillar;
    Caterpillar->import();
}
```
- 예제의 순서가 잘못된 게 아니다. 펄이 멍청한 것 뿐이다.
- `use` 호출은 `BEGIN` 블럭이 변장한 것이다. 같은 경고가 적용된다. `use` 문은 언제나 파일 윗부분에 두고 **결코 조건문안에 두지 마라.**
- `import()`는 펄의 내장 함수가 아니다. 이것은 **사용자 정의 클래스 메서드이다.** `Caterpillar` 패키지의 프로그래머는 `import()`를 정의하거나 또는 상속해야만 한다. 그리고 이 메서드는 이론적으로 어떠한 것도 인자로 받을 수 있으며 그 인자로 어떠한 것도 할 수 있다. `use Caterpillar` 는 무엇이든 할 수 있다. 정확히 무슨 일이 일어날지 알려면 `Caterpillar.pm`의 문서를 참조해야 한다.
- `require Caterpillar`는 모듈 `Caterpillar.pm`을 로드하고, `Caterpillar->import()`는 `Caterpillar` 패키지에 선언되어 있는 `import()` 서브루틴을 호출한다는 점을 유의깊게 봐라. 모듈과 패키지가 일치하기를 기원하자!

Exporter
------
`import()` 메서드를 정의하는 방법으로 Exporter 모듈로부터 이 메서드를 상속하는 방법이 가장 흔하게 쓰인다. Exporter는 펄의 핵심 모듈(core module)에 속하며 사실상 펄 프로그래밍 언어의 핵심 특징 중 하나다. `Exporter`의 `import()` 구현에서는 인자 목록은 서브루틴 이름 목록으로 해석된다. 서브루틴이 `import()`되면 그 자신의 패키지에서처럼 현재 패키지에서 사용할 수 있게 된다.

예제를 보면 이해하기 쉽다. `Caterpillar.pm`은 다음과 같다:
```
use strict;
use warnings;

package Caterpillar;

# Inherit from Exporter
use parent ("Exporter");

sub crawl  { print "inch inch";   }
sub eat    { print "chomp chomp"; }
sub pupate { print "bloop bloop"; }

our @EXPORT_OK = ("crawl", "eat");

return 1;
```
패키지 변수인 `@EXPORT_OK`에는 서브루틴 이름 목록이 들어있어야 한다.

다른 코드에서 이 서브루틴을 이름으로 `import()`한다. 보통 `use` 구문을 이용한다:
```
use strict;
use warnings;
use Caterpillar ("crawl");

crawl(); # "inch inch"
```
이 경우, 현재 패키지는 `main`이다. 따라서 `craw()` 호출은 실제로 `main::call()` 호출이다. 그리고 이것은 `import()`에 의해 `Caterpillar::crawl()`에 매핑되어있다.

주의점: `@EXPORT_OK`의 내용과 상관없이 모든 메서드는 패키지 명까지 붙여서 호출하면 호출할 수 있다:
```
use strict;
use warnings;
use Caterpillar (); # no subroutines named, no import() call made

# and yet...
Caterpillar::crawl();  # "inch inch"
Caterpillar::eat();    # "chomp chomp"
Caterpillar::pupate(); # "bloop bloop"
```
펄에는 private 메서드가 없다. 관례적으로 내부적인 목적으로만 쓰일 메서드에는 이름을 "_" 문자 하나 또는 두개로 시작한다.

**@EXPORT**

Exporter 모듈에는 또한 `@EXPORT`라는 패키지 변수가 있다. 이 변수에도 또한 서브루틴 이름들을 저장한다.
```
use strict;
use warnings;

package Caterpillar;

# Inherit from Exporter
use parent ("Exporter");

sub crawl  { print "inch inch";   }
sub eat    { print "chomp chomp"; }
sub pupate { print "bloop bloop"; }

our @EXPORT = ("crawl", "eat", "pupate");

return 1;
```
`@EXPORT` 안에 이름이 있는 서브루틴들은 `import()`가 인자없이 호출되면 export된다. 이제 다음을 보면:
```
use strict;
use warnings;
use Caterpillar; # calls import() with no arguments

crawl();  # "inch inch"
eat();    # "chomp chomp"
pupate(); # "bloop bloop"
```
그렇지만 여기서 우리는 다시 `crawl()`이 어디서 정의되었는지 다른 단서가 없이는 쉽게 알 수 없는 지점에 되돌아왔다. **The moral of this story is twofold:**

1. Exporter를 이용하는 모듈을 만들 때, 서브루틴들을 `@EXPORT`를 사용하여 기본적으로 export하지 않는다. 항상 사용자가 서브루틴을 직접 일일히 지정하여 호출하거나 또는 명시적으로 `import()`하게 한다(즉 `use Caterpillar ("crawl")` 식으로 사용하게 한다. 그럼으로써 `crawl()`의 정의를 보려면 `Caterpillar.pm`을 찾아봐야 한다는 걸 쉽게 알 수 있다.).

2. Exporter를 이용하는 모듈을 사용할 때는 항상 `import()`하고 하는 서브루틴들을 명시적으로 이름을 이용하여 `import()`한다. 어떤 서브루틴도 `import()`하지 않고 일일이 패키지명을 지정하여 호출하고자 한다면, 명시적으로 빈 목록을 넘겨준다: 즉 `use Caterpillar ()` 식으로 쓴다.

그밖의 것들
------
- 핵심 모듈인 Data::Dumper는 임의의 스칼라 변수를 출력할 때 쓸 수 있다. 필수적인 디버그 도구이다.

- 배열을 선언할 때 편하게 쓸 수 있는 qw{} 구문이 있다. 특히 use 문을 쓸 때 자주 볼 수 있다:
```
use Account qw{create open close suspend delete};
```
그밖에 quote-like 연산자가 여럿 있다.

- =~ m//와 =~ s/// 연산에서 슬래시(/) 대신에 괄호 등을 정규식 구분자로 쓸 수 있다. 정규식 안에 슬래시 문자가 많아서 일일히 역슬래시를 이용하여 탈출(escape)시키는 것이 번거로울 때 유용하게 쓸 수 있다. 예로 =~ m{///}은 세개의 슬래시에 매칭되며 =~ s{^https?://}{}는 URL에서 프로토콜 부분을 삭제한다.

- 펄에는 CONSTANTS가 있다. 현재는 권장되지 않지만 항상 권장되지 않았던 건 아니다. Constants는 실제로는 괄호가 생략된 서브루틴 호출이다.

- 때때로 $hash{"key"}가 아니라 $hash{key}처럼 해시키를 둘러싼 따옴표를 생략할 때가 있다. 이 경우는 key가 서브루틴 호출 key()가 아닌 문자열 "key"로 해석되기 때문이다.

- <<EOF과 같이 <<을 구분자로 포맷되지 않은 코드가 둘러싸여 있을 때, "here-doc" 이라는 검색어로 찾아봐라.

- 내장 함수 중 많은 것들이 인자없이 실행할 수 있으며 이때는 $_ 변수를 대상으로 실행된다. 이 사실을 알면 다음 코드들를 이해할 수 있을 것이다:
```
print foreach @array;
```
그리고
```
foreach ( @array ) {
    next unless defined;
}
```
나는 리팩터링할 때 문제를 일으킬 수 있기 때문에 이러한 식의 코드를 싫어한다.

끝.

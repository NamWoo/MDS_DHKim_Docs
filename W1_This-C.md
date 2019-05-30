
[이것이 C언어다] 완독 후 핵심정리 & 독자서평
====
> # Linux 개발환경 구축
 >  ## 1. Linux Setup (Ubuntu 16.04)
  >    ### ~$ sudo apt-get install git
  >    ### ~$ sudo apt-get install vim
  >    ### ~$ sudo apt-get install bulid-essential 
 > ## 2. 에디터
   >   #### vim 사용
   >   #### /.vimrc 파일 내용
   >
   >   ```c
   >    (내용추가예정)   
   >   ```

> # Windows 개발환경 구축
 >  ## 1. Windows Setup (Windows 10)
  > [ **Dev-cpp**](https://sourceforge.net/projects/orwelldevcpp/) (링크) 사용
  > ### 윈도우 유저라도 Git은 사랑입니다 



# 0.INDEX
 * 이것이 C언어다 (서현우님 저서) 학습 후 학습내용 복습겸 정리와 서평 문서입니다.
  ```
    1. 프로그램 만들기 
    2. 상수와 데이터 출력
    3. 변수와 데이터 입력
    4. 연산자
    5. 선택문
    6. 반복문
    7. 함수
    8. 배열
    9. 포인터
    10. 배열과 포인터
    11. 문자
    12. 문자열
    13. 변수의 영역과 데이터 공유
    14. 다차원 배열과 포인터 배열
    15. 응용 포인터
    16. 메모리 동적 할당
    17. 사용자 정의 자료형
    18. 파일입출력
    19. 전처리와 분할 컴파일
    20. DHKim 서평
  ```

# 1.프로그램 만들기 
 
 - 프로그램의 탄생 순서
  ```
    0. edit(Coding)
    1. Preprocess
    2. complie
    3. Link
  ```
 - 추가로 공부해야 할 사항들
  ```
    1. Linker의 역할과 정의, 링크 오브젝트, 링크 스크립트
    2. Makefile 은 뭐하는놈임?
  ```
 - 결론 : 프로그램을 직접 작성하고 실행시켜보는것이 C언어를 배우는 지름길 입니다. (백문이 불여일타)

# 2.상수와 데이터 출력
 - C언어 기본 정리
  ```
    1. <,> (꺽쇠) : 시스템 헤더 파일을 의미
    2. #include : 전처리기 지시자, Preprocess에게 파일 포함을 지시
    3. C언어는 Main() 함수에서 시작한다
    4. Block구분자 {,} : 함수의 영역 표시, 함수의 시작과 끝을 명시
    5. printf() 함수는 콘솔화면에 문자를 출력하며, 가변인자 함수
  ```
 
# 3.변수와 데이터 입력
 - 변수와 상수 그리고 배열에 관한 이야기
  ```
   - 컴퓨터에서 모든 데이터는 0과 1의 조합이다(비트열)
   - 인간은 키보드를 치지만, 컴퓨터는 악속된 비트열의 나열인 ACSII 형식으로 저장함
   - ASCII또한 비트열 종류 중의 하나
   - 변수의 선언이란 메모리공간의 확보
   - 할당연산자 = 에는 엄청난 책임이 따른다 :: 좌항과 우항의 데이터형이 같아야한다. 
   - 다른경우 허용범위 내에서 묵시적 형변환이 일어나며 형변환시 삽입되는 데이터는 변화한다
   - 형변환시의 데이터 변환은 뒤에서 자세히 다루도록 한다.
   - 형 변환이 필요한 경우 코드상에 명시적으로 변환 해 주기를 권함(묵시변환 X)
   - 지역변수는 Stack영역에 거주하며, 초기화해야함.
   - 문자열은 Read-Only공간에 생성됨
   - 전역변수는 초기화시 ()공간에 생성된다
   - 초기화되지 않은 전역변수는 BSS 공간에 생성된다
   - 문자 'A'는 아스키코드값으로 표시되며 65이다. (char type = 65)
   - x86아키텍쳐에서는 특별한 경우가 아니고서는 int(4byte, 32bit)형의 연산이 일 빠르다.
   - Char 배열을 선언하여 문자열을 저장할 때는 경계값 침범에 주의 해야한다.
   - 문자열은 Read-Only 공간에 생성됨
   - 배열은 메모리의 연속된 공간에 생성
   - 예약어는 컴파일러와 사용자간에 특별한 용도로 사용하기로 약속한 단어이며, 사용자가 예약어를 식별자로 사용할 수 없음
   - 배열명의 본질은 주소이며, 주소는 상수이다
   - char배열로 문자열을 받을때, 배열보다 문자열의 길이가 길 경우 프로그램이 중단될 수 있음
  ```
# 4.연산자
 - 연산자 그리고 형 변환(Type Casting)에 관한 이야기

 ```
   - 할당연산자는 좌항과 우항이 서로 같은 형태이어야만한다, 다를경우 형 변환이 발생
   - 연산자는 컴퓨터에게 계산을 지시하는 명령어이다
   - 98p 연산자 우선순위 참조( ^ 연산은 Exclusive-OR 연산 )
   - 증감연산자의 후휘형은 다른 연산자와 사용될때 가장 마지막에(세미콜론 시점에)변경
   - "a=5"의 코드와 "a==5"의 차이를 명확하게 구분(대입연산과 비교연산)
   - 형 변환에는 묵시적 형변환과 명시적 형변환이 존재함
   - 대입연산에서 l-value와 i-value의 형(type)이일치하지 않으면 형 변환 발생
   - 형 변환의 기본 규칙은 크기가 작은 자료형이 큰 자료형으로
   - int 32bit 자료형과 float 32bit 자료형의 경우, float형으로 형변환

 ```
# 5.선택문
 - if문( "if" , "if + else" , "if + elseif + else" ) 3형제
 ```
   - if문은 중괄호를 사용하고 들여쓰기하여 실행문을 명확히 구분하는것이 좋습니다
   - 실행할 문장이 두문장 이상이면 반드시 중괄호로 묶어야 합니다
   - 선행조건이 없는 경우에도 실행 효율을 위해 의도적으로 중첩해 쓸 수 있습니다
   - 분할 정복 기법 -> 비교항목이 많은경우 if문 중첩으로 효율적 프로그래밍 ->> MECE하게 if문을 구성해야함(Mutually exclusive & closely Exhausty) ->> 선행조건이 필요하다고 오해할 소지가 있으며 코드의 가독성이 떨어질 가능성 존재
   - if-else문을 사용할때는 중괄호 사용 : Dangling-else문제를 발생시킬 수 있음
 ```
 - switch case문
  ```
    - Switch 문에서 조건식은 정수식만 사용함
    - break;구문이 없을 시 Case분기 동작하지 않고 하위항목 순차실행됨 (의도적인 break생략도 가능)
  ```
 - 참조 : [ **Dangling-else와 Short-curcuit-Eval 문제** ](https://m.blog.naver.com/PostView.nhn?blogId=satyee&logNo=140128571147&proxyReferer=https%3A%2F%2Fwww.google.com%2F) <-클릭하여 링크로 연결

# 6.반복문
 - 3 가지 형태의 반복문이 존재합니다
 ```
   1. for문
     - 조건식이 참인동안 실행(거짓이 되면 중단)
     - 최초 조건이 거짓이면 한번도 실행되지 않음

   2. while문
     - 조건식이 참인동안 실행(거짓이 되면 중단)
     - 최초 조건이 거짓이면 한번도 실행되지 않음
     - 
   3. do while문
     - 조건식이 참인동안 실행(거짓이 되면 중단)
     - 실행문을 수행한 후 조건을 검사
     - 최소 1번 실행을 보장
       ->> 커널코드에 매크로로 사용됨 
 ```
# 7.함수
 - C언어에서 함수명은 유일합니다 (__weak 키워드는 논 외로 합니다)
 - (번외) C++ 에서는 namespace를 통해 동일한 이름의 함수명을 중복해서 사용할 수 있습니다.
 - 함수는 3가지 형태가 존재하는데 선언/호출/정의 입니다.
 - 함수가 호출될때 Stack영역에 복사되며, parameter또한 복사됩니다.
 - 함수내 선언된 자동변수는(일반변수) 호출시 생성되어 종료시 소멸됩니다.
 - 함수의 선언(Prototype)은 없는경우 컴파일러마다 동작이 다르므로 명시하기를 권합니다
 - 함수포인터 / 함수포인터 변수 배열은 뒤에서 다루겠습니다
# 8.배열
 - 배열이란 동일한 자료형이 메모리상에 연속적으로 나열되어 있는 자료구조 입니다.
 - 배열의 각 저장 공간을 이름과 첨자로 구분합니다.
 - 배열의 첨자는 0부터 시작합니다.
 - 배열은 저장 공간이 연속적으로 할당되며 (메모리에) 배열의 이름은 전체 공간의 이름입니다
 - 배열명은 첫번째 요소의 주소 입니다 (주소 이므로 상수) 
 - 자동변수(지역변수) 배열도 최초 할당 공간에는 쓰래기값이 들어있습니다.(초기화되어 있지 않습니다) 
 - 배열의 초기화중 중괄호를 사용한 초기화는 선언시에만 사용할 수 있습니다.
  ```c
    //<배열의 중괄호초기화는 최초 선언시에만 사용 가능>
    //----------------------------------------
    int arr[3] = {0,1,2} // Good

    //----------------------------------------
    int arr[3];
    arr = {0,1,2}; // Bad
  ```

 - 이외에도 배열은 다양한 방법으로 초기화 할 수 있습니다.
 - 배열의 요소 갯수를 Runtime에 알아내는 Code : sizeof(arr)/sizeof(arr[0])
  ```c
    //<배열의 요소 갯수를 Runtime에 알아내는 Code>
    int arr[5] = {0,1,2,3,4};
    int num_elemnet=0;
    num_elemnet = sizeof(arr)/sizeof(arr[0]);
    pritnf("%d",num_elemnet);
    //===============================
    //결과 : 5
  ```
 - 다른 함수에 배열명(첫쨰요소의 주소)을 전달할때 주의사항으로 다른 함수는 배열의 크기를 알수 없습니다.
 - 컴파일러는 배열의 경계를 검사하지 않습니다.
 - 배열의 경계를 침범하는경우 Runtime Error 를 발생시킬 가능성이 존재합니다.
 - 문자열 또한 Char(캐릭터, 문자)의 배열입니다.
 - C언어에서는 String 자료형을 제공하지 않습니다.
 - NULL문자('\0') 는 문자열의 끝을 나타내는 용도로 사용합니다(표준)
 - Visual C 컴파일러에서는 scanf_s 함수를 사용합니다.
 - But! C언어에 대하여 좀더 깊은 이해를 원하시는 분들은 Linux환경의 GCC 컴파일러 사용을 추천합니다.
 - 



# 9.포인터
 - 포인터의 종류
  ```
  int* ptr [3] : 포인터 배열 변수
  int** ptr [3] : 이중 포인터 배열 변수 
  int (*ptr) [3] : 배열 포인터 변수

  ```
 - [ * ] : 간접참조 연산자 (아스테리스크)
 - [ & ] : 주소 연산자 (엠피센드)
 - 포인터 표현시 정규식 [ %x ]
 - 포인터의 4가지 특징
  ```
   1.포인터 또한 메모리공간에 존재하므로 고유의 주소값을 갖는다.
   2.포인터 변수는 오로지 주소만을 값으로 갖는다.
   3. 포인터 주소는 상수이다 고로 불변한다.
   4.포인터 변수는 간접참조 연산을 통해 데이터에 접근 할 수 있다. 

  ```
 - const int *ptr 과 int const *ptr의 차이
 - 어떠한 경우에도 l-value로 엠피센드 연산자가 올수 없다(지극히 당연)
  ```c
   int *ptr;
   &ptr = @@@... 
   //bad case

  ```
 - malloc과 포인터 -> 동적할당 (Head영역) 사용시 힙영역의 데이터를 접근하기 위해서는 주소가 반드시 필요 함.

# 10.배열과 포인터
 - 포인터의 배열요소 표현식

 -  ubuntu no hangul T.T git test 
# 11.문자
# 12.문자열
# 13.변수의 영역과 데이터 공유
# 14.다차원 배열과 포인터 배열
# 15.응용 포인터
# 16.메모리 동적 할당
# 17.사용자 정의 자료형
# 18.파일입출력
# 19.전처리와 분할 컴파일
# 20.DHKim 서평


















=========================================================

makrdown을 잘 활용합시다!!
# JAVA

- JAVA
  - [JVM 에 대해서, GC의 원리](#jvm-에-대해서-gb의-원리)
  - [Collection](#collection)
  - [Annotation](#1)
    - Reference
  - [Generic](#generic)
  - [Final keyword](#final-keyword)
  - [Overriding vs Overloading](#overriding-vs-overloading)
  - Access Modifier
  - Wrapper class
    - [Autoboxing](#)
  - [Multi-Thread 환경에서의 개발](#멀티-스레드란)
    - Field member
    - 동기화(Synchronized)
    - ThreadLocal
      - Personal Recommendation
  - [추상클래스와 인터페이스의 차이](#인터페이스와-추상클래스를-사용하는-이유)
  - [Enum 이란?](#enum이란)
  - [Enum 활용 & Enum 리스트 가져오기](#enum-활용--enum-리스트-가져오기)
  - [람다란?](#람다란)

## JVM 에 대해서, Gb의 원리

VM은 크케 Class Loader, Excution Engine, Garbage Collector, Runtime Data Area(Memory Area) 로 나누어져 구성되어 있다.

JVM의 기본적인 수행과정

프로그램 실행시 개발자의 의해 작성된 소스코드(.java)는 자바 컴파일러 (javac)를 통해
Byte Code(.class) 파일로 변환 된다.

이렇게 변환 된 Byte Code 파일을 JVM 내부의 Class Loader가 읽어 들여 Runtime Data Area에 저장하게된다.

저장된 코드들을 Excution Engine이 하나의 명령어 단위로 읽어들여 프로그램을 실행하게 되고, 사용이 끝나 더 이상 사용하지 않는 코드들을 Garbage Collector 가 모아서 메모리에서 해제한다.

Class Loader : .class 파일을 가져와서(로드) 이를 Runtime Data Area에 올려주는 역할. 여기올라간 파일을 Excution Engine 에 의해 실행된다.

Runtime Data Area(=memory Area) : JVM 이 프로그램을 수행하기 위해 OS로 부터 할당받은 메모리 공간!

또한 이 메모리 영역은 크게 Method Area,Stack,Heap,PC Register, Native Method Area로 구분 된다.

Method Area -자바 프로그램에서 사용되는 클래스에 대한 정보와 클래스 변수 저장(필드, 메소드, 생성자,...등)
Heap

- new 연산자를 이용해 동적으로 생성된 객체, 배열 등을 저장하는 영역.
  Stack 영역에는 주소가, Heap영역에는 주소에 해당하는 실제 값이 저장된다.
  JVM이 중단되거나 GC가 실행되기 전까지 영구적으로 저장된다.
  여기서 사용이 끝난 객체들을 Garbage Colletor가 모아서 처리하게 된다.
  Stack -프로그램 실행 중 메서드가 호출되면, Stack 영역에 각각의 메서드를 위한 메모리가 할당된다(즉 각 메서드는 하나씩의 Stack을 가지게 된다)
  Stack 영역은 이 메서드들 안에서 사용되어지는 값을 저장하며, 호출된 메서드의 지역변수,매개변수, 리턴 값 및 연산값들을 임시로 저장하는 공간이다. 임시로 저장하기 때문에 사용이 끝나면 Stack 영역에서 해제된다.

---

## JVM 이란?

JVM 이란 JAVA Virtual Machinem 자바 가상 머신의 약자를 따서 줄여 부르는 용어이다.
JVM 역활은 자바 애플리케이션을 클래스 로더를 통해 읽어 들여 자바 API와 함께 실행 하는 것이다.
그리고 JVM은 JAVA와 OS사이에서 중계자 역할을 수행하여 JAVA가 OS에 구애받지 않고 재사용을 가능하게 해준다.
그리고 가장 중요한 메모리관리,Garbage collection 을 수행한다. 그리고 JVM은 스택기반의 가상머신이다.
ARM 아키 텍쳐같은 하드웨어는 레지스터 기반으로 동작하는데에 비해 JVM은 스택 기반으로 동작한다

## 왜 자바 가상 머신을 알아야 하는가?

한정된 메모리를 효율적으로 사용하여 최고의 성능을 내기 위해서가 그 답이 될지 모르겠다. 메모리 효율성을 위해 메모리 구조를 알아야 하기 때문이다. 동일한 기능의 프로그램이더라도 메모리 관리에 따라 성능이 좌우된다.
메모리 관리가 되지 않은 경우 속도저하 현상이나 튕김 현상 등이 일어날 수 있다. 그래서, 알아두면 좋다를 넘어 알아야 하는 것 이다.

## 자바 프로그램 실황과정

1. 프로그램이 실행 되면 JVM은 OS로 부터 이 프로그램이 필요로 하는 메모리르 할당받는다.
   JVM은 이 메모리를 용도에 따라 여러 영역으로 나누어 관리한다.
2. 자바 컴파일러(javac)가 자바 소스코드(.java)를 읽어 들여 자바 바이트코드(.class)로 변환 시킨다.
3. Class Loader를 통해 class 파일들을 JVM으로 로딩한다.
4. 로딩된 class 파일들은 Execution engine 을 통해 해석된다.
5. 해석된 바이트 코드는 Runtime Data Areas 에 배치되어 실질적인 수행이 이루어 지게 된다.
   이러한 실행과정 속에서 JVM은 필요에 따라 Thread Synchroniztion 과 GC같은 관리 작업을 수행한다.

![image](https://user-images.githubusercontent.com/104490310/193031201-d85b5e8f-0c90-4656-9f85-6d8c19f00947.png)

## **JVM 구성**

### **Class Loader(클래스 로더)**

JVM 내로 클래스(.class파일)을 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈이다.Runtime 시에 동적으로 클래스를 로드한다. jar 파일 내 저장된 클래스들을 JVM위에 탑재하고 사용하지 않는 클래스들은 메모리에서 삭제 한다.(컴파일러 역활) 자바는 동적 코드, 컴파일 타임이 아니라 런타임에 참조한다. 즉,
클래스를 처음으로 참조할때 , 해당 클래스를 로드하고 링크한다는 것이다. 그 역할을 클래스 로더가 수행한다.

### **Execution Engine(실행 엔진)**

클래스를 실행시키는 역할이다. 클래스 로더가 JVM내의 런타임 데이터 영역에 바이트 코드를 배치시키고, 이것은 실행엔진에 의해 실행된다. 자바 바이트코드는 기계가 바로 수행할 수이 ㅆ는 언어보다는 비교적 인간이 이 보기 편한 형태로 기술 된 것이다. 그래서 실행 엔진은 이와 같은 바이트코드를 실제로 JVM 내부에서 기계가 실행할 수 있는 형태로 변경한다. 이 때 두 가지 방식을 사용하게 된다.

### **Interpreter(인터프리터)**

실행 엔진은 자바 바이트 코드를 명령어 단위로 읽어서 실행한다. 하지만 입 방식은 인터프리터 언어의 단점을 그대로 가지고 있다. 한 줄 씩 수행하기 떄문에 느리다는 것이다.

### **JIT(Just-In-Time)**

인터프리터 방식의 단점을 보완하기 위해 도입된 JIT 컴파일러이다. 인터프리터 방식으로 실행하다가 적절한 시점에 바이트코드 전체를 컴파일하여 네이티브 코드로 변경하고, 이후에는 해당 더 이상 인터프리팅 하지 않고
네이티브 코드로 직접 실행하는 방식이다. 네이티브 코드는 캐시에 보관하기 떄문에 한번 컴파일된 코드는 빠르게 수행하게 된다. 물론 JIT 컴파일러가 컴파일하는 과정은 바이트코드를 인터프리팅하는 것보다 훨씬 오래걸리므로 한 번만 실행되는 코드라면 컴파일 하지 않고 인터프리팅하는 것이 유리하다. 따라서 JIT 컴파일러를 사용하는 JVM들은 내부적으로 해당 메서드가 얼마나 자주 수행되는지 체크하고, 일정 정도를 넘을 때에만 컴파일을 수행한다.

### **Garbage collector**

GC를 수행하는 모듈(쓰레드)이 있다.

> ## Runtime Data Area
>
> 프로그램을 수행하기 위해 OS에서 할당받은 메모리 공간
> ![image](https://user-images.githubusercontent.com/104490310/193033208-63ef217f-cd53-49a2-9156-ce2fd6779b20.png)

### **1)PC Register**

Thread가 시작될 때 생성되며 생성될때마다 생성되는 공간으로 스레드마다 하나씩 존재한다. Thread가 어떤
부분을 어떤 명령으로 실행해야할지에 대한 기록을 하는 부분으로 현재 수행준인 JVM의 명령의 주소를 갖는다.

### **2)JVM 스택 영역**

프로그램 실행과정에서 임시로 할당되었다가 메소드를 빠져나가면 바로 소멸되는 특성의 데이터를 저장하기 위한
영역이다. 각종 형태의 변수나 임시데이터, 스레드나 메소드의 정보를 저장한다. 메소드 호출 시마다 각각의 스택 프레임(그 메서드만을 위한공간)이 생성된다. 메서드 수행이 끝나면 프레임 별로 삭제를 한다. 메소드 안에서 사용되는 값들(local variable)을 저장한다.또 호출된 메소드의 매개변수, 지역변수, 리턴 값 및 연산 시일어나는 값들을 저장한다.

### **3)Native method stack**

자바 프로그램이 컴파일되어 생성되는 바이트 코드가 아닌 실제 실행할 수 있는 기계어로 작성된 프로그램을 실행 시키는 영역이다.JAVA가 아닌 다른 언어로 작성된 코드를 위한 공간이다.JAVA Native Interface를 통해 바이트 코드로 전환하여 저장하게 된다. 일반 프로그램처럼 커널이 스택을 잡아 독자적으로 프로그램을 실행시키는 역영이다. 이 부분을 통해 C code를 실행히켜 Kernel에 접글할 수 있다.

### **4)Method Area (=Class area =Static area)**

클래스정보를 처음 메모리 공간에 올릴 떄 초기화되는 대상을 저장하기 위한 메모리 공간. 올라가게 되는 메소드의 바이트 코드는 프로그램의 흐름을 구성하는 바이트 코드이다. 자바 프로그램은 main 메소드의 호출에서 부터 계속된 메소드의 호출로 흐름을 이어가기 때문이다. 대부분 인스턴스의 생성도 메소드 내에서 명령하고 호출한다. 사실상 컴파일 된 바이트코드의 대부분이 메소드 바이트코드이기 떄문에 거의 모든 바이트코드가 올라간다고 봐도 상관없다. 이 공간에는 Runtim Constant Pool 이라는 별도의 관리 영역도 함께 존재한다. 이는 상수자료형을 저 장하고 참조하고 중복을 막는 역할을 수행한다.

올라가는 정보의 종류

**1). Field Informaiton**

멤버변수의 이름, 데이터 타입, 접근 제어자에 대한 정보

**2). Method Information**

메소드의 이름,리턴 타입,매개변수,접근제어자에 대한 정보

**3). Type Information**

class 인지 interface인지의 여부저장 /Type의 속성,전체이름 super class의 전체이름 (interface이거나 object 인 경우 제외)

Method Area는 클래스 데이터를 위한 공간이라면
Heap 영역이 객체를 위한 공간이다.
Heap과 마찬가지로 Gc의 관리 대상에 포함된다.

### 5)Heap(힙 영역)

객체를 저장하는 가상 메모리 공간이다. new 연산자로 생성된 객체와 배열을 저장하낟. 물론 class area 영역에 올라온 클래스 들만 객체로 생성할 수 있다. 힙은 세부분으로 나눌 수 있다.

![image](https://user-images.githubusercontent.com/104490310/193035384-55b429d2-c83a-431b-a0db-d0429c29b7d3.png)

**Permanent Generation**

생생된 객체들의 정보의 주소값이 저장되는 공간이다. Class loader에 의해 load 되는 Class,Method 등에 대한 Meta 정보가 저장되는 영역이고 JVM에 의해 사용된다. Reflection 을 사용하여 동적으로 클래스가 로딩되는 경우에 사용된다. 내부적으로 Reflection 기능을 자주 사용하는 Spring Freamwork를 이용할 경우 이영역에 대한 교려가 필요하다

**New/Young 영역**

-Eden : 객체들이 최초로 생성되는 공간

-Survioer 0 / 1: Eden에서 참조되는 객체들이 저장되는 공간

**Old 영역**
New area에서 일정 시간 참조되고 있는, 살아남은 객체들이 저장되는 공간 Eden 영역에 객체가 가득차게 되면 첫번쨰GC(minor GC)가 발생한다. Eden영역에 있는 값들을 Survivor 1 영역에 복사하고 이 영역을 제외한 나머지 영역의 객체를 삭제 한다.

인스턴스는 소멸 방법과 소멸 시점이 지 변수와는 다르기에 힙이라는 별도의 영역에 할당된다. 자바 가상 머신은 매루 합리적으로 인스턴스를 소멸시킨다. 더이상 인스턴스의 존재 이유가 없을때 소멸시킨다.

<br/>
<br/>
<br/>
<br/>
<br/>

# 가비지 컬렉션은 어떤 원리로 소멸시킬 대상을 선정하는가?

## 가비지 컬렉션,GC(Garbage Collection)

### Minor GC

새로 생성된 대분의 객체(Instance)는 Eden 영역에 위치한다. Eden 영역에서 GC가 한번 발생한 후 살아남은 객체는 Survior 영역 중 하나로 이동한다. 이 과정을 반복하다가 계속해서 살아남아 있는 객체는 일정시간 참조되고 있다는 뜻이므로 Old영역으로 이동시킨다.

### Major GC

Old 영역에 있는 모든 객체들을 검사하여 참조되지 않은 객체들을 한꺼번에 삭제한다. 시간이 오래 걸리고 실행 중 프로세스가 정징된다. 이것을 `stop-the-world`라고 하는데 Major GC가 발생하면 GC를 실행하는 스레드를 제외한 나머지 스레드는 모두 작업을 멈춘다. GC 작업을 완료한 이후에야 중단했던 작업을 다시 시작한다.

`가비지 컬렉션은 어떤 원리로 소멸시킬 대상을 선정하는가?`
알고리즘에 따라 동작 방식이 매우 다양하지만 공통적인 원리가 있다. Gargabe Collector는 힙 내의 객체 중에서 가바지를 찾아내고 찾아낸 가비지를 처리해서 힙의 메모리를 회수한다. 참조되고 있지 않은 객체를 가비지라고 하며 객체가 가비지인지 아닌지 판단하기 위해서 reachability라는 개념을 사용한다. 어떤 힙 영역에 할당된 객체가 유효한 참조가 있으면 reachability,없다면 unreachability로 판단한다. 하나의 객체는 다른 객체를 참조하고, 다른 객체는 또 다른 객체를 참조할 수 있기 대문에 참조 사슬이 형성이 되는데, 이 참조 사슬중 최조에 참조한 것을 Root Set이라고 칭한다.힙영역에 있는 객체들은 총 4가지 경우에 대한 참조를 하게 된다.

![image](https://user-images.githubusercontent.com/104490310/193040041-522aea49-d59d-4145-9415-fb4d0df7857c.png)

1= 힙 내의 다른 객체에 의한 참조
2= Java스택, 즉 Java 메서드 실행 시에 사용하는 지역변수와 파라미터들에 의한 참조
3= 네이티브 스택(JNI,Java Native Interface)에 의해 생성된 객체에 대한 참조
4= 메서드 영역의 정적 변수에 의한 참조

2,3,4는 Root set이다.
즉 참조 사슬중 최초에 참조한 것이다.

인스턴스가 가비지 컬렉션의 대상이 되었다고해서 바로 소멸이 되는 것은 아니다. 빈번한 가비지 컬렉션의 실행은 시스템에 부담이 될 수 있기에 성능에 영향을 미치지 않도록 가비지 컬렉션 실행 타이밍은 별도의 알고리즘을 기반으로 계산이 되며, 이 계산 결과를 기반으로 가비지 컬렉션이 수행된다.

**Serial Gc**

적은 메모리와 CPU 코어 개수가 적을 때 적합한 방식으로 Young 영역에서는

Paraller GC

기본적인 Gc알고리즘은 Serial GC와 동일하지만 Parallel GC는 GC를 처리하는 스레드가 여러 개라서 보다 빠른 GC를 수행할 수 있다. 메모리가 충분하고 코어의 개수가 많을 떄 유리하다.

Parallel Old GC(Parallel Compactiong GC)

JDK 5 update6부터 제공한 GC방식이다. 별도로 살아있는 객체를 식별한다는 부분에서 보다 복잡한 단계로 수행된다.

[더 자세한 GC]("https://d2.naver.com/helloworld/1329")

[출처:https://asfirstalways.tistory.com/158]("https://asfirstalways.tistory.com/158")

# Collection

Java Colletion 에는 List,Map,Set 인터페이스를 기준으로 여러 구현체가 존재한다. 이에 더해 Stack과 Queue 인터페이스도 존재한다. 왜 이러한 Collection 을 사용 하는 것 일까? 그 이유는 다수의 Data를 다루는데 표준화된 클래스들을 제공해주기 떄문에 DataStructure를 직접 구현하지 않고 편하게 사용할 수 있기 때문이다. 또한 배열과 다르게 객체를 보관하기 위한 공간을 미리 정하지 않아도 되므로, 상황에 따라 객체의 수를 동적으로 정할 수 있다. 이는 프로그램의 공간적인 효율성 또한 높여준다.

- List

  `List` 인터페이스를 직접 `@Override`를 통해 사용자가 정의하여 사용할 수도 있으며, 대표적인 구현체로는 `ArrayList`가 존재한다. 이는 기존에 있었던 `Vector`를 개선한 것이다. 이외에도 `LinkedList` 등의 구현체가 있다.

- Map

  대표적인 구현체로 `HashMap`이 존재한다.
  key-value 의 구조로 이루어져 있으며 Map 에 대한 구체적인 내용은 DataStructure 부분의 hashtable 과 일치한다. key를 기준으로 중복된 값을 저장하지 않으며 순서를 보장하지 않는다. key에 대해서 순서를 보장하기 위해서는 `LinkedHashMap` 을 사용한다

* Set
  대표적인 구현체로는 `HashSet`이 존재한다. `value`에 대해서 중복된 값을 저장하지 않는다.
  사실 Set 자료구조는 Map의 key-value 구조에서 key 대신에 value가 들어가 value를 key로 하는 자료 구조일 뿐이다. 마찬가지로 순서를 보장하지 않으며 순서를 보장해주기 위해서는 `LinkedHashSet`을 사용한다.
* Stack 과 Queue
  `Stack` 객체는 직접 `new` 키워드로 사용할 수 있으며,`Queue` 인터페이스는 JDK 1.5 부터 `LinkedList`에 `new` 키워드를 적용하여 사용할 수 있다.

# Generic

제넥은 자바에서 안정성을 맡고 있다고 할 수있다. 다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스에서 사용하는 것으로, 컴파일 과정에서 타입체크를 해주는 기능이다. 객체의 타입을 컴파일시 에 체크하기 떄문에 객체의 타입 안정성을 높이고 형변환의 번거로움을 줄여준다. 자연스럽게 코드도 더 간결해진다. 예를 들면,Collection에 특정 객체만 추가될 수있도록, 또는 특정한 클래스의 특징을 갖고 있는 경우에만 추가 될 수 있도록 하는 것이 제네릭이다.이로 인한 장점은 collection 내부에서 들어온 값이 내가 원하는 값인지 별도의 로직처리를 구현할 필요가 없어진다. 또한 api를 설계하는데 있어서 보다 명확한 의사 전달이 가능해진다.

# Final Keyword

- final class

다른 클래스에서 상속하지 못한다.

- final method

다른 메소드에서 오버라이딩하지 못한다.

- final variable
  변하지 않는 상수값이 되어 새로 할당할 수 없는 변수가 된다.

추가적으로 혼동할 수 있는 두가지

- finally
  `try-catch` or `try-catch-resoure` 구문을 사용할 때 , 정상적으로 작업을 한 경우와 에러가 발생했을 경우를 포함아여 마무리 해줘야 하는 작업이 존재하는 경우에 해당 코드를 작성해주는 코드 블록이다.

- finalize()

keyword도 아니고 code block도 아닌 메소드이다.GC에 의해 호출되는 함수로 절대 호출해서는 안되는 함수이다. Object 클래스에 정의 되어 있으며 GC가 발생하는 시점이 불분명하기 때문에 해당 메소드가 실행된다는 보장이 없다. 또한 finalize() 메소드가 오버라이딩 되어 있으면 GC가 이루어질 때 바로 Garbage Collection 되지 않는다. Gc가 지연되면서 OOME 이 발생할 수 있다.

# Overriding vs Overloading

둘 다 다형성을 높여주는 개념이고 비슷한 이름이지만, 전혀 다른 개념이라고 봐도 무방할 만큼 차이가 있다(오버로딩은 다른 시그니쳐를 만든다는 관점에서 다형성으로 보지 않는 의견도 있다). 공톰점으로는 같은 이름의 다른 함수를 호출한다는것이다.

- 오버라이딩은 부모클래스에서 상속받은 서브클래스 즉 자식 클래스에서 부모클래스, 즉 상위클래스에서 만들어진 메서드를 자신의 입맛대로 다 시 재장초해서 사용하는 것을 말한다.

- 오버로딩은 하나의 클래스 안에서 같은 이름의 메서드를 사용하지만 각 메서드 마다 다른 용도로 사용되며 그결과물도 다르게 구현할 수 있게 만드는 개념인데 오버로딩이 가능하려면 메서드끼리 이름은 같지만 매개변수의 갯구나 데이터 타입이 다르면 오버로딩이 적용되어 메서드 이름은 같아도 문법 에러가 나지않는다.

다시 정리하면 같은 행위를 하지만 용도와 목적에 부합하여 다양한 기능수행과 처리, 결과를 낳을 수 있는 것이다.

- 오버라이딩(Overriding)

  상위 클래스 혹은 인터페이스에 존재하는 메소드를 하위클래스에서 필요에 맞게 재정의하는 것을 의미한다. 자바의 경우 오버라이딩시 동적바인딩된다.

  예) 아래와같은 경우,SpuerClass의 fun 이라는 인터페이스를 통해 SubClass의 fun이 실행된다

  ```java
  SuperClasss object = new SubClass();
  object.fun();
  ```

- 오버로딩은(Overloading) 메소드의 이름은 같다. return 타입은 동일하거나 다를 수 있지만, return 타입만 다를 수는 없다. 매개변수의 타입이나 갯수가 다른 메소드를 만드는것을 의미한다. 다양한 상황엣허 메소드가 호출될 수 있도록 한다. 언어마다 다르지만, 자바의 경우 오버로딩은 다른 시그니쳐를 만드는것으로, 아예 다른함수를 만드는것과 비슷하다고 생각하면 된다. 시그니쳐가 다르므로 정적바인딩으로 처리 가능하며, 자바의 경우 정적으로 바인딩된다.

  예)
  아래와 같은 경우 , fun(SuperClass super)이 실행된다.

  ```java
  main(blabla){
    SuperClass object = new SubClass();
    fun(object);
  }

  fun(SuperClass super){
    blabla...
  }

  fun(SubClass sub){
    blabla...
  }
  ```

궁금해서 따로 찾아 본자료
출처
https://secretroute.tistory.com/entry/140819

## 정적 바인딩(Static binding) vs. 동적 바인딩(Dynamic binding)

> Binding

- 프로그램 구성 요소의 성격을 결정해주는 것
  ex) 변수의 데이터 타입이 무엇인지 정해지는 것

| 종류   | 정적 바인딩                                         | 동적 바인딩                                                           |
| ------ | --------------------------------------------------- | --------------------------------------------------------------------- |
| 정의   | 컴파일 시간에 성격이 결정되는것                     | 실행시간에 성격이 결적되는것                                          |
| 예시   | C언커 컴파일 시간에 변수의 데이터 타입이 결정       | Python(interpreter 언어) 런타임에 값에 따라 변수의 데이터 타입이 결정 |
| 장단점 | 컴파일 시간에 많은 정보가 결정되므로 실행 효율 상승 | 런타임에 자유롭게 성격이 바뀌므로 적응성 상승                         |

- 함수의 바인딩

  -함수를 만들어 컴파일을 하면 각각의 코드가 메모리 어딘가에 저장된다.

  그리고 함수를 호출하는 부분에는 그함수가 저장된 메모리 번지수(주소값)이 저장된다.

  프로그램 실행 -> 함수 호출 -> 함수가 저장된 주소로 점프 -> 함수 실행 -> 원래 위치

  위 과정에서 함수를 호출하는 부분에 함수가 위치한 메모리 번지로 연결시켜 주는 것을 바인딩(Binding) 이라고 한다.

- 함수를 바인딩 하는 방법

(1) 정적 바인딩(일반 함수)
컴파일 시간에 호출될 함수로 점프할 주소가 결정되어 바인딩 되는 것.

(2) 동적 바인딩(가상 함수)

실행 파일을 만들 때 바인딩 되지 않고 보류 상태로 둔다.
점프할 메모리 번지를 저장하기 위한 메모리 공간(4 byte)를 가지고 있다가 런타임에 결정

=> 단점: 타입 체킹으로 인한 수행 속도 저하/ 메모리공간 낭비

=> 가급적 정적 바인딩 사용

**2가지 단점이 있음도 불구하고 동적 바인딩을 사용하는 이유** ??

-어떤 포인터의 의해 접근되었는 지에 상관없이 참조된 인스턴스의 실제 클래스형에 따라 재정의된 함수 호출이 가능.

## 프로그래밍 언어에서의 2가지 Type System

(1)
정적 타입 (Static Type)

# ENUM이란?

### Enum class 란 ?

우리가 흔히 상수를 정의할 때 final static string 과 같은 방식으로 상수를 정의를합니다. 하지만 이렇게 상수를 정의해서 코딩하는 경우 다양한 문제가 발생합니다. 따라서 이러한 문제점들을 보완하기 위해 자바 1.5버전부터 새롭게 추가된 것이 바로 "Enum" 입니다.
Enum은 열겨형이라 불리며 , 서로 연관된 상수들의 집합을 의미 합니다.

기존의 상수를 정의하는 방법이였던 final static string 과 같이 문자열이나 숫자들을 나타내는 기본자료형의 값을 enum을 이용해 같은 효과를 낼 수 있습니다.

## Enum의 장점

Enum을 사용하면 우리가 얻을 수 있는 이점은 다음과 같습니다.

1. 코드가 단순해지며, 가독성이 좋습니다.
2. 인스턴스 생성과 상속을 방지하여 상수값의 타입안정성이 보장됩니다.
3. enum class를 사용해 새로운 상수들의 타입을 정의함으로 정의한 타입이외의 타입을 가진 데이터값을 컴파일시 체크한다.
4. 키워드 enum을 사용하기 때문에 구현의 의도가 열거임을 분명하게 알 수 있습니다.

# 람다란?

- y= f(x) 형태의 함수로 구성된 프로그래밍 기법
- 데이터 포장 객체를 생성후 처리하는 것 보다 데이터를 바로 처리하는 것이 속도에 유리

* 자바는 람다식을 함수적 인터페이스의 익명 구현 객체로 취급
  - 함수적 인터페이스 : 한 개의 메서드를 가지고 있는 인터페이스

```java
Runnable runnable = new Runnable(){
  public void run(){...}//익명 구현 객체
}

Runnable runnable = () -> {...}// 람다식
// ()는 run의 ()를 의미하고, {}는 run 뒤에 중괄호를 의미
```

매개변수는 ()로 들어오고 코드 블록은 {}이다. 람다식은 인터페이스의 메서드를 구현한 익명구현 객체가 된다.
<br>
<br>

## 람다식 기본 문법

<hr>

```java
(타입 매개변수,..) -> {실행문;...}
(int a) -> {System.out.println(a);}

매개 타입은 런타임시 대입값에 따라 타입 자동 인식 -> 생략 가능
(a) -> {System.out.println(a);}

매개변수가 하나일 경우 () 생략 가능
a -> {System.out.println(a);}

실행문이 하나일 경우 중괄호 생략 가능

a -> System.out.println(a);

매개변수가 없다면 괄호 생략 불가능
() -> System.out.println(a);

리턴값이 있는 경우 return 문 사용
(x,y) -> { return x+y;}

중괄호에 return 문만 있다면 리턴 생략 가능
(x,y) -> x + y;
```

## 람다식을 이용한 forEach 사용법

배열,List, Map 등에 들어있는 값을 순서대로 꺼내거나 처리를 해야 할 때 for 문을 사용하는 경우가 많다.
forEach 함수를 for 같은 반복문을 처리할 때 사용하는 함수이다.

> forEach 사용 방법

```java
collection.forEach(변수 -> 반복처리(변수))
```

```java
public static void main(String[] args) {

		List<String> list = new ArrayList<>();
		list.add("a");
		list.add("p");
		list.add("p");
		list.add("l");
		list.add("e");

		// 확장 for문
		System.out.println("확장 for문 출력");
		for (String s : list) {
			System.out.println(s);
		}

		// forEach 함수
		System.out.println("forEach 함수 출력");
		list.forEach(s -> System.out.println(s));
	}
```

for 문을 사용해서 출력한 결과와 forEach 함수를 사용해 출력한 결과는 같다.
forEach 함수는 람다식으로 사용하기 떄문에 소스도 간결하게 작성할수 있다.
-> 앞에 있는 s 에 리스트 변수인 list에 저장되있는 값을 하나씩 대입합니다
그리고 출력문은 s안에 저장되어있는 값을 출력합니다.

> 배열에서 forEach 사용 방법
> 배열에서 forEach 함수를 사용하기 위해서는 Stream API를 이용해야 한다.

**배열 forEach 예제**

```java
public class Main {

	public static void main(String[] args) {

		String[] strArray = { "a", "p", "p", "l", "e" };
		Arrays.stream(strArray).forEach(s -> System.out.println(s));
```

**Map forEach 사용 예제**

```java

public class Main {

	public static void main(String[] args) {

		Map<String, String> map = new HashMap<>();
		map.put("korea", "korean");
		map.put("usa", "english");
		map.put("japan", "japanese");

		map.forEach((key, value) -> System.out.println(key + " : " + value));
	}
}
```

**List forEach 사용 예제**

```java
public static void main(String[] args) {

    	List<String> list = new ArrayList<>();
    	list.add("a");
    	list.add("p");
    	list.add("p");
    	list.add("l");
    	list.add("e");

    	list.forEach(s -> System.out.println(list.indexOf(s) + " : " + s));
    }
```

> 정리

람다식을 사용하기 떄문에 소스를 간결하게 작성할 수 있다.
간단한 반복을 처리할 때에는 forEach와 람다식을 활용해보자.

### 함수형 인터페이스 타입의 매개변수와 반환타입

```java
@FunctionalInterface
interface MyFunction {
  void myMethod();
}  //추상 메서드
```

메서드의 매개변수가 MyFunction 타입이면,

이 메서드를 호출할 때 람다식을 참조하는 참조변수를 매겨변수로 지정해야한다는 뜻이다.

```java
void aMethod(MyFunction f){ //매개변수의 타입이 함수형 인터페이스
  f.myMethod(); //Myfuntion에 정의된 메서드 호출
}
MyFunction f = () => System.out.println("MyMethod()");
aMethod.(f);
```

또는 참조변수 없이 아래와 같이 직접 람다식을 매개변수로 지정하는 것이 가능하다.

```java
aMethod(()-> System.out.println("MyMethod()"));
//람다식을 매개 변수로 지정
```

그리고 메서드의 반환타입이 함수형 인터페이스 타입이라면, 이 함수형 인터페이스의 추상메서드와 동등한 람다식을 가리키는 참조변수를 반환하거나 람다식을 직접 반환할 수 있다.

```java
MyFuntion myMethod() {
  Myfunction f = () -> {};
  return f; // 이 줄과 윗줄을 한 줄로 합치면, return ()->{};
}
```

람다식을 참조변수로 다룰 수 있는 것은 메서드를 통해 람다식을 주고 받을 수 있다는 것을 의미한다. 즉 , 변수처럼 메서드를 주고 받는 것이 가능해 진다.

사실상 메서드가 아니라 객체르 주고받는 것이라 근복적으로 달라진 것은 아무것도 없지만, 람다식 덕분에 예전보다 코드가 간결하고 이해하기 쉬워졌다.

```java
@FuntionalInterface
interFace Myfuntion(){
  void run(); // public abstract void run();
}

class Ex1{
  static void execute(Myfunction f) { //매개변수의 타입이 MyFunction인 메서드
    f.run();
  }
  static MyFuntion getMyFunction() { //반환 타입이 MyFuntion 인 메서드
    MyFunction f = () -> System.out.println("f3.run()");
    return f;
  }

  public static void main(String[] args) {
    //람다식으로 MyFunction 의 run() 을 구현

    MyFunction f1 = () -> System.out.println("f1.run()");

    MyFunction f2 = new MyFunction() { //익명 클래스로 run() 을 구현
      public void run () //public 을 반드시 붙여야함
        System.out.println("f2.run()");
    };

    My Function f3 = getMyFunction();

    f1.run();
    f2.run();
    f3.run();

    execute(f1);
    execut ( ()-> System.out.println("run()"));
  }
}

실행 결과->
f1.run();
f2.run()
f3.run()
f1.run()
run()

```

## Stream

> Stream의 특징

스트림은 컬레션(배열 포함)의 저장 요소를 하나씩 참조해서 람다식으로 처리할 수 있도록 해주는 반복자입니다.

우리는 사실 반복자를 스트림이 아니더라도 계속 사용해 왔습니다. 단적인 예로 Iterator 반복자가 있다.

```java
import java.util.*;

public class Main {

  public static void main(String[] args) {
    List<Integer> list = Arrays.asList(1, 2, 3);
    Iterator<Integer> it = list.iterator();
    while (it.hasNext()) {
      int num = it.next();
      System.out.println(num);
    }
  }

}
```

정수가 있는 리스트를 하나씩 순회하면서 값을 출력하는 단순한 코드입니다. 이제,이를 스트림으로 바꿔보겠습니다.

```java
import java.util.*;
import java.util.stream.*;

public class Main {

  public static void main(String[] args) {
    List<Integer> list = Arrays.asList(1, 2, 3);
    Stream<Integer> stream = list.stream();
    stream.forEach(System.out::println);
  }

}
```

(1) 람다식으로 요소 처리 코드를 제공한다.
위에 코드에서 볼 수 있듯이, 스트림은 람다식 또는 메소드 참초를 이용합니다.따라서
코드가 간결해지는 장점이 있습니다.

(2) 내부 반복자를 사용하므로 병렬 처리가 쉽다.

외부 반복자란 개발자가 코드로 직접 컬렉션의 요소를 반복해서 가져오는 코드 패턴을 말합니다. 우리가 흔히 사용하는 index를 이용한 반복문이나 Iterator를 사용한 while 문은 모두 외부 반복자를 이용하는 것입니다.
반면, 내부 반복자는 컬렉션 내부에서 요소들을 반복시키고, 개발자는 요소당 처리해야 할 코드만 제공하는 코드 패턴을 말합니다.

(3) 중간 처리와 최종 처리가 존재한다.
스트림은 컬렉션 요소에 대해 중간 처리와 최종 처리를 수행할 수있는데, 중간 처리에서는 매핑,필터링,정렬을 수행하고 최종 처리에 반복,카운팅,평균,총합 등의 집계 처리를 수행합니다.

## 스트림 sort

스트림을 정렬할때는 sorted()를 사용하면 된다.

```java
Stream<T> sorted()
Stream<T> sorted(Comparator<? super T> comparator)
```

sorted()는 지정된 Comparator 로 스트림을 정렬하는데, Comparator 대신 int 값을 반환하는 람다식을 사용하는 것도 가능하다. Comparator를 지정하지 않으면 스트림 요소의 기본 정렬 기준 Comparable으로
정렬한다.

```java
 Stream<String> strStream -> 을 sorted() 하게 되면 String에 구현된 Comparable 기준으로 정렬됨.
```

정렬에 사용되는 메서드의 개수가 많지만, 가장 기본적인 메서드는 Comparing()이다.

```java
comparing(Function<T, U> ketExtractor)
comparing(Function<T, U> ketExtractor, Comparator<U> keyComparator)
```

스트림의 요소가 Comparable을 구현한 경우, 매개변수 하나짜리를 사용하면 되고 그렇지 않은 경우, 추가적인 매개변수로 정렬기준(Comparator)를 따로 지정해 줘야한다.

```java
comparingInt(ToIntFunction<T> keyExtractor)
comparingLong(ToLongFunction<T> keyExtractor)
comparingDouble(ToDoubleFunction<T> keyExtractor)
```

비교 대상이 기본형인 경우, comparing() 대신 위의 메서드를 사용하면 오토박싱과 언방싱과정이 없어서 더 효율적이다. 그리고 정렬 조건을 추가할 때는 thenComparing()을 사용한다.

```java
thenComparing(Comparator<T> other)
t0henComparing(Functuon<T, U> keyExtractor)
t0henComparing(Functuon<T, U> keyExtractor, Comparator<U> key)
```

예를 들어 학생 스트림(studentStream)을 반 별 , 성적순, 그리고 이름순으로정렬하여 출력하려면

```java
studentStream.sorted(Comparator.comparing(Student::getBan)
                            .thenComparing(Student::getTotalScore)
                            .thenComparing(Student::getName
                            .forEach(System.out.println)));
```

## 스트림 변환 - (map)

## 멀티 스레드란?

우선 프로세스와 스레드를 이해하고 넘어가야 한다.

운영체제는 실행 준인 하나의 어플리케이션을 **프로세스** 라고 부른다.

사용자가 어플리케이션을 실행하면 운영체제로 부터 필요한 메모리를 할당받아 코드를 실행한다. 이것이 바로 프로세스다.

또 하나의 어플리케이션은 2개 이상의 프로세스를 가질 수 있다.
예를 들어 chrome 이라는 어플리케이션을 더블 클릭하면 chrome 이라는 exe 프로세스가 2개 생긴다.

또 **멀티태스킹** 이란 두가지 이상의 일을 동시에 처리 하는 것을 말한다.

` 결국 멀티스레드는 멀티 태스킹을 하기 위함이다`

운영체제에서 멀티태스킹을 지원하기 위해 CPU및 메모리 자원을 각 프로세스에서 적당히 할당시키고 병렬 실행을 시킨다.

예를 들어 노래를 들으면서 문서 작업을 하는 것이다.

하지만 멀티태스킹은 꼭 멀티 프로세스를 뜻하지는 않는다.

한 프로세스 내에서 멀티태스킹을 할 수있게 만들어진 어플리케이션도 있다. 예를 들어 카톡을하면서 파일전송을 할 수 도 있다.

그렇다면 어떻게 멀티 태스킹을 할 수 있을까? 그것을 가능케하는 것이다 바로 `멀티스레드` 라는 녀석이다.

스레드는 한가락의 실이라는 뜻이다. 멀티 스레드는 멀티 스레드는 여러 가닥의 실이라고 해석할 수 있는데 이 실가닥의 흐름은 코드 실행의 흐름이라고 생각하면 쉽다.

즉 여러가닥의 실은 곧 여러개의 코드 실행이 실행된다, 는 것을 뜻한다.

또한 멀티프로세스가 어플리케이션 단위의 멀티태스킹이라면,

멀티스레드는 하나의 어플리케이션 내부에서의 멀티태스킹이라고 할 수 있다.

`또 하나 알아야 할 것은`

멀티 프로세스들은 각자의 다른 메모리를 운영체제로 부터 할당 받기 때문에 프로세스간의 영향을 끼치지 않는다.

하지만 멀티 스레드는 하나의 프로세스 내에서 생성 되었기 떄문에 어떤 스레드가 문제가 발생하면, 프로세스 자체에 영향을 줄 수 있다.

즉 프로세스 내부에 존재하기 떄문이다.

따라서 멀티스레드의 예외처리는 특히 중요하다.

마지막으로

만약 싱글 스레드 어플리케이션에서 메인 스레드
가 종료되면 프로세스도 당연히 종료될것이다.

그렇다면, 멀티스레드 환경에서 종료되지 않는 스레드가 한개라도 존재한다면, 프로세스는 종료되지 않는다.

설령 메인 스레드가 작업스레드보다 먼저 종료된다 하여도 살아있는 스레드가 한개라도 존재한다면 프로세스는 종료되지 않는다는 점이다.

### 인터페이스와 추상클래스를 사용하는 이유

- 설계시 인터페이스와 추상클래스를 미리 선언해 두면 개발시 기능 구현에만 집중할 수 있다. 즉 개발자는 비즈니스 로직에만 집중할 수 있게 된다.

- 공통의 인터페이스와 추상클래스를 선언해두면,
  선언과 구현을 구분할 수 있다.

### 추상 클래스

클래스를 설계도라 하면, 추상클래스는 미완성 설계도에 비유할 수 있다.

예를 들면, 같은 크기의 TV라도 기능의 차이에 따라 여러 종류의 모델이 있지만 설계도 90%는
동일할테니,

어느정도 틀을 갖춘 상태에서 진행 하는 것이다 좋다.

이때 사용할 수 있는것이 추상 클래스이다.

**추상 메서드?**
선언부만 작성하고 구현부는 작성하지 않는 채로 남겨 둔 것이 추상 메서드 이다.
추상 메서드는 상속받는 클래스에 따라 달라질 수 있다.

### 규칙

- 추상 클래스는 키워드 abstract를 붙여 표현한다.
  추상 메서드를 포함하지 않은 클래스에서도 abstarct를 붙여서 추상 클래스로 지정할 수 있다.

- 클래스를 abstarct로 지정하면 new 를 통해 객체를 직접 생성할 수 없다.

- 메소드에 abstarct를 사용할 경우 interface의 메소드와 같이 구현 부분은 없다.

- abstarct로 선언한 메소드를 자식 클래스에서 반드시 구현해야 한다.(오버라이딩) 이는 자식 클래스에서 추상 메서드를 반드시 구현하도록 반 강제 하는 것이다.

## 인터페이스

인터페이스는 일종의 추상 클래스로, 추상 메서드를 갖지만 추상 클래스보다 추상화 정도가 높아

추상클래스와 달리 몸통을 갖춘 일반 메서드, 멤버 변수를 구성원으로 가질 수 없다.

추상클래스를 미완성 설계도라고 하면, 인터페이스는
구현된것이 아무것도 없는, 밑그림만 그려진 기본 설계도라고 할 수 있다.

## 인터페이스 규칙

- 추상 클래스처럼 불완전한 것이기 때문에 그 자체만으로 사용되기 보다,

다른 클래스를 작성하는데 도움을 줄 목적으로 작성한다.

- 일반 메서드 또는 멤버 변수를 구성원으로 가질 수 없다.

- 모든 멤버 변수는 public static final 이여야 하며, 이를 생략할 수 있다.

- 모든 메서드는 public abstract이어야 하며, 이를 생략할 수 있다.
  (단 java8 부터는 static 메서드와 default 메서드를 사용할 수 있다.)

**public static final의 사용목적?**
인스턴스 변수는 아무 인스턴스도 존재하지 않는 시점이기 때문에 스스로 초기화 될 권한이 없다
때문에 public static fianl을 사용해 구현 객체의 같은 상태를 보장한다.

```java
public interface Movable {
    void move(int x, int y);
}

interface Attackable {
    void attack(Unit u);
}

interface Fightable extends Movable, Attackable {
}
```

클래스의 상속과 마찬가지로 자식 인터페이스는 부모 인터페이스에 정의된 멤버 모두 상속받는다.

**왜 인터페이스는 다중 상속이 허용될까?**

- 인터페이스는 구현된 메서드가 없기 때문에 가능하다

반대로 말하면 추상클래스나 클래스는 이미 구현된 메서드가 존재하기 떄문에 다중 상속이 불가능 하다.

만약 자식이 둘 이상의 클래스로 부터 다중 상속을 받는다고 가정하면.

부모클래스A move()라는 메서드가 이미 정의도이있고

부모클래스B 에도 move() 라는 동일한 시그니처를 가진 메서드가 존재한다면 , 자식클래스는 둘 중 어느것을 상속 받아야할까?

자식클래스 입장에서는 , move() 메서드를 상속 받을때, 어느 부모의 메서드를 상속받야하 할지 파악할 수 없게 되므로 이미 구현이 완료된 동일 시그니처의 메서드를 상속받는 일은 결국 불가능하다.

# Enum 활용 & Enum 리스트 가져오기

### 기본 설정

예를 들어 **중개료 계약서 관리** 라는 시스템을 만든다.
계약서의 항목은 다음과 같습니다.

- 회사명
- 수수료
- 수수료 타입
  - 기록된 수수료를 %로 볼지, 실제 원단위의 금액으로 볼지를 나타냅니다.
- 수수료절삭

  - 수수료의 일정 자리수를 반올림/올림/버림할 것인지를 나타냅니다.

  가장 쉽게 domain 클래스를 작성해보자.

```java

Contract.java

@Entity
public class Contract{

  @Id
  @GeneratedValue
  private Long id;

  @Colum(nullable = false)
  private String company;

  @Colum(nullable = false)
  private double commission; //수수료

  @Colum(nullable = false)
  private String commissionType; //수수료타입

  @Colum(nullable = false)
  private String commissionCutting; //수수료 절삭

  pulbic Contract() {}

  public Contract(String company, double commission, String commissionType, String commissionCutting) {
        this.company = company;
        this.commission = commission;
        this.commissionType = commissionType;
        this.commissionCutting = commissionCutting;
    }

    public Long getId() {
        return id;
    }

    public String getCompany() {
        return company;
    }

    public double getCommission() {
        return commission;
    }

    public String getCommissionType() {
        return commissionType;
    }

    public String getCommissionCutting() {
        return commissionCutting;
    }
}
```

대부분이 String 으로 이루어진 간단한 domain 입니다.

domain 클래스를 보면 setter가 없습니다. 이는 의도한 것인데, getter 와 달리 setter는 무분별하게 생성하지 않습니다.

domain 인스턴스에 변경이 필요한 이벤트가 있을 경우 그 이벤트를 나타낼 수 있는 메소드를 만들어야하 하며, 무분별하게 값을 변경하는 setter는 최대한 멀리하는게 좋다.

예를 들어, 주문취소 같은 경우 `setOrderStatus` 가 아니라 `cancleOrder()를 만들어서 사용하는것

똑같이 orderStatus를 변경 할지라도, 그 의도와 사용범위가 명확한 메소드를 만드는것이 중요합니다.

그리고 이 domain 을 관리할 repository를 생성

```java
ContractRepository.java

public interface ContractRepository extends JpaRepository<Contract, Long>{
    Contract findByCommissionType(String commissionType);
    Contract findByCommissionCutting(String commissionCutting);
}
```

domain 클래스와 repository 클래스가 생성되었으니 간단하게 테스트 클래스를 생성

ApplicationTest.java

```java
import static org.hamcrest.CoreMatchers.is;
import static org.hamcrest.MatcherAssert.assertThat;

@RunWith(SpringRunner.class)
@SpringBootTest
public class ApplicationTests {

	@Autowired
	private ContractRepository repository;

	@Test
	public void add() {
		Contract contract = new Contract(
				"우아한짐카",
				1.0,
				"percent",
				"round"
		);
		repository.save(contract);
		Contract saved = repository.findAll().get(0);
		assertThat(saved.getCommission(), is(1.0));
	}
}

```

save & find가 잘되는 것을 확인할 수 있습니다.
자 여기서부터 본격적으로 시작해보겠습니다.

## 문제 파악

위 코드를 토대로 시스템을 만든다고 생각해보시면 어떨까요?
몇가지 문제점이 보이시나요?
생각하시는것과 다를 수는 있지만, 제가 생각하기엔 다음과 같은 문제가 있습니다.

- commissionType과 commissionCutting 은 IDE 지원을 받을 수 없다.

  - 자동완성, 오타검증 등등

- commissionType과 commissionCutting의 변경 범위가 너무 크다.

  - 예를 들어, commissionType의 money를 mount로 변경해야 한다면 프로젝트 전체에서 money를 찾아 변경해야 합니다.
    추가로 commissionType의 money 인지, 다른 domain의 money인지 확인하는 과정도 추가되어 비용이 배로 들어가게 됩니다.

* commissionType과 commissionCutting에 잘못된 값이 할당되도 검증하기가 어렵다.
  - percent, money가 아닌 값이 할당되는 경우를 방지하기 위해 검증 메소드가 필요합니다.

- commissionType과 commissionCutting의 허용된 값 범위를 파악하기 힘들다.
  - 예를들어 commissionType과commissionCutting을 select box로 표기해야 한다고 생각해보겠습니다.
  - 이들의 가능한 값 리스트가 필요한데, 현재 형태로는 하드코딩 할 수 밖에 없습니다.

더 있을 수 있지만 위 4가지 문제가 바로 생각나느것 샅습니다.
그러면 이 문제들을 해결하기 위해서는 어떻게 코드를 수정하면 좋을까요?

## 문제 해결 1

Commission.java

```java
public interface Commission {
    String TYPE_PERCENT = "percent";
    String TYPE_MONEY = "money";

    String CUTTING_ROUND = "round";
    String CUTTING_CEIL = "ceil";
    String CUTTING_FLOOR = "floor";
}
```

`static 상수` 선언을 함으로써 IDE의 지원을 받을수있고, 혹시나 값을 변경할 일이 있어도
Commision 인터페이스 값들만 변경되므로 변경범위도 최소화 되었습니다.
하지만 나머지 2가지 문제가 해결 되지 않습니다.

## 문제 해결 -2

EnumContract.java

```java
 @Column(nullable = false)
    @Enumerated(EnumType.STRING) // enum의 name을 DB에 저장하기 위해, 없을 경우 enum의 숫자가 들어간다.
    private CommissionType commissionType; // 수수료 타입 (예: 퍼센테이지, 금액)

    @Column(nullable = false)
    @Enumerated(EnumType.STRING)
    private CommissionCutting commissionCutting; // 수수료 절삭 (예: 반올림, 올림, 버림)

    public enum CommissionType {

        PERCENT("percent"),
        MONEY("money");

        private String value;

        CommissionType(String value) {
            this.value = value;
        }

        public String getKey() {
            return name();
        }

        public String getValue() {
            return value;
        }
    }

    public enum CommissionCutting {
        ROUND("round"),
        CEIL("ceil"),
        FLOOR("floor");

        private String value;

        CommissionCutting(String value) {
            this.value = value;
        }

        public String getKey() {
            return name();
        }

        public String getValue() {
            return value;
        }
    }

```

이젠 다른 개발자들이 개발을 진행할때도 타입 제한으로 enum외에 다른 값들은 못받도록 하였습니다.

자 여기까지는 쉽게 온것 같습니다. 하지만! 마지막 문제인 commissionType, commissionCutting의 리스트를 보여주는 것은 어떻게 해야할까요?
enum을 어떻게 잘 활용하면 될 것 같은 느낌이 들지 않으신가요?
한번 진행해보겠습니다.

### Enum 관리 모듈

특정 enum 타입이 갖고 있는 모든 값을 출력시키는 기능은 Class의 getEnumConstants() 메소드를 사용하면 쉽게 해결할 수 있습니다.

enum의 리스트는 select box 즉, view 영역에 제공되어야 하기 떄문에 Controller에서도 전달하도록 만들어 보겠습니다.

### ApiController.java

```java
@RestController
public class ApiController {

    @GetMapping("/enum")
    public Map<String, Object> getEnum() {
        Map<String, Object> enums = new LinkedHashMap<>();

        Class commissionType = EnumContract.CommissionType.class;
        Class commissionCutting = EnumContract.CommissionCutting.class;

        enums.put("commissionType", commissionType.getEnumConstants());
        enums.put("commissionCutting", commissionCutting.getEnumConstants());
        return enums;
    }
}
```

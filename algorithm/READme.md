# 백준 알고리즘 문제풀이

## 그리디 유형

### 1343번 문제 <hr>

민식이는 다음과 같은 폴리오미노 2개를 무한개만큼 가지고 있다. AAAA와 BB

이제 '.'와 'X'로 이루어진 보드판이 주어졌을 때, 민식이는 겹침없이 'X'를 모두 폴리오미노로 덮으려고 한다. 이때, '.'는 폴리오미노로 덮으면 안 된다.

폴리오미노로 모두 덮은 보드판을 출력하는 프로그램을 작성하시오.

첫째 줄에 사전순으로 가장 앞서는 답을 출력한다. 만약 덮을 수 없으면 -1을 출력한다.

### 입력 <hr>

풀이

- 입력 받은 보드판중 가장큰 문자열인 "XXXX"를 "AAAA"로 변환
- "XX"를 "BB"로 변환
- 보드판에 X가 포함되어있다면 -1 출력

```java
    String board = scanner.next();

    String s= board.replaceAll("XXXX","AAAA");
    String answer= s.replaceAll("XX","BB");
    if(answer.contains("X")){
        System.out.println(-1);
    }
    System.out.println(answer);

```

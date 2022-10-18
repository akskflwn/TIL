# Algorithm

- [코딩 테스트를 위한 TIP](#코딩-테스트를-위한tip)

* [문제해결을 위한 전략적 접근](#문제-해결을-위한-전략적-접근)
* [백준 알고리즘 문제풀이](#백준-알고리즘-문제풀이)

# 코딩 테스트를 위한TIP

# 문제 해결을 위한 전략적 접근

## 코딩 테스트의 목적

1. 문제 해결 여부
2. 예외 상환과 경계값 처리
3. 코드 가독성과 중복 제거 여부 등 코드 품질
4. 언어 이해도
5. 효율성

궁극적으로는 문제 해결 능력을 측정하기 위함이며 이는 어떻게 이문제를 창의적으로 해결할 것인가를 측적하기 위함이라고 볼 수 있다.

### 접근하기

1. 문제를 공격적으로 받아들이고 필요한 정보를 추가적으로 요구하여, 해당 문제에 대해 완벽하게
   이해하는게 우선이다.
2. 해당 문제를 익숙한 용어로 재정의하거나 문제를 해결하기 위한 정보를 추출한다. 이 과정을 추상화라고 한다.

3. 추상화된 데이터를 기반으로 이 문제를 어떻게 해결할 지 계획을 세운다. 이 때 사용할 알고리즘과 자료 구조를 고민한다.,
4. 세운 계획에 대해 검증을 해본다. 수도 코드 작성도 해당될 수 있고 문제 출제자에게 의견을 물어볼 수도 있다.
5. 세운 계획으로 문제를 해결해본다.
   해결이 안된다면 앞선 과정을 되짚어 본다.

### 생각할때

- 비슷한 문제를 생각해본다
- 단순한 방법으로 시작하여 개선해나간다.
- 작은 값을 생각해본다.
- 그림으로 그려본다.
- 수식으로 표현해본다.
- 순서를 강제해본다.
- 뒤에서 부터 생각해본다.

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

### 1817번 문제 <hr>

![image](https://user-images.githubusercontent.com/104490310/196038449-7d713284-ea0d-4869-ad05-f5798c998023.png)

`풀이` : 책을 순차적으로 넣고, 만약 박스에 남는 무게보다 책의 무개가 크면 새 박스에다가 넣고 박스의 숫자를 올려준다.

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {

    private void solution() throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int bookCnt = Integer.parseInt(st.nextToken());
        int MAX_WEIGHT = Integer.parseInt(st.nextToken());

        if (bookCnt == 0) {
            System.out.println(0);
            return;
        }

        st = new StringTokenizer(br.readLine());

        int boxCnt = 1;
        int currentBoxWeight = MAX_WEIGHT;

        while (bookCnt-- > 0) {
            int bookWeight = Integer.parseInt(st.nextToken());
            if (currentBoxWeight - bookWeight >= 0) {
                currentBoxWeight -= bookWeight;
            } else {
                boxCnt++;
                currentBoxWeight = MAX_WEIGHT - bookWeight;
            }
        }
        System.out.println(boxCnt);
    }

    public static void main(String[] args) throws IOException {
        new Main().solution();
    }
}
```

백준 1260 번 DFS, BFS 문제

```JAVA
import java.util.*;

public class Main {
    public int N;

    public void solve() {
        Scanner sc = new Scanner(System.in);
        N = sc.nextInt() + 1;
        int M = sc.nextInt();
        int start = sc.nextInt();
        int[][] arr = new int[N][N];
        int[][] visited = new int[N][N];

        /**
         * map[from][to] == 1;
         * edge from -> exist
         */

        for (int i = 0; i < N; ++i) {
            int fr = sc.nextInt();
            int to = sc.nextInt();
            arr[fr][to] = 1;
            arr[to][fr] = 1;
        }

        DFS(arr, visited, start);

        System.out.println("\n");

        visited = new int[N][N];
        Queue<Integer> que = new LinkedList<>();
        que.add(start);
        BFS(arr, visited, que);

    }

    private void BFS(int[][] arr, int[][] visited, Queue<Integer> que) {
        while (que.size() > 0) {
            Integer fr = que.poll();
            System.out.printf("%d ", fr);

            for (int i = 0; i < N; i++) {
                visited[i][fr] = 1;
            }

            for (int to = 0; to < N; to++) {
                if (arr[to][fr] == 1 && visited[fr][to] == 0) {
                    if (!que.contains(to))
                        que.add(to);
                }

            }
        }
    }

    public void DFS(int[][] arr, int[][] visited, int fr) {
        System.out.printf("%d ", fr);

        for (int i = 0; i < N; ++i) {
            visited[i][fr] = 1;
        }
        for (int to = 0; to < N; ++to) {
            if (arr[fr][to] == 1 && visited[fr][to] == 0) {
                DFS(arr, visited, to);
            }
        }
    }


    public static void main(String[] args) {
        Main main = new Main();
        main.solve();

    }
}

```

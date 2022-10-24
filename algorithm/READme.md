# Algorithm

- [코딩 테스트를 위한 TIP](#코딩-테스트를-위한tip)

* [문제해결을 위한 전략적 접근](#문제-해결을-위한-전략적-접근)
* [백준 알고리즘 문제풀이](#백준-알고리즘-문제풀이)

- [그리디 유형](#그리디-유형)
- [DFS와 BFS유형](#dfs와-bfs)
- [DP 유형](#DP)
- [정렬 유형](#정렬)
- [이진 탐색](#이진탐색)

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

### 설탕 배달 2839번 문제

## 문제

### 설탕 배달 2839번 문제

## 문제

상근이는 요즘 설탕공장에서 설탕을 배달하고 있다. 상근이는 지금 사탕가게에 설탕을 정확하게 N킬로그램을 배달해야 한다. 설탕공장에서 만드는 설탕은 봉지에 담겨져 있다. 봉지는 3킬로그램 봉지와 5킬로그램 봉지가 있다.

상근이는 귀찮기 때문에, 최대한 적은 봉지를 들고 가려고 한다. 예를 들어, 18킬로그램 설탕을 배달해야 할 때, 3킬로그램 봉지 6개를 가져가도 되지만, 5킬로그램 3개와 3킬로그램 1개를 배달하면, 더 적은 개수의 봉지를 배달할 수 있다.

상근이가 설탕을 정확하게 N킬로그램 배달해야 할 때, 봉지 몇 개를 가져가면 되는지 그 수를 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 N이 주어진다. (3 ≤ N ≤ 5000)

## 출력

상근이가 배달하는 봉지의 최소 개수를 출력한다. 만약, 정확하게 N킬로그램을 만들 수 없다면 -1을 출력한다.

### 예제 입력

> 18

### 예제 출력

> 4

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int cnt = 0;


        while (n % 5 != 0) {
            n -= 3;
            cnt++;
        }
        if(n <0){
            System.out.println(-1);
        }else {
            cnt += n / 5;
            System.out.println(cnt);
        }

    }
}


```

# DFS와 BFS

내가 생각하는 DFS

dfs는 한놈만 팬다. 보통 재귀함수나 스택으로 구현하며 연결된 노드를 깊이 있게 탐색한다.
미로나 지도같은 유형을 풀때 자주 사용된다.

내가 생각하는 BFS
bfs는 이름이 넓이 우선 탐색
bfs 는 구현하기위해 Queue를 사용할 수 있다.

백준 1260 번 문제

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

2606번 문제 바이러스

dfs 풀이

```java
import java.util.*;

public class Main {
    static int cnt = 0;

    static void dfs(int[][] a, boolean[] check, int v) {
        if (check[v] == true) return;

        check[v] = true;
        cnt++;

        for (int j = 0; j < a[v].length; j++) {
            if (a[v][j] == 1 && !check[j])
                dfs(a, check, j);
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();
        int e = sc.nextInt();

        int a[][] = new int[n + 1][n + 1];
        boolean check[] = new boolean[n + 1];

        for (int i = 0; i < e; i++) {
            int v1 = sc.nextInt();
            int v2 = sc.nextInt();

            a[v1][v2] = 1;
            a[v2][v1] = 1;
        }

        dfs(a, check, 1);

        System.out.println(cnt - 1);
    }


}


```

bfs 풀이

```java
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

public class Main {
    static int[][] node;
    static int[] check;


    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        int n = scanner.nextInt();
        int m = scanner.nextInt();

        node = new int[n + 1][n + 1];
        check = new int[n + 1];

        for (int i = 0; i < m; i++) {
            int from = scanner.nextInt();
            int to = scanner.nextInt();

            node[from][to] = 1;
            node[to][from] = 1;

        }
        bfs(1);
    }

    private static void bfs(int start) {
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(start);
        int cnt = 0;
        check[start] = 1;

        while (!queue.isEmpty()){
            int from = queue.poll();
            for (int i = 1; i < node.length ; i++) {
                if(node[from][i]==1 && check[i]!=1){
                    queue.offer(i);
                    check[i]= 1;
                    cnt++;
                }
            }
        }
        System.out.println(cnt);

    }
}
```

1012번 유기농 배추 dfs,bfs

dfs 풀이

```java
import java.util.*;

public class Main {
    static int M;
    static int N;
    static int K;
    static int[][] arr;
    static boolean[][] visit;
    static int[] dirX = {0, 0, -1, 1};
    static int[] dirY = {-1, 1, 0, 0};


    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int T = sc.nextInt();

        for (int i = 0; i < T; i++) {
            M = sc.nextInt();
            N = sc.nextInt();
            K = sc.nextInt();
            arr = new int[M][N];
            visit = new boolean[M][N];
            for (int j = 0; j < K; j++) {
                arr[sc.nextInt()][sc.nextInt()] = 1;
            }
            int count = 0;
            for (int m = 0; m < M; m++) {
                for (int n = 0; n < N; n++) {
                    if (arr[m][n] == 1 && !visit[m][n]) {
                        dfs(m, n);
                        count++;
                    }
                }
            }
            System.out.println(count);
        }

    }

    private static void dfs(int i, int j) {
        visit[i][j] = true;

        for (int k = 0; k < 4; k++) {
            int row = i + dirX[k];
            int col = j + dirY[k];

            if (row >= 0 && col >= 0 && row < M && col < N) {
                if (arr[row][col] == 1 && !visit[row][col])
                    dfs(row, col);
            }
        }

    }

}


```

bfs 풀이

```java
import java.util.*;

public class Main {
    static int M,N,K;
    static int[][] cabbage;
    static boolean[][] visit;
    static int count;
    static int[] dx = {0,0,-1,1};
    static int[] dy = {-1,1,0,0};

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int T = sc.nextInt();

        for (int i = 0; i < T; i++) {
            M = sc.nextInt();
            N = sc.nextInt();
            K = sc.nextInt();
            cabbage = new int[M][N];
            visit = new boolean[M][N];
            count=0;

            for (int j = 0; j < K; j++) {
                cabbage[sc.nextInt()][sc.nextInt()] = 1;
            }

            for (int j = 0; j < M; j++) {
                for (int k = 0; k < N; k++) {
                    if(cabbage[j][k]==1 && !visit[j][k]){
                        bfs(j,k);
                        count++;
                    }

                }
            }
            System.out.println(count);
        }
    }

    private static void bfs(int j, int k) {
        Queue<int[]> qu = new LinkedList<>();
        qu.offer(new int[] {j,k});

        while (!qu.isEmpty()){
            j = qu.peek()[0];
            k = qu.peek()[1];
            visit[j][k] = true;
            qu.poll();
            for (int i = 0; i < 4; i++) {
                int cx = j + dx[i];
                int cy = k + dy[i];
                if(cx>=0 && cy >= 0 && cx<M&&cy<N){
                    if(!visit[cx][cy] && cabbage[cx][cy]==1){
                        qu.add(new int[]{cx,cy});
                        visit[cx][cy] = true;
                    }
                }

            }
        }
    }
}

```

# DP

재귀 기본문제 피보나치

```java
import java.util.Collections;
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

class Main {
    static Map<Integer,Integer> fiboCache = new HashMap<>();
    public static void main(String[] args) {
       Scanner sc = new Scanner(System.in);
       int n = sc.nextInt();

        System.out.println(fibonacci(n));
    }


    private static int fibonacci(int n) {
        fiboCache.put(0, 0);
        fiboCache.put(1, 1);
        return cachedFib(n);
    }

    private static int cachedFib(int n) {
        if(fiboCache.containsKey(n)){
            return fiboCache.get(n);
        }

        int value = cachedFib(n-1)+ cachedFib(n-2);
        fiboCache.put(n,value);
        return value;
    }
}
```

# 정렬

선택 정렬

```java
public class Selection_Sort {

    public static void selection_sort(int[] a) {
        selection_sort(a, a.length);
    }

    private static void selection_sort(int[] a, int size) {

        for(int i = 0; i < size - 1; i++) {
            int min_index = i;

            // 최솟값을 갖고있는 인덱스 찾기
            for(int j = i + 1; j < size; j++) {
                if(a[j] < a[min_index]) {
                    min_index = j;
                }
            }

            // i번째 값과 찾은 최솟값을 서로 교환
            swap(a, min_index, i);
        }
    }

    private static void swap(int[] a, int i, int j) {
        int temp = a[i];
        a[i] = a[j];
        a[j] = temp;
    }

}
```

삽입 정렬

```java
import java.util.Arrays;

public class Insertion_Sort {

    public static void main(String[] args) {
        int []arr = {7,8,2,9,4,6};
        insertion_sort(arr);
        System.out.println(Arrays.toString(arr));
    }
    public static void insertion_sort(int[] a) {
        insertion_sort(a, a.length);
    }

    private static void insertion_sort(int[] a, int size) {
        for (int i = 1; i < size; i++) {
            int key = a[i];
            int j = i-1;

            while (j>=0 && key < a[j]){
                a[j+1]=a[j];
                j--;
            }
            a[j+1]=key;
        }


    }
}
```

퀵소트 O(n log n)

```java
import java.util.ArrayList;
import java.util.Arrays;

public class Quick_sort {

    public static void sort(int[] a) {
        l_pivot_sort(a, 0, a.length - 1);
    }

    /**
     *  왼쪽 피벗 선택 방식
     * @param a		정렬할 배열
     * @param lo	현재 부분배열의 왼쪽
     * @param hi	현재 부분배열의 오른쪽
     */
    private static void l_pivot_sort(int[] a, int lo, int hi) {

        /*
         *  lo가 hi보다 크거나 같다면 정렬 할 원소가
         *  1개 이하이므로 정렬하지 않고 return한다.
         */
        if(lo >= hi) {
            return;
        }

        /*
         * 피벗을 기준으로 요소들이 왼쪽과 오른쪽으로 약하게 정렬 된 상태로
         * 만들어 준 뒤, 최종적으로 pivot의 위치를 얻는다.
         *
         * 그리고나서 해당 피벗을 기준으로 왼쪽 부분리스트와 오른쪽 부분리스트로 나누어
         * 분할 정복을 해준다.
         *
         * [과정]
         *
         * Partitioning:
         *
         *   a[left]          left part              right part
         * +---------------------------------------------------------+
         * |  pivot  |    element <= pivot    |    element > pivot   |
         * +---------------------------------------------------------+
         *
         *
         *  result After Partitioning:
         *
         *         left part          a[lo]          right part
         * +---------------------------------------------------------+
         * |   element <= pivot    |  pivot  |    element > pivot    |
         * +---------------------------------------------------------+
         *
         *
         *  result : pivot = lo
         *
         *
         *  Recursion:
         *
         * l_pivot_sort(a, lo, pivot - 1)     l_pivot_sort(a, pivot + 1, hi)
         *
         *         left part                           right part
         * +-----------------------+             +-----------------------+
         * |   element <= pivot    |    pivot    |    element > pivot    |
         * +-----------------------+             +-----------------------+
         * lo                pivot - 1        pivot + 1                 hi
         *
         */
        int pivot = partition(a, lo, hi);

        l_pivot_sort(a, lo, pivot - 1);
        l_pivot_sort(a, pivot + 1, hi);
    }



    /**
     * pivot을 기준으로 파티션을 나누기 위한 약한 정렬 메소드
     *
     * @param a		정렬 할 배열
     * @param left	현재 배열의 가장 왼쪽 부분
     * @param right	현재 배열의 가장 오른쪽 부분
     * @return		최종적으로 위치한 피벗의 위치(lo)를 반환
     */
    private static int partition(int[] a, int left, int right) {

        int lo = left;
        int hi = right;
        int pivot = a[left];		// 부분리스트의 왼쪽 요소를 피벗으로 설정

        // lo가 hi보다 작을 때 까지만 반복한다.
        while(lo < hi) {

            /*
             * hi가 lo보다 크면서, hi의 요소가 pivot보다 작거나 같은 원소를
             * 찾을 떄 까지 hi를 감소시킨다.
             */
            while(a[hi] > pivot && lo < hi) {
                hi--;
            }

            /*
             * hi가 lo보다 크면서, lo의 요소가 pivot보다 큰 원소를
             * 찾을 떄 까지 lo를 증가시킨다.
             */
            while(a[lo] <= pivot && lo < hi) {
                lo++;
            }

            // 교환 될 두 요소를 찾았으면 두 요소를 바꾼다.
            swap(a, lo, hi);
        }


        /*
         *  마지막으로 맨 처음 pivot으로 설정했던 위치(a[left])의 원소와
         *  lo가 가리키는 원소를 바꾼다.
         */
        swap(a, left, lo);

        // 두 요소가 교환되었다면 피벗이었던 요소는 lo에 위치하므로 lo를 반환한다.
        return lo;
    }

    private static void swap(int[] a, int i, int j) {
        int temp = a[i];
        a[i] = a[j];
        a[j] = temp;
    }

    public static void main(String[] args) {
        int arr[] = {1,3,2};
        sort(arr);
        System.out.println(Arrays.toString(arr));
    }
}
```

# 이진 탐색

이진탐색은 배열의 데이터가 정렬되어 있으만 사용할 수 있는 알고리즘이다.

- 데이터가 무작위 일때 는 사용할 수 없지만 , 이미 정렬되어 있다면 매우 빠르게
  탐색할 수 있다는 특징이 있다.
- 이진 탐색은 탐색 범위를 절반씩 좁혀가며 데이터를
  탐색하는 특징이있다.

```java
public class binarySearch {


    static int binarySearch1(int key, int lo, int hi) {
        int mid;
        if (lo <= hi) {
            mid = (lo + hi) / 2;

            if (key == mid) {
                return mid;
            } else if (key < arr[mid]) {
                return binarySearch1(key, lo, mid - 1);
            } else
                return binarySearch1(key, mid + 1, hi);
        }
        return -1;
    }

    static int binarySearch2(int key, int lo, int hi) {
        int mid;
        while (lo <= hi) {
            mid = (lo + hi) / 2;
            if (mid == key) {
                return mid;
            } else if (key < arr[mid]) {
                hi = mid - 1;
            } else {
                lo = mid + 1;
            }
        }
        return -1;
    }
}
```

백준 10816 숫자카드 2

lowerBound = 하한선
upperBound = 상향선

하한선 = 하한은 찾고자 하는 값 이상의 값이 처음으로 나타나는 위치를 의미한다
상향선 = 상한은 찾고자 하는 값을 초과한 값을 처음 만나는 위치다

숫자가 중복된 중복된 횟수 = upperBound - lowerBound

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Main {
    public static int[] arr;

    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int N = Integer.parseInt(br.readLine());
        arr = new int[N];

        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        for (int i = 0; i < N; i++) {
            arr[i] = Integer.parseInt(st.nextToken());
        }

        Arrays.sort(arr);

        int M = Integer.parseInt(br.readLine());

        st = new StringTokenizer(br.readLine(), " ");
        StringBuilder sb = new StringBuilder();

        for (int i = 0; i < M; i++) {
            int key = Integer.parseInt(st.nextToken());
            sb.append(upperBound(arr, key) - lowerBound(arr, key)).append(' ');
        }
        System.out.println(sb);

    }

    private static int upperBound(int[] arr, int key) {
        int lo = 0;
        int hi = arr.length;

        while (lo < hi) {

            int mid = (lo + hi) / 2;

            if (key < arr[mid]) {
                hi = mid;
            } else {
                lo = mid + 1;
            }
        }
        return lo;
    }

    private static int lowerBound(int[] arr, int key) {
        int lo = 0;
        int hi = arr.length;

        while (lo < hi) {
            int mid = (lo + hi) / 2;

            if (key <= arr[mid]) {
                hi = mid;
            } else {
                lo = mid + 1;
            }
        }
        return lo;
    }
}
```

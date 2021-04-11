무식해 보여도 사실은 최고의 방법일 때가 있지요

# 모의고사

### 문제 설명

수포자는 수학을 포기한 사람의 준말입니다. 수포자 삼인방은 모의고사에 수학 문제를 전부 찍으려 합니다. 수포자는 1번 문제부터 마지막 문제까지 다음과 같이 찍습니다.

1번 수포자가 찍는 방식: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, ...
2번 수포자가 찍는 방식: 2, 1, 2, 3, 2, 4, 2, 5, 2, 1, 2, 3, 2, 4, 2, 5, ...
3번 수포자가 찍는 방식: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, ...

1번 문제부터 마지막 문제까지의 정답이 순서대로 들은 배열 answers가 주어졌을 때, 가장 많은 문제를 맞힌 사람이 누구인지 배열에 담아 return 하도록 solution 함수를 작성해주세요.

### 제한 조건

- 시험은 최대 10,000 문제로 구성되어있습니다.
- 문제의 정답은 1, 2, 3, 4, 5중 하나입니다.
- 가장 높은 점수를 받은 사람이 여럿일 경우, return하는 값을 오름차순 정렬해주세요.

### 입출력 예

| answers     | return  |
| ----------- | ------- |
| [1,2,3,4,5] | [1]     |
| [1,3,2,4,2] | [1,2,3] |

### 입출력 예 설명

입출력 예 #1

- 수포자 1은 모든 문제를 맞혔습니다.
- 수포자 2는 모든 문제를 틀렸습니다.
- 수포자 3은 모든 문제를 틀렸습니다.

따라서 가장 문제를 많이 맞힌 사람은 수포자 1입니다.

입출력 예 #2

- 모든 사람이 2문제씩을 맞췄습니다.

---

```c++
#include <vector>
#include <iostream>
#include <algorithm>
using namespace std;

vector<int> solution(vector<int> answers) {
    vector<int> answer;
    int a[5]={1,2,3,4,5};
    int b[8]={2,1,2,3,2,4,2,5};
    int c[10]={3, 3, 1, 1, 2, 2, 4, 4, 5, 5};
    vector<int> cnt(3); // 이미 0으로 초기화
    
    for(int i = 0; i < answers.size();i++){
        if(a[i % (sizeof(a)/sizeof(a[0]))]==answers[i])
            cnt[0]++;
        if(b[i % (sizeof(b)/sizeof(b[0]))]==answers[i])
            cnt[1]++;
        if(c[i % (sizeof(c)/sizeof(c[0]))]==answers[i])
            cnt[2]++;
    }
    int maxcnt = *max_element(cnt.begin(),cnt.end());	// =  max( max( cnt[0], cnt[1] ), cnt[2] )
    for(int i = 0; i < 3; i++){
        if(cnt[i] == maxcnt)
            answer.push_back(i+1);
    }
    
    return answer;
}
```

**Issues**

❗처음엔 일일이 서로가 최댓값인지 확인하는 알고리즘을 생각했었다. 그런데 최댓값 찾아주는 라이브러리가 있다면 이것을 사용해서 이게 최댓값과 같은지만 확인해주면 된다. *max_element를 사용하기 위해 vector 또는 배열을 사용해야 한다.

---

# ⭐소수 찾기

### 문제 설명

한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.

각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.

### 제한사항

- numbers는 길이 1 이상 7 이하인 문자열입니다.
- numbers는 0~9까지 숫자만으로 이루어져 있습니다.
- "013"은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.

### 입출력 예

| numbers | return |
| ------- | ------ |
| "17"    | 3      |
| "011"   | 2      |

### 입출력 예 설명

예제 #1
[1, 7]으로는 소수 [7, 17, 71]를 만들 수 있습니다.

예제 #2
[0, 1, 1]으로는 소수 [11, 101]를 만들 수 있습니다.

- 11과 011은 같은 숫자로 취급합니다.

---

### 솔루션

#### 문제 나누기

- 길이별 순열 구하기
- 문자를 숫자로 바꾸기
- 소수 판단하기

```c++
// 다른 사람의 풀이

#include <string>
#include <vector>
#include <iostream>
#include <algorithm>
#include <set>
#define MAX 9999999999
using namespace std;

bool isPrime(int number)
{
    if (number == 1)
        return false;
    if (number == 2)
        return true;
    if (number % 2 == 0)
        return false;

    bool isPrime = true;
    for (int i = 2; i <= sqrt(number); i++)
    {
        if (number% i == 0)
            return false;
    }

    return isPrime;
}

bool compare(char a, char b)
{
    return a > b;
}

int solution(string numbers) {
    int answer = 0;

    string temp;
    temp = numbers;

    sort(temp.begin(), temp.end(),compare);		// 문자열 정렬

    vector<bool> prime(std::stoi(temp)+1);		// 소수 판단값을 저장하는 벡터

    //cout << stoi(temp) << endl;
    prime[0] = false;
    for (long long i = 1; i < prime.size(); i++)
    {
        prime[i] = isPrime(i);
    }
    //cout << "chk1" << endl;
    //int num = std::stoi(numbers);

    string s, sub;

    s = numbers;

    sort(s.begin(), s.end());
    set<int> primes;
    int l = s.size();
    do {
        for (int i = 1; i <= l; i++)
        {
            sub = s.substr(0, i);
        //  cout << "chk2" <<  " " << sub<<  endl;
            if (prime[std::stoi(sub)])
            {
                primes.insert(std::stoi(sub));
            }
        }
    } while (next_permutation(s.begin(), s.end()));

    //cout << primes.size();
    answer = primes.size();
    return answer;
}
```



---

# 카펫

### 문제 설명

Leo는 카펫을 사러 갔다가 아래 그림과 같이 중앙에는 노란색으로 칠해져 있고 테두리 1줄은 갈색으로 칠해져 있는 격자 모양 카펫을 봤습니다.

<img src="images/carpet.png" alt="carpet.png" style="zoom: 33%;" />

Leo는 집으로 돌아와서 아까 본 카펫의 노란색과 갈색으로 색칠된 격자의 개수는 기억했지만, 전체 카펫의 크기는 기억하지 못했습니다.

Leo가 본 카펫에서 갈색 격자의 수 brown, 노란색 격자의 수 yellow가 매개변수로 주어질 때 카펫의 가로, 세로 크기를 순서대로 배열에 담아 return 하도록 solution 함수를 작성해주세요.

### 제한사항

- 갈색 격자의 수 brown은 8 이상 5,000 이하인 자연수입니다.
- 노란색 격자의 수 yellow는 1 이상 2,000,000 이하인 자연수입니다.
- 카펫의 가로 길이는 세로 길이와 같거나, 세로 길이보다 깁니다.

### 입출력 예

| brown | yellow | return |
| ----- | ------ | ------ |
| 10    | 2      | [4, 3] |
| 8     | 1      | [3, 3] |
| 24    | 24     | [8, 6] |



### 솔루션

#### 🌕Approach 1: math

**실마리**

테두리 한 줄은 갈색으만 이루어져 있다.

갈색, 노란색 개수가 각각 정해지면 하나의 가로, 세로가 결정이 된다. 고 본다

갈색, 노란색의 수를 합한 총합은 여러 가로, 세로 쌍이 나올 수 있다.

여기서 하나를 결정하는 공식을 찾아내자.



**Algorithm**

가장 먼저 카펫 총합의 수가 만들어지는 가로, 세로 길이 쌍을 구한다.

이를 위해 모든 카펫의 수의 약수를 구한다.(절반까지만) 그리고 그 약수와 곱해서 모든 카펫의 수가 되는 나머지 수를 구한다. 

한쌍 마다 (가로 * 2 + (세로-1) * 2)가 갈색 테두리의 총합과 같은지 확인한다. 

```c++
#include <string>
#include <vector>
using namespace std;
vector<int> solution(int brown, int yellow) {
    vector<int> answer;
    int total = brown + yellow;
    int row;    // 가로
    int col;    // 세로
    
    for(int i = 3; i < total/2; i++){
        if(total % i == 0){ // 약수, 가로, 세로 찾기
            row = total/i;
            col = i;
            if(row*2+(col-2)*2==brown)
                return vector<int> {row, col};
        }
    }
    return answer;
}
// Status: ⭕ Success
```

```c++
// 다른 사람 풀이. 아직 이해 못한 상태
#include <string>
#include <vector>
using namespace std;
vector<int> solution(int brown, int red) {

    int len = brown / 2 + 2;
    int w = len - 3;
    int h = 3;

    while(w >= h){
        if(w * h == (brown + red)) break;
        w--;
        h++;
    }
    return vector<int>{w, h};
}
// Status: ⭕ Success
```


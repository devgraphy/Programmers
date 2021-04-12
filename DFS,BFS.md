깊이/너비 우선 탐색을 사용해 원하는 답을 찾아봐

# 타겟 넘버

### 문제 설명

n개의 음이 아닌 정수가 있습니다. 이 수를 적절히 더하거나 빼서 타겟 넘버를 만들려고 합니다. 예를 들어 [1, 1, 1, 1, 1]로 숫자 3을 만들려면 다음 다섯 방법을 쓸 수 있습니다.

```
-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3
```

사용할 수 있는 숫자가 담긴 배열 numbers, 타겟 넘버 target이 매개변수로 주어질 때 숫자를 적절히 더하고 빼서 타겟 넘버를 만드는 방법의 수를 return 하도록 solution 함수를 작성해주세요.

### 제한사항

- 주어지는 숫자의 개수는 2개 이상 20개 이하입니다.
- 각 숫자는 1 이상 50 이하인 자연수입니다.
- 타겟 넘버는 1 이상 1000 이하인 자연수입니다.

### 입출력 예

| numbers         | target | return |
| --------------- | ------ | ------ |
| [1, 1, 1, 1, 1] | 3      | 5      |

```c++
#include <string>
#include <vector>
#include <iostream>
using namespace std;
int chk[25];
vector<int> nums;
int sum;
int cnt;
int tar;
void dfs(int len){
    if(len >= nums.size()){
        sum = 0;
        for(int i = 0; i < nums.size(); i++){
            //1은 +, 0은 - 연산
            cout << nums[i] << "(" << chk[i] << ")" ;
            if(chk[i]==1) sum+=nums[i];
            else sum-=nums[i];
            
        }
        cout << ": " << sum << endl;
        if(sum == tar)
            cnt++;
        return;
    }
        
    for(int i = len; i < nums.size();i++){
        chk[i] = 1;
        dfs(len+1);
        chk[i] = 0;
        dfs(len+1);
    }
}
int solution(vector<int> numbers, int target) {
    nums.assign(numbers.begin(), numbers.end());// assign을 이용한 복사
    tar = target;
    dfs(0);
    return cnt;
}
// Status: ❌
```

**Issues**

- 바둑판에서 4방향으로 for문 돌리는 것과 헷갈림

```
1(1)1(1)1(1)1(1)1(1): 5
1(1)1(1)1(1)1(1)1(0): 3
1(1)1(1)1(1)1(0)1(1): 3
1(1)1(1)1(1)1(0)1(0): 1
1(1)1(1)1(1)1(0)1(1): 3 <- for문에 의해 같은 재귀함 수 내에서 다음 칸으로 또 넘어감
1(1)1(1)1(1)1(0)1(0): 1
```

```c++
#include <string>
#include <vector>
using namespace std;
int chk[25];
vector<int> nums;
int sum;
int cnt;
int tar;
void dfs(int len){
    if(len >= nums.size()){
        sum = 0;
        for(int i = 0; i < nums.size(); i++){
            //1은 +, 0은 - 연산
            if(chk[i]==1) sum+=nums[i];
            else sum-=nums[i];
        }
        if(sum == tar)
            cnt++;
        return;
    } 
    chk[len] = 1;
    dfs(len+1);
    chk[len] = 0;
    dfs(len+1); 
}
int solution(vector<int> numbers, int target) {
    nums.assign(numbers.begin(), numbers.end());// assign을 이용한 복사
    tar = target;
    dfs(0);
    return cnt;
}
// Status: ⭕Success
// 테스트 1 〉	통과 (19.07ms, 3.95MB)
// 테스트 2 〉	통과 (19.16ms, 3.96MB)
// 테스트 3 〉	통과 (0.02ms, 3.98MB)
// 테스트 4 〉	통과 (0.06ms, 3.95MB)
// 테스트 5 〉	통과 (0.54ms, 3.96MB)
// 테스트 6 〉	통과 (0.03ms, 3.83MB)
// 테스트 7 〉	통과 (0.02ms, 3.96MB)
// 테스트 8 〉	통과 (0.13ms, 3.94MB)
```

**키워드 라이브러리**

- `assign`







# 네트워크



# 단어 변환



# 여행경로


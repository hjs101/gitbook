# 10~11일차

프로그래머스 LV1 풀이 (page3)

### 실패율
[실패율](https://school.programmers.co.kr/learn/courses/30/lessons/42889)
```
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
using namespace std;


bool compare(vector<double> a ,vector<double> b ){
    if(a[0] == b[0]){
        
        if(a[1]> b[1]){
            return false;
        }else{
            return true;
        }
        
    }else if (a[0] > b[0]){
        return true;
    }else{
        return false;
    }
    
}

vector<int> solution(int N, vector<int> stages) {
    vector<int> answer;
    
    vector<int> cnt1(N+2,0);
    vector<int> cnt2(N+2,0);
    for(int i=0,s=stages.size();i<s;i++){
        // 스테이지 도달한 사람
        for(int j=stages[i];j>0;j--){
            cnt1[j]++;
        }
        
        // 클리어 카운트
        for(int j=stages[i]-1;j>0;j--){
            cnt2[j]++;
        }
    }
    
    vector<vector<double>> lose(N+1,vector<double>(2,0));
    
    for(int i=1;i<=N;i++){
        if(cnt1[i] != 0){
            double dn = (double)(cnt1[i]-cnt2[i])/(double)cnt1[i];
            lose[i][0] = dn;
            lose[i][1] = i;
        }else if(cnt1[i] == 0 && i != 0){
            lose[i][1] = i;
        }
    }
    
    sort(lose.begin(),lose.end(),compare);
    for(int i=0;i<=N;i++){
        if(lose[i][1] == 0){
            continue;
        }
        answer.push_back(lose[i][1]);
    }
    
    return answer;
}
```
정렬과 관련된 문제로, 풀이방법은 class를 만들어 비교연산자 오버라이딩을 하거나, 2차원 vector와 정렬 함수의 compare를 이용하는 등의 방법이 생각났고, 그 중 저번에 사용하지 않았던 compare함수를 만들어 사용하는 방법을 택했다.

### 비밀지도
[비밀지도](https://school.programmers.co.kr/learn/courses/30/lessons/17681);
```
#include <string>
#include <vector>
#include <iostream>
using namespace std;

vector<string> toBinary(vector<int> arr){
    vector<string> res;
    for(int i=0,n=arr.size();i<n;i++){
        string str= "";
        while(arr[i] >= 1){
            if(arr[i] % 2 == 0){
                str = "0" + str;
            }else{
                str = "1" + str;
            }
            arr[i] = arr[i]/2;
        }
        
        while(str.size() < arr.size()){
            str = '0' + str;
        }
        
        res.push_back(str);
        
    }
    return res;
}

vector<string> solution(int n, vector<int> arr1, vector<int> arr2) {
    
    vector<string> strArr1=toBinary(arr1);
    vector<string> strArr2=toBinary(arr2);
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++){
            if(strArr1[i][j] == '0' && strArr2[i][j] == '0'){
                strArr1[i][j] = ' ';
            }else{
                strArr1[i][j] = '#';
            }
        }
    }
    
    return strArr1;
}
```

10진법 -> 이진법 변화와 두 배열의 논리연산이 엮여있는 문제로, 주어진 설명대로 구현하여 풀었다.

### 옹알이(2)
[옹알이(2)](https://school.programmers.co.kr/learn/courses/30/lessons/133499)
```
#include <string>
#include <vector>
#include <algorithm>
#include <iostream>
using namespace std;

string strList[] = {"aya","ye","woo","ma"};

bool isC(string str, string pos){
    
    for(int i=0,n=str.size()-3;i<n;i++){
        bool check = true;
        for(int j=0,s=pos.size();j<s;j++){
            if(str[i+j] != pos[j]){
                check = false;
                break;
            }
        }
        if(check){
            i = i + pos.size();
            bool check2 = true;
            for(int j=0,s=pos.size();j<s;j++){
                if(str[i+j] != pos[j]){
                    check2 = false;
                    break;
                }
            }
            
            if(check2){
                return false;
            }
        }
    }
    return true;
    
}

int solution(vector<string> babbling) {
    int answer = 0;
    
    for(int i=0,n=babbling.size();i<n;i++){
        string str = babbling[i];
        bool check = true;
        for(int j=0;j<4;j++){
            
            if(!isC(str, strList[j])){
                check = false;
            }
            
        }
        
        if(!check){
            continue;
        }
        
        for(int j=0;j<4;j++){
            
            while(str.find(strList[j]) != string::npos){
                str.replace(str.find(strList[j]),strList[j].size()," ");
            }
            
        }
        bool check2 = true;
        for(int j=0,s=str.size();j<s;j++){
            if(str[j] != ' '){
                check = false;
            }
        }
        if(check){
            answer++;
        }
        
    }
    
    return answer;
}
```

옹알이 1에서 진화한 문제, 각각의 영어단어가 한 번만 등장한다는 제한사항이 삭제되고, 같은 단어의 연속된 발음을 체크해야 되는 조건이 추가로 생겼다. 일단, 기존 풀이 방식은 각 단어가 한 번만 등장한다는 전제 하에 구현하여 사용하지 못했다. JAVA였다면 replaceALL 함수를 사용했을텐데라는 아쉬움이 드는 순간에 그냥 그 부분을 구현하면 된다는 생각이 들었고, 각 단어를 " "(공백문자열)으로 더이상 단어가 존재하지 않을때까지 replace시키는 방식으로 풀었다.


### 다트 게임
[다트 게임](https://school.programmers.co.kr/learn/courses/30/lessons/17682)
```
#include <string>
#include <iostream>
using namespace std;

int solution(string dartResult) {
    int answer = 0;
    
    int before = 0;
    for(int i=0,n=dartResult.size();i<n;i++){
        
        int size = 0;
        string str = "";
        
        while(dartResult[i] >= '0' && dartResult[i] <='9'){
            size++;
            str+= dartResult[i++];
        }
        int score = stoi(str);
        if(dartResult[i] == 'D'){
            score = score * score;    
        }else if(dartResult[i] == 'T'){
            score = score * score * score;
        }
        if(!(dartResult[i+1] >= '0' && dartResult[i+1] <='9')){
            if(dartResult[++i] == '*'){
                
                if(i-(2+size) < 0){
                    score = score*2;
                }else{
                    score = score*2;
                    if(dartResult[i-(2+size)] == '*'){ 
                        answer= answer + before;
                    }else if(dartResult[i-(2+size)] == '#'){
                        answer = answer + before;
                    }else{
                        answer = answer + before;
                    }
                    
                }
                
            }else if(dartResult[i] == '#'){
                
                score = -score;
                
            }
            
        }
        answer += score;
        before = score;     
    }
    
    return answer;
}
```

가장 정답률이 낮은 문제였지만 생각만큼 어렵지는 않았던 문제. 조건이 명확하여 해당 조건대로 시뮬레이션 하듯이 구현하면 되는 문제였다. 다트에서 가능한 점수중 10이 있어 해당 부분에 의해 문자열에서 각각의 문자 위치가 꼬이게 되는 문제가 있었고, 정수부분을 따로 별도로 문자열로 꺼낸 뒤에 stoi 함수를 사용하는 방법으로 변경 하는 방법으로 변경하여 풀이하였다.

![lv1-3page](images/14.png)


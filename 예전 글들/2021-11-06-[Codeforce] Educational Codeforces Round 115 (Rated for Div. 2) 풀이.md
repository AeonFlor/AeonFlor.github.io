##### A : Computer Game (0:19)

***

(0,0) 위치에서 (1,n) 까지 갈 수 있는지 확인하는 문제이다. 오랜만에 알고리즘 문제를 풀어서 Virtual participation(이하 VP) 당시 얼탔다. 그냥 느낌대로 풀었는데 시간도 20분 가까이 걸렸고, 코드도 안 예쁘다. 어차피 오른쪽 아래로만 가니 8방위가 아니라 오른쪽 위, 오른쪽 아래, 아래, 이렇게 3 방향만 고려하면 됐다. 다음에는 예쁘게 풀어보자.

```c++
#include <iostream>
#include <vector>
#include <string>
#include <queue>
 
using namespace std;
 
vector<vector<int>> field;
vector<vector<bool>> visited;
 
int dx[8] = { 0,1,1,1,0,-1,-1,-1 };
int dy[8] = { 1,1,0,-1,-1,-1,0,1 };
 0
int main(void)
{
	int T, n;
	string input;
 
	cin >> T;
 
	while (T--)
	{
		cin >> n;
 
		field.assign(2, vector<int>(n+1, 1));
		visited.assign(2, vector<bool>(n + 1, false));
 
		for (int i = 0; i < 2; ++i)
		{
			input = "";
			cin >> input;
 
			for (int j = 0; j<input.size(); ++j)
			{
				field[i][j] = input[j] -'0';
			}
		}
 
		queue<pair<int, int>> bfs;
 
		bfs.push({ 0,0 });
 
		while (!bfs.empty())
		{
			int y = bfs.front().first;
			int x = bfs.front().second;
 
			if (y == 1 && x == n-1)
				break;
 
			bfs.pop();
 
			for (int i = 0; i < 8; ++i)
			{
				if (y + dy[i] < 0 || y + dy[i] >1 || x + dx[i] < 0 || x + dx[i] == n)
					continue;
 
				if (field[y + dy[i]][x + dx[i]] == 0)
				{
					if (visited[y + dy[i]][x + dx[i]])
						continue;
 
					bfs.push({ y + dy[i], x + dx[i] });
					visited[y + dy[i]] [x + dx[i]] = true;
				}
			}
		}
 
		if (bfs.empty())
			cout << "NO\n";
 
		else
			cout << "YES\n";
	}
}
```



아래는 같이 스터디를 하는 기만러 Dreamw 님의 코드인데 허락을 구하고 글에 첨부했다. 오른쪽 부분이 아예 막혀있는지 확인하는 코드인데 나랑은 다르게 짧고 예쁘게 짠 걸 확인할 수 있다. 

```C++
#include <bits/stdc++.h>
#define endl "\n"
#define MAX 100001
#define INF 1e9+7
#define pii pair<int,int>
#define piii pair<pair<int,int>,int>
#define ll long long
using namespace std;
 
int main(){
    ios_base::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
 
    int tt;
    cin >> tt;
    while(tt--){
        int n, flag=0; cin >> n;
        string s1, s2; cin >> s1 >> s2;
        for(int i=1; i<n; i++){
            if(s1[i]-48 && s2[i]-48){
                flag=1;
                break;
            }
        }
        cout << (flag?"NO":"YES") << endl;
        
    }
 
    return 0;
}
```



##### B : Groups (0:43 - 대회 끝나고 5분 뒤에 AC 받음 ㅠㅡㅠ)

***

처음에 B 번 보고 감이 안 잡혀서, C 번 푼 뒤 B 번을 풀었는데 시간이 모자라서 대회 종료 후 5분 뒤에 AC 를 받았다 ㅠㅠ 뭐.. 내 실력이지.. 문제는 학생별로 수업을 들을 수 있는 요일을 입력받고, 학생을 그룹 2개로 나눴을 때 모두 문제없이 들을 수 있는지를 확인하는 문제이다. (단, 그룹의 인원 수는 서로 같아야 한다.) 나는 요일별로 가능한 학생 수를 집계한 뒤, 가능한 학생 수가 n/2 를 넘는 요일에 대해서만 다른 요일과 비교하여 가능한지 판단했는데 조건이 복잡해서 그런지 코드도 복잡했다. 그래도 풀긴 풀었는데 코드가 길고 안 예쁘다 ㅠㅡㅠ

```c++
#include <iostream>
#include <vector>

using namespace std;

int main(void)
{
	ios_base::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);

	int t, n, input;
	bool flag;
	vector<int> student;
	vector<int> day_count;

	cin >> t;

	while (t--)
	{
		flag = false;

		cin >> n;

		if (n % 2 == 1)
		{
			cout << "NO\n";
			continue;
		}

		student.assign(n, 0);
		day_count.assign(5, 0);

		for (int i = 0; i < n; ++i)
		{
			for (int j = 0; j < 5; ++j)
			{
				cin >> input;

				if (input == 1)
				{
					++day_count[j];
					student[i] += 1 << j;
				}
			}
		}

		for (int i = 0; i < 5 && !flag; ++i)
		{
			if (day_count[i] >= n / 2)
			{
				vector<int> count(5, 0);
				vector<int> both(5, 0);

				for (int j = 0; j < 5 && !flag; ++j)
				{
					if (i == j)
						continue;

					for (int k = 0; k < student.size(); ++k)
					{
						if ((student[k] & 1 << j) == 1 << j)
						{
							if ((student[k] & 1 << i) == 0)
								++count[j];

							else if ((student[k] & 1 << i) == 1 << i)
								++both[j];
						}
					}
				}

				for (int j = 0; j < 5 && !flag; ++j)
				{
					if (i == j)
						continue;

					int supplement = n / 2 - count[j];

					if (count[j] >= n / 2)
						flag = true;

					else if (both[j] >= supplement && day_count[i] - n / 2 >= supplement)
						flag = true;
				}
			}
		}

		if (flag)
			cout << "YES\n";

		else
			cout << "NO\n";
	}
}
```



아래는 마찬가지로 같이 스터디를 하는 기만러 Dreamw 님의 코드인데 허락을 구하고 글에 첨부했다. 나와는 달리 조건이 깔끔하다. i 번 요일과 j 번 요일이 모두 안 되는 학생이 있다면 건너뛰고, 하나라도 된다면 각각 cnt1, cnt2 에 1을 더해서 가능한 학생 수를 집계한다. 이렇게 집계된 값이 둘 다 n/2 보다 크거나 같다면 가능하다고 판단하여 "YES"를 출력한다.

```c++
#include <bits/stdc++.h>
#define endl "\n"
#define MAX 100001
#define INF 1e9+7
#define pii pair<int,int>
#define piii pair<pair<int,int>,int>
#define ll long long
using namespace std;

int main(){
    ios_base::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);

    int tt;
    cin >> tt;
    while(tt--){
        int n; cin >> n;
        bool arr[1001][5]={0};
        for(int i=0; i<n; i++){
            for(int j=0; j<5; j++) cin >> arr[i][j];
        }
        int cflag=0;
        for(int i=0; i<4; i++){
            for(int j=i+1; j<5; j++){
                
                int cnt1=0, cnt2=0, flag=0;
                for(int k=0; k<n; k++){
                    if(!arr[k][i] && !arr[k][j]){
                        flag=1; break;
                    }

                    if(arr[k][i]) cnt1++;
                    if(arr[k][j]) cnt2++;
                }

                if(flag) continue;

                if(cnt1>=n/2 && cnt2>=n/2){
                    cflag=1;
                    break;
                }
            }
        }

        cout << (cflag?"YES":"NO") << endl;
    }

    return 0;
}
```





##### C : Delete Two Elements (1:03)

***

이건 그래도 lower_bound 와 upper_bound 를 이용해서 깔끔하게 풀었다! 히히~ 문제는 수열이 주어졌을때 수열의 수 중 2개를 제외해도 평균값이 변하지 않을 때, 제외할 수 있는 조합의 개수를 구하는 문제이다. 정렬한 뒤 하나씩 탐색하여 더했을 때 평균 * 2 가 되도록하는 수의 개수를 찾아서 ans 에 더해줬다. 다만 부동소수점 오류를 고려한다고 삽질을 하고, 형변환을 안해서 틀리고, 별 이상한 삽질 때문에 20분만에 푼 문제를 40분동안 고쳤다 ㅠㅠ 계속 하다보면 좋아지겠지..?

```C++
#include <iostream>
#include <vector>
#include <algorithm>
#include <cmath>

using namespace std;

vector<unsigned long long> arr;

int main(void)
{
	ios_base::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);

	unsigned long long t, n, sum, ans;
	int address, next;
	double ave;
	
	cin >> t;

	while (t--)
	{
		cin >> n;

		sum = ans = 0;
		arr.assign(n, 0);

		for (int i = 0; i < n; ++i)
		{
			cin >> arr[i];
			sum += arr[i];
		}

		ave = (double)sum / n;

		sort(arr.begin(), arr.end());
	
		for (int i = 0; i < n; ++i)
		{
			address = lower_bound(arr.begin() + i + 1, arr.end(), ave * 2 - arr[i]) - arr.begin();
			next = upper_bound(arr.begin() + i + 1, arr.end(), ave * 2 - arr[i]) - arr.begin();
			
			ans += next - address;
		}

		cout << ans << '\n';
	}
}
```



##### D : Training Session (대회 이후 1:52 동안 풀어서 TLE 받고, 풀이 봄)

***

사실 감이 잘 안 와서 TLE 나올 거 알았는데도 그냥 풀고, 풀이를 봤다. 다음 링크에서 봤는데 정리한 내용을 이미지로 첨부하겠다. (https://www.youtube.com/watch?v=rnplha61a1E)



![노트](..\..\..\..\assets\images\2021-11-06-[Codeforce] Educational Codeforces Round 115 (Rated for Div. 2) 풀이\note.jpg)



```C++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main(void)
{
	int t, n;
	long long ans, topic_2, difficulty_2;

	vector<pair<int, int>> topic, difficulty;

	cin >> t;

	while (t--)
	{
		cin >> n;

		topic.resize(n);
		difficulty = topic;

		ans = 1LL * n * (n - 1) * (n - 2) / 6;

		for (int i = 0; i < n; ++i)
		{
			cin >> topic[i].first >> topic[i].second;

			difficulty[i].first = topic[i].second;
			difficulty[i].second = topic[i].first;
		}

		sort(topic.begin(), topic.end());
		sort(difficulty.begin(), difficulty.end());

		for (int i = 0; i < n; ++i)
		{
			topic_2 = upper_bound(topic.begin(), topic.end(), make_pair(topic[i].first + 1, 0)) - lower_bound(topic.begin(), topic.end(), make_pair(topic[i].first, 0)) - 1;
			difficulty_2 = upper_bound(difficulty.begin(), difficulty.end(), make_pair(topic[i].second + 1, 0)) - lower_bound(difficulty.begin(), difficulty.end(), make_pair(topic[i].second, 0)) - 1;

			ans -= (topic_2 * difficulty_2);
		}

		cout << ans << '\n';
	}
}
```



##### 후기

***

오랜만에 코드포스 VP 를 했는데, 바보가 된 것 같으면서도 성장한 게 느껴진다. 몇 달전에는 Div. 3 에서 A,B 밖에 못 풀었는데 조금 늦긴 해도 Div. 2 에서 C 까지는 자력으로 풀 수 있으니까 기분이 좋다. 단기적인 목표는 C 까지 안정적으로 푸는 것이다! 화이팅!





이현수 그만 기만해~

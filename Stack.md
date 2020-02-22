# 列车调度例题(Create stack without STL)
描述
某列车调度站的铁道联接结构如图所示。
![figure](https://github.com/WangleiO/Data-Structure/blob/master/03bc70595803464554b5f6b69a21962beb038264.png?raw=true)

其中，A为入口，B为出口，S为中转盲端。所有铁道均为单轨单向式：列车行驶的方向只能是从A到S，再从S到B；另外，不允许超车。因为车厢可在S中驻留，所以它们从B>端驶出的次序，可能与从A端驶入的次序不同。不过S的容量有限，同时驻留的车厢不得超过m节。
设某列车由编号依次为{1, 2, ..., n}的n节车厢组成。调度员希望知道，按照以上交通规则，这些车厢能否以{a1, a2, ..., an}的次序，重新排列后从B端驶出。如果可行，应该以怎样的次序操作?  
**输入**  
共两行。
第一行为两个整数n，m。
第二行为以空格分隔的n个整数，保证为{1, 2, ..., n}的一个排列，表示待判断可行性的驶出序列{a1，a2，...，an}。  
**输出**  
若驶出序列可行，则输出操作序列，其中push表示车厢从A进入S，pop表示车厢从S进入B，每个操作占一行。
若不可行，则输出No。
```
Example 1
Input:
5 2
1 2 3 5 4
Output:
push
pop
push
pop
push
pop
push
push
pop
pop
Example 2
Input:
5 5
3 1 2 4 5
Output:
No
```
```c++
#include <iostream>
using namespace std;
const int maxn = 16e5 + 5;

struct stack {
    int n = 0;
    int ss[maxn];

    void push(int a) {
        n++;
        ss[n] = a;
    }
    void pop() {
        n--;
    }
    int top() {
        return ss[n];
    }
    bool empty() {
        if(n == 0)
            return true;
        else
            return false;
    }
    int size() {
        return n;
    }
};

stack a,s;
int ans[maxn];
bool way[maxn];

int main(int argc, const char * argv[]) {

    int n,m;
    cin>>n>>m;
    for (int i=n; i>=1; i--) {
        a.push(i);
    }
    for (int i=0; i<n; i++) {
        cin>>ans[i];
    }
    int j = 0, k = 0;
    bool flag = false;
    while (!flag) {
        if (k == n) {
            flag = true;
            break;
        }
        if (s.empty()) {
            s.push(a.top());
            a.pop();
            way[j] = true;
        }
        else
            if (ans[k] == s.top()) {
                s.pop();
                k++;
                way[j] = false;
            }
        else
            if (!a.empty()) {
                s.push(a.top());
                a.pop();
                way[j] = true;
            }
            else{
                cout<<"No"<<endl;
                break;
            }
        if (s.size()>m) {
            cout<<"No"<<endl;
            break;
        }
        ++j;
    }

    if (flag) {
        for (int i=0; i<j; i++) {
            if (way[i]) {
                cout<<"push"<<endl;
            }
            else
                cout<<"pop"<<endl;
        }
    }
    return 0;
}
```

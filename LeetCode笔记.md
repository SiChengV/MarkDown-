# LeetCode

## 快速幂

快速求$$x^n$$：

$$x^{11}$$中的11可以转成二进制 1011，二进制的每位都代表了对应的权重：$$x^{2^3}$$、$$x^{2^2}$$、$$x^{2^1}$$、$$x^{2^0}$$，每位的值为1则代表此权重有效，因此就是将每一位的1 所对应的权重相乘即可 $$x^{11} = x^{2^3} * x^{2^1} * x^{2^0}$$ 。

## 贪心算法

贪心算法的应用场景：解决一个问题需要多个步骤，每一个步骤有多种选择。可以使用贪心算法解决的问题，每一步只需要解决一个子问题，只做出一种选择，就可以完成任务。

### 使用「贪心算法」的问题需要满足的条件：

* 最优子结构：规模较大的问题的解由规模较小的子问题的解组成，区别于「动态规划」，可以使用「贪心算法」的问题「规模较大的问题的解」只由其中一个「规模较小的子问题的解」决定；
* 无后效性：后面阶段的求解不会修改前面阶段已经计算好的结果；
* 贪心选择性质：从局部最优解可以得到全局最优解。

## 修改输入输出流的默认数字基数

cin >> hex; cin >> oct; cin >> dec; 

cout << hex; cout << oct; cout << dec;

二进制输出：`cout << bitset<8>(100) << endl;` 按照8位输出二进制的100

修改之后会将十进制数字以指定进制输出，记得用完之后改回默认的dec

## 约瑟夫环

假设有n个人，从0开始编号，从0号开始报数，每隔m个人kill掉一个人，在接着从下个人开始报数，求最后幸存的是哪个人？

```c++
int lastRemaining(int n, int m) {
    int res = 0;// 最后只剩下一位，坐标肯定是0   // 从结果往回推最后一个剩下的元素下标肯定为0
    for (int i = 2; i <= n; i++) {
        res = (res + m) % i;   // 每一次的res代表该轮最后幸存的那个人所在数组的index
    }
    return res;  // 最后在数组中的index
}
```

## 字符串处理

getline配合stringstream可以做到按指定字符切割字符串 `getline(ss, str, '指定字符')`

`regex_search(str, regex("正则表达式"))` 返回str是否匹配目标正则表达式（str的子串匹配也算）

`regex_match(str, regex("正则表达式"))` 返回str是否匹配正则表达式（要str完全匹配，一点不匹配就返回false）

`str.substr(n, m)`截取子串，从n开始往后m个

## KMP算法

```c++
// 计算next数组 next[x]表示当下标为x+1的字符匹配失败时应该回退到的位置    
void generateNext(string &s, int next[]){
        int length = s.size();
        for(int i = 1, j = 0; i < length; ++i){  // i从1开始是因为i为1时如果匹配失败只能回退到0
            while(j > 0 && s[i] != s[j]){
                j = next[j - 1];
            }
            if(s[i] == s[j])
                ++j;
            next[i] = j;
        }
    }

    int kmp(string S, string T) {
        int next[500001] = {0};
        int ans = 0;
        generateNext(S, next);
        for(int i = 0, j = 0; i < T.size(); ++i){   
            while(j > 0 && S[j] != T[i]){    // 匹配失败 模式串回退
                j = next[j - 1];
            }
            if(S[j] == T[i]){
                ++j;
            }
            if(j == S.size()){
                ++ans;
                j = next[j - 1];
            }
        }
        return ans;
    }
```


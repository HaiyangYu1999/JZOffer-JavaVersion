
输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。

 

**示例 1：**

```
输入: "the sky is blue"
输出: "blue is sky the"
```

**示例 2：**

```
输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
```

**示例 3：**

```
输入: "a good   example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
```

 

**说明：**

- 无空格字符构成一个单词。
- 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
- 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

<!--more-->

### 1 正则表达式切分

直接正则表达式, 简单粗暴, 就是速度慢点. 这时就体现出java的好处了, 直接split, c++就没有

```java
class Solution {
    public String reverseWords(String s) {
        String[] strings = s.strip().split("\\s+");
        StringBuilder sb = new StringBuilder();
        for(int i = strings.length - 1; i >= 0; --i){
            sb.append(strings[i]);
            if(i != 0){
                sb.append(" ");
            }
        }
        return sb.toString();
    }
}
```

### 2 两次反转

思路很简单, 但是实现起来还是比较麻烦的. 但是比第一种要快啊

第一步是去除s两端的空格

第二步是去除s中间多余的空格

第三步是反转整个字符串

第四步是反转每个单词

```
"  a   good   example  "
"a   good   example"
"a good example"
"elpmaxe doog a"
"example good a"
```



```c++
class Solution {
public:
    string reverseWords(string s) {
        strip(s);
        reverse(s,0,s.size());
        int k = 0;
        for(int i = 0; i < s.size(); ++i){
            if(i == 0 && s[i] == ' '){
                continue;
            }
            else if(i > 0 && s[i-1] == ' ' && s[i] == ' '){
                continue;
            }
            else{
                s[k++] = s[i];
            }
        }
        s.erase(k, s.size() - k);
        int left = 0;
        int right = 0;
        while(left < s.size()){
            while(right < s.size() && s[right] != ' '){
                ++right;
            }
            reverse(s, left, right);
            left = right;
            ++left;
            ++right;
        }
        return s;
    }
private:
    void reverse(string& s, int begin, int end){   //反转字符串[begin,end)的字符
        if(end - begin < 2){
            return;
        }
        for(int i = begin; i < begin + (end - begin) / 2; ++i){
            std::swap(s[i], s[end + begin - 1 - i]);
        }
    }
    void strip(string& s){
        size_t begin = s.find_first_not_of(" ");
        s.erase(0, begin);
        size_t end = s.find_last_not_of(" ");
        s.erase(end + 1, s.size() - end);
    }
};
```


在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。

示例:
```
s = "abaccdeff"
返回 "b"
```
```
s = "" 
返回 " "
```

限制：
```
0 <= s 的长度 <= 50000
```
<!--more-->

### hashmap

构造字符到下标的map, 遍历一遍字符串, 如果字符第一次出现, 就把(字符, 下标)加入到map中, 如果字符已经在hashmap中, 就把下标设置为-1.

然后再遍历一遍map, 找到下标为非负数并且最小的字符.

```c++
class Solution {
public:
    char firstUniqChar(string s) {
        unordered_map<char,int> map;
        for(int i = 0; i < s.size(); ++i){
            char ch = s[i];
            if(map.find(ch) == map.end()){
                map[ch] = i;
            }
            else{
                map[ch] = -1;
            }
        }
        int min_index = INT_MAX;
        char res = ' ';
        for(auto& pair : map){
            if(pair.second >= 0 && pair.second < min_index){
                min_index = pair.second;
                res = pair.first;
            }
        }
        return res;
    }
};
```


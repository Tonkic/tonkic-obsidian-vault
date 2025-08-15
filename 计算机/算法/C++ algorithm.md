```C++
//std::sort
bool cmp(auto &a,auto &b){...}，如果返回true，就代表a应该排在b前面，否则是b在a前
//使用lambda函数代替cmp
//[capture list](parameter list) ->return type {function body}
//比如说，我们要捕获b数组，就写成[&b]即可(&代表引用捕获)
sort(flag.begin(),flag.end(),[&](int a,int b){return heights[a]>heights[b];});
//[&]表示捕获所有外部变量
```
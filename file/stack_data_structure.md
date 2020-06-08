## 栈

```
#include <iostream>

class MyStack {
    private:
        vector<int> data;               // store elements
    public:
        /** Insert an element into the stack. */
        void push(int x) {
            data.push_back(x);
        }
        /** Checks whether the queue is empty or not. */
        bool isEmpty() {
            return data.empty();
        }
        /** Get the top item from the queue. */
        int top() {
            return data.back();
        }
        /** Delete an element from the queue. Return true if the operation is successful. */
        bool pop() {
            if (isEmpty()) {
                return false;
            }
            data.pop_back();
            return true;
        }
};

int main() {
    MyStack s;
    s.push(1);
    s.push(2);
    s.push(3);
    for (int i = 0; i < 4; ++i) {
        if (!s.isEmpty()) {
            cout << s.top() << endl;
        }
        cout << (s.pop() ? "true" : "false") << endl;
    }
}
```

大多数流行的语言都提供了内置的栈库，因此你不必重新发明轮子。除了`初始化`，我们还需要知道如何使用两个最重要的操作：`入栈`和`退栈`。除此之外，你应该能够从栈中`获得顶部元素`。下面是一些供你参考的代码示例：

```
#include <iostream>
int main() {
    // 1. Initialize a stack.
    stack<int> s;
    // 2. Push new element.
    s.push(5);
    s.push(13);
    s.push(8);
    s.push(6);
    // 3. Check if stack is empty.
    if (s.empty()) {
        cout << "Stack is empty!" << endl;
        return 0;
    }
    // 4. Pop an element.
    s.pop();
    // 5. Get the top element.
    cout << "The top element is: " << s.top() << endl;
    // 6. Get the size of the stack.
    cout << "The size is: " << s.size() << endl;
}
```

### 最小栈实现

设计一个支持 push，pop，top 操作，并能在常数时间内检索到最小元素的栈。

- push(x) -- 将元素 x 推入栈中。
- pop() -- 删除栈顶的元素。
- top() -- 获取栈顶元素。
- getMin() -- 检索栈中的最小元素。

**示例:**

```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```

```
class MinStack {
private:
    stack<int> s;
    stack<int> minS;
public:
    /** initialize your data structure here. */
    MinStack() {}
    
    void push(int x) {
        s.push(x);
        if(minS.empty()||x<=minS.top()){
            minS.push(x);
        }
    }
    
    void pop() {
        if(s.top() == minS.top()){
            minS.pop();
        }
        s.pop();
    }
    
    int top() {
        return s.top();
    }
    
    int getMin() {
        return minS.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */
```

### 有效的括号

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。

注意空字符串可被认为是有效字符串。

**示例 1:**

```
输入: "()"
输出: true
```

**示例 2:**

```
输入: "()[]{}"
输出: true
```

**示例 3:**

```
输入: "(]"
输出: false
```

**示例 4:**

```
输入: "([)]"
输出: false
```

**示例 5:**

```
输入: "{[]}"
输出: true
```

```
class Solution {
public:
    bool isValid(string s) {
        if(s.length()==0)return true;
        stack<char> st;
        for(int i=0;i<s.length();i++){
            if(!st.empty()){
                if(s[i]==')'&&st.top()=='('){
                    st.pop();
                }else if(s[i]=='}'&&st.top()=='{'){
                    st.pop();
                }else if(s[i]==']'&&st.top()=='['){
                    st.pop();
                }else{
                    st.push(s[i]);
                }
            }else{
                st.push(s[i]);
            }
            
        }
        return st.empty();
    }
};
```

### 每日温度

根据每日 `气温` 列表，请重新生成一个列表，对应位置的输出是需要再等待多久温度才会升高超过该日的天数。如果之后都不会升高，请在该位置用 `0` 来代替。

例如，给定一个列表 `temperatures = [73, 74, 75, 71, 69, 72, 76, 73]`，你的输出应该是 `[1, 1, 4, 2, 1, 1, 0, 0]`。

**提示：**`气温` 列表长度的范围是 `[1, 30000]`。每个气温的值的均为华氏度，都是在 `[30, 100]` 范围内的整数。

使用栈来进行存储只需要O(n)的时间复杂度就可以解决，对于某值先入栈若找到比他大的值则通过下标计算差值，否则继续入栈。当然使用暴力解也是可行的，从前往后遍历，但是时间复杂度为O($n^2$)，

```
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& T) {
        vector<int> a(T.size());
        stack<int> s;
        for(int i=0;i<T.size();i++){
            while(!s.empty()&&T[i]>T[s.top()]){
                a[s.top()]=i-s.top();
                s.pop();
            }
            s.push(i);
            
        }
        return a;
    }
};
```


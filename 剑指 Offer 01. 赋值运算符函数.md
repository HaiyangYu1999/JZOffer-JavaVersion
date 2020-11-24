如下为类型CMyString的声明, 请为该类型添加赋值运算符函数.

```c++
class CMyString
{
public:
    CMyString(char* pData = nullptr);
    CMyString(const CMyString& str);
    ~CMyString(void);
    
private:
    char* m_pData;
}
```

<!--more-->

## 1. override clone() 方法

由于Java里面不支持运算符重载, 所以只能用实现`clone()`方法来模拟`=`运算符重载.

```java
import java.util.Arrays;

public class CMyString implements Cloneable {
    private char[] m_pData;

    public CMyString(char[] pData) {
        m_pData = pData;
    }
    public CMyString(){
        this(null);
    }
    @Override
    protected void finalize() throws Throwable{     //finalize方法在jdk9及之后被标记为Deprecated
        super.finalize();                           //并且极度不推荐使用
    }
    @Override
    public CMyString clone() throws CloneNotSupportedException {
        CMyString another = (CMyString) super.clone();
        another.m_pData = (this.m_pData == null) ? null : this.m_pData.clone();
        return another;
    }
    @Override
    public String toString(){
        if(m_pData == null){
            return null;
        }
        return new String(m_pData);
    }

    public void changeArray(){
        Arrays.fill(m_pData, '?');
    }
    public static void main(String[] args) throws CloneNotSupportedException {
        CMyString string = new CMyString(new char[]{'h','e','l','l','o'});
        CMyString other = string.clone();
        string.changeArray();
        System.out.println(string);
        System.out.println(other);
        CMyString string1 = new CMyString(null);
        CMyString other1 = string1.clone();
        string.changeArray();
        System.out.println(string1);
        System.out.println(other1);
    }
}
```


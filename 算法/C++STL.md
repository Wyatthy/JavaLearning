

# 一些小函数

## 判断字符串  

```c++
#include<cctype>
int isalpha ( int c );			//判断是否为字母
int isdigit ( int c);			//判断是否为0-9数字
```

## atoi() 和 stoi()

相同点：
①都是C++的字符处理函数，把数字字符串转换成int输出
②头文件都是#include<cstring>

不同点：
①atoi()的参数是 const char* ,因此对于一个字符串str我们必须调用 c_str()的方法把这个string转换成 const char*类型的,而stoi()的参数是const string*,不需要转化为 const char*；
②stoi()会做范围检查，默认范围是在int的范围内的，如果超出范围的话则会runtime error！

```c++
cout<<atoi(s3);		//报错
cout<<atoi(s3.c_str());		//正确

// stoi example
#include <iostream>   // std::cout
#include <string>     // std::string, std::stoi

int main ()
{
  std::string str_dec = "2001, A Space Odyssey";
  std::string str_hex = "40c3";
  std::string str_bin = "-10010110001";
  std::string str_auto = "0x7f";

  std::string::size_type sz;   // alias of size_t

  int i_dec = std::stoi (str_dec,&sz);
  int i_hex = std::stoi (str_hex,nullptr,16);
  int i_bin = std::stoi (str_bin,nullptr,2);
  int i_auto = std::stoi (str_auto,nullptr,0);

  std::cout << str_dec << ": " << i_dec << " and [" << str_dec.substr(sz) << "]\n";
  std::cout << str_hex << ": " << i_hex << '\n';
  std::cout << str_bin << ": " << i_bin << '\n';
  std::cout << str_auto << ": " << i_auto << '\n';

  return 0;
}

/*输出：
2001，《太空漫游》：2001和[，《太空漫游》
40c3：16579
-10010110001：-1201
0x7f：127
*/
```



## to_string()

```c++
// to_string example
#include <iostream>   // std::cout
#include <string>     // std::string, std::to_string

int main ()
{
  std::string pi = "pi is " + std::to_string(3.1415926);
  std::string perfect = std::to_string(1+2+4+7+14) + " is a perfect number";
  std::cout << pi << '\n';
  std::cout << perfect << '\n';
  return 0;
}

/*输出：
pi is 3.141593
28 is a perfect number
*/
```



# 输入输出

## 格式化输出

### printf格式输出数字，位数不够前面补0

```c++
int a = 4;
printf("%03d",a);
/*输出：
004
*/
//也可以用 * 代替位数，在后面的参数列表中用变量控制输出位数；
int a = 4;
int n = 3;
printf("%0*d",n,a);
输出：004
```

printf格式输出数字，保留小数点

```c++
float p=1f;
printf("%.2f",p);
/*输出：
1.00
*/
```



# 体排序

```c++
#include<bits/stdc++.h>
using namespace std;
typedef struct{
	string name,id;
	int score;
}stu; 

bool cmp(stu &a,stu &b)            //当return的是ture时，a先输出，所以是升序
{
     return a.score < b.score;
}
int main(){
	stu li[n];
    //li的初始化
	sort(li,li+n,cmp);
	return 0;
}
```

## 容器

### Unordered Set

Unordered sets are containers that store unique elements in no particular order, and which allow for fast retrieval of individual elements based on their value.




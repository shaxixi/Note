[toc]

# ACM——note

## 输入输出

* 输入不说明有多少个Input Block,则以EOF为结束标志

  > scanf() 函数返回值就是读出的变量个数。
  > 如果只有一个整数输入，返回值是1；如果有两个整数输入，返回值是2；如果一个都没有，则返回值是-1；
  >
  > EOF 是一个预定义的常量，等于 -1
  
  ~~~c
  while(scanf("%d %d",&a, &b) != EOF) {....} 
  ~~~

  ~~~c++
  while( cin >> a >> b ) {.... } 
  ~~~
  
* scanf() 多个字符串之间用一个或多个空格分隔；gets() 字符串之间用回车符作分隔；

  > 通常情况下，接受短字符用scanf函数，用scanf("%d %d",&a,&a)表示；接受长字符用gets函数，用 gets(str1)表示。 

  ~~~c
  char buf[20];
  gets(buf);
  ~~~

* getline函数用法

  > 可以接受用户的输入的字符，直到已达到指定个数，或者用户输入了特定的字符。
  > istream& getline(char line[], int size, char endchar = '\n');
  >
  > 参数：
  >
  > 1. char line[]：字符数组，用户输入的内容将存入在该数组内
  > 2. int size[]：用户超过size的输入都将不被接受
  > 3. char endchar：当用户输入endchar 指定的字符时，自动结束。默认是回车符。

  ~~~c++
  char name[4];
  cin.getline(name,4,'\n');
  ~~~

* 不要在for语句中定义类型

* 字符串 ==>> 整数：’2‘ - ’0‘ = 2；整数 ==>> 字符串：2 + ‘0’ = ‘2’；

* getchar() 就是从输入流缓冲区一位一位地取内容。

  > 当输入流缓冲区还没有内容的时候，getchar()处于待命状态。没有结束标志，输入内容都处于缓冲区待命。

~~~c
#include<stdio.h>
void main(){
	//1089 1.0
	int a,b;
	while(scanf("%d %d",&a,&b) != EOF){
		printf("%d\n",a+b);
	}
	
	//1090 1.0
	int n;
	int a,b,i;
	scanf("%d",&n);
	for(i = 0;i < n;i++){
		scanf("%d %d",&a,&b);
		printf("%d\n",a+b);
	}
	
	//1091 1.0
	int a,b;
	while(scanf("%d %d",&a,&b) != EOF){
		if(a == 0 && b == 0) break;
		printf("%d\n",a+b);
	}
	
	//1092 1.0
	int m;
	while(scanf("%d",&m) != EOF){
		if(m == 0) break;
		int s = 0,j,a; 
		for(j = 0;j < m;j++){
			scanf("%d",&a);
			s += a;
		}
		printf("%d\n",s);
	}
	
	
	//1093 1.0
	int n,i;
	scanf("%d",&n);
	for(i = 0;i < n;i++){
		int m,j,a,s = 0;
		scanf("%d",&m);
		for(j = 0;j < m;j++){
			scanf("%d",&a);
			s += a;
		}
		printf("%d\n",s);
	} 
	
	//1094 1.0
	int m;
	while(scanf("%d",&m) != EOF){
		int s = 0,j,a; 
		for(j = 0;j < m;j++){
			scanf("%d",&a);
			s += a;
		}
		printf("%d\n",s);	
	}
	
	//1095 1.0
	int a,b;
	while(scanf("%d %d",&a,&b) != EOF){
		printf("%d\n\n",a+b);
	}
	
	//1096 1.0
	int n,i;
	scanf("%d",&n);
	for(i = 0;i < n;i++){
		int m,j,a,s = 0;
		scanf("%d",&m);
		for(j = 0;j < m;j++){
			scanf("%d",&a);
			s += a;
		}
		printf("%d\n\n",s);
	}
	
	//1096 1.1
	int n,i;
	scanf("%d",&n);
	for(i = 0;i < n;i++){
		int m,j,a,s = 0;
		scanf("%d",&m);
		for(j = 0;j < m;j++){
			scanf("%d",&a);
			s += a;
		}
		//更：you must note that there is a blank line between outputs.
		if(i == n-1) printf("%d\n",s);
		else printf("%d\n\n",s); 
	} 	
} 


~~~



## 字符串

头文件：string.h

* strcpy 复制

  > extern char *strcpy(char *dest,char *src);	
  > 把src字符串复制到dest所指的数组中。
  >
  > 注意:
  > src和dest所指内存区域不可以重叠且dest必须有足够的空间来容纳src的字符串。返回指向dest的指针。

* strcat 串接

  > extern char *strcat(char *dest,char *src);	
  > 把src所指字符串添加到dest结尾处(覆盖dest结尾处的'\0')并添加'\0'。
  >
  > 注意：
  > src和dest所指内存区域不可以重叠且dest必须有足够的空间来容纳src的字符串。返回指向dest的指针。

* strcmp 比较

  > extern int strcmp(char *s1,char * s2);
  > 比较字符串s1和s2。
  >
  > 注意：
  > 当s1<s2时，返回值<0；当s1=s2时，返回值=0；当s1>s2时，返回值>0。即：两个字符串自左向右逐个字符相比（按ASCII值大小相比较），直到出现不同的字符或遇'\0'为止。

* strlen 长度

  > extern unsigned int strlen(char *s);
  > 计算字符串s的(unsigned int型）长度
  >
  > 注意：
  > 返回s的长度，不包括结束符NULL。

* 读取字符

  > scanf() 以Space、Enter、Tab结束一次输入，不会舍弃最后的回车符（即回车符会残留在缓冲区中）；
  >
  > getchar() 以Enter结束输入，也不会舍弃最后的回车符；

* 读取字符串

  > scanf() 以Space、Enter、Tab结束一次输入，不会舍弃最后的回车符（即回车符会残留在缓冲区中）；
  >
  > gets() 以Enter结束输入，接受空格，会舍弃最后的回车符。

~~~c++
#include<string.h>
#include <iostream>
#define MAX 50 //根据题意不同，设置不同的最大值
using namespace std;

int main(){
    //1020 1.1
	int n;
	scanf("%d",&n);
	if(n <= 100 && n >=1){
		for(n;n>0;n--){	
			char c[MAX];
			scanf("%s",c);
			int i = 0;
			int sum=0;
			while(c[i] != '\0'){
				if(c[i] == c[i+1]){
					sum++;
				}else{
					sum++;
					if(sum ==1){
						printf("%c",c[i]);
					}else{
						printf("%d%c",sum,c[i]);
					}
					sum=0;
				}
				i++;			
			}
			printf("\n");				
		}		
	}
    
	//1020 1.0 AABBACC ==>> 3A2B2C ; AABBACC ==>> 2A2BA2C 
    int n;
    scanf("%d",&n);
    for(n;n>0;n--){    
        char c[MAX];
        scanf("%s",c);
        int i = 0;
        int num[26]={0};
        while(c[i] != '\0'){
            num[c[i]-65]++;
            i++;            
        }
        for(int j = 0;j<26;j++){
            if(num[j] != 0){
                if(num[j] != 1) printf("%d%c",num[j],j+65);
                else printf("%c",j+65);
            }
        }
        printf("\n");
//        printf("A=%d B=%d C=%d\n",num[0],num[1],num[2]);                 
    }
 

	//2017 1.0
	int n;
	scanf("%d",&n);
	for(n;n>0;n--){
		char c[MAX];
		scanf("%s",c);
		int i = 0;
		int sum = 0;
		while(c[i] != '\0'){
			if(c[i] >=48 && c[i] <=57) sum++;
			i++;			
		}
		printf("%d\n",sum); 
	}
	
	//2000 1.0
	char c[4];
    while(scanf("%c%c%c%c",&c[0],&c[1],&c[2],&c[3]) != EOF){
        //		printf("c[0]=%c,c[1]=%c,c[2]=%c,c[3]=%c\n",c[0],c[1],c[2],c[3]);
        //		printf("%d %d\n",(int)c[0],(int)c[1],(int)c[2]);
        if(((int)c[0]) <= ((int)c[1]) && ((int)c[0]) <= ((int)c[2])){
            if(((int)c[1]) < ((int)c[2])) printf("%c %c %c\n",c[0],c[1],c[2]);
            else printf("%c %c %c\n",c[0],c[2],c[1]);
        }
        else{
            if(((int)c[1]) <= ((int)c[0]) && ((int)c[1]) <= ((int)c[2])){
                if(((int)c[0]) < ((int)c[2])) printf("%c %c %c\n",c[1],c[0],c[2]);
                else printf("%c %c %c\n",c[1],c[2],c[0]);
            }
            else{
                if(((int)c[2]) <= ((int)c[1]) && ((int)c[2]) <= ((int)c[0])){
                    if(((int)c[1]) < ((int)c[0])) printf("%c %c %c\n",c[2],c[1],c[0]);
                    else printf("%c %c %c\n",c[2],c[0],c[1]);
                }				
            }
        }
    }	
    return 0;
}
~~~



## 蛮力算法——枚举法

~~~
for each s in S // s 是 S 中所有可能的情况
	if s is a solution then
		begin
			Write(s);
			exit the program;
		end;
~~~

~~~c++
#include<string.h>
#include <iostream>
#define MAX 50
using namespace std;

int main(){
	//1326 1.2
	int total=0;
	int n;
	while(scanf("%d",&n) != EOF){
		if(n == 0) break;
		int a[MAX];
		int sum=0;
		for(int i = 0;i < n;i++){
			scanf("%d",&a[i]);
			sum+=a[i];
		}
		int avg = sum/n;
//		if(sum%n != 0) avg++;
		int result=0;
		for(int i = 0;i < n;i++){
			if(a[i]<avg) result+=avg-a[i];
		}
		printf("Set #%d\n",++total);
		printf("The minimum number of moves is %d.\n\n",result);
	}
	
	//1326 1.1
	int total=0;
    int n=1;
    while(n != 0){
        total++;
        scanf("%d",&n);
        if(n == 0) break;
        int a[MAX];
        int sum=0;
        for(int i = 0;i < n;i++){
            scanf("%d",&a[i]);
            sum+=a[i];
        }
        int avg = sum/n;
        if(sum%n != 0) avg++;
        int result=0;
        for(int i = 0;i < n;i++){
            if(a[i]>avg) result+=a[i]-avg;
        }
        printf("Set #%d\n",total);
        printf("The minimum number of moves is %d\n\n",result); // 输出格式错误 %d ==>> %d. 
    }
    
	//1326 1.0 
	int total=0;
    int n=1;
    while(n != 0){
        total++;
        scanf("%d",&n);
        int a[MAX];
        int sum=0;
        for(int i = 0;i < n;i++){
            scanf("%d",&a[i]);
            sum+=a[i];
        }
        int avg = sum/n; //n=0 错误  
        if(sum%n != 0) avg++;
        int result=0;
        for(int i = 0;i < n;i++){
            if(a[i]>avg) result+=a[i]-avg;
        }
        printf("Set #%d\n",total);
        printf("The minimum number of moves is %d\n\n",result);
    }
    return 0;
}
~~~



## 数学问题

* 基础数学

* 概率问题

* 几何问题

* 数论问题（整除 、同余、最小公倍数、最大公约数、素数、奇数、偶数）

* 素数问题
  素数是自然数除了1以外，只能被1和它本身整除的数。
  合数n的所有因子不超过 sqrt(n)~n 的开方

  ~~~c++
  bool isPrime[n+1];
  //遍历：下标奇数为true,偶数为false
  for(i=3;i<=sqrt(n);i+=2){
      if(isPrime[i])
          for(j=i+i;j<=n;j+=i) isPrime[j]=false;
  }
  //输出：isPrime[]值为true的下标，就是n以内的素数
  prime_num=0;
  for(int i =2;i<MAX;i++){//将所有素数放在一个数组里面
      if(isPrime[i]){
          prime[prime_num++]=i;
      }
  }
  ~~~

* 组合问题

~~~c++
#include<string.h>
#include <iostream>
#define MAX 50
#include<math.h>
using namespace std;

int main(){
	//乘积的个位 
	int t;
	int a[4] = {7,9,3,1};
	scanf("%d",&t);
	for(int i = 0;i < t;i++){	
		int n;
		scanf("%d",&n);
		int r = n%4;
		printf("%d\n",a[r-1]);
		
	}

	//星际探索	
	int n;
	scanf("%d",&n);
	for(int i = 0;i < n;i++){
		int x1,y1,x2,y2;
		scanf("%d %d %d %d",&x1,&y1,&x2,&y2);
		while(x1 < x2){
			if(y1>y2){
				x1++;
				y1--;
			}else{
				x1++;
				y1++;
			}
		}
		if(x1 == x2 && y1 == y2) printf("Yes\n");
		else printf("No\n");
	}

	//ACM比赛 
	while(true){
		int c[5],c_sum=0;
		int w[5],w_sum=0;
		for(int i = 0;i < 5;i++){
			if(scanf("%d",&c[i]) == EOF) break;
//			scanf("%d",&c[i]);
			c_sum+=c[i];
		} 
		for(int i = 0;i < 5;i++){
			if(scanf("%d",&w[i]) == EOF) break;
//			scanf("%d",&w[i]);
			w_sum+=w[i];
		}
		if(c_sum < w_sum) printf("Chang win\n");
		if(c_sum > w_sum) printf("wto win\n");
		if(c_sum == w_sum) printf("TIE\n");			
	}
    	return 0;
}
~~~



## 分治、递归、递推

* 分治算法：一个规模较大的问题分解为若干个规模较小的部分（这些小问题的难度应该比原问题小），求出各部分的解，然后再把各部分的解组合成整个问题的解。

  > 基本思路
  >
  > （1）对求解建立数学模型和问题规模描述。
  >
  > （2）建立把一个规模较大的问题划分为规模较小问题的途径。
  >
  > （3）定义可以立即解决（规模最小）的问题的解决方法。
  >
  > （4）建立把若干个小问题的解合成大问题的方法。

  ~~~
  divide-and-conquer(P)
  {
      if (|P| <= n0) adhoc(P);   // 解决小规模的问题
      divide P into smaller subinstances P1,P2,...,Pk；//分解问题
      for (i=1,i<=k,i++)
        yi=divide-and-conquer(Pi);  //递归的解各子问题
      return merge(y1,...,yk);  //将各子问题的解合并为原问题的解
  }
  ~~~

* 递推：构造低阶的规模(如规模为i，一般i=0)的问题，并求出解，推导出问题规模为i+1的问题以及解，依次推到规模为n的问题。(知道第一个，推出下一个，直到达到目的。关键要找到递推公式）

* 递归：将问题规模为n的问题，降解成若干个规模为n-1的问题，依次降解，直到问题规模可求，求出低阶规模的解，代入高阶问题中，直至求出规模为n的问题的解。（要知道第一个，需要先知道下一个，直到一个已知的，再反回来，得到上一个，直到第一个。）

## 大数运算

* 乘法

  12345*4=12345+12345+12345+12345
  多位数乘一位数**，**可以直接使用加法完成

  12345\*20=123450*2
  多位数乘形如d\*10n的数，可以转换成多位数乘一位数来处理

  12345\*24=12345\*20+12345*4
  多位数乘多位数，可以转换为若干个“多位数乘形如d\*10n的数”之和

  ~~~
  算法描述如下：
  function multiply(s1,s2:string):string;
  var i:Integer;
        C:Char;
  begin
          Result:='0';
  	//多位数乘多位数，可以转换为若干个“多位数乘形如d*10n的数”之和
          for i:=Length(s2) downto 1 do
              begin
                //多位数乘一位数，可以直接相加
                for C:='1' to S2[i] do
                  Result:=addition(Result,S1);
                  //多位数乘形如d*10n的数转换成多位数乘一位数来处理
                  S1:=S1+'0';
                end;
                  Result:=Clear(Result);
                  multiply:=Result;
              end;
  
  
  ~~~

* 除法

  ~~~
  function division(s1,s2:string):string;
  begin
          //Result中存放的是商
          Result:='';
          //S中存放的是余数，S1是被除数，S2是除数
          S:='';
          //逐位试商
          for i:=1 to Length(S1) do
          begin
                  S:=S+S1[i];
                  //先试商0
                  Result:=Result+'0';
                  //然后从1开始不断往上试
                  While Bigger(S,S2) do
                     begin 
                       Inc(Result[Length(Result)]);
                       S:=Subtraction(S,S2);
                     end;
           end;
           //删除多余的0
           Result:=Clear(Result);division:=Result;
   end;           
  ~~~

  例：144/12算法执行如下

  ![image-20211109085230363](E:\软工\typora笔记\ACM笔记\ACM_note.assets\image-20211109085230363.png)

* 加法
  ![image-20211109090230988](E:\软工\typora笔记\ACM笔记\ACM_note.assets\image-20211109090230988.png)
* 减法
  ![image-20211109090317650](E:\软工\typora笔记\ACM笔记\ACM_note.assets\image-20211109090317650.png)

~~~c++
#include <iostream>
#include <stdlib.h>
#include <string>

using namespace std;
string clear(string &s){
   if(s=="")
     s='0';    
   while(s.length() >0 && s[0] == '0')
     s.erase(0,1);
   if(s=="")
     s = '0';
   return s;     
}

bool Bigger(string s1, string s2) {
   clear(s1);
   clear(s2);
   if(s1.length() > s2.length())
     return true;
   if(s1.length() == s2.length() && s1 >= s2)
     return true;
   return false;
  // return s1 >= s2;    
}

string add(string s1, string s2){
   //对齐 
   while(s1.length() < s2.length())  s1 = '0' + s1;
   while(s1.length() > s2.length())  s2 = '0' + s2;  
   //cout << x.length() << " " << y.length() << endl;
   //前面补0
   s1 = '0' + s1;
   s2 = '0' + s2; 
   for(int i=s1.length()-1; i>=0; i--)
   {
       s1[i] = s1[i] + s2[i] - '0';
       if(s1[i] > '9')
       {
          s1[i] = s1[i] - 10;
          s1[i-1] += 1;             
       }    
           
   }
   clear(s1);
   return s1;  
}

string substract(string s1, string s2){
     
     bool firstBig = true;
    //对齐
     while(s1.length() < s2.length())  s1 = '0' + s1;
     while(s1.length() > s2.length())  s2 = '0' + s2;
     if(s1 < s2 )
     {
         firstBig = false;
         string temp = s2;
         s2 = s1;
         s1 = temp;
     }  
      for(int i=s1.length()-1; i>=0; i--){
         if(s1[i] >=s2[i])
             s1[i] = s1[i]-(s2[i]-'0');
         else {
              s1[i] = s1[i] + 10;
              s1[i-1] -= 1;
              s1[i] = s1[i] - (s2[i]-'0'); 
         }            
      }             
       
     clear(s1);
     if(firstBig == false)
      s1 = '-' + s1;  
     return s1;
}

string multiply(string s1, string s2){
     
     string result = "0";
     for(int i=s2.length()-1; i>=0; i--) {
        for(int k=1; k<= s2[i]-'0'; k++) {
          result = add(result, s1);
         //cout << result << endl;          
        }
        s1 = s1 + '0' ;        
     }
       
     clear(result);
     if(result == "")
       result = "0";
     return result;
}

string divide(string s1, string s2){
     string result = "";
     string remainder = "";
     for(int i=0; i<s1.length(); i++){
        remainder = remainder + s1[i];
        result = result + '0';
        while(Bigger(remainder, s2)) {
           result[result.length()-1]++;
           remainder = substract(remainder, s2);                     
        }     
     }  
     clear(result);
     return result;
}

string module(string s1, string s2)
{
    string result = "";
    string remainder = "";
    if(Bigger(s2,s1))
      return s1;
     for(int i=0; i<s1.length(); i++){
        remainder = remainder + s1[i];
        result = result + '0';
        while(Bigger(remainder, s2)) {
           result[result.length()-1]++;
           remainder = substract(remainder, s2);                     
        }     
     }  
     clear(remainder);
     return remainder;   
}

//两个高精度数求最大公约数 gcd(a,b)
//依赖求余 
string Gcd(string a, string b){
     if(!Bigger(a, b)){
         string buf = a;
         a = b;
         b = buf;
     }
     //使用辗转相除法求最大公约数
     if(b == "0"){
         return a;
     }else{
         return Gcd(b, module(a, b)); 
     }
}
 
//两个高精度数求最小公倍数 lcm(a,b)
//依赖乘法
//依赖除法
//依赖最大公约数 
string Lcm(string a, string b){
    string buf = multiply(a, b);
    if(buf == "0"){
        return "0";
    }else{
        return divide(buf, Gcd(a, b)); 
    }
}


int main()
{
    int command;
    command = -1;
    string s1, s2, result;
    while(command !=0){
         cout << "1 - Addtion  2 - Substraction" << endl
              << "3 - Multiply  4 - Division" << endl
              << "5 - Mod      " << endl
              << "6 - GCD      7 - LCM" << endl;
         cin >> command;
         cout << "A: " ;   
         cin >> s1;
         cout << "B: ";
         cin >> s2; 
         switch(command){
            case 1: 
                 result = add(s1, s2);
                 cout << "A+B = " << result << endl << endl;
                 break;
            case 2:
                 result = substract(s1, s2);
                 cout << "A-B = " << result << endl << endl;
                 break;
            case 3:
                 result = multiply(s1, s2);
                 cout << "A*B = " << result << endl << endl;
                 break;
            case 4:                
                 result = divide(s1, s2);
                 cout << "A/B = " << result << endl << endl;
                 break; 
            case 5:
                 result = module(s1,s2);
                 cout << "A%B = " << result << endl << endl;
                 break;      
            case 6:
                 result = Gcd(s1,s2);
                  cout << "Gcd(A,B) = " << result << endl << endl;
                 break; 
            case 7:
                 result = Lcm(s1,s2);
                  cout << "Lcm(A,B) = " << result << endl << endl;
                 break;      
         }
    }     
    return 0;
}
~~~




### 1.  “&”的引用 代替指针

~~~c++
#include <iostream>
using namespace std;
#define MAXSIZE 1000
#define elementtype int

typedef struct{
	elementtype* elem;
	int length;
}Sqlist;

bool InitList(Sqlist &S);	

int main(int args, char** argv){
	Sqlist S;	//** 1
	if(InitList(S) == true) cout<< "OK";	//** 2
	else cout<< "ERROR";
}

bool InitList(Sqlist &S){	//** 3
	S.elem = new elementtype[MAXSIZE];
	if(!S.elem) {
		return false;
	}
	else{
		S.length = 0;
		return true;
	}
}
~~~


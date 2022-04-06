#### if语句（和python区别）

~~~c
#include <stdio.h>
int main()
{
    int val;
    printf("输入一个数:");
    scanf("%d",&val);
    
    if(5<val<10)
    {
        printf("1\n");
    }
    
    if(10<val<15)
    {
        printf("2");
    }
}
~~~

如果输入的是一个 6，那么结果是 1 2，而不是 1

![image-20210507194211287](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210507194211287.png)

但这显然不符合逻辑，按照正常的话，应该输出 1

这是因为C语言不支持 5<val<10 这样的写法，必须加一个 &&（与）运算符，才能保证运行结果的正确

```c
#include <stdio.h>

int main()
{
	int val;
	printf("输入一个数值:");
	scanf("%d",&val);

	if(5<val && val<10)
	{
		printf("1\n");
	}
	if(10<val && val<15)
	{
		printf("2");
	}
}
```

![image-20210507194712147](C:\Users\22773\AppData\Roaming\Typora\typora-user-images\image-20210507194712147.png)

这样子就正常了。。。

python没有 && 这个运算符，只能用and来替代，两种写法都正确。


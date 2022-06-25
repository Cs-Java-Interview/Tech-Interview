# Call by Value(값에 의한 호출)

- 기본적으로 `C언어`에서 지원하는 방식이다.
- `함수`에서 **값을 복사**해서 전달하는 방식이다.
- 인자에 전달되는 변수를 **함수의 매개변수에 복사**한다.

즉, 이렇게 복사되면 인자로 전달한 변수와는 별개의 변수가 되며, 매개변수를 변경해도 원래의 변수에는 영향을 미치지 않는다. 따라서 **원본 값을 바꿀 필요가 없는 경우**에는 Call by Value 방식을 이용하면 된다.

```c
#include <stdio.h>

void swap(int a, int b)
{
	int temp = a;
	a = b;
	b = temp;
}

int main()
{
	int a = 10;
	int b = 20;
	
	printf("swap 전 : %d %d\n", a, b);	// a = 10 b = 20
	
	swap(a, b);
	
	printf("swap 후 : %d %d\n", a, b);	// a = 10, b = 20
	
	return 0;
}
```

# Call by Reference(참조에 의한 호출)

- **주소 값을 복사해서 넘겨주는 방식**이다. 따라서 `Call by Address` 라고도 불린다.
- `C언어`에서는 이 방식을 **공식적으로 지원하지는 않지만**, Call by Address를 이용해서 Call by Reference와 같이 사용할 수 있기 때문에 일반적으로 C언어에서 `포인터`를 이용해서 주소값을 넘겨주는 방식을 Call by Reference 라고 한다.

```c
#include <stdio.h>

void swap(int *a, int *b)
{
	int temp = *a;
	*a = *b;
	*b = temp;
}

int main()
{
	int a = 10;
	int b = 20;

	printf("swap 전 : %d %d\n", a, b);

	swap(&a, &b);

	printf("swap 후 : %d %d\n", a, b);

	return 0;
}
```
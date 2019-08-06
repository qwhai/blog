
```
题目：给你一组数(未排序)，请你写设计一个程序：求出里面个数最多的数。并输出这个数的长度。
例如：给你的数是：1、 2、 3、 3、 4、 4、 5、 5、 5 、6, 其中只有6组数：1, 2, 3-3, 4-4, 5-5-5 and 6.
最长的是5那组，长度为3。所以输出3.
```

```c
#include <stdio.h>
#define MAXN 10000

/* 找出最多数目数字的个数 */
int Max(int array[], int n)
{
    int max, i;
    for(max = array[0], i = 1; i < n; ++i)
    {
        if(array[i] > max) max = array[i];
    }
    return max;
}

/* 快速排序 */
int QuickSort(int r[], int s, int t)  
{
    int i = s, j = t, temp;
    if(s < t)
    {
        temp = r[s];
        while(i != j)
        {
            while(j > i && r[j] >= temp)
                j--;
            r[i] = r[j];
            while(i < j && r[i] <= temp)
                i++;    
            r[j] = r[i];
        }    
        r[i] = temp;
        QuickSort(r, s, i-1);
        QuickSort(r, i+1, t);
    }
}

int main()
{
    int t, n, a[MAXN+1], b[MAXN];
    scanf("%d",&t);
    int i;
    for(i = 1;i <= t; ++i)
    {
        scanf("%d", &n);
        int j;
        for(j = 0; j < n; ++j)
        {
            scanf("%d", &a[j]);
            b[j] = 1;// 起始元素位置上都有一个元素，赋1
        }

        QuickSort(a, 0, n-1);// 对a数组进行快速排序(若不是用快排，可能会出现Time OUT错误)

        int k;
        for(k = 0, j = 1; j < n; ++j)// a[j-1]和a[j]组成了一个可移动的窗口向右移动
        {
            if(a[j] == a[j-1]) ++b[k];
            else ++k;
        }
        printf("%d\n", Max(b, k+1));
    }
    return 0;
}
```

# 全排列
```c++
void perm(char A[], int start, int end)//A是要排列的数组，start、end表示对A[start]与A[end]之间的元素进行全排列
{
    if (start == end)
    {
        for (int i = 0;i <= end; i++)
            cout << A[i] << "  ";
        cout << endl;
    }
    else
    {
        for (int i = start; i <= end; i++)
        {
            swap(A[i],A[start]);
            perm(A, start + 1, end);
            swap(A[i], A[start]);
        }
    }
}
```

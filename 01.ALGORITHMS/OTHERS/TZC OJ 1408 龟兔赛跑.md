
```c++
#include <iostream>
#include <cstring>
using namespace std;

int main()
{
    int i, j;
    int n;
    int N, C, T;// N:充电站点, C:电动车冲满电后行驶的距离, T:每次充电所需要的时间
    int L, VR, VT1, VT2;//L:路长, VR:兔子跑步的速度, VT1:乌龟开电动车的速度, VT2:乌龟脚蹬电动车的速度
    int station[105], s;// station保存路中的充电站点离起始点的距离
    double time, Min;
    double dp[105];
    while (cin>>L)
    {
        cin>>N>>C>>T;
        cin>>VR>>VT1>>VT2;
        memset(dp, 0, sizeof(dp));
        memset(station, 0, sizeof(station));
        for(i = 1; i <= N; ++i)
        {
            cin>>station[i];
        }

        station[N+1] = L;// 设终为最后一个充电站点

        for (i = 1;i <= N + 1; ++i)
        {
            Min = 0x7fffffff;// 取一个很大的值
            for (j = 0; j < i; ++j)// 计算每次从第0个站点(出发站点)出发到目前站点的time
            {
                if (station[i] - station[j] > C)// 如果两个站点间距离比满电状态能行驶距离大
                {
                    time = 1.0 * C / VT1 + (station[i] - station[j] - C) * 1.0 / VT2;
                }
                else// 如果两个站点间距离比满电状态能行驶距离小
                {
                    time = 1.0 * (station[i] - station[j]) / VT1;
                }
                if (j)// 不是在初始站的充满电都要加上充电时间
                {
                    time += T;
                }
                time += dp[j];
                Min = min(Min, time);
            }
            dp[i] = Min;// 记录乌龟到每个站点的最小时间
        }
        if (dp[N+1] < 1.0 * L / VR)
        {
            cout<<"What a pity rabbit!"<<endl;
        }
        else
        {
            cout<<"Good job,rabbit!"<<endl;
        }
    }
    return 0;
}
```

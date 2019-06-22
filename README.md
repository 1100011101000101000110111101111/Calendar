/ /calendar日历 C++
//////////////////////////////////////////////////////////////////
#include <iostream>
using namespace std;
short Getweek(short Y, short M, short Day); //根据年份，月份，及该月号数，返回该年该月该日的星期数
bool GetLeapY(short Y);                     //根据年份判断该年是否闰年
short GetMday(short M, short Y);            //根据年份月份 返回该月总天数

int main()
{
  int i = 0;
  short Calendar_Y, Calendar_M, Calendar_D,Calendar_W; //Calendar_Y/M/Day/D/W年份，月份，该月份的总天数，该月1号星期
  for (;;)
  {
    short Calendar_Data[42] = {0}; //日期表格
    cout << "please enter Year: "; //输入年份
    cin >> Calendar_Y;
    cout << "please enter Mon: "; //输入月份
    cin >> Calendar_M;
    cout << "Sun   "                              //周日
         << "Mon   "                              //周一
         << "Tue   "                              //周二
         << "Wed   "                              //周三
         << "Thur  "                              //周四
         << "Fri   "                              //周五
         << "Sat   " << endl;                     //周六
    Calendar_D = GetMday(Calendar_M, Calendar_Y); //该年该月总天数
  // cout<<"Calendar_D  value:"<<Calendar_D<<endl;
    if (Calendar_M > 2)                           //大于2月偶30天，奇31天
    {
      Calendar_W = Getweek(Calendar_Y, Calendar_M, 1); //该月1号时星期数
    }
    else
    {
      Calendar_W = Getweek(Calendar_Y - 1, Calendar_M + 12, 1); //根据蔡勒公式可以知道1月和2月为上一年的13月和14月
    }
   // cout << "Calendar_W  value:" << Calendar_W << endl;
    Calendar_Data[Calendar_W] = 1; //第一周的该星期数就是1号
    for (i = 0; i < Calendar_D - 1; i++)
      Calendar_Data[Calendar_W + i + 1] = Calendar_Data[Calendar_W + i] + 1;
      //下个月日期
    for (i = Calendar_D + Calendar_W, Calendar_Data[i] = 1; i < 41; ++i)
      Calendar_Data[i + 1] = Calendar_Data[i] + 1;
      //上个月日期
    if (Calendar_M>1) {
        Calendar_D=GetMday(Calendar_M-1,Calendar_Y);
      }
      else
      {
        Calendar_D=GetMday(12,Calendar_Y-1);
      }
     for(i=Calendar_W-1,Calendar_Data[i]=Calendar_D;i>0;i--)
      Calendar_Data[i-1]=Calendar_Data[i]-1;
    //打印输出
    for (i = 0; i < 42; i++)
    {
      cout.setf(ios_base::left, ios_base::adjustfield); //使用左对齐
      cout.width(5);                                    //输出宽度为5
      cout << Calendar_Data[i] << " ";
      if ((i + 1) % 7 == 0) //每周换行一次
      {
        cout << "\n";
        //if ((i > 28 && Calendar_Data[i] < 7)||(Calendar_Data[i]==31))//去掉多余的日期显示
         // break;
      }
    }
  };
  return 0;
}

bool GetLeapY(short Y) //求闰年
{
  bool GResult = false;
  GResult = (Y % 4 == 0 && Y % 100 == 0) || (Y % 400 == 0);
  return GResult;
}

short Getweek(short Y, short GM, short Day) //求该年该月该日星期数
{
  short Gweek;
  short GCentury = 21;
  Gweek = Y + (short)Y / 4 - 2 * GCentury + (short)26 * (GM + 1) / 10 + GCentury - 1;
  return Gweek % 7;
}
short GetMday(short M, short Y) //获取
{
  short Gday = 0;
  if (M == 1 || M == 3 || M == 5 || M == 7 || M == 8 || M == 10 ||M==12)
    Gday = 31;
  else if (M == 4 || M == 6 || M == 9 || M == 11)
    Gday = 30;
  else if (M == 2 && (GetLeapY(Y)))
    Gday = 29;
  else
    Gday = 28;
  return Gday;
}

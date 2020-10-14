#include"Big_Data.h"

int main()
{
	Big_Data b; int na;
	cout << "请输入要计算的数字：" << endl;
	cin >> na;
	b.Factor(na);
	cout << "n=" << na<<", n!=";
	b.Print();
	cout << endl;
	//system("pause");
	return 0;
}

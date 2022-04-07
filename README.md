# windows1
这是一个有关委托的作业
using System; using System.Collections.Generic; using System.Linq; using System.Text; using System.Threading.Tasks;

namespace qq {
class Program { //储蓄卡类 class depositCard { //储蓄名 private string name; //储蓄金额 private int depositMoney; //基础储蓄金额 public depositCard(string name, int money) { this.name = name; this.depositMoney = money; } //获取储蓄金额 public int getDepositMoney() { return this.depositMoney; } public void setDepositMoney(int r) { this.depositMoney = r; } } //信用卡类 class creditCard { //信用卡持有者名称 public string name; //信用卡余额 public int creditMoney; //扣款日期 public int dueDay; //绑定储蓄卡对象 public depositCard DC; //初始化 public creditCard(string name, int CM, int DD, depositCard DC) { this.name = name; this.creditMoney = CM; this.dueDay = DD; this.DC = DC; } //还款 public void repayM() { Console.WriteLine("今天是{0}", DateTime.Now); Console.WriteLine("用户{0}当前金额{1}", this.name, this.DC.getDepositMoney()); DC.setDepositMoney(this.DC.getDepositMoney() + creditMoney); Console.WriteLine("用户已还款"); Console.WriteLine("还款金额:{0}", Math.Abs(this.creditMoney)); Console.WriteLine("余下金额为{0}", DC.getDepositMoney()); Console.WriteLine(); } //无需还款 public void norepayM() { Console.WriteLine("今天是{0}", DateTime.Now); Console.WriteLine("用户{0}当前金额{1}", this.name, this.DC.getDepositMoney()); Console.WriteLine("用户未到还款日期，无需还款", this.name); Console.WriteLine(); } }
//扣款委托类
    class repayDelegate
    {
        //扣款委托
        public delegate void repayMoney();
        //扣款事件
        public event repayMoney DoRepay;
        //事件执行
        public void NotifyRepay()
        {
            if (DoRepay != null)
            {
                Console.WriteLine("触发事件：");
                // 触发事件
                DoRepay();
            }
        }
    }

    static void Main(string[] args)
    {
        //初始化数据
        depositCard D1 = new depositCard("小明", 10000);
        depositCard D2 = new depositCard("小花", 10000);
        depositCard D3 = new depositCard("小璟", 10000);

        creditCard C1 = new creditCard("小明", -3500, 1, D1);
        creditCard C2 = new creditCard("小花", -3000, 8, D2);
        creditCard C3 = new creditCard("小璟", -1000, 20, D3);
        List<creditCard> cCards = new List<creditCard>();
        cCards.Add(C1);
        cCards.Add(C2);
        cCards.Add(C3);
        //创建委托对象
        repayDelegate rD = new repayDelegate();

        foreach (creditCard C in cCards)
        {
            //判断是否到了该还款的日期
            if (C.dueDay == int.Parse(DateTime.Now.ToString("yyyy-MM-dd").Split('-')[2]))
            {
                //事件添加
                rD.DoRepay += new repayDelegate.repayMoney(C.repayM);
            }
            else
            {
                rD.DoRepay += new repayDelegate.repayMoney(C.norepayM);
            }
        }
        //事件执行
        rD.NotifyRepay();
    }
	
}

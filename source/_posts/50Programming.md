---
title: 50道经典的JAVA编程题
---
【程序1】 TestRabbit.java 
题目：古典问题：有一对兔子，从出生后第3个月起每个月都生一对兔子，小兔子长到第三个月后每个月又生一对兔子，假如兔子都不死，问每个月的兔子总数为多少？ 
1.程序分析： 兔子的规律为数列1,1,2,3,5,8,13,21....
解答：
	package test50;

	public class TestRabbit {

    public static int sumRabbitNumber(int m){

        int n = 1;//第0个月对数

        int e = 0;//第0个月对数

        int cup = 0;

        for(int i=1; i<m; i++){

            cup = n;

            n = e + n;

            e = cup;

        }

        return n;

    }

    

    public static void main(String[] args) {

        for(int i=1; i<=10; i++){

            System.out.print(sumRabbitNumber(i)+",");

        }

    }

}
【程序2】 FindPrimeNumber.java 
题目：判断101-200之间有多少个素数，并输出所有素数。 
1.程序分析：判断素数的方法：用一个数分别去除2~sqrt(n)或者2~n/2,常用2~n/2，

因为一个数的一半的平方大于其本身是从5开始的，解方程：n/2的平方>n .如果能被整除， 
则表明此数不是素数，反之是素数。
解答：
	package test50;
	public class FindPrimeNumber {

    public static boolean isPrimeNumber(int n){

        if(n == 2) return true;

        for(int i=2; i<=n/2; i++){

            if(n % i == 0) return false;

        }

        return true;

    }

    public static void main(String[] args) {

        int n = 0;

        for(int i=101; i<=200; i++){

            if(isPrimeNumber(i)){

                n++;

                System.out.print(i + ",");

            }

        }

        System.out.println("\n101-200之间有"+n+"个素数");

    }

}
【程序3】FindDaffodilNumber.java 
题目：打印出所有的"水仙花数"，所谓"水仙花数"是指一个三位数，其各位数字立方和等于该数本身。例如： 
153是一个"水仙花数"，因为153=1的三次方＋5的三次方＋3的三次方。 
1.程序分析：利用for循环控制100-999个数，每个数分解出个位，十位，百位。
解答：
	package test50;
	public class FindDaffodilNumber {

    public static boolean isDaffodNumber(int n){

        char[] ch = String.valueOf(n).toCharArray();

        int cup = 0;

        for(int i=0; i<ch.length; i++){

            cup = cup + (int)Math.pow(Integer.parseInt(String.valueOf(ch[i])), 3) ;

        }

        return (cup == n);

    }

    

    public static void main(String[] args) {

        for(int i=100; i<1000; i++){

            if(isDaffodNumber(i)){

                System.out.print(i + ",");

            }

        }

    }

}
【程序4】FenJie.java 
题目：将一个正整数分解质因数。例如：输入90,打印出90=2*3*3*5。 
程序分析：对n进行分解质因数，应先找到一个最小的质数k，然后按下述步骤完成： 
(1)如果这个质数恰等于n，则说明分解质因数的过程已经结束，打印出即可。 
(2)如果n<>k，但n能被k整除，则应打印出k的值，并用n除以k的商,作为新的正整数你n,重复执行第一步。 
(3)如果n不能被k整除，则用k+1作为k的值,重复执行第一步。
解答：
	package test50;

	import java.io.BufferedReader;

	import java.io.IOException;

	import java.io.InputStreamReader;

	/**

 *程序分析：对n进行分解质因数，应先找到一个最小的质数k，然后按下述步骤完成： 

 *(1)运用两层循环

 *(2)外循环得到2~n之间的所有质数，内循环将n循环除以质数，知道不能整除

 *(3)要是内循环n等于1了就说明n被完全整除了

 */

	public class Explode {

    public static boolean isPrimeNumber(int n){

        if(n == 2) return true;

        for(int i=2; i<=n/2; i++){

            if(n % i == 0) return false;

        }

        return true;

    }

    public static void main(String[] args) {

        BufferedReader buffer = new BufferedReader(new InputStreamReader(

                System.in));

        int N = 0;
        try {

            N = Integer.parseInt(buffer.readLine());

        } catch (IOException e) {

            e.printStackTrace();

        }

        System.out.print(N+"=");

        for(int i=2; i<N; i++){

            if(!isPrimeNumber(i)) continue;

            while(N%i == 0){

                System.out.print(i);

                N = N/i;

                if(N != 1) System.out.print("*");

                else break;

            }

        }

        if(N != 1) System.out.println(N);

    }

}
【程序5】 ConditionOperator.java 
题目：利用条件运算符的嵌套来完成此题：学习成绩>=90分的同学用A表示，60-89分之间的用B表示，60分以下的用C表示。 
1.程序分析：(a>b)?a:b这是条件运算符的基本例子。
解答：
	package test50;

	import java.io.BufferedReader;

	import java.io.IOException;

	import java.io.InputStreamReader;


	public class ConditionOperator {

		public static void main(String[] args) {

			BufferedReader buffer = new BufferedReader(new InputStreamReader(

					System.in));

			int N = 0;

			try {

				N = Integer.parseInt(buffer.readLine());

			} catch (IOException e) {

				e.printStackTrace();

			}

			System.out.println("学习成绩为：" + ((N < 60) ? "C" : (N < 90) ? "B" : "A"));

		}

	}
【程序6】GcdTest.java辗转相除法 
题目：输入两个正整数m和n，求其最大公约数和最小公倍数。 
1.程序分析：利用辗除法。
 2.辗转相除法基于如下原理：两个整数的最大公约数等于其中较小的数和两数的相除余数的最大公约数。 

 3.最小公倍数等于两数之积除以最大公约数

解答：
	package test50;

	import java.io.BufferedReader;

	import java.io.IOException;

	import java.io.InputStreamReader;


	public class GCDAndLCM {

		public static int getGCDNormal(int m, int n){//最大公约数普通求法

			int i = (m > n ? n : m);

			for(; i>1; i--){

				if(m%i == 0 && n%i == 0)

					return i;

			}

			return 1;

		}

		

		public static int getGCD(int m, int n){//最大公约数辗转相除法

			if(m%n == 0) return n;

			else return getGCD(n, m%n);//递归辗转相除

		}

		

		public static int getLCM(int m, int n){

			return (m * n)/getGCD(m, n);//最小公倍数等于两数之积除以最大公约数

		}

		

		public static void main(String[] args) {

			BufferedReader buffer = new BufferedReader(new InputStreamReader(

					System.in));

			int m = 0, n = 0;

			try {

				m = Integer.parseInt(buffer.readLine());

				n = Integer.parseInt(buffer.readLine());

			} catch (IOException e) {

				e.printStackTrace();

			}

			System.out.println("最小公倍数:" + getLCM(m, n));

			System.out.println("最大公约数:" + getGCD(m, n));

		}

	}
【程序7】 StChar.java 
题目：输入一行字符，分别统计出其中英文字母、空格、数字和其它字符的个数。 
1.程序分析：利用while语句,条件为输入的字符不为'n'.
解答：
	package test50;

	import java.io.BufferedReader;

	import java.io.IOException;

	import java.io.InputStreamReader;

	import java.util.HashMap;

	import java.util.Iterator;

	import java.util.Map;

	import java.util.Set;

	/**

	*1.程序分析：利用循环,使用Map存储数据.其实完全可以使用4个变量来解决,这边舍近求远复习下Map啦

	*/

	public class StChar {

		public static Map<Integer, Integer> countChar(String str){

			Map<Integer, Integer> m = new HashMap<Integer, Integer>();

			m.put(1, 0);

			m.put(2, 0);

			m.put(3, 0);

			m.put(4, 0);

			char[] ch = str.toCharArray();

			for(int i=0; i<ch.length; i++){

				if(ch[i] >= 'a' && ch[i] <= 'z' || ch[i] >= 'A' && ch[i] <= 'Z')

					m.put(1, m.get(1) + 1);

				else if(ch[i] == ' ')

					m.put(2, m.get(2) + 1);

				else if(ch[i] >= '0' && ch[i] <= '9')

					m.put(3, m.get(3) + 1);

				else

					m.put(4, m.get(4) + 1);

			}

			return m;

		}

		

		public static void main(String[] args) {

			BufferedReader buffer = new BufferedReader(new InputStreamReader(

					System.in));

			String str = null;

			try {

				str = buffer.readLine();

			} catch (IOException e) {

				e.printStackTrace();

			}

			Map<Integer, Integer> m = countChar(str);

			Set<Integer> keys = m.keySet();

			Iterator<Integer> it = keys.iterator();

			int n, k;

			while(it.hasNext()){

				k = it.next();

				n = m.get(k);

				switch (k) {

				case 1:

					System.out.println("英文字母: "+n);

					break;

				case 2:

					System.out.println("空格: "+n);

					break;

				case 3:

					System.out.println("数字: "+n);

					break;

				case 4:

					System.out.println("其它字符: "+n);

					break;

				default:

					break;

				}

			}

		}

	}
【程序8】 TestAdd.java 
题目：求s=a+aa+aaa+aaaa+aa...a的值，其中a是一个数字。例如2+22+222+2222+22222(此时共有5个数相加)，几个数相加有键盘控制。 
1.程序分析：关键是计算出每一项的值。
解答：
	package test50;

	import java.io.BufferedReader;

	import java.io.IOException;

	import java.io.InputStreamReader;



	public class TestAdd {

		public static long sumAdd(int a, int n){

			long cup = 0;

			long ans = 0;

			for(int i=0; i<n; i++){

				cup = 0;

				for(int j=0; j<=i; j++){

					cup = cup + a * (long)Math.pow(10, j);

				}

				ans = ans + cup;

			}

			return ans;

		}

		public static void main(String[] args) {

			BufferedReader buffer = new BufferedReader(new InputStreamReader(

					System.in));

			int n = 0;

			try {

				n = Integer.parseInt(buffer.readLine());

			} catch (IOException e) {

				e.printStackTrace();

			}

			System.out.println(sumAdd(2, n));

		}

	}
【程序9】 WanShu.java 
题目：一个数如果恰好等于它的因子之和，这个数就称为"完数"。例如6=1＋2＋3.编程 找出1000以内的所有完数。
解答：
	package test50;

	public class WanShu {

		public static boolean isWanShu(int a){

			int cup = 0;

			for(int i=1; i<a; i++){

				if(a%i == 0)

					cup = cup + i;

			}

			return (cup == a);

		}

		

		public static void main(String[] args) {

			for(int i=1; i<1000; i++){

				if(isWanShu(i)){

					System.out.print(i + ",");

				}

			}

		}

	}
【程序10】TestBall.java 
题目：一球从100米高度自由落下，每次落地后反跳回原高度的一半；再落下，求它在第10次落地时，共经过多少米？第10次反弹多高？
1.程序分析:递归实现
解答：
	package test50;

	public class TestBall {

		public static double sumBallHeight(double h, int n){

			if(n == 1) return h/2;

			else return sumBallHeight(h/2, n-1);

		}

		

		public static void main(String[] args) {

			System.out.println(sumBallHeight(100, 10));

		}

	}
【程序11】 TestTN.java 
题目：有1、2、3、4个数字，能组成多少个互不相同且无重复数字的三位数？都是多少？ 
1.程序分析：可填在百位、十位、个位的数字都是1、2、3、4。组成所有的排列后再去 掉不满足条件的排列。
解答：
	package test50;

	public class TestTN {
	    public static void main(String[] args) {
	        for(int i=1; i<=4; i++){
	            for(int j=1; j<=4; j++){
	                if(j == i) continue;
	                for(int k=1; k<=4; k++){
	                    if(k == j || k == i) continue;
	                    System.out.print(i*100 + j*10 + k + ",");
	                }
	            }
	        }
	    }
	}
【程序12】 MoneyAward.java 
题目：企业发放的奖金根据利润提成。利润(I)低于或等于10万元时，奖金可提10%；利润高于10万元，低于20万元时，低于10万元的部分按10%提成，高于10万元的部分，可可提成7.5%；20万到40万之间时，高于20万元的部分，可提成5%；40万到60万之间时高于40万元的部分，可提成3%；60万到100万之间时，高于60万元的部分，可提成1.5%，高于100万元时，超过100万元的部分按1%提成，从键盘输入当月利润I，求应发放奖金总数？ 
1.程序分析：请利用数轴来分界，定位。注意定义时需把奖金定义成长整型。
解答：
	package test50;
	import java.io.BufferedReader;
	import java.io.InputStreamReader;

	public class MoneyAward {
	    public static double sumMoneyAward(double i){
	        if(i <= 10){
	            return i * 0.1;
	        }else if(i < 20){
	            return ((i - 10) * 0.075 + 1);
	        }else if(i < 40){
	            return (i - 20) * 0.05;
	        }else if(i < 60){
	            return (i - 40) * 0.03;
	        }else if(i < 100){
	            return (i - 60) * 0.015;
	        }else{
	            return (i - 100) * 0.001;
	        }
	    }
	     
	    public static void main(String[] args) {
	        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	        double I = 0;
	        try {
	            System.out.println("请输入当月利润I：(单位：万)");
	            I = Integer.parseInt(br.readLine());
	        } catch (Exception e) {
	            e.printStackTrace();
	        }
	        System.out.println("奖金总数:" + sumMoneyAward(I) + "万");
	    }
	}
【程序13】FindNumber.java 
题目：一个整数，它加上100后是一个完全平方数，再加上168又是一个完全平方数，请问该数是多少？ 
1.程序分析：在10万以内判断，先将该数加上100后再开方，再将该数加上268后再开方，如果开方后的结果满足如下条件，即是结果。请看具体分析：
解答：
	package test50;
	public class FindNumber {
	    public static void main(String[] args) {
	        for(int i=1; i<100000; i++){
	            if(Math.sqrt(i + 100) % 1 == 0 && Math.sqrt(i + 268) % 1 == 0){
	                System.out.println(i);
	//              break;
	            }
	        }
	    }
	}
【程序14】 TestDay.java 
题目：输入某年某月某日，判断这一天是这一年的第几天？ 
1.程序分析：以3月5日为例，应该先把前两个月的加起来，然后再加上5天即本年的第几天，特殊情况，闰年且输入月份大于3时需考虑多加一天。
解答：
	package test50;
	import java.io.BufferedReader;
	import java.io.InputStreamReader;

	public class TestDay {
		public static boolean isLeapYear(int y){
			if((y%4 == 0 && y%100 != 100) || y%400 == 0)
				return true;
			else 
				return false;
		}
		
		public static int sumDays(int y, int m, int d){
			int[] MonthDays = {31,28,31,30,31,30,31,31,30,31,30,31};
			if(isLeapYear(y)) MonthDays[1] = 29;
			int ans = 0;
			for(int i=0; i<m-1; i++){
				ans = ans + MonthDays[i];
			}
			return ans + d;
		}
		
		public static void main(String[] args) {
			BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
			String in = null;
			try {
				System.out.println("请输入年月日，例如：2014-01-01");
				in = br.readLine();
			} catch (Exception e) {
				System.out.println("格式错误");
			}
			int y = Integer.parseInt(in.substring(0, in.indexOf('-')));
			int m = Integer.parseInt(in.substring(in.indexOf('-') + 1, in.lastIndexOf('-')));
			int d = Integer.parseInt(in.substring(in.lastIndexOf('-') + 1));
			System.out.println(sumDays(y, m, d));
		}
	}
【程序15】Sort.java 
题目：输入三个整数x,y,z，请把这三个数由小到大输出。 
1.程序分析：我们想办法把最小的数放到x上，先将x与y进行比较，如果x>y则将x与y的值进行交换，然后再用x与z进行比较，如果x>z则将x与z的值进行交换，这样能使x最小。
解答：
	package test50;
	import java.io.BufferedReader;
	import java.io.IOException;
	import java.io.InputStreamReader;
	import java.util.ArrayList;
	import java.util.Collections;
	import java.util.Iterator;
	import java.util.List;

	/**
	* 1.程序分析：舍近求远，练习容器，可以使用List容器很简单实现。
	*/
	public class Sort {
		public static List<Double> readDouble(String str, String sp){
			List<Double> l = new ArrayList<Double>();
			int j = 0;
			for(int i=0; i<str.length(); i++){
				if(str.substring(i, i+1).equalsIgnoreCase(sp) ){
					l.add(Double.parseDouble(str.substring(j, i)));
					j = i + 1;
				}
			}
			return l;
		}
		public static void main(String[] args) {
			BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
			List<Double> l = null;
			try {
				System.out.println("输入数据，如:1,2,3,4,");
				l = readDouble(br.readLine(), ",");
			} catch (IOException e) {
				e.printStackTrace();
			}
			System.out.println(l.isEmpty());
			Collections.sort(l);
			Iterator<Double> it = l.iterator();
			while(it.hasNext()){
				System.out.print(it.next() + " ");
			}
		}
	}
【程序16】Nine.java 
题目：输出9*9口诀。 
1.程序分析：分行与列考虑，共9行9列，i控制行，j控制列。
解答：
	package test50;

	public class Nine {
	    public static void main(String[] args) {
	        for(int i=1; i<=9; i++){
	            for(int j=1; j<=i; j++){
	                System.out.print(j + "*" + i + "=" + i*j + "  ");
	            }
	            System.out.println();
	        }
	    }
	}
【程序17】MonkeyEatPeach.java 
题目：猴子吃桃问题：猴子第一天摘下若干个桃子，当即吃了一半，还不瘾，又多吃了一个 第二天早上又将剩下的桃子吃掉一半，又多吃了一个。以后每天早上都吃了前一天剩下的一半零一个。到第10天早上想再吃时，见只剩下一个桃子了。求第一天共摘了多少。 
1.程序分析：采取逆向思维的方法，从后往前推断。
解答：
	package test50;
	public class MonkeyEatPeach {
	    public static int getNum(int d){
	        if(d == 0) return 1;
	        else return (getNum(d-1) + 1) * 2;
	    }
	     
	    public static void main(String[] args) {
	        System.out.println(getNum(10));
	    }
	}
【程序18】 Prog.java 
题目：两个乒乓球队进行比赛，各出三人。甲队为a,b,c三人，乙队为x,y,z三人。已抽签决定比赛名单。有人向队员打听比赛的名单。a说他不和x比，c说他不和x,z比，请编程序找出三队赛手的名单。 
1.程序分析：判断素数的方法：用一个数分别去除2到sqrt(这个数)，如果能被整除， 则表明此数不是素数，反之是素数。
解答：
	package test50;
	public class Prog {
		public static void main(String[] args) {
			String[] team1 = {"a","b","c"};
			String[] team2 = {"x","y","z"};
			for(int i=0; i<3; i++){
				for(int j=0; j<3; j++){
					if(i == 0 && j == 0)//a说他不和x比
						continue;
					else if(i == 2 && (j == 0 || j == 2))
						continue;//c说他不和x,z比
					else{
						System.out.println(team1[i] + "<-->" + team2[j]);
					}
				}
			}
		}
	}
【程序19】LingXing.java 
题目：打印出如下图案（菱形） 
      * 
    *** 
  ***** 
******* 
  ***** 
    *** 
      * 
1.程序分析：先把图形分成两部分来看待，前四行一个规律，后三行一个规律，利用双重 for循环，第一层控制行，第二层控制列。
解答：
	package test50;
	public class LingXing {
		public static void print(int n){
			int i = 0;
			int j = 0;
			for(i=0; i<n; i++){//前四行
				for(j=0; j<n+i;j++){
					if(j < n-i-1)
						System.out.print(" ");
					else
						System.out.print("*");
				}
				System.out.println();
			}
			for(i=1; i<n; i++){//后三行
				for(j=0; j<(2*n-i-1); j++){
					if(j < i)
						System.out.print(" ");
					else
						System.out.print("*");
				}
				System.out.println();
			}
		}
		
		public static void main(String[] args) {
			print(4);
		}
	}
【程序20】TestAdd2.java 
题目：有一分数序列：2/1，3/2，5/3，8/5，13/8，21/13...求出这个数列的前20项之和。 
1.程序分析：请抓住分子与分母的变化规律。
解答：
	package test50;
	public class TestAdd2 {
		public static double add(int n){
			double ans = 0;
			double a = 2;
			double b = 1;
			double cup = 0;
			for(int i=0; i<n; i++){
				ans = ans + a/b;
				cup = a;
				a = a + b;
				b = cup;
			}
			return ans;
		}
		
		public static void main(String[] args) {
			System.out.println(add(20));
		}
	}
【程序21】TestJieCheng.java 
题目：求1+2!+3!+...+20!的和 
1.程序分析：此程序只是把累加变成了累乘。
解答：
	package test50;
	public class TestJieCheng {
		public static long jieCheng(int n){
			if(n == 1) return 1;
			else return jieCheng(n-1)*n;
		}
		
		public static void main(String[] args) {
			long ans = 0;
			for(int i=1; i<=20; i++){
				ans = ans + jieCheng(i);
			}
			System.out.println(ans);
		}
	}
【程序22】TestJieCheng2.java 
题目：利用递归方法求5!。 
1.程序分析：递归公式：fn=fn_1*4!
解答：
	public static long jieCheng(int n){
		if(n == 1) return 1;
		else return jieCheng(n-1)*n;
	}
【程序23】TestAge.java 
题目：有5个人坐在一起，问第五个人多少岁？他说比第4个人大2岁。问第4个人岁数，他说比第3个人大2岁。问第三个人，又说比第2人大两岁。问第2个人，说比第一个人大两岁。最后问第一个人，他说是10岁。请问第五个人多大？ 
1.程序分析：利用递归的方法，递归分为回推和递推两个阶段。要想知道第五个人岁数，需知道第四人的岁数,依次类推，推到第一人（10岁），再往回推。
解答：
	package test50;
	public class TestAge {
		public static int getAge(int n){
			if(n == 1) return 10;
			else return getAge(n-1) + 2;
		}
		
		public static void main(String[] args) {
			System.out.println(getAge(5));
		}
	}
【程序24】TestNumber.java 
题目：给一个不多于5位的正整数，要求：一、求它是几位数，二、逆序打印出各位数字。
解答：
	package test50;

	import java.util.ArrayList;
	import java.util.Iterator;
	import java.util.List;
	import java.util.Scanner;

	public class TestNumber {
		public static List<Integer> explodeNumber(long n){
			List<Integer> l = new ArrayList<Integer>();
			long cup = n;
			while(cup != 0){
				l.add((int) (cup%10));
				cup = cup/10;
			}
			return l;
		}
		
		public static void main(String[] args) {
			System.out.println("给一个不多于5位的正整数:");
			Scanner s = new Scanner(System.in);
			long n = 0;
			if(s.hasNext()) n = s.nextInt();
			List<Integer> l = explodeNumber(n);
			System.out.println("它是"+l.size()+"位数");
			Iterator<Integer> it = l.iterator();
			while(it.hasNext()){
				System.out.print(it.next());
			}
		}
	}
【程序25】 HuiWenShu.java 
题目：一个5位数，判断它是不是回文数。即12321是回文数，个位与万位相同，十位与千位相同。
解答：
	package test50;
	import java.util.ArrayList;
	import java.util.List;
	import java.util.Scanner;

	public class HuiWenShu {
		public static List<Integer> explodeNumber(long n){
			List<Integer> l = new ArrayList<Integer>();
			long cup = n;
			while(cup != 0){
				l.add((int) (cup%10));
				cup = cup/10;
			}
			return l;
		}
		
		public static void main(String[] args) {
			System.out.println("输入一个数:");
			Scanner s = new Scanner(System.in);
			long n = 0;
			if(s.hasNext()) n = s.nextLong();
			List<Integer> l = explodeNumber(n);
			Integer[] a = (Integer[])l.toArray(new Integer[]{});
			for(int i=0; i<=a.length/2; i++) {
				if(!a[i].equals(a[a.length-i-1])){
					System.out.println("不是回文");
					return;
				}
			}
			System.out.println("是回文");
		}
	}
	【程序26】Ex26.java (跳过了，好没意思的题啊) 
题目：请输入星期几的第一个字母来判断一下是星期几，如果第一个字母一样，则继续 判断第二个字母。 
1.程序分析：用情况语句比较好，如果第一个字母一样，则判断用情况语句或if语句判断第二个字母。
解答：
使用字符数组搞定
【程序27】 SuShu.java(详见【程序2】 FindPrimeNumber.java) 
题目：求100之内的素数
解答：
参考见【程序2】 FindPrimeNumber.java 
    public static boolean isPrimeNumber(int n){
        if(n == 2) return true;
        for(int i=2; i<=n/2; i++){
            if(n % i == 0) return false;
        }
        return true;
    }
【程序28】 TestSort.java 
题目：对10个数进行排序 
1.程序分析：可以利用选择法，即从后9个比较过程中，选择一个最小的与第一个元素交换， 下次类推，即用第 
二个元素与后8个进行比较，并进行交换。
解答：
参见【程序15】Sort.java
【程序29】 TestAdd3.java 
题目：求一个3*3矩阵对角线元素之和 
1.程序分析：利用双重for循环控制输入二维数组，再将a[i][i]累加后输出。
解答：
没啥意思的说。。。
【程序30】 ArraySort.java 
题目：有一个已经排好序的数组。现输入一个数，要求按原来的规律将它插入数组中。 
1. 程序分析：首先判断此数是否大于最后一个数，然后再考虑插入中间的数的情况，插入后此元素之后的数， 
依次后移一个位置。
解答：
	package test50;

	/**
	*1. 程序分析：使用二分法
	*/
	public class ArraySort {
		public static int[] addNumber(int[] a, int n){
			int i=0, j=a.length-1;
			int cup = 0;
			if(a[j]<n) cup = j+1;
			else if(a[i]>n) cup = i;
			else{
				while(j-i>1){
					cup = (i + j) /2;
					if(n > a[cup]){//n大于中间数
						i = cup;
					}else if(n < a[cup]){
						j = cup;
					}else{
						break;
					}
				}
				cup = j;
			}
			//插入n
			int[] newa = new int[a.length+1];
			for(i=0,j=0; i<newa.length; i++){
				if(i == cup){
					newa[i] = n;
				}else{
					newa[i] = a[j];
					j++;
				}
			}
			return newa;
		}
		
		public static void printArray(int[] a){
			for(int i=0; i<a.length; i++){
				System.out.print(a[i]+" ");
			}
		}
		
		public static void main(String[] args) {
			int[] a = new int[]{2,3,5,6,7,9,12,16};//已经排好序的数组
			a = addNumber(a, 10);
			printArray(a);
		}
	}
【程序31】 ArrayConverse.java 
题目：将一个数组逆序输出。 
1.程序分析：用第一个与最后一个交换。
解答：
	package test50;
	public class ArrayConverse {
		public static void arrayConverse(int[] a){
			int cup=0;
			for(int i=0,j=a.length-1; i<j; i++,j--){
				cup = a[i];
				a[i] = a[j];
				a[j] = cup;
			}
		}
		
		public static void printArray(int[] a){
			for(int i=0; i<a.length; i++){
				System.out.print(a[i]+" ");
			}
			System.out.println();
		}
		
		public static void main(String[] args) {
			int[] a = new int[]{1,2,3,4,5};
			printArray(a);
			arrayConverse(a);
			printArray(a);
		}
	}
【程序32】 Ex32.java 
题目：取一个整数a从右端开始的4～7位。 
程序分析：可以这样考虑： 
(1)先使a右移4位。 
(2)设置一个低4位全为1,其余全为0的数。可用~(~0<<4) 
(3)将上面二者进行&运算。
解答：
	package test50;
	public class Ex32 {
		public static int[] getNum(int a, int m, int n){
			int[] ans = new int[n-m+1];
			for(int i=1,j=0; j<ans.length && a>0;i++){
				if(i>=m && i<=n){
					ans[j] = a%10;
					j++;
				}
				a = a / 10;
			}
			return ans;
		}
		
		public static void printArray(int[] a){
			for(int i=a.length-1; i>=0; i--){
				System.out.print(a[i]+" ");
			}
			System.out.println();
		}
		
		public static void main(String[] args) {
			int a = 123456789;
			int[] ns = getNum(a, 4, 7);
			System.out.println(a);
			printArray(ns);
		}
	}
【程序33】YangHui.java 
题目：打印出杨辉三角形（要求打印出10行如下图） 
1.程序分析：  
       1 
      1 1 
     1 2 1 
    1 3 3 1 
   1 4 6 4 1 
1 5 10 10 5 1
解答：
	package test50;
	public class YangHui {
		public static int[][] getArray(int n){
			int[][] a = new int[n][n];
			for(int i=0; i<n; i++){
				for(int j=0; j<=i; j++){
					if(j == 0 || j == i){
						a[i][j] = 1;
					}else{
						a[i][j] = a[i-1][j-1] + a[i-1][j];
					}
				}
			}
			return a;
		}
		
		public static void print(int[][] a){
			for(int i=0; i<a.length; i++){
				for(int j=0; j<a.length-i-1; j++){
					System.out.print(" ");
				}
				for(int j=0; j<a[i].length && a[i][j]>0; j++){
					System.out.print(a[i][j]+" ");
				}
				System.out.println();
			}
		}
		
		public static void main(String[] args) {
			print(getArray(6));
		}

	}
【程序34】 略 前面更复杂的已经做过了 
题目：输入3个数a,b,c，按大小顺序输出。 
1.程序分析：利用指针方法。
解答：
参见【程序15】Sort.java
【程序35】 ArrayChange.java 
题目：输入数组，最大的与第一个元素交换，最小的与最后一个元素交换，输出数组。
解答：
	package test50;
	public class ArrayChange {
		public static void sort(int[] a){//类似冒泡排序
			int cup = 0;
			int l = a.length-1;
			for(int i=1; i<a.length-1; i++){
				if(a[i] > a[0]){
					cup = a[i];
					a[i] = a[0];
					a[0] = cup;
				}
				if(a[i] < a[l]){
					cup = a[i];
					a[i] = a[l];
					a[l] = cup;
				}
			}
		}
		
		public static void printArray(int[] a){
			for(int i=0; i<a.length; i++){
				System.out.print(a[i]+" ");
			}
			System.out.println();
		}
		
		public static void main(String[] args) {
			int[] a = new int[]{2,3,5,1,2,34,1,0,24};
			printArray(a);
			sort(a);
			printArray(a);
		}
	}
【程序36】 Array1.java 
题目：有n个整数，使其前面各数顺序向后移m个位置，最后m个数变成最前面的m个数
解答：
注:我不是很理解这个题,按我的理解来做吧比如n={1,2,3,4,5,6,7,8,9} 
m=2,则应该得到的n={1,2,1,2,3,4,5,1,2} 
m=3,则应该得到的n={1,2,3,1,2,3,1,2,3}
【程序37】 Test3Quit.java 
题目：有n个人围成一圈，顺序排号。从第一个人开始报数（从1到3报数），凡报到3的人退出圈子，问最后留下 
的是原来第几号的那位。
解答：
	package test50;
	import java.util.ArrayList;
	import java.util.List;
	import java.util.Scanner;
	/**
	*有200个小朋友拉成一个圆圈，从其中一个小朋友开始依次编号1－200，从1号小朋友开始循环1－3报数，
	*数到3的小朋友就退出。编写一个Java应用程序，计算出最后一个小朋友的号码是多少。
	*/
	public class Test3Quit {
		public static int play(List<Integer> l, int n, int m){
			int s = l.size();
			if(s <= 1) return l.get(0);
			else {
				n = n + m - 1;
				while(n >= s)
					n = n-s;
				l.remove(n);
				return play(l, n, m);
			}
		}
		
		public static void main(String[] args) {
			System.out.println("输入有多少个小朋友:");
			Scanner s = new Scanner(System.in);
			int n = s.nextInt();
			List<Integer> l = new ArrayList<Integer>();
			for(int i=1; i<=n; i++){
				l.add(i);
			}
			System.out.println(play(l, 0, 3));
		}
	}
【程序38】 TestLength.java 
题目：写一个函数，求一个字符串的长度，在main函数中输入字符串，并输出其长度。
解答：
	package test50;
	import java.util.Scanner;

	public class TestLength {
		public static int getLength(String str){
			return str.toCharArray().length;
		}
		
		public static void main(String[] args) {
			System.out.println("输入你的字符串：");
			String str = new Scanner(System.in).next();
			System.out.println("你的字符串长度为:"+getLength(str));
		}
	}
【程序39】 Test2.java 
题目：编写一个函数，输入n为偶数时，调用函数求1/2+1/4+...+1/n,当输入n为奇数时，调用函数 
1/1+1/3+...+1/n(利用指针函数)
注：java里面貌似没有指针函数吧！这个题是不是C++的啊！我就不纠结指针函数了，实现功能就行了 
解答：
	package test50;
	import java.util.Scanner;

	public class Test2 {
		public static double sum(int n){
			double ans = 0;
			int i = 1;
			if(n%2 == 0)
				i = 2;
			for(; i<=n;i=i+2){
				ans = ans + 1.0/i;
			}
			return ans;
		}
		
		public static void main(String[] args) {
			int n = new Scanner(System.in).nextInt();
			System.out.println(sum(n));
		}
	}
【程序40】 Test3.java 
题目：字符串排序。
注:把字符串转成char，进行排序 
解答：
	package test50;
	import java.util.Scanner;

	public class Test3 {
		public static String bubbleSort(String str){
			boolean flag = true;
			char[] chs = str.toCharArray();
			char cup = 0;
			for(int i=0; i<str.length()-1 && flag; i++){
				flag = false;
				for(int j=0; j<str.length()-1-i; j++){
					if((int)chs[j+1] < (int)chs[j]){
						flag = true;
						cup = chs[j+1];
						chs[j+1] = chs[j];
						chs[j] = cup;
					}
				}
			}
			return new String(chs);
		}
		
		public static void main(String[] args) {
			String str = new Scanner(System.in).next();
			System.out.println(bubbleSort(str));
		}
	}
【程序41】 MonkeyPeach.java 
题目：海滩上有一堆桃子，五只猴子来分。第一只猴子把这堆桃子凭据分为五份，多了一个，这只猴子把多的一 
个扔入海中，拿走了一份。第二只猴子把剩下的桃子又平均分成五份，又多了一个，它同样把多的一个扔入海中 
，拿走了一份，第三、第四、第五只猴子都是这样做的，问海滩上原来最少有多少个桃子？
解答：
	package test50;
	public class MonkeyPeach {
		public static int getNum(int n, int m){
			if(n > 5) return m*4;
			else {
				double ans = getNum(n+1, m) / 4.0 * 5 + 1;
				if(ans%1 != 0.0 || ans == 1){//判断结果是否为整数，或者结果不为1
					return 0;
				}else
					return (int)ans;
			}
		}
		
		public static void main(String[] args) {
			int ans = 0;
			for(int i=1; ; i++){
				ans = getNum(1, i);
				if(ans != 0){
					System.out.println("当最后一只猴子拿走" + i + "个桃子时，海滩上原来桃子得到最小值为：");
					System.out.println(ans);
					break;
				}
			}
		}
	}
	感觉上面的实现方案有点小题大做了，要是直接验证结果的话会更快的得到答案，看代码：
	package test50;
	public class MonkeyPeach_1 {
		public static boolean isRight(int n) {
			for(int i=0; i<5; i++) {
				if(n % 5 == 1) {
					n = n - 1;
					n = n - n / 5;
				}else
					return false;
			}
			return true;
		}

		public static void main(String[] args) {
			for(int n=1; ; n++) {
				if(isRight(n)) {
					System.out.println("海滩上原来桃子得到最小值为：" + n);
					return;
				}
			}
		}
	}
【程序42】 Test4.java 
题目：809*??=800*??+9*??+1 
其中??代表的两位数,8*??的结果为两位数，9*??的结果为3位数。求??代表的两位数，及809*??后的结果。
解答：
第一种：所有的？？代表同一个数
	package test50;
	public class Test4 {
		public static void main(String[] args) {
			for(int i=10; i<100 && 8*i<100 && 9*i<1000; i++){
				if(809*i == (800*i + 9*i + 1)){
					System.out.println("??代表的两位数:" + i);
					System.out.println("809*??后的结果" + i*809);
					return;
				}
			}
			System.out.println("没有符合的数");
		}
	}

	 
	第二种：所有的？？都不同
	package test50;
	public class Test4_1 {
		public static void main(String[] args) {
			for(int i=10; i<100; i++){
				for(int j=10; j<100 && 8*j<100; j++){
					for(int l=10; l<100 && 9*l<1000; l++){
						if(809*i == 800*j + 9*l + 1){
							System.out.println("809*"+i+" == 800*"+j+"+ 9*"+l+" + 1");
							System.out.println("809*??后的结果" + i*809);
							return;
						}
					}
				}
			}
			System.out.println("没有符合的数");
		}
	}
【程序43】 Test5.java 
题目：求0—7所能组成的奇数个数。
注：当有一位数时：有1.3.5.7这4个奇数
当有两位数时：最高为有7种（除0）选择，最低为有4（1.3.5.7为奇数的条件）种  总数为4*7
当有三位数时：最高为有7中（除0）选择，第二位有8中选择，最后一位4种（1.3.5.7） 总数 4*8*7
当有四位数时：最高为有7中（除0）选择，第三位有8中选择，第二位有8中选择，最后一位4种（1.3.5.7） 总数 4*8*8*7
解答：
	package test50;
	public class Test5 {
		public static void main(String[] args) {
			int cup = 7*4;//二位数
			int count = cup + 4;
			for(int i=2; i<8; i++){
				cup = 8*cup;
				count = count + cup;
			}
			System.out.println("0—7所能组成的奇数个数:" + count);
		}
	}
【程序44】 TestEven.java 
题目：一个偶数总能表示为两个素数之和。
解答：
	package test50;
	import java.util.Scanner;

	public class TestEven {
		public static boolean isPrimeNumber(int n){
			if(n < 2)return false;
			for(int i=2; i<n; i++){
				if(n%i == 0)
					return false;
			}
			return true;
		}
		
		public static void main(String[] args) {
			int n = new Scanner(System.in).nextInt();
			
			if(n%2 != 0){
				System.out.println("输入的不是偶数");
				return;
			}
			for(int i=2; i<n; i++){
				if(isPrimeNumber(i) && isPrimeNumber(n - i)){
					System.out.println(n + " = " + i + "+" + (n-i));
					return;
				}
			}
		}
	}
【程序45】TestPrime9.java 
题目：判断一个素数能被几个9整除
解答：
这个题貌是有问题吧，素数只能被1和自身整除啊，9都不是素数。。。无解
【程序46】 TestString.java 
题目：两个字符串连接程序
注：转换成数组的连接,实际使用String.concat()
解答：
	package test50;
	import java.util.Scanner;

	public class TestString {
		public static String connextString(String str1, String str2){
			byte[] buf = new byte[str1.length() + str2.length()];
			str1.getBytes(0, str1.length(), buf, 0);
			str2.getBytes(0, str2.length(), buf, str1.length());
			return new String(buf);
		}
		
		public static void main(String[] args) {
			Scanner s = new Scanner(System.in);
			System.out.println("输入第一个字符串:");
			String str1 = s.next();
			System.out.println("输入第二个字符串:");
			String str2 = s.next();
			System.out.println(connextString(str1, str2));
		}
	}
【程序47】 TestPrint.java 
题目：读取7个数（1—50）的整数值，每读取一个值，程序打印出该值个数的＊。
解答：
	package test50;
	import java.util.Scanner;
	public class TestPrint {
		public static void main(String[] args) {
			Scanner s = new Scanner(System.in);
			int[] a = new int[7];
			for(int i=0; i<7; i++){
				System.out.print("输入第"+(i+1)+"个整数：");
				a[i] = s.nextInt();
			}
			for(int i=0; i<7; i++){
				for(int j=0; j<a[i]; j++){
					System.out.print("*");
				}
				System.out.println();
			}
		}
	}
【程序48】 TestCode.java 
题目：某个公司采用公用电话传递数据，数据是四位的整数，在传递过程中是加密的，加密规则如下：每位数字 
都加上5,然后用和除以10的余数代替该数字，再将第一位和第四位交换，第二位和第三位交换。
解答：
	package test50;
	import java.util.ArrayList;
	import java.util.List;
	import java.util.Scanner;

	public class TestCode {
		public static void encrypt(int[] a){
			int cup = 0;
			for(int i=0; i<4; i++){
				a[i] = (a[i] + 5) % 10;
			}
			cup = a[0];
			a[0] = a[3];
			a[3] = cup;
			
			cup = a[1];
			a[1] = a[2];
			a[2] = cup;
		}
		
		public static void main(String[] args) {
			Scanner s = new Scanner(System.in);
			int[] a = new int[4];
			for(int i=0; i<4; i++){
				System.out.print("输入第"+(i+1)+"位数：");
				a[i] = s.nextInt();
			}
			encrypt(a);
			for(int i=0; i<4; i++){
				System.out.print(a[i]);
			}
		}
	}
【程序49】 TestString2.java 
题目：计算字符串中子串出现的次数
解答：
	package test50;
	import java.util.Scanner;

	public class TestString2 {
		public static int findString(String str1, String str2){
			int count = 0;
			char[] chs1 = str1.toCharArray();
			char[] chs2 = str2.toCharArray();
			for(int i=0,j=0; i<chs1.length; i++){
				for(j=0; j<chs2.length; j++){
					if(chs1[i+j] != chs2[j])
						break;
				}
				if(j == chs2.length){//完全匹配
					count++;
					i = i + j - 1;
				}
			}
			return count;
		}
		
		public static void main(String[] args) {
			Scanner s = new Scanner(System.in);
			System.out.println("输入字符串:");
			String str1 = s.next();
			System.out.println("输入要查找的字符串:");
			String str2 = s.next();
			System.out.println("查找到的个数：" + findString(str1, str2));
		}
	}
【程序50】TestStu.java 
题目：有五个学生，每个学生有3门课的成绩，从键盘输入以上数据（包括学生号，姓名，三门课成绩），计算 
出平均成绩，况原有的数据和计算出的平均分数存放在磁盘文件"stud"中。
解答：
	package test50;
	import java.io.BufferedWriter;
	import java.io.File;
	import java.io.FileWriter;
	import java.io.IOException;
	import java.util.Scanner;

	public class TestStu {
		public static void main(String[] args) {
			int n = 5;//人数
			Scanner s = new Scanner(System.in);
			String[] num = new String[n];
			String[] name = new String[n];
			float[][] grade = new float[n][3];
			float[] ave = new float[n];
			//输入数据
			for(int i=0; i<n; i++){
				System.out.print("学号:");
				num[i] = s.next();
				System.out.print("姓名:");
				name[i] = s.next();
				for(int j=0; j<3; j++){
					System.out.print("第"+(j+1)+"门课成绩:");
					grade[i][j] = s.nextFloat();
				}
			}
			//处理数据
			for(int i=0; i<n; i++){
				for(int j=0; j<3; j++){
					ave[i] += grade[i][j];
				}
				ave[i] /= 3;
			}
			//写入文件
			try {
				FileWriter fw = new FileWriter(new File("d://stud.txt"));
				BufferedWriter bw  = new BufferedWriter(fw);
				for(int i=0; i<n; i++){
					bw.write("学号:" + num[i] + "  ");
					bw.write("姓名:" + name[i] + "  ");
					bw.write("成绩:{  ");
					for(int j=0; j<3; j++){
						bw.write(grade[i][j] + "  ");
					}
					bw.write("} ");
					bw.write("平均成绩: " + ave[i]);
					bw.newLine();
				}
				bw.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	}

原文地址：https://www.cnblogs.com/VellBibi/p/3500671.html
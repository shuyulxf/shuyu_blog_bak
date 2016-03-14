title: PSO
tags: [learning,AI]
type: tags
date: 2015-12-20 15:37:55
---
## ** 粒子群优化算法解特定问题（java实现） **
实现一个粒子群优化算法，求解函数
f(x)= x(3) − 5x(2) − 2x + 3 (其中x(x)为x的n次方)
在取值范围[-2,5]之间的最小值和最大值
<!-- more -->
** 问题描述 **
计算每一个x值的f(x)的取值
设置位置x的取值范围，粒子的速度值，一般粒子的速度取值为位置的10%~20%大小
<pre>
public class Problem {
	private static int three = 1;
	private static int two = -5;
	private static int one = -2;
	private static int zero = 3;
	
	public static double X_LOW  = -2;
	public static double X_HIGH = 5;
	public static double V_LOW  = -0.4;
	public static double V_HIGH = 1.5;
	
	public static double getResult(double x) {
		double y = 0;
		
		y = Problem.three*x*x*x +
			Problem.two*x*x +
			Problem.one*x + Problem.zero;
		return y;
	}
}
</pre>

** 定义粒子的类 **
定义粒子的位置x和速度v，和该粒子从出现到当前最好的位置和取值即Pbest
每一个粒子在一个状态下,都有一个x、v和Pbest
<pre>
public class Particle {
	private double x;
	public double v;
	private double [] pBest = {0, 0};
	
	public Particle(){};
	public Particle(double x, double v) {
		//super();
		this.x = x;
		this.v = v;
		this.pBest[0] = x;
		this.pBest[1] = Problem.getResult(x);
	}
	
	public double getX() {
		return this.x;
	}
	
	public void setX(double x) {
		this.x = x;
	}
	
	public double getV() {
		return this.v;
	}
	
	public void setV(double v) {
		this.v = v;
	}
	
	public double [] getPbest() {
		return this.pBest;
	}
	
	public void setPbest(double x, boolean flag ) {
		double newPbest = Problem.getResult(x);
		if ( flag && newPbest > this.pBest[1] ){
			 this.pBest[0] = x;
		     this.pBest[1] = newPbest;
		}
	}
}

</pre>

** 算法实现 **
<pre>
import java.util.Random;
import java.util.Vector;
public class PSO {
	private double [] gBest;
	private static int gNum = 0;
	private static int MAX_N_UPT_NUM = 100; 
	private int particleNum;
	private int [] c = {2, 2};
	private double [] r = {0, 0};
	//定义粒子，一般问题粒子数目取30~100个
	private Vector<Particle> paticals = new Vector<Particle>();
	
	Random rdm = new Random();
	double x_high = Problem.X_HIGH;
	double x_low  = Problem.X_LOW;
	double v_high = Problem.V_HIGH;
	double v_low  = Problem.V_LOW;
	double x_gap = x_high - x_low;
	double v_gap = v_high - v_low;
	
	private PSO(int particleNum){
		this.particleNum = particleNum;
	}
	
	//生成一个粒子并返回，完成该粒子的所有初始化
	private Particle initParticle(){
		double x = x_low + rdm.nextDouble() * x_gap;
		double vx = v_low + rdm.nextDouble() * v_gap;
		Particle p = new Particle(x,vx);
	
		return  p;
	}
	
	//生成所有粒子，并且初始化全局最优解
	private void init( boolean flag ) {
		gBest = new double[2];
		gBest[0] = 5;
		gBest[1] = 3;
		
		for( int i = 0; i < this.particleNum; i++ ){
			Particle p = initParticle();
			this.paticals.add(p);
			this.updateGBest( p, flag );
		}
	}
	
	// 更新全局最优
	private void updateGBest( Particle p, boolean flag) {
		double [] rlt = p.getPbest();
		if( flag && rlt[1] > this.gBest[1] || !flag && rlt[1] < this.gBest[1] ) {
			this.gBest[0] = rlt[0];
			this.gBest[1] = rlt[1];
		}
	}
	
	//更新粒子的速度，并返回
	private double updateV( Particle p ) {
		double r1 = rdm.nextDouble();
		double r2 = rdm.nextDouble();
		double v = p.getV() + this.c[0] * r1 * ( p.getPbest()[0] - p.getX() )
					 		  + this.c[1] * r2 * ( this.gBest[0] - p.getX() );
		if( v >  v_high ) v = v_high;
		if( v < v_low ) v = v_low;
		p.setV(v);
		
		return v;
	}
	
	// 更新粒子的位置，并返回
	private double updateX( Particle p, double v, boolean flag ) {
		double x = p.getX() + v;
		if( x >  x_high ) x = x_high;
		if( x < x_low ) x = x_low;
	    p.setX( x );
	    p.setPbest(x, flag);
	    
		return x;
	}
	
	// 更新所有粒子的状态，即从x[n]就算x[n+1]的所有粒子的状态(x[n]表示第n次迭代)
	// 包括计算速度，计算当前x，计算某粒子的历史最优，和所有粒子的全局最优
	private void updateState( boolean flag ) {
		for( int i = 0; i < this.particleNum; i++ ) {
			Particle p = this.paticals.get(i);
			double v = updateV(p);
			updateX( p, v, flag);
			updateGBest( p, flag );
		} 
	}

	//执行求解
	public void execute(){
		//求最大值
		init( true );
		while( PSO.gNum < 100){
			updateState(true);
			PSO.gNum++;
		}
		System.out.printf("The max value is %.16f, and the x is %.16f\n", this.gBest[1], this.gBest[0] );
		this.paticals.clear();
		
		//求最小值
		init( false );
		while( PSO.gNum < 100000){
			updateState(false);
			PSO.gNum++;
		}
		System.out.printf("The min value is %.16f, and the x is %.16f", this.gBest[1], this.gBest[0] );
	}
	
	public static void main(String[] args) {
		PSO pso = new PSO(30);
		pso.execute();
	}

}
</pre>
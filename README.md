package calculator;//包名

//导入对应的jar包
import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Dimension;
import java.awt.Font;
import java.awt.GridLayout;
import java.awt.Window;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowStateListener;
import java.text.DecimalFormat;
import javax.swing.BorderFactory;
import javax.swing.ButtonGroup;
import javax.swing.JButton;
import javax.swing.JCheckBoxMenuItem;
import javax.swing.JFrame;
import javax.swing.JMenu;
import javax.swing.JMenuBar;
import javax.swing.JMenuItem;
import javax.swing.JPanel;
import javax.swing.JRadioButtonMenuItem;
import javax.swing.JTextField;

//计算器类
public class Calucator extends JFrame {
	private JTextField tf; // 显示计算结果框
	private JPanel panel1, panel2, panel3, panel4; // 四个面板
	private JMenuBar myBar; // 创建一个水平菜单栏
	private JMenu menu;// 菜单项
	private JMenuItem game2048; // 游戏菜单子项
	private boolean IfResult = true, flag = false;
	private String oper = "=";// 运算操作符
	private double result = 0;// 保存计算结果
	private Num numActionListener; // 数字监听类对象
	private DecimalFormat df; // 将数字格式化

	// 无参(默认)构造函数
	public Calucator() {
		super("GD的科学计算器");// 设置标题栏（调用超类的构造函数）
		df = new DecimalFormat("#.####");// 保留四位小数
		this.setLayout(new BorderLayout(10, 5));// 按钮水平、纵向间距分别为10,5
		panel1 = new JPanel(new GridLayout(1, 3, 10, 10));
		panel2 = new JPanel(new GridLayout(5, 6, 5, 5));// 5行6列 ,按钮水平、纵向间距均为5
		panel3 = new JPanel(new GridLayout(4, 1, 5, 5));
		panel4 = new JPanel(new BorderLayout(5, 5));

		/**
		 * 菜单栏
		 */
		myBar = new JMenuBar();
		menu = new JMenu("小游戏(G)");
		menu.setFont(new Font("微软雅黑", Font.PLAIN, 15));

		/**
		 * 小游戏
		 */
		game2048 = new JMenuItem("2048小游戏");
		menu.add(game2048);
		myBar.add(menu);
		this.setJMenuBar(myBar);
		numActionListener = new Num();// 实现数字监听

		/**
		 * 文本域，即为计算器的屏幕显示区域
		 */
		tf = new JTextField();
		tf.setEditable(false);// 文本区域不可编辑
		tf.setBackground(Color.white);// 文本区域的背景色
		tf.setHorizontalAlignment(JTextField.RIGHT);// 文字右对齐
		tf.setText("0");
		tf.setBorder(BorderFactory.createLoweredBevelBorder());
		init();// 对计算器进行初始化

	}

	/**
	 * 初始化操作 添加按钮
	 */
	private void init() {
		game2048.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent e) {
				// TODO Auto-generated method stub
                Copy2048 copy2048=new Copy2048();
                copy2048.main(null);
			}
		});

		addButton(panel1, "Producer",new Signs(), Color.magenta);
		addButton(panel1, "Message", new Signs(), Color.magenta);
		addButton(panel1, "Backspace", new Clear(), Color.red);
		addButton(panel1, "Clear", new Clear(), Color.red);
		
		
		addButton(panel2, "1/x", new Signs(), Color.magenta);
		addButton(panel2, "ln", new Signs(), Color.magenta);
		addButton(panel2, "7", numActionListener, Color.blue);
		addButton(panel2, "8", numActionListener, Color.blue);
		addButton(panel2, "9", numActionListener, Color.blue);
		addButton(panel2, "÷", new Signs(), Color.red);

		addButton(panel2, "n!", new Signs(), Color.magenta);
		addButton(panel2, "sqrt", new Signs(), Color.magenta);
		addButton(panel2, "4", numActionListener, Color.blue);
		addButton(panel2, "5", numActionListener, Color.blue);
		addButton(panel2, "6", numActionListener, Color.blue);
		addButton(panel2, "×", new Signs(), Color.red);

		addButton(panel2, "sin", new Signs(), Color.magenta);
		addButton(panel2, "x^2", new Signs(), Color.magenta);
		addButton(panel2, "1", numActionListener, Color.blue);
		addButton(panel2, "2", numActionListener, Color.blue);
		addButton(panel2, "3", numActionListener, Color.blue);
		addButton(panel2, "-", new Signs(), Color.red);

		addButton(panel2, "cos", new Signs(), Color.magenta);
		addButton(panel2, "x^3", new Signs(), Color.magenta);
		addButton(panel2, "0", numActionListener, Color.blue);
		addButton(panel2, "-/+", new Clear(), Color.blue);
		addButton(panel2, ".", new Dot(), Color.blue);
		addButton(panel2, "+", new Signs(), Color.red);

		addButton(panel2, "tan", new Signs(), Color.magenta);
		addButton(panel2, "cube", new Signs(), Color.magenta);
		addButton(panel2, "π", numActionListener, Color.orange);
		addButton(panel2, "e", numActionListener, Color.orange);
		addButton(panel2, "′″", new Signs(), Color.orange);
		addButton(panel2, "=", new Signs(), Color.red);

		addButton(panel3, "MC", null, Color.black);
		addButton(panel3, "MR", null, Color.black);
		addButton(panel3, "MS", null, Color.black);
		addButton(panel3, "M+", null, Color.black);

		panel4.add(panel1, BorderLayout.NORTH);
		panel4.add(panel2, BorderLayout.CENTER);
		this.add(tf, BorderLayout.NORTH);
		this.add(panel3, BorderLayout.WEST);
		this.add(panel4);

		pack();
		this.setResizable(true);// 设置窗口是否可改变大小
		this.setLocation(300, 200);
		this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
	}

	/**
	 * 统一设置按钮的的使用方式
	 */
	private void addButton(JPanel panel, String name, ActionListener action, Color color) {
		JButton bt = new JButton(name);
		panel.add(bt);// 在面板上增加按钮
		bt.setForeground(color);// 设置前景（字体）颜色
		bt.addActionListener(action);// 增加监听事件
	}

	/**
	 * 计算器的基础操作（+ - × ÷）
	 * 
	 * @param x
	 */
	private void getResult(double x) {
		if (oper == "+") {
			result += x;
		} else if (oper == "-") {
			result -= x;
		} else if (oper == "×") {
			result *= x;
		} else if (oper == "÷") {
			result /= x;
		} else if (oper == "=") {
			result = x;
		}
		tf.setText(df.format(result));
	}

	/**
	 * 运算符号的事件监听
	 */
	class Signs implements ActionListener{
		public void actionPerformed(ActionEvent e) {
			/*
			 * 用ActionEvent对象的getActionCommand()方法
			 * 取得与引发事件对象相关的字符串
			 */
			String str = e.getActionCommand();

			/* sqrt求平方根 */
			if(str.equals("sqrt")){
				double i = Double.parseDouble(tf.getText());
				if(i>=0){
					/*
					 * String.valueOf() 转换为字符串
					 * df.format() 按要求保留四位小数
					 * Math.sqrt() 求算数平方根
					 */
					tf.setText(String.valueOf(df.format(Math.sqrt(i))));
				}
				else{
					tf.setText("负数不能开平方根");
				}
			}
			/*producer*/
			else if (str.equals("Producer")){
				tf.setText("这是一款由邓帅和小帅设计的帅锅美女专用计算器");
			}
		 
			/*message*/
			else if (str.equals("Message")){
				double i = Double.parseDouble(tf.getText());
				       if (i==2014111367){tf.setText("Name:林康, Tel:18301729598, Birthday:4.10（阳）");} 
			      else if (i==2014110839){tf.setText("Name:邓志文, Tel:18301872218, Birthday:8.22（阳）");}
			      else if (i==2014111824){tf.setText("Name:张新龙, Tel:13127696065, Birthday:7.28（农）");}
			      else if (i==2014111823){tf.setText("Name:曾杰, Tel:15185968032, Birthday:7.1（农）");}
			      else if (i==2014111624){tf.setText("Name:张锐, Tel:13127697076 Birthday:3.27（阳）");}
			      else if (i==2014111498){tf.setText("Name:夏明鑫, Tel:18317056950, Birthday:小年");}
			      else if (i==2014111366){tf.setText("Name:丁浩浩, Tel:15212079095, Birthday:10.18（阳）");}
			      else if (i==2014111365){tf.setText("Name:成子浩, Tel:15805625293, Birthday:9.3（阳）");}
			      else if (i==2014111301){tf.setText("Name:朱荣鑫, Tel:18301878936, Birthday:7.19（阳）");}
			      else if (i==2014111299){tf.setText("Name:刘四维, Tel:18221908170, Birthday:3.11（阳）");}
			      else if (i==2014111057){tf.setText("Name:赵志烽, Tel:18317055960, Birthday:9.18（阳）");}
			      else if (i==2014111055){tf.setText("Name:卯生润, Tel:18317030081, Birthday:8.21（阳）");}
			      else if (i==2014111054){tf.setText("Name:李柏宏, Tel:15618158255, Birthday:12.29（阳）");}
			      else if (i==2014110867){tf.setText("Name:周建宇, Tel:13661934831, Birthday:1.13（农）");}
			      else if (i==2014110863){tf.setText("Name:郭校杰, Tel:18301887512, Birthday:6.22（农）");}
			      else if (i==2014110840){tf.setText("Name:张真, Tel:13916095078, Birthday:11.12（阳）");}
			      else if (i==2014110535){tf.setText("Name:朱易为, Tel:15221004121, Birthday:4.16");}
			      else if (i==2014110530){tf.setText("Name:丁奇, Tel:13381708229, Birthday:1.17（阳）");}
			      else if (i==2014110529){tf.setText("Name:陈欣云, Tel:15921990703, Birthday:7.39（阳）");}
			      else if (i==2014110119){tf.setText("Name:马志强, Tel:18648141264, Birthday:8.18（农）");}
			      else if (i==2014110116){tf.setText("Name:侯名伟, Tel:18301887270, Birthday:8.11（农）");}
			      else if (i==2014111859){tf.setText("Name:黄璨, Tel:18121403332, Birthday:4.22（阳）");}
			      else if (i==2014111773 ){tf.setText("name：边利菁，tel：18683984340，birthday：8.4（农）");}
			      else if (i==2014111301 ){tf.setText("name：吴漏，tel：18221967178，birthday：1.1（农）");}
			      else if (i==2014111053){tf.setText("name：陈丽冲，tel：15000309602，birthday：7.15（阳）");}
			      else if (i==2014111192){tf.setText("name：彭冬旎，tel：18630031105，birthday：11.5(阳））");}
			      else if (i==2014111191 ){tf.setText("name：刘佳琪。tel：18683984340，birthday：10.10（阳）");}
			      else if (i==2014111300){tf.setText("name：王韵涵，tel：18221996209，birthday：12.25（阳）");}
			      else if (i==2014110528){tf.setText("name：蔡令予，tel：15221211535，birthday：12.17(阳）");}
			      else if (i==2014110118){tf.setText("name：刘童，tel：18221936003，birthday：3.25（阳）");}
			      else if (i==2014111625){tf.setText("name：赵雨霁，tel：18221953997，birthday：10.29（阳））");}
			      else if (i==2014110117){tf.setText("name：刘蕊	。tel：18301722620，birthday：7.26（农）");}
			      else if (i==2104110531){tf.setText("name：刘承蓉，tel：18202152335，birthday：11.4（阳）");}
			      else if (i==2014110864){tf.setText("name：韩馨雨，tel：18221939260，birthday：1.21（农）");}
			      else if (i==2014110115){tf.setText("name：韩璐	，tel：18221930087，birthday：10.8（阳）");}
			      else if (i==2014111190){tf.setText("name：侯天童，tel：13127673591，birthday：12.2（农））");}
			      else if (i==2014110865){tf.setText("name：李岩。tel：18301716708，birthday：6.5（阳）");}
			      else if (i==2014111056	){tf.setText("name：张歆雨。tel：18301788513，birthday：7.2（阳）");}
			      else{
						tf.setText("查无此人，请检查学号输入是否正确");}
			}
			/* log求常用对数 */
			else if(str.equals("ln")){
				double i = Double.parseDouble(tf.getText());
				if(i>0){
					tf.setText(String.valueOf(df.format(Math.log(i))));
				}else{
					tf.setText("负数不能求对数");
				}
			}
			
			
			/* 开三次方*/
			else if(str.equals("cube")){
				double i = Double.parseDouble(tf.getText());
				if(i >= 0){
					tf.setText(String.valueOf(df.format(Math.pow(i,1.0/3))));
				}else{
					tf.setText(String.valueOf(df.format(-Math.pow(-i,1.0/3))));
				}
			}
			/* 1/x求倒数 */
			else if(str.equals("1/x")){
				if(Double.parseDouble(tf.getText()) == 0){
					tf.setText("除数不能为零");
				}else{
					tf.setText(df.format(1 / Double.parseDouble(tf.getText())));
				}
			}

			/* sin求正弦函数 */
			else if(str.equals("sin")){
				double i = Double.parseDouble(tf.getText());
				tf.setText(String.valueOf(df.format(Math.sin(i))));
			}

			/* cos求余弦函数 */
			else if(str.equals("cos")){
				double i = Double.parseDouble(tf.getText());
				tf.setText(String.valueOf(df.format(Math.cos(i))));
			}

			/* tan求正切函数 */
			else if(str.equals("tan")){
				double i = Double.parseDouble(tf.getText());
				tf.setText(String.valueOf(df.format(Math.tan(i))));
				}
	

			/* n!求阶乘 */
			else if(str.equals("n!")){
				double i = Double.parseDouble(tf.getText());
				if((i%2==0)||(i%2==1))//判断为整数放进行阶乘操作
				{
					int j = (int)i;//强制类型转换
					int result=1;
					for(int k=1;k<=j;k++)
						result *= k;
					tf.setText(String.valueOf(result));
				}
				else
				{
					tf.setText("无法进行阶乘");
				}
			}

			/* x^2求平方 */
			else if(str.equals("x^2")){
				double i = Double.parseDouble(tf.getText());
				tf.setText(String.valueOf(df.format(i*i)));
			}

			/* x^3求立方 */
			else if(str.equals("x^3")){
				double i = Double.parseDouble(tf.getText());
				tf.setText(String.valueOf(df.format(i*i*i)));
			}

			/* ′″角度转换 */
			/**
			 * 将角度值转换成弧度值，方便三角函数的计算
			 */
			else if(str.equals("′″")){
				double i = Double.parseDouble(tf.getText());
				tf.setText(String.valueOf(i/180*Math.PI));
			}
			
				
			
			else{
				if(flag){
					IfResult = false;
				}
				if(IfResult){
					oper = str;
				}else{ 
					getResult(Double.parseDouble(tf.getText()));
					oper = str;
					IfResult = true;
				}
			}
		}
	}

	/**
	 * 清除按钮的事件监听
	 */
	class Clear implements ActionListener {
		public void actionPerformed(ActionEvent e) {
			/*
			 * 用ActionEvent对象的getActionCommand()方法 取得与引发事件对象相关的字符串
			 */
			String str = e.getActionCommand();
			if (str == "Clear") {
				tf.setText("0");
				IfResult = true;
				result = 0;
			} else if (str == "-/+") {
				double i = 0 - Double.parseDouble(tf.getText().trim());
				tf.setText(df.format(i));
			} else if (str == "Backspace") {
				if (Double.parseDouble(tf.getText()) > 0) {
					if (tf.getText().length() > 1) {
						tf.setText(tf.getText().substring(0, tf.getText().length() - 1));
						// 使用退格删除最后一位字符
					} else {
						tf.setText("0");
						IfResult = true;
					}
				} else {
					if (tf.getText().length() > 2) {
						tf.setText(tf.getText().substring(0, tf.getText().length() - 1));
					} else {
						tf.setText("0");
						IfResult = true;
					}
				}
			} 
		}
	}

	/**
	 * 数字输入的事件监听
	 */
	class Num implements ActionListener{
		public void actionPerformed(ActionEvent e) {
			String str = e.getActionCommand();
			if(IfResult){
				tf.setText("");
				IfResult = false;
			}
			if(str=="π")
			{
				tf.setText(String.valueOf(Math.PI));
			}
			else if(str=="e")
			{
				tf.setText(String.valueOf(Math.E));
			}
			else{
				tf.setText(tf.getText().trim() + str);
				if(tf.getText().equals("0")){
					tf.setText("0");
					IfResult = true;
					flag = true;
				}
			}
		}
	}

	/**
	 * 小数点的事件监听
	 */
	class Dot implements ActionListener {
		public void actionPerformed(ActionEvent e) {
			IfResult = false;
			if (tf.getText().trim().indexOf(".") == -1) {
				tf.setText(tf.getText() + ".");
			}
		}
	}

	/**
	 * main方法
	 */
	public static void main(String[] args) {
		new Calucator().setVisible(true);
	}
}
package calculator;
import java.awt.Color;
import java.awt.EventQueue;
import java.awt.BorderLayout;
import java.awt.FlowLayout;
import java.awt.Font;
import java.awt.event.*;
import java.util.Random;

import javax.swing.BorderFactory;
import javax.swing.Icon;
import javax.swing.ImageIcon;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.SwingConstants;
import javax.swing.border.*;
import javax.swing.JTextField;

public class Copy2048 extends JFrame{
	private JPanel scoresPane;
	private JPanel mainPane;
	private JLabel labelMaxScores ;
	private JLabel labelScores;
	private JLabel tips;					//提示操作标签
	private JTextField textMaxScores;
	private JLabel textScores;
	private JLabel[][] texts;
	private Icon icon2;
	private int times = 16;					//记录剩余空方块数目
	private int scores = 0;					//记录分数
	private int l1,l2,l3,l4,l5;				//用于判断游戏是否失败
	Font font = new Font("", Font.BOLD,14);			//设置字体类型和大小
	Font font2 = new Font("", Font.BOLD,30);
	Random random = new Random();
	
	public static void main(String[] args){
		EventQueue.invokeLater(new Runnable(){
			public void run(){
				try{
					Copy2048 frame = new Copy2048();
					frame.setVisible(true);
				//	Thread thread = new Thread(frame);
				//	thread.start();
				}
				catch(Exception e1){
					e1.printStackTrace();
				}
			}
		});
	}
	/**
	 * 构造方法
	 */
	public Copy2048(){
		super();
		setResizable(false);			//禁止调整窗体大小
		getContentPane().setLayout(null);	//设置空布局
		setBounds(500, 50, 500, 615);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setTitle("2048计算器版");			//设置窗体标题
		
		scoresPane = new JPanel();					//创建分数显示面板
		scoresPane.setBackground(Color.green);		//设置分数显示面板的背景色
		scoresPane.setBounds(20, 20, 460, 40);
		scoresPane.setBorder(BorderFactory.createMatteBorder(2, 2, 2, 2, Color.YELLOW));	//设置得分面板的边框
		getContentPane().add(scoresPane);			//将得分面板添加到窗体
		scoresPane.setLayout(null);					//设置面板空布局
		
		labelMaxScores = new JLabel("最高分:");		//最高分标签
		labelMaxScores.setFont(font);				//设置字体类型和大小
		labelMaxScores.setBounds(10, 5, 50, 30);	//设置最懂啊分标签的位置尺寸
		scoresPane.add(labelMaxScores);	//将最高分标签添加到得分容器中
		
		textMaxScores = new JTextField("暂不可用");			//得分标签
		textMaxScores.setBounds(60, 5, 150, 30);
		textMaxScores.setFont(font);
		textMaxScores.setEditable(false);
		scoresPane.add(textMaxScores);				//将得分标签添加到分数面板中
		
		labelScores = new JLabel("得    分:");
		labelScores.setFont(font);					//设置字体类型和大小
		labelScores.setBounds(240, 5, 50, 30);
		scoresPane.add(labelScores);
		
		textScores = new JLabel(String.valueOf(scores));
		textScores.setFont(font);
		textScores.setBounds(290, 5, 150, 30);
		scoresPane.add(textScores);
		
		mainPane = new JPanel();				//创建游戏主面板
		mainPane.setBounds(20, 70, 460, 500);	//设置主面板位置尺寸
		
		this.getContentPane().add(mainPane);
		mainPane.setLayout(null);				//设置空布局
		
		texts = new  JLabel[4][4];			//创建文本框二维数组
		for(int i = 0; i < 4; i++){				//遍历数组
			for(int j = 0; j < 4; j++){
				texts[i][j] = new JLabel();	//创建标签
				texts[i][j].setFont(font2);
				texts[i][j].setHorizontalAlignment(SwingConstants.CENTER);
				texts[i][j].setText("");
				texts[i][j].setBounds(120 * j, 120 * i, 100, 100);		//设置方块的大小位置
				setColor(i, j, "");
				texts[i][j].setOpaque(true);
				texts[i][j].setBorder(BorderFactory.createMatteBorder(2, 2, 2, 2, Color.green));//设置方块边框颜色
				mainPane.add(texts[i][j]);							//将创建的文本框放在	
				
			}
		}
		
		tips = new JLabel("Tips：使用上、下、左、右键或者W、S、A、D键控制");
		tips.setFont(font);
		tips.setBounds(60,480,400,20);
		mainPane.add(tips);
		
		textMaxScores.addKeyListener(new KeyAdapter(){				//为最高分标签添加按键监听器
			public void keyPressed(  KeyEvent e){
				 do_label_keyPressed(e);				//调用时间处理方法
			}
		});
	
		Create2();
		Create2();
	}
	
	/**
	 * 按键输入事件的处理方法
	 * @param e
	 */
	protected  void do_label_keyPressed(final KeyEvent e){
		int code = e.getKeyCode();	//获取按键代码
		int a ;						//a 的引入是为了防止连加的情况出现
		String str ;								
		String str1;
		int num;
		switch(code){
		case KeyEvent.VK_LEFT:
		case KeyEvent.VK_A:						//如果按键代码是左方向键或者A键
			for(int i = 0; i < 4; i++){	
				a = 5;
				for(int k = 0; k < 3; k++){
					for(int j = 1; j < 4; j++){					//遍历16个方块
						str = texts[i][j].getText();			//获取当前方块标签文本字符
						str1 = texts[i][j-1].getText();			//获取当前左1方块标签文本字符
						
						if(str1.compareTo("") == 0){				//如果左1方块文本为空字符
							texts[i][j-1].setText(str);				//字符左移
							setColor(i, j-1,str);
							texts[i][j].setText("");				//当前方块字符置空
							setColor(i, j, "");
						}else if((str.compareTo(str1) == 0) && (j !=a) && (j != a-1)){			//如果当前方块和左1方块文本字符相等
							num  = Integer.parseInt(str);
							scores += num;
							times ++;
							str = String.valueOf(2 * num);
							texts[i][j-1].setText(str);		//左1方块文本字符变为两方块之和
							setColor(i, j-1, str);
							texts[i][j].setText("");				//当前方块字符置空
							setColor(i, j, "");
							a = j;
						}
					}	
				}
			}
			l1 = 1;				//用于判断游戏是否失败
			Create2();
			break;
		case KeyEvent.VK_RIGHT:
		case KeyEvent.VK_D:
			for(int i = 0; i < 4; i ++){
				a = 5;
				for(int k = 0; k < 3; k++){
					for(int j = 2; j >= 0; j--){
						str = texts[i][j].getText();
						str1 = texts[i][j + 1].getText();
						
						if(str1.compareTo("") == 0){
							texts[i][j + 1].setText(str);
							setColor(i, j+1, str);
							texts[i][j].setText("");
							setColor(i, j, "");
						}
						else if(str.compareTo(str1) == 0 && j !=a && j != a+ 1){
							num  = Integer.parseInt(str);
							scores += num ;
							times ++;
							str = String.valueOf(2 * num);
							texts[i][j + 1].setText(str);
							setColor(i, j+1, str);
							texts[i][j].setText("");
							setColor(i, j, "");
							a = j;
						}
					}
				}
			}
			l2 = 1;
			Create2();
			break;
		case KeyEvent.VK_UP:
		case KeyEvent.VK_W:
			for(int j = 0; j < 4; j++){
				a = 5;
				for(int k = 0; k < 3; k++){
					for(int i = 1; i < 4; i++){
						str = texts[i][j].getText();
						str1 = texts[i - 1][j].getText();
					
						if(str1.compareTo("") == 0){
							texts[i - 1][j].setText(str);
							setColor(i-1, j, str);
							texts[i][j].setText("");
							setColor(i, j, "");
						}
						else if(str.compareTo(str1) == 0 && i != a && i != a -1){
							num  = Integer.parseInt(str);
							scores += num ;
							times ++;
							str = String.valueOf(2 * num);
							texts[i - 1][j].setText(str);
							setColor(i-1, j, str);
							texts[i][j].setText("");
							setColor(i, j, "");
							a = i;
						}
					}
				}
			}
			l3 =1;
			Create2();
			break;
		case KeyEvent.VK_DOWN:
		case KeyEvent.VK_S:
			for(int j = 0; j < 4; j ++){
				a = 5;
				for(int k = 0; k < 5; k++){
					for(int i = 2; i >= 0; i--){
						str = texts[i][j].getText();
						str1 = texts[i + 1][j].getText();
						
						if(str1.compareTo("") == 0){
							texts[i + 1][j].setText(str);
							setColor(i+1, j, str);
							texts[i][j].setText("");
							setColor(i, j, "");
						}
						else if(str.compareTo(str1) == 0 && i != a && i != a + 1){
							num  = Integer.parseInt(str);
							scores += num ;
							times ++;
							str = String.valueOf(2 * num);
							texts[i + 1][j].setText(str );
							setColor(i+1, j, str);
							texts[i][j].setText("");
							setColor(i, j, "");
							a = i;
						}
					}
				}
			}
			l4 = 1;
			Create2();
			break;
			default:
				break;
		}
		textScores.setText(String.valueOf(scores));
	}
	
	/**
	 * 在随机位置产生一个2号方块的方法
	 * @param i,j
	 */
	public void Create2(){
		int i ,j;
		boolean r = false;
		String str;
		
		if(times > 0){
			while(!r){
				i = random.nextInt(4);
				j = random.nextInt(4);
				str = texts[i][j].getText();
				if((str.compareTo("") == 0)){
					texts[i][j].setIcon(icon2);
					texts[i][j].setText("2");
					setColor(i, j, "2");
					
					times --;
					r = true;
					l1 = l2 = l3 = l4 = 0;			
				}
			}
		}
		else if(l1 >0 && l2 >0 && l3 > 0 && l4 > 0){		//l1到l4同时被键盘赋值为1说明任何方向键都不能产生新的数字2，说明游戏失败
				tips.setText("                            GAME　OVER ！");
			
		}
	}
	
	/**
	 * 设置标签颜色
	 * @param i , j ,str
	 */
	public void setColor(int i, int j, String str){
		switch(str){
		case "2":
			texts[i][j].setBackground(Color.yellow);
			break;
		case "4":
			texts[i][j].setBackground(Color.red);
			break;
		case "8":
			texts[i][j].setBackground(Color.pink);
			break;
		case "16":
			texts[i][j].setBackground(Color.orange);
			break;
		case "32":
			texts[i][j].setBackground(Color.magenta);
			break;
		case "64":
			texts[i][j].setBackground(Color.LIGHT_GRAY);
			break;
		case "128":
			texts[i][j].setBackground(Color.green);
			break;
		case "256":
			texts[i][j].setBackground(Color.gray);
			break;
		case "512":
			texts[i][j].setBackground(Color.DARK_GRAY);
			break;
		case "1024":
			texts[i][j].setBackground(Color.cyan);
			break;
		case "2048":
			texts[i][j].setBackground(Color.blue);
			break;
		case "":
		case "4096":
			texts[i][j].setBackground(Color.white);
			break;
		default:
			break;
		}

	}

}

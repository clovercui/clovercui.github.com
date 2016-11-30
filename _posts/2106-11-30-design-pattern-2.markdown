---
layout:     post
title:      "PHP设计模式-Part2 创建型"
subtitle:   "简单工厂模式，工厂方法，抽象工厂模式，单例模式，生成器模式，原型模式"
date:       2016-11-29
author:     "Clover"
header-img: "img/post-bg-js-module.jpg"
catalog: true
tags:
    - Clover
    - 设计模式
    - PHP

---

> 参考资料《大话设计模式》
> 作者程杰

> 参考 http://blog.csdn.net/jhq0113/article/details/44906491

# 设计模式-创建型

## 1.简单工厂模式

`简单工厂模式`:不属于23种常用面向对象设计模式之一。简单工厂模式是由`一个工厂对象决定创建出哪一种产品类的实例`。简单工厂模式是工厂模式家族中最简单实用的模式，可以理解为是不同工厂模式的一个特殊实现。其实质是由一个工厂类`根据传入的参数，动态决定应该创建哪一个产品类`（这些产品类继承自一个父类或接口）的实例
![pp](http://clover.htmhub.com/img/phpdesign/20150409224555911.jpeg)

* **角色和职责**：

	* `工厂(SimpleFactory)角色`：简单工厂模式的核心，它负责实现创建所有实例的内部逻辑。`工厂类可以被外界直接调用，创建所需的产品对象`
	* `抽象产品（IProduct）角色`:简单工厂模式所创建的所有对象的父类，它负责描述所有实例所共有的公共接口。
	* `具体产品（Concrete Product）角色`:是简单工厂模式的创建目标，所有创建的对象都是充当这个角色的某个具体类的实例
* **需求**：根据提供相应的属性值由简单工厂创建具有相应特性的产品对象
* **代码示例**：

	```php
		<?php  
		/** 
		 * Created by PhpStorm. 
		 * User: Jiang 
		 * Date: 2015/4/9 
		 * Time: 21:48 
		 */  
		  
		/**抽象产品角色 
		 * Interface IProduct   产品接口 
		 */  
		interface IProduct  
		{  
		    /**X轴旋转 
		     * @return mixed 
		     */  
		    function XRotate();  
		  
		    /**Y轴旋转 
		     * @return mixed 
		     */  
		    function YRotate();  
		}  
		  
		/**具体产品角色 
		 * Class XProduct        X轴旋转产品 
		 */  
		class XProduct implements IProduct  
		{  
		    private $xMax=1;  
		    private $yMax=1;  
		  
		    function __construct($xMax,$yMax)  
		    {  
		        $this->xMax=$xMax;  
		        $this->yMax=1;  
		    }  
		  
		    function XRotate()  
		    {  
		        echo "您好，我是X轴旋转产品，X轴转转转。。。。。。";  
		    }  
		  
		    function YRotate()  
		    {  
		        echo "抱歉，我是X轴旋转产品，我没有Y轴。。。。。。";  
		    }  
		}  
		  
		/**具体产品角色 
		 * Class YProduct        Y轴旋转产品 
		 */  
		class YProduct implements IProduct  
		{  
		    private $xMax=1;  
		    private $yMax=1;  
		  
		    function __construct($xMax,$yMax)  
		    {  
		        $this->xMax=1;  
		        $this->yMax=$yMax;  
		    }  
		  
		    function XRotate()  
		    {  
		        echo "抱歉，我是Y轴旋转产品，我没有X轴。。。。。。";  
		    }  
		  
		    function YRotate()  
		    {  
		        echo "您好，我是Y轴旋转产品，Y轴转转转。。。。。。";  
		    }  
		}  
		  
		/**具体产品角色 
		 * Class XYProduct        XY轴都可旋转产品 
		 */  
		class XYProduct implements IProduct  
		{  
		    private $xMax=1;  
		    private $yMax=1;  
		  
		    function __construct($xMax,$yMax)  
		    {  
		        $this->xMax=$xMax;  
		        $this->yMax=$yMax;  
		    }  
		  
		    function XRotate()  
		    {  
		        echo "您好，我是XY轴都可旋转产品，X轴转转转。。。。。。";  
		    }  
		  
		    function YRotate()  
		    {  
		        echo "您好，我是XY轴都可旋转产品，Y轴转转转。。。。。。";  
		    }  
		}  
		  
		/**工厂角色 
		 * Class ProductFactory 
		 */  
		class ProductFactory  
		{  
		    static function GetInstance($xMax,$yMax)  
		    {  
		        if($xMax>1 && $yMax===1)  
		        {  
		            return new XProduct($xMax,$yMax);  
		        }  
		        elseif($xMax===1 && $yMax>1)  
		        {  
		            return new YProduct($xMax,$yMax);  
		        }  
		        elseif($xMax>1 && $yMax>1)  
		        {  
		            return new XYProduct($xMax,$yMax);  
		        }  
		        else  
		        {  
		            return null;  
		        }  
		    }  
		}  
	```
	
* **代码示例**：
	
	```php
			<?php  
		/** 
		 * Created by PhpStorm. 
		 * User: Jiang 
		 * Date: 2015/4/9 
		 * Time: 21:54 
		 */  
		require_once "./SimpleFactory/SimpleFactory.php";  
		  
		header("Content-Type:text/html;charset=utf-8");  
		  
		$pro=array();  
		$pro[]=ProductFactory::GetInstance(1,12);  
		$pro[]=ProductFactory::GetInstance(12,1);  
		$pro[]=ProductFactory::GetInstance(12,12);  
		$pro[]=ProductFactory::GetInstance(0,12);  
		  
		foreach($pro as $v)  
		{  
		    if($v)  
		    {  
		        echo "<br/>";  
		        $v->XRotate();  
		        echo "<br/>";  
		        $v->YRotate();  
		    }  
		    else  
		    {  
		        echo "非法产品！<br/>";  
		    }  
		    echo "<hr/>";  
		}  
	```
	
   用浏览器访问测试代码，我们可以发现创建的对象依次是YProduct,XProduct,XYProduct,null。简单工厂的核心代码在于工厂(ProductFactory)这个角色，这里根据传入的xMax与yMax值去创建不同的对象，这便是简单工厂的实质，而且我们在测试调用客户端根本不知道具体的产品类是什么样，这样就做到了调用与创建的分离
   
* **优点**：让对象的调用者和对象创建过程分离，当对象调用者需要对象时，直接向工厂请求即可。从而`避免了对象的调用者与对象的实现类以硬编码方式耦合，以提高系统的可维护性、可扩展性。`

* **缺点**：当产品修改时，工厂类也要做相应的修改，比如要增加一种操作类，如求M数的N次方，就得改case,修改原有类，`违背了开放-封闭原则`。

## 2.工厂方法

* **具体案例**：请MM去麦当劳吃汉堡，不同的MM有不同的口味，要每个都记住是一件烦人的事情，我们一般采用FactoryMethod模式，带着MM到服务员那儿，说“要一个汉堡”，具体要什么样的汉堡呢，让MM直接跟服务员说就行了

工厂方法模式核心工厂类不再负责所有产品的创建，而是将具体创建的工作交给子类去做，成为一个抽象工厂角色，仅负责给出具体工厂类必须实现的接口，而不接触哪一个产品类应当被实例化这种细节，如下图：![ww](http://clover.htmhub.com/img/phpdesign/20150416222830823.png)

* **角色和职责**：
	* `抽象工厂角色（IServerFactory）`：是工厂方法模式的核心，与应用程序无关。任何在模式中创建的对象的工厂类必须实现这个接口
	* `具体工厂角色(ChickenLegBaoFactory)`:这是实现抽象工厂接口的具体工厂类，包含与应用程序密切相关的逻辑，并且受到应用程序调用以创建产品对象
	* `抽象产品角色(IHanbao)`:工厂方法模式所创建的对象的超类型，也就是产品对象的共同父类或共同拥有的接口
	* `具体产品角色(ChickenLegBao)`:这个角色实现了抽象产品角色所定义的接口。某具体产品有专门的具体工厂创建，它们之间往往一一对应

* **代码示例**：
	
	```php
		<?php  
		/** 
		 * Created by PhpStorm. 
		 * User: Jiang 
		 * Date: 2015/4/16 
		 * Time: 22:12 
		 */  
		  
		/**抽象产品角色       汉堡 
		 * Interface IHanbao 
		 */  
		interface IHanbao  
		{  
		    function Allay();  
		}  
		  
		/**具体产品角色         肉松汉堡 
		 * Class RouSongBao 
		 */  
		class RouSongBao implements IHanbao  
		{  
		    function Allay()  
		    {  
		        echo "I am 肉松汉堡，小的给主人解饿了！<br/>";  
		    }  
		  
		}  
		  
		/**鸡肉汉堡 
		 * Class ChickenBao 
		 */  
		class ChickenBao implements IHanbao  
		{  
		    function Allay()  
		    {  
		        echo "I am 鸡肉汉堡，小的给主人解饿了！<br/>";  
		    }  
		  
		}  
		  
		/**抽象工厂角色 
		 * Interface IServerFactory 
		 */  
		interface IServerFactory  
		{  
		    function MakeHanbao();  
		}  
		  
		/**具体工厂角色     肉松汉堡工厂 
		 * Class RouSongFactory 
		 */  
		class RouSongFactory implements IServerFactory  
		{  
		  
		    function MakeHanbao()  
		    {  
		        return new RouSongBao();  
		    }  
		}  
		  
		class ChickenFactory implements IServerFactory  
		{  
		  
		    function MakeHanbao()  
		    {  
		        return new ChickenBao();  
		    }  
		} 
	```
	
* **测试代码**：
	
	```php
		header("Content-Type:text/html;charset=utf-8");  
		//------------------------工厂方式测试代码------------------  
		require_once "./FactoryMethod/FactoryMethod.php";  
		  
		//-----------------餐厅厨师-----------  
		$chickenFactory=new ChickenFactory();  
		$rouSongFactory=new RouSongFactory();  
		  
		//-----------点餐------------  
		$chicken1=$chickenFactory->MakeHanbao();  
		$chicken2=$chickenFactory->MakeHanbao();  
		$rouSong1=$rouSongFactory->MakeHanbao();  
		$rouSong2=$rouSongFactory->MakeHanbao();  
		  
		//------------------顾客吃饭---------  
		$chicken1->Allay();  
		$chicken2->Allay();  
		$rouSong1->Allay();  
		$rouSong2->Allay();  
	```	
	用浏览器运行测试代码我们可以发现，顾客都享用了自己的食物
	
* **优点**:`克服了简单工厂模式违背开放-封闭的原则`，保持了封装对象创建过程的优点	
* **缺点**：当增加产品时，就得增加一个产品工厂的类，增加额外的开发量。避免不了分支判断的问题

* **简单工厂模式与工厂方法模式的比较**：
	* `1.结构复杂度`:简单工厂模式要占优。简单工厂模式只需一个工厂类，而工厂方法模式的工厂类随着产品类个数增加而增加，从而增加了结构的复杂程度。
	* `2.代码复杂度` :代码复杂度和结构复杂度是一对矛盾，既然简单工厂模式在结构方面相对简洁，那么它在代码方面肯定是比工厂方法模式复杂的了。简单工厂模式的工厂类随着产品类的增加需要增加很多方法（或代码），而工厂方法模式每个具体工厂类只完成单一任务，代码简洁。
	* `3.管理上的难度`:假如某个具体产品类需要进行一定的修改，很可能需要修改对应的工厂类。当同时需要修改多个产品类的时候，对工厂类的修改会变得相当麻烦。反而简单工厂没有这些麻烦，当多个产品类需要修改是，简单工厂模式仍然仅仅需要修改唯一的工厂类。

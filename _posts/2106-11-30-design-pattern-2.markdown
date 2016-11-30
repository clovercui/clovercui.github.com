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

	

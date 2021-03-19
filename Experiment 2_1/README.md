代码在 out 文件夹里面

# 第二次实验
## 实验目标
* 学习利用工厂方法完成对象创建，理解多态等面向对象思想
* 学习利用拷贝方法完成对象创建，体会深拷贝与浅拷贝的差异
实验说明
本次实验由于测试需要，分为两个仓库提交
任务内容    course 链接    gitlab仓库后缀
任务一（为编程题）    任务一    exp_2
任务二（为编程题）    任务二    exp_2_2
实验内容
任务一：工厂模式
背景知识：
简单工厂模式：
简单工厂模式是由一个工厂对象根据收到的消息决定要创建的类的对象实例。在一个简单工厂中一般包含下面几个部分：
* 工厂类：当我们想要获得由此工厂生产的产品时，只要调用工厂类里的生产产品的方法，传入我们想要生产的产品名即可。
* 产品接口：一个接口，工厂所生产的产品需要实现这个接口。可以理解为该工厂所生产的产品的一个共同的模具。
* 产品类：具体生产的产品，产品需要实现产品接口所要求的方法。
工厂方法模式：
上面介绍的简单工厂模式中，我们每需要生产新的产品系列时，都需要重新去写新的工厂。所以对上面的简单工厂模式进一步抽象后就得到了工厂方法模式。
相比于简单工厂模式，工厂方法模式增加定义了一个创建工厂的工厂接口，提高了工厂实现的扩展性。
引入工厂方法模式，将上面一个工厂生产多个型号的产品改为一个工厂生产一个型号的产品，然后有多个工厂，这多个工厂共同实现一个工厂接口。
抽象工厂模式：
在只有一个等级结构的情况下，使用工厂方法模式就足够，但是对于多个等级结构的工厂而言，工厂方法模式就显得有些不合适。基于以上考虑，我们引入抽象工厂模式。总体而言与工厂方法模式相同，仍然包括工厂接口、工厂类、产品接口、产品类。不同之处在于，在工厂类中，我们需要实现一个等级结构下的不同产品的创建方法。
具体示例见文末参考资料
问题描述：
现在你需要生产小汽车 Car，洒水车 Sprinkler，公共汽车 Bus这三种车辆。课程组已经实现抽象类 Vehicle，接口 Manned，Engineered (详见 实验二任务一公共仓库)
请根据不同交通设备的设计要求实现对应的类和类之间的继承逻辑关系，建议使用工厂模式来解决遇到的问题；对于输入的格式化处理，可以与工厂模式配合处理。
已实现的内容：
车辆抽象类Vehicle
功能：
* 构造车辆
* 驱动车辆
* 获取车辆价格

```
    public abstract class Vehicle {
        
        private int id;

        private int price;
        
        Vehicle(int id,int price) {
            this.id = id;
            this.price = price;
        }
        
        public abstract void run();
        
        public int getPrice() { return price; }
        
    }
```

可以看到，该抽象类约束了车辆的基本信息，所有的车辆都有 id 和 price，其子类都需要实现 run 方法，子类中的 getPrice 方法可以进行重写。
载人有关接口Manned
功能：
* 乘客上车
* 乘客下车
```
    public interface Manned {
    
        void getIn();
    
        void getOff();
    }
```

功能有关接口Engineered
功能：
* 功能性车辆工作
```
    public interface Engineered {
       
       void work();
    }
```

需要实现的内容：
小汽车类Car
继承自 Vehicle，增加了属性最大速度 maxSpeed，需要按如下方式重写 run() 和 getPrice() 方法，实现以下功能：
* 驱动小汽车，输出 Wow, I can Run (maxSpeed:最大速度)!
* 获取小汽车价格，输出 price is: 小汽车价格
    * 小汽车价格 = 车辆类基准价格 + 因速度过大而造成的车辆磨损维修费
    * 因速度过大而造成的车辆磨损维修费:

        if (maxSpeed < 1000) then 因速度过大而造成的车辆磨损维修费 = 0

        else if (maxSpeed >= 1000 then 因速度过大而造成的车辆磨损维修费 = 1000
```
    public void run() {
        System.out.println("Wow, I can Run (maxSpeed:" + maxSpeed + ")!");
    }   

    public int getPrice() {
        // TODO
    }
```
洒水车类Sprinkler
继承自 Vehicle，增加了属性 volume 表示洒水车总容量，需要按如下方式重写 run() 和 getPrice() 方法，并按如下方式实现 Engineered 接口中的 work() 方法，以实现以下功能：
* 驱动洒水车，输出 Wow, I can Run and clear the road!
* 获取洒水车价格，为车辆类基准价格的两倍，并输出 price is: 洒水车价格
* 使洒水车工作，输出 Splashing! 洒水车总容量L water used!
```
    public void run() {
        
        System.out.println("Wow, I can Run and clear the road!");
    }

    public int getPrice() {

        // TODO
    }

    public void work() {
        
        System.out.println("Splashing!" + " " + this.volume + "L water used!");
    }
```
公共汽车类Bus
继承自 Vehicle ，增加了属性 volume 表示公共汽车总油量，增加了属性 passenger 表示车上乘客数，需要按如下方式重写 run() 方法，并按如下方式实现 Engineered 接口中的 work() 方法和 Manned 接口中的 getIn() , getOff() 方法, 以实现以下功能：
* 驱动公共汽车，输出 Wow, I can Run all day!
* 使公共汽车工作，输出 Working! 公共汽车总油量L diesel oil used!
* 乘客上车（一人）。并输出 Wow! We have a new passenger!
* 乘客下车。若车上没有乘客了，则直接输出 Wow! Only Driver! ; 否则先使一名乘客下车，然后输出 Wow! A passenger arrived at his or her destination!
```
    public void run() {
        
        System.out.println("Wow, I can Run all day!");
    }

    public void work() {
    
        System.out.println("Working!" + " " + this.volume + "L diesel oil used!");
    }

    public void getIn() {
        
        passenger = passenger + 1;
        System.out.println("Wow! We have a new passenger!");
    }

    public void getOff() {
    
        // TODO
    }
```
工厂类Factory
```
    public class Factory {
        public static Vehicle getNew(List<String> ops) {
            String type = ops.get(1);
            if ("Car".equals(type)) {
                // TODO
            } else if ("Sprinkler".equals(type)) {
                // TODO
            } else {
                // TODO
            }
        }
    }
```
主类Main
```
    public class Main {
        public static void main(String[] args) {
            InputFactory in = new InputFactory();
            Map<Integer, Vehicle> idToVec = new HashMap<>(1000);
            while (in.hasNextOperation()) {
                String opStr = in.getNewOperation();
                List<String> ops = new ArrayList<>(Arrays.asList(opStr.split(" ")));

                // for create
                if ("create".equals(ops.get(0))) {
                    int id = Integer.parseInt(ops.get(2));
                    Vehicle vec = Factory.getNew(ops);
                    idToVec.put(id, vec);
                }
                // for call
                else {
                    int id = Integer.parseInt(ops.get(1));
                    Vehicle vec = idToVec.get(id);
                    switch (ops.get(2)){
                        case "run":
                            vec.run();
                            break;
                        case "getPrice":
                            // only consider the output statement in getPrice method without outputting the return value
                            vec.getPrice();
                            break;
                        // TODO
                    }
                }
            }
        }
    } 
```
注：继承实现结构与现实不符，如有雷同，纯属巧合
输入格式：
多行输入，每行一个操作，一行的各个参数之间均由一个空格隔开，操作有两种类型
1. 生产一辆交通设备，格式为 create vehicle_type id price
    * 有特殊参数的交通设备有特殊的生产格式
        * car: create Car id price max_speed
        * sprinkler: create Sprinkler id price volume
        * bus: create Bus id price volume
注：price 为车辆类基准价格
2. 调用当前某一个 id 的交通设备的某一种非构造性方法，格式为 call id method_name
数据约束
* 保证输入中所生产的所有交通设备 id 不重复
* 保证第二种操作中所涉及的 id 的交通设备均已在第一种操作中生产
输出格式：
对于第二种操作，需要调用其相关方法，对有输出的方法输出结果
请注意输出语句中的空格格式。

样例：

stdin:

create Car 1010 1000 60

call 1010 run

call 1010 getPrice

create Sprinkler 111 2000 100

create Bus 121 3000 120 

call 111 work

call 111 run

call 111 getPrice

call 121 run

call 121 work

call 121 getIn

call 121 getOff

call 121 getOff

stdout:

Wow, I can Run (maxSpeed:60)!

price is: 1000

Splashing! 100L water used!

Wow, I can Run and clear the road!

price is: 4000

Wow, I can Run all day!

Working! 120L diesel oil used!

Wow! We have a new passenger!

Wow! A passenger arrived at his or her destination!

Wow! Only Driver!

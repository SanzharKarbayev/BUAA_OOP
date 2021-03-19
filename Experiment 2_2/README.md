代码在 out 文件夹里面

# 第二次实验
## 实验目标
* 学习利用工厂方法完成对象创建，理解多态等面向对象思想
* 学习利用拷贝方法完成对象创建，体会深拷贝与浅拷贝的差异
   实验说明
   由于测试需要，本次实验分为两个仓库提交
   任务内容    course 链接    gitlab 仓库后缀
   任务一（为编程题）    任务一    exp_2
   任务二（为编程题）    任务二    exp_2_2
### 实验内容
任务二：深浅拷贝
背景知识：
拷贝方法的出现，方便了开发人员，在对象内部数据层次复杂的情况下，不通过 new 关键字和众多参数的传递，来完成拷贝对象的创建。
Object 类提供了一个受保护的 clone 方法，用以创建已有对象的一个浅拷贝。通过阅读 Object 类的源码，可以了解到的内容如下：
* clone 方法的通用约定，在此不展开叙述。
* 通过重写 clone 方法，可以创建已有对象的一个深拷贝。clone 方法的通用重写规则如下：
    * 首先，调用 super.clone() 。这会获得一个对象，此对象的字段与被克隆对象的字段对应相等。
    * 而后，修改获得对象的某些字段。通常，这些字段引用的对象为可变对象，且构成了被克隆对象内部的深层结构，应当分别对其进行拷贝。如果一个类的字段仅包含基本类型和不可变对象类型，则通常没有字段需要被修改。
* 待重写 clone 方法的类，应当声明实现 java.lang 包下的 Cloneable 接口，否则，调用该类的 clone 方法时，将抛出 CloneNotSupportedException 。
问题描述：
现在需要调查本市市民乘坐公交车的情况，调查方式为，在某时刻某站点，记录当前公交车上所有人员的信息。指令式输入包括，乘坐公交车的人的上下车情况以及对不同公交车的调查计划，待所有调查结束后，依次输出所有调查结果。(详见 实验二任务二公共仓库)
不允许对已经实现的类的内容进行修改。随意更改视为作弊，一经发现，本任务成绩记为0分。
已实现的内容：
处理输入输出的Main类
```
public class Main {
    public static void main(String[] args) throws Exception {
        Map<Integer, Bus> idToBus = new HashMap<>();
        Map<Integer, Person> idToPerson = new HashMap<>();
        List<List<Person>> res = new ArrayList<>();
        int personId;
        int busId;
        Bus bus;
        Person person;
        try (Scanner input = new Scanner(System.in)) {
            while (input.hasNext()) {
                String[] cmd = input.nextLine().split("\\s+");
                switch (cmd[0]) {
                    case "1":
                        personId = Integer.parseInt(cmd[1]);
                        busId = Integer.parseInt(cmd[2]);
                        bus = idToBus.get(busId);
                        person = idToPerson.get(personId);
                        if (person != null) {
                            person.setBoardTime(cmd[3]);
                        } else {
                            person = new Person(personId, cmd[3]);
                            idToPerson.put(personId, person);
                        }
                        if (bus != null) {
                            bus.addPerson(person);
                        } else {
                            bus = new Bus();
                            bus.addPerson(person);
                            idToBus.put(busId, bus);
                        }
                        break;
                    case "2":
                        bus = idToBus.get(Integer.parseInt(cmd[2]));
                        person = new Person(Integer.parseInt(cmd[1]));
                        if (bus != null) {
                            bus.removePerson(person);
                        } else {
                            throw new Exception("no such bus");
                        }
                        break;
                    case "3":
                        bus = idToBus.get(Integer.parseInt(cmd[1]));
                        if (bus != null) {
                            res.add(bus.getPersonList());
                        } else {
                            throw new Exception("no such bus");
                        }
                        break;
                    default:
                        throw new Exception("wrong command");
                }
            }
        }
        for (List<Person> personList : res) {
            if (personList.isEmpty()) {
                System.out.println("empty bus");
            } else {
                 personList.sort(Comparator.comparing(Person::getPersonId));
                for (Person per : personList) {
                    System.out.print(per);
                }
                System.out.println();
            }
        }
    }
}
```
乘坐公交车的人Person类
```
public class Person implements Cloneable {
    private final int personId;
    private String boardTime;

    public Person(int personId, String boardTime) {
        this.personId = personId;
        this.boardTime = boardTime;
    }

    public Person(int personId) {
        this.personId = personId;
    }

    public void setBoardTime(String newBoardTime) {
        this.boardTime = newBoardTime;
    }

    public int getPersonId() {
        return personId;
    }

    @Override
    public String toString() {
        return personId + " " + boardTime + " ";
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) {
            return true;
        }
        if (o == null || !(o instanceof Person)) {
            return false;
        }
        Person person = (Person) o;
        return personId == person.personId;
    }

    @Override
    public Person clone() {
        Person clone = null;
        try {
            clone = (Person) super.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return clone;
    }
}
```
需要实现的内容：
公交车Bus类
```
public class Bus {
    private List<Person> personList;

    public Bus() {
        personList = new ArrayList<>();
    }

    public void addPerson(Person person) {
        personList.add(person);
    }

    public void removePerson(Person person) {
        personList.remove(person);
    }

    public List<Person> getPersonList() {
        // TODO: return the current list of persons in the bus by implementing the deep clone
    }
}
```

输入：
依次输入三类指令：首先输入指令类别
i, i∈[1,3]
i, i\in[1,3]
i, i∈[1,3] ，再输入指令的具体内容（假设人的编号为 person_id ，公交车的编号为 bus_id ，乘车时间为 board_time ，其中编号为 int 类型，乘车时间为 String 类型）

1.某人在某个时间上了某辆公交车：
   
   1 person_id bus_id board_time

2.某人从某辆公交车下车：
   
   2 person_id bus_id

3.对某辆公交车进行调查：
   
   3 bus_id

数据限制：
* 乘客的编号唯一
* 任意时刻，乘客的状态唯一（不在任何车上或在某一辆车上）
* 数据符合基本逻辑，乘客在上车前必须处于不在任何车上的状态，在下车前必须处于在某一辆车上的状态；输入指令中的时间严格按照从早到晚的顺序。
总而言之，输入数据保证正确的程序在运行过程中不会抛出任何异常

输出：

待指令输入结束后，按照第三类指令的输入顺序依次输出每次的调查结果，每次的调查结果占一行，每行输出如下：
person_id_1 board_time_1 person_id_n board_time_n
其中person_id_i,i∈[1,n]
person\_id\_i, i \in [1,n]
person_id_i,i∈[1,n] 表示调查指令发出时公交车内人的编号，board_time_i,i∈[1,n]
board\_time\_i, i\in[1,n]
board_time_i,i∈[1,n] 表示调查指令发出时编号为
person_id_i
person\_id\_i
person_id_i 的乘客乘坐被调查公交车的乘车时间，
person_id_
iperson\_id\_
iperson_id_i 和board_time_i
board\_time\_i
board_time_i 以一个空格分隔

如果调查结果内不包含任何乘客请输出

empty bus

样例：
```
stdin:
1 1 1 10:00
3 1
2 1 1
1 1 2 10:10
3 1
3 2

stdout:
1 10:00
empty bus
1 10:10
```

提交前请务必确保Main.java与Person.java与官方包下发的保持一致

# 第六章案例补充

## 接口示例

### 接口与回调

一个简单类似于回调(callback)的例子

```
package timer;

/**
 * Created by tpwy on 7/14/17.
 */

import java.awt.*;
import java.awt.event.*;
import java.util.*;
import javax.swing.*;
import javax.swing.Timer; // to resovle conflict with java.util.Timer

public class TimerTest {
    public static void main(String[] args)
    {
        ActionListener listener = new TimePrinter();

        Timer t = new Timer(10000, listener); //第一个参数是发出通告的时间间隔，第二个是监听器对象
        t.start(); // 启动Timer定时器
        JOptionPane.showConfirmDialog(null, "Quit program?"); // 调用GUI对话框来终止程序运行
        System.exit(0);
    }
}

class  TimePrinter implements ActionListener
{
    public void actionPerformed(ActionEvent event)
    {
        System.out.println("At the tone， the time is " + new Date()); // 打印一条信息
        Toolkit.getDefaultToolkit().beep(); // 响一声
    }
}
```

### 对象克隆

```
package clone;

/**
 * Created by tpwy on 7/14/17.
 */
public class cloneTest {
    public static void main(String[] args)
    {
        try
        {
            Employee original = new Employee("John Q. Public", 5000);
            original.setHireDay(2000, 1,2);
            Employee copy = original.clone();
            copy.raiseSalary(10);
            copy.setHireDay(2002,12,30);
            System.out.println("original=" + original);
            System.out.println("copy=" + copy);
        }
        catch (CloneNotSupportedException e)
        {
            e.printStackTrace();
        }
    }
}
```

```
package clone;

import java.util.Date;
import java.util.GregorianCalendar;

/**
 * Created by tpwy on 7/14/17.
 */
public class Employee implements Cloneable{
    private String name;
    private double salary;
    private Date hireDay;

    public Employee(String name, double salary)
    {
        this.name = name;
        this.salary = salary;
        hireDay = new Date();
    }

    public Employee clone() throws CloneNotSupportedException
    {
        // call Object.clone()
        Employee cloned = (Employee) super.clone();

        // clone mutable fields
        cloned.hireDay = (Date) hireDay.clone();

        return cloned;
    }

    public void setHireDay(int year, int month, int day)
    {
        Date newHireDay = new GregorianCalendar(year, month -1, day).getTime();
        hireDay.setTime(newHireDay.getTime());
    }

    public void raiseSalary(double byPercent)
    {
        double raise = salary * byPercent / 100;
        salary += raise;
    }

    public String toString()
    {
        return "Employee[name=" + name + ",slary=" + salary + ",hireDay=" + hireDay + "]";
    }

}
```

## lambda 表达式示例

```
String[] planets = new String[] {"Mercury", "Venus", "Earth", "Mars",
                                "jupiter", "Saturn", "Uranus", "Nepture"};
System.out.println(Arrays.toString(planets));
System.out.println("sorted in dictionary order: ");
Arrays.sort(planets);
System.out.println(Arrays.toString(planets));
System.out.println("sorted by length: ");
Arrays.sort(planets, (first, seond) -> first.length() - seond.length());
System.out.println(Arrays.toString(planets));

Timer t = new Timer(1000, event -> System.out.println("The time is " + new Date()));
t.start();

JOptionPane.showConfirmDialog(null, "Quit program?");
System.exit(0);
```
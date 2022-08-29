---
sort: 10
comments: true
---

# 집계 함수 : Sum, Max, Min, Average, Count, Aggregate

**C#의 Linq 집계 함수란 무엇입니까?**  
Linq 집계 함수는 여러 행의 값을 입력으로 그룹화한 다음 출력을 단일 값으로 반환하는 데 사용됩니다. 간단히 말해서 C#의 집계 함수는 항상 단일 값을 반환한다고 말할 수 있습니다. 

**C#에서 집계 함수는 언제 사용합니까?**
컬렉션의 숫자 속성에 대해 Sum, Count, Max, Min, Average 및 Aggregate와 같은 일부 수학 연산을 수행하려면 Linq 집계 함수를 사용해야 합니다.

**Linq에서 제공하는 집계 방법은 무엇입니까?**
다음은 컬렉션에 대한 수학 연산을 수행하기 위해 Linq에서 제공하는 집계 메서드입니다.

1. Sum(): 이 메서드는 컬렉션의 total(sum) 값을 계산하는 데 사용됩니다.
2. Max(): 이 메서드는 컬렉션에서 가장 큰 값을 찾는 데 사용됩니다.
3. Min(): 이 메서드는 컬렉션에서 가장 작은 값을 찾는 데 사용됩니다.
4. Average(): 이 메서드는 컬렉션의 숫자 형식의 평균 값을 계산하는 데 사용됩니다.
5. Count(): 이 메서드는 컬렉션에 있는 요소의 수를 계산하는 데 사용됩니다.
6. Aggregate(): 이 메서드는 컬렉션 값에 대해 사용자 지정 집계 작업을 수행하는 데 사용됩니다.


## <font color='dodgerblue' size="6">1) Sum 함수</font>     

- ### A. Sum 함수란?
    Linq Sum() 메서드는 집계 함수 범주에 속합니다. C#의 Linq Sum 메서드는 컬렉션에 있는 숫자 값의 합계 또는 합계를 계산하는 데 사용됩니다. 몇 가지 예를 들어 Sum() 메서드를 이해합시다.

- ### B. 메서도 또는 쿼리 구문을 사용하는 Sum 함수 예제
    **예제 1**  
    다음 예제에서는 컬렉션에 있는 모든 정수의 합계를 계산합니다.

    ```cs
   using System;
    using System.Linq;
    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                int[] intNumbers = new int[] { 10, 30, 50, 40, 60, 20, 70, 90, 80, 100 };

                //Using Method Syntax
                int MSTotal = intNumbers.Sum();

                //Using Query Syntax
                int QSTotal = (from num in intNumbers
                            select num).Sum();

                Console.WriteLine("Sum = " + QSTotal);

                Console.ReadKey();
            }
        }
    }
    ```

    결과  
    ![10_01_SumExam1Result.png](image/10/10_01_SumExam1Result.png)  

    참고: Linq 쿼리 구문에는 sum이라는 연산자가 없습니다. 따라서 여기서는 혼합 구문을 사용해야 합니다.

    **예제 2: 필터가 있는 Linq Sum 메서드**  
    이제 50보다 큰 모든 숫자의 합을 계산해야 합니다.

    ```cs
    using System;
    using System.Linq;
    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                int[] intNumbers = new int[] { 10, 30, 50, 40, 60, 20, 70, 90, 80, 100 };

                //Using Method Syntax
                int MSTotal = intNumbers.Where(num => num > 50).Sum();

                //Using Query Syntax
                int QSTotal = (from num in intNumbers
                            where num > 50
                            select num).Sum();

                Console.WriteLine("Sum = " + QSTotal);

                Console.ReadKey();
            }
        }
    }
    ```

    ![10_02_SumExam2Result.png](image/10/10_02_SumExam2Result.png)  

    **예제 3: 서술부가 있는 Linq Sum 메서드**  
    where 메서드를 사용하여 데이터를 필터링하는 대신 서술부를 사용하여 데이터를 필터링하는 논리를 작성하는 Sum 메서드의 다른 오버로드된 버전을 사용할 수도 있습니다.

    ```cs
    using System;
    using System.Linq;
    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                int[] intNumbers = new int[] { 10, 30, 50, 40, 60, 20, 70, 90, 80, 100 };

                //Using Method Syntax with a Predicate
                int MSTotal = intNumbers.Sum(num => {
                    if (num > 50)
                        return num;
                    else
                        return 0;
                });
                
                Console.WriteLine("Sum = " + MSTotal);

                Console.ReadKey();
            }
        }
    }
    ```

    ![10_03_SumExam3Result.png](image/10/10_03_SumExam3Result.png)  

- ### C. 복합 유형과 함께 LINQ Sum 메서드 사용
    다음 Employee 클래스로 작업할 것입니다.

    ```cs
    using System.Collections.Generic;
    namespace LINQDemo
    {
        public class Employee
        {
            public int ID { get; set; }
            public string Name { get; set; }
            public int Salary { get; set; }
            public string Department { get; set; }

            public static List<Employee> GetAllEmployees()
            {
                List<Employee> listStudents = new List<Employee>()
                {
                    new Employee{ID= 101,Name = "Preety", Salary = 10000, Department = "IT"},
                    new Employee{ID= 102,Name = "Priyanka", Salary = 15000, Department = "Sales"},
                    new Employee{ID= 103,Name = "James", Salary = 50000, Department = "Sales"},
                    new Employee{ID= 104,Name = "Hina", Salary = 20000, Department = "IT"},
                    new Employee{ID= 105,Name = "Anurag", Salary = 30000, Department = "IT"},
                    new Employee{ID= 106,Name = "Sara", Salary = 25000, Department = "IT"},
                    new Employee{ID= 107,Name = "Pranaya", Salary = 35000, Department = "IT"},
                    new Employee{ID= 108,Name = "Manoj", Salary = 11000, Department = "Sales"},
                    new Employee{ID= 109,Name = "Sam", Salary = 45000, Department = "Sales"},
                    new Employee{ID= 110,Name = "Saurav", Salary = 25000, Department = "Sales"}
                };

                return listStudents;
            }
        }
    }
    ```

    이것은 ID, Name, Salary 및 Department 와 같은 4가지 속성이 있는 매우 간단한 Employee 클래스입니다 . 우리는 또한 모든 직원의 목록을 반환할 GetAllEmployees() 와 같은 하나의 메서드를 만듭니다.

    **예제4:**
    다음 예에서는 모든 직원의 급여 합계를 계산합니다.

    ```cs
    using System;
    using System.Linq;
    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Using Method Syntax
                var TotalSalaryMS = Employee.GetAllEmployees()
                                .Sum(emp => emp.Salary);

                //Using Query Syntax
                var TotalSalaryQS = (from emp in Employee.GetAllEmployees()
                                    select emp).Sum(e => e.Salary);
                
                Console.WriteLine("Sum Of Salary = " + TotalSalaryMS);

                Console.ReadKey();
            }
        }
    }
    ```

    결과  
    ![10_04_SumExam4Result.png](image/10/10_04_SumExam4Result.png)  

    **예제5:**  
    다음 예에서는 IT 부서에 속한 모든 직원의 급여 합계를 계산합니다.

    ```cs
    using System;
    using System.Linq;
    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Using Method Syntax
                var TotalSalaryMS = Employee.GetAllEmployees()
                                .Where(emp => emp.Department == "IT")
                                .Sum(emp => emp.Salary);

                //Using Query Syntax
                var TotalSalaryQS = (from emp in Employee.GetAllEmployees()
                                    where emp.Department == "IT"
                                    select emp).Sum(e => e.Salary);
                
                Console.WriteLine("IT Department Total Salary = " + TotalSalaryQS);

                Console.ReadKey();
            }
        }
    }
    ```
    결과  
    ![10_05_SumExam5Result.png](image/10/10_05_SumExam5Result.png) 

     **예제6:**  
    사용자 정의 서술부를 사용하여 이전 예제를 다시 작성해 보겠습니다.

    ```cs
    using System;
    using System.Linq;
    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Using Method Syntax and Predicate
                var TotalSalaryMS = Employee.GetAllEmployees()
                                .Sum(emp => {
                                    if (emp.Department == "IT")
                                        return emp.Salary;
                                    else
                                        return 0;
                                });
                
                Console.WriteLine("IT Department Total Salary = " + TotalSalaryMS);

                Console.ReadKey();
            }
        }
    }
    ```
    결과  
    ![10_06_SumExam6Result.png](image/10/10_06_SumExam6Result.png) 


































































































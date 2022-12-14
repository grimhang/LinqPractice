---
sort: 5
comments: true
---

# LINQ 연산자들

이 기사에서는 C# 의 LINQ 연산자 에 대해 설명합니다 . LINQ 확장 메서드 와 구현 방법 에 대해 논의한 이전 기사를 읽어보십시오 . 이 기사의 일부로 다음 두 가지 사항에 대해 논의할 것입니다.

1. LINQ 연산자란 무엇인가?
2. LINQ 연산자들의 카테고리별 분류



## <font color='dodgerblue' size="6">1). LINQ 연산자란 무엇인가?</font>
LINQ 연산자는 LINQ 쿼리를 작성하는 데 사용되는 확장 메서드 집합일 뿐입니다. 이러한 LINQ 확장 메서드는 데이터 원본에 적용할 수 있는 매우 유용한 기능을 많이 제공합니다. 일부 기능은 데이터 필터링, 데이터 정렬, 데이터 그룹화 등입니다.




## <font color='dodgerblue' size="6">2) LINQ 연산자들의 카테고리별 종류</font> 
LINQ에서 연산자는 다음 범주로 나뉩니다.

1. SELECT 프로젝션 연산자
2. 정렬 연산자
3. 필터링 연산자
4. 집합 연산자
5. 수량자 연산자
6. 그룹화 연산자
7. 파티셔닝 연산자
8. 등호 연산자
9. 요소 연산자
10. 변환 연산자
11. 연결 연산자
12. 집계 연산자
13. 생성 연산자
14. 조인 연산자
15. 사용자 지정 시퀀스 연산자
16 기타 연산자


## <font color='dodgerblue' size="6">3) Select 프로젝션 연산자</font> 
예제와 함께 C#의 LINQ Select Projection 연산자에 대해 설명한다. C#에서 LINQ 연산자 와 LINQ 연산자의 다양한 범주에 대해 논의한 이전 기사를 읽어보십시오. 이 기사의 끝에서 C#의 LINQ Select Projection Operator와 관련된 다음 중요사항들 이해하게 될 것입니다.

1. 프로젝션이란 무엇입니까?
2. LINQ의 프로젝션 연산자 및 메서드는 무엇입니까?
3. SELECT 연산자를 사용하여 단일 속성을 선택하는 방법
4. SELECT 연산자를 사용하여 다른 클래스로부터 데이터를 선택하는 방법
5. SELECT 연산자를 사용하여 익명 타입의 데이터를 선택하는 방법
6. SELECT 연산자를 사용하여 선택한 데이터에 대한 계산을 수행하는 방법
7. 인덱스 값으로 데이터를 선택하는 방법은 무엇입니까?

- ### A. 프로젝션이란 무엇?
    프로젝션은 데이터 소스에서 데이터를 선택하는 데 사용되는 메커니즘일 뿐이다. 원본데이터 중에서 동일한 일부분의 데이터만 선택할 수도 있고 아니면 원본을 약간 가공하여 약간만 틀린 형태로 만들어 가져올수도 있다.

- ### B. LINQ의 프로젝션 연산자 및 메서드는 무엇입니까?
    프로젝션에는 두 가지 방법이 있다.

    1. SELECT
    2. SELECTMANY

    앞부분에서는 SELECT 메서드에 대해 설명하고 뒤부분에서는 SELECTMANY 메서드에 대해서도 설명할 것이다.

    - **연산자 선택:**  
        우리가 알고 있듯이  SQL의 Select 절을 사용하면 특정 열을 가져오던지 아니면 모든 모든 열 또는 특정열들만을 지정하여 가져올수 있다.

        같은 방식으로 LINQ의 Select 연산자를 사용하면 동일하게 작업을 할수 있다. 표준 LINQ Select 연산자를 사용하면 일부 계산을 수행할 수도 있습니다.

        - **예시:**  
            몇 가지 예를 들어 선택 프로젝션 연산자를 이해하자. 여기서는 콘솔 응용 프로그램을 사용할 것이며 먼저 LINQDemo 라는 이름으로 프로젝트를 만들어 보자. 그런 다음 Employee.cs라는 이름의 새 클래스 파일을 추가한다. 그리고 Employee.cs의 소스를 다음에서 복사하여 붙여넣는다.

            ```cs
            using System.Collections.Generic;
            namespace LINQDemo
            {
                public class Employee
                {
                    public int ID { get; set; }
                    public string FirstName { get; set; }
                    public string LastName { get; set; }
                    public int Salary { get; set; }

                    public static List<Employee> GetEmployees()
                    {
                        List<Employee> employees = new List<Employee>
                        {
                            new Employee {ID = 101, FirstName = "Preety", LastName = "Tiwary", Salary = 60000 },
                            new Employee {ID = 102, FirstName = "Priyanka", LastName = "Dewangan", Salary = 70000 },
                            new Employee {ID = 103, FirstName = "Hina", LastName = "Sharma", Salary = 80000 },
                            new Employee {ID = 104, FirstName = "Anurag", LastName = "Mohanty", Salary = 90000 },
                            new Employee {ID = 105, FirstName = "Sambit", LastName = "Satapathy", Salary = 100000 },
                            new Employee {ID = 106, FirstName = "Sushanta", LastName = "Jena", Salary = 160000 }
                        };

                        return employees;
                    }
                }
            }
            ```

            보시다시피 ID, FirstName, LastName 및 Salary와 같은 네 가지 속성을 사용하여 Employee 클래스를 만들었으며 또한 데이터 소스 역할을 할 직원 목록을 반환하는 하나의 정적 메서드도 만들었다. LINQ Select 연산자를 이해하기 위한 몇 가지 예를 살펴보자.

            - 예1 :  
                메써드 및 쿼리 구문을 모두 사용하여 데이터 원본에서 모든 데이터를 선택하는 예제이다.

                ![05_01_Example1.png](image/05/05_01_Example1.png)   

                Program 클래스를 아래와 같이 수정한 전체 코드이다.

                ```cs
                using System;
                using System.Collections.Generic;
                using System.Linq;

                namespace LINQDemo
                {
                    class Program
                    {
                        static void Main(string[] args)
                        {
                            //Using Query Syntax
                            List<Employee> basicQuery = (from emp in Employee.GetEmployees()
                                            select emp).ToList();

                            foreach (Employee emp in basicQuery)
                            {
                                Console.WriteLine($"ID : {emp.ID} Name : {emp.FirstName} {emp.LastName}");
                            }

                            //Using Method Syntax
                            IEnumerable<Employee> basicMethod = Employee.GetEmployees().ToList();
                            foreach (Employee emp in basicMethod)
                            {
                                Console.WriteLine($"ID : {emp.ID} Name : {emp.FirstName} {emp.LastName}");
                            }
                            
                            Console.ReadKey();
                        }
                    }
                }
                ```
    
- ### C. SELECT 연산자를 사용하여 단일 속성을 선택하는 방법  
    메서드와 쿼리 구문을 각각 사용하여 모든 직원 ID만 선택하는 방법이다.

    ![05_02_Select.png](image/05/05_02_Select.png)   

    ```note
    쿼리 구문에서 basicPropQuery의 데이터 형식은 List<int>입니다. 이는 쿼리 구문에 적용한 ToList() 메서드 때문이다. 그리고 이 ToList() 메서드가 쿼리가 실행되는 지점이다.
    ```

    하지만 Method 구문의 경우 ToList() 메서드를 적용하지 않았으며 이것이 basicPropMethod 변수의 데이터 유형이 IEnumerable<int> 유형인 이유이다. 그리고 더 중요한 것은 그 시점에서 쿼리가 생성되기만 하고 실행되지는 않는다는 것이다. foreach 루프 내에서 basicPropMethod를 사용할 때 쿼리가 실행된다.

    전체 예제
    ```cs
    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Using Query Syntax
                List<int> basicPropQuery = (from emp in Employee.GetEmployees()
                                            select emp.ID)
                                            .ToList();

                foreach (var id in basicPropQuery)
                {
                    Console.WriteLine($"ID : {id}");
                }

                //Using Method Syntax
                IEnumerable<int> basicPropMethod = Employee.GetEmployees()
                                                .Select(emp => emp.ID);
                
                foreach (var id in basicPropMethod)
                {
                    Console.WriteLine($"ID : {id}");
                }
                
                Console.ReadKey();
            }
        }
    }
    ```

    **또다른 예제:**  

    이번의 요구 사항은 직원 이름, 성 및 급여 속성만 선택하는 것이다. ID 속성을 선택할 필요가 없다.
    
    ![05_03_SelectMany.png](image/05/05_03_SelectMany.png)   

    전체 코드

    ```cs
    using System;
    using System.Collections.Generic;
    using System.Linq;

    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Query Syntax
                IEnumerable<Employee> selectQuery = (from emp in Employee.GetEmployees()
                                                    select new Employee()
                                                    {
                                                        FirstName = emp.FirstName,
                                                        LastName = emp.LastName,
                                                        Salary = emp.Salary
                                                    });
                
                foreach (var emp in selectQuery)
                {
                    Console.WriteLine($" Name : {emp.FirstName} {emp.LastName} Salary : {emp.Salary} ");
                }
                
                //Method Syntax
                List<Employee> selectMethod = Employee.GetEmployees().
                                            Select(emp => new Employee()
                                            {
                                                FirstName = emp.FirstName,
                                                LastName = emp.LastName,
                                                Salary = emp.Salary
                                            }).ToList();

                foreach (var emp in selectMethod)
                {
                    Console.WriteLine($" Name : {emp.FirstName} {emp.LastName} Salary : {emp.Salary} ");
                }

                Console.ReadKey();
            }
        }
    }
    ```

- ### D. SELECT연산자를 사용하여 다른 클래스로부터 데이터를 선택하는 방법  
    LINQ Select 연산자를 사용하여 다른 클래스로부터의 데이터를 선택할 수도 있다.  
    이전 예제에서는 동일한 Employee 클래스에서 First Name, Last Name 및 Salary 속성을 선택하였다. 이번에는 위의 세 가지 속성만을 가진 새 클래스를 만들어 보겠다. 따라서 EmployeeBasicInfo.cs라는 이름의 새 클래스 파일을 추가하고 다음 코드를 복사하여 붙여넣는다.

    ```cs
    namespace LINQDemo
    {
        public class EmployeeBasicInfo
        {
            public string FirstName { get; set; }
            public string LastName { get; set; }
            public int Salary { get; set; }
        }
    }
    ```

    이제 여기서 해야 할 일은 위에서 새로 만든 EmployeeBasicInfo 클래스에 First Name, Last Name 및 Salary 속성을 반영해야 한다는 것입니다.
    
    ![05_04_SelectNew.png](image/05/05_04_SelectNew.png)   

    ```cs
    using System;
    using System.Collections.Generic;
    using System.Linq;

    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Query Syntax
                IEnumerable<EmployeeBasicInfo> selectQuery = (from emp in Employee.GetEmployees()
                                                    select new EmployeeBasicInfo()
                                                    {
                                                        FirstName = emp.FirstName,
                                                        LastName = emp.LastName,
                                                        Salary = emp.Salary
                                                    });
                
                foreach (var emp in selectQuery)
                {
                    Console.WriteLine($" Name : {emp.FirstName} {emp.LastName} Salary : {emp.Salary} ");
                }


                //Method Syntax
                List<EmployeeBasicInfo> selectMethod = Employee.GetEmployees().
                                            Select(emp => new EmployeeBasicInfo()
                                            {
                                                FirstName = emp.FirstName,
                                                LastName = emp.LastName,
                                                Salary = emp.Salary
                                            }).ToList();
                foreach (var emp in selectMethod)
                {
                    Console.WriteLine($" Name : {emp.FirstName} {emp.LastName} Salary : {emp.Salary} ");
                }

                Console.ReadKey();
            }
        }
    }
    ```

- ### E. SELECT 연산자를 사용하여 익명 타입의 데이터를 선택하는 방법
    Employee 또는 EmployeeBasicInfo와 같은 특정 형식에 데이터를 투영하는 대신 LINQ의 익명 형식에 데이터를 투영할 수도 있다.

    ![05_05_SelectNewAnony.png](image/05/05_05_SelectNewAnony.png)   

    ```cs
    using System;
    using System.Collections.Generic;
    using System.Linq;

    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Query Syntax
                var selectQuery = (from emp in Employee.GetEmployees()
                                                    select new
                                                    {
                                                        FirstName = emp.FirstName,
                                                        LastName = emp.LastName,
                                                        Salary = emp.Salary
                                                    });
                
                foreach (var emp in selectQuery)
                {
                    Console.WriteLine($" Name : {emp.FirstName} {emp.LastName} Salary : {emp.Salary} ");
                }
                
                //Method Syntax
                var selectMethod = Employee.GetEmployees().
                                            Select(emp => new
                                            {
                                                FirstName = emp.FirstName,
                                                LastName = emp.LastName,
                                                Salary = emp.Salary
                                            }).ToList();
                foreach (var emp in selectMethod)
                {
                    Console.WriteLine($" Name : {emp.FirstName} {emp.LastName} Salary : {emp.Salary} ");
                }

                Console.ReadKey();
            }
        }
    }
    ```

- ### F. SELECT 연산자를 사용하여 선택한 데이터에 대한 계산을 수행하는 방법  
    먼저 우리가 달성하고자 하는 것에 대한 수식은 다음과 같다.

    1. 연간 급여 = 급여*12
    2. 성명 = 이름 + " " + 성
    
    그런 다음 익명 유형으로 ID, AnnualSalary 및 FullName을 SELECT로 만들어내자.

    ![05_06_SelectCalc.png](image/05/05_06_SelectCalc.png)  

    ```cs
    using System;
    using System.Collections.Generic;
    using System.Linq;

    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Query Syntax
                var selectQuery = (from emp in Employee.GetEmployees()
                                select new
                                {
                                    EmployeeId = emp.ID,
                                    FullName = emp.FirstName + " " + emp.LastName,                  
                                    AnnualSalary = emp.Salary * 12
                                });
                
                foreach (var emp in selectQuery)
                {
                    Console.WriteLine($" ID {emp.EmployeeId} Name : {emp.FullName} Annual Salary : {emp.AnnualSalary} ");
                }

                //Method Syntax
                var selectMethod = Employee.GetEmployees().
                                            Select(emp => new
                                            {
                                                EmployeeId = emp.ID,
                                                FullName = emp.FirstName + " " + emp.LastName,
                                                AnnualSalary = emp.Salary * 12
                                            }).ToList();
                foreach (var emp in selectMethod)
                {
                    Console.WriteLine($" ID {emp.EmployeeId} Name : {emp.FullName} Annual Salary : {emp.AnnualSalary} ");
                }

                Console.ReadKey();
            }
        }
    }
    ```

- ### G. 인덱스 값으로 데이터를 선택하는 방법은?
    정수 인덱스를 사용하여 값을 선택하는 것도 가능하다. 인덱스는 0을 기준.

    ![05_07_SelectIndex.png](image/05/05_07_SelectIndex.png)  

    ```cs
    using System;
    using System.Linq;

    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {

                //Query Syntax
                var query = (from emp in Employee.GetEmployees().Select((value, index) => new { value, index })
                            select new
                            {
                                IndexPosition = emp.index,
                                FullName = emp.value.FirstName + " " + emp.value.LastName,
                                Salary = emp.value.Salary
                            }).ToList();
                foreach (var emp in query)
                {
                    Console.WriteLine($" Position {emp.IndexPosition} Name : {emp.FullName} Salary : {emp.Salary} ");
                }

                //Method Syntax
                var selectMethod = Employee.GetEmployees().
                                            Select((emp, index) => new
                                            {
                                                IndexPosition = index,
                                                FullName = emp.FirstName + " " + emp.LastName,
                                                Salary = emp.Salary
                                            });
                
                foreach (var emp in selectMethod)
                {
                    Console.WriteLine($" Position {emp.IndexPosition} Name : {emp.FullName} Salary : {emp.Salary} ");
                }

                Console.ReadKey();
            }
        }
    }
    ```

## <font color='dodgerblue' size="6">4) C# SelectMany 프로젝션 연산자(예제 포함)</font>
예제와 함께 C#의 LINQ SelectMany에 대해 논의한다. C#의 Select 연산자에 대해 몇 가지 예와 함께 논의한 이전 기사를 읽어보십시오 . LINQ SelectMany 메서드는 프로젝션 범주 연산자에 속합니다. 이 기사의 일부로 다음 포인터에 대해 자세히 논의할 것입니다.

1. LINQ SelectMany란 무엇입니까?
2. C#에서 쿼리 및 메서드 구문을 모두 사용하는 예.

- ### A. LINQ SelectMany란 무엇입니까?

    LINQ의 SelectMany는 시퀀스의 각 요소를 IEnumerable<T> 에 투영 한 다음 결과 시퀀스를 하나의 시퀀스로 병합하는 데 사용됩니다. 즉, SelectMany 연산자는 결과 시퀀스의 레코드를 결합한 다음 하나의 결과로 변환합니다. 이것이 현재 명확하지 않은 경우 실제에서 볼 수 있으므로 걱정하지 마십시오.

    ```cs
    using System;
    using System.Collections.Generic;
    using System.Linq;

    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                List<string> nameList = new List<string>(){"Pranaya", "Kumar" };
                IEnumerable<char> methodSyntax = nameList.SelectMany(x => x);
                
                foreach(char c in methodSyntax)
                {
                    Console.Write(c + " ");
                }

                Console.ReadKey();
            }
        }
    }
    ```

    위의 코드에서 SelectMany 메서드가 IEnumerable<char> 를 반환하는 것을 볼 수 있습니다. 이는 SelectMany 메서드가 시퀀스의 모든 요소를 ​​반환하기 때문입니다. 여기서 List는 시퀀스이며 문자열이 포함된다. 여기 List에는 두 개의 문자열이 있다. 따라서 SelectMany 메서드는 위의 두 문자열에서 모든 문자를 가져온 다음 하나의 시퀀스(예: IEnumerable<char> )로 변환합니다 .

    따라서 위의 프로그램을 실행하면 다음과 같은 결과가 나옵니다.

    ![06_01_SelectMany.png](image/05/05_08_SelectMany.png)  


- ### B. C#에서 쿼리 및 메서드 구문을 모두 사용하는 예.
    가장 중요한 점은 쿼리 구문을 작성하기 위한 LINQ SelectMany 연산자가 없다는 것이다. 그러나 아래 예제와 같이 쿼리에 여러 "from 절"을 작성하여 이를 달성할 수 있다.

    ```cs
    using System;
    using System.Collections.Generic;
    using System.Linq;

    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                List<string> nameList = new List<string>(){"Pranaya", "Kumar" };
            
                IEnumerable<char> querySyntax = from str in nameList
                                                from ch in str
                                                select ch;

                foreach (char c in querySyntax)
                {
                    Console.Write(c + " ");
                }

                Console.ReadKey();
            }
        }
    }
    ```

    이제 프로그램을 실행하면 예상대로 메서드 구문과 동일한 출력이 표시됩니다

    **예2:**  
    C#의 복합 유형이 있는 LINQ SelectMany

    이름이 Student.cs인 클래스 파일을 만들고 다음 코드를 복사하여 붙여넣습니다.

    ```cs
    using System.Collections.Generic;
    namespace LINQDemo
    {
        public class Student
        {
            public int ID { get; set; }
            public string Name { get; set; }
            public string Email { get; set; }
            public List<string> Programming { get; set; }

            public static List<Student> GetStudents()
            {
                return new List<Student>()
                {
                    new Student(){ID = 1, Name = "James", Email = "James@j.com", Programming = 
                        new List<string>() { "C#", "Jave", "C++"} },
                    new Student(){ID = 2, Name = "Sam", Email = "Sara@j.com", Programming = 
                        new List<string>() { "WCF", "SQL Server", "C#" }},
                    new Student(){ID = 3, Name = "Patrik", Email = "Patrik@j.com", Programming = 
                        new List<string>() { "MVC", "Jave", "LINQ"} },
                    new Student(){ID = 4, Name = "Sara", Email = "Sara@j.com", Programming = 
                        new List<string>() { "ADO.NET", "C#", "LINQ" } }
                };
            }
        }
    }
    ```

    보시다시피 4개의 속성이 있는 Student 클래스를 만들었습니다. 그 중 Programming속성은 List<string>을 반환한다는 것을 기억하십시오. 여기서 우리는 또한 데이터 소스를 작동할 학생 목록을 반환하는 하나의 메서드를 만들었습니다.

    모든 학생의 모든 프로그래밍 문자열을 단일  IEnumerable<string> 에 투영해야 합니다 . 보시다시피 4명의 학생이 있으므로 4개의 IEnumerable<string> 시퀀스가 ​​있습니다. 그런 다음 단일 시퀀스, 즉 단일 IEnumerable<string> 시퀀스를 형성하기 위해 평면화해야 합니다.

    ![06_02_SelectDifficult.png](image/05/05_09_SelectDifficult.png)  

    ```cs
    using System;
    using System.Collections.Generic;
    using System.Linq;

    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Using Method Syntax
                List<string> MethodSyntax = Student.GetStudents().SelectMany(std => std.Programming).ToList();

                //Using Query Syntax
                IEnumerable<string> QuerySyntax = from std in Student.GetStudents()
                                from program in std.Programming
                                select program;

                //Printing the values
                foreach (string program in MethodSyntax)
                {
                    Console.WriteLine(program);
                }

                Console.ReadKey();
            }
        }
    }
    ```

    프로그램을 실행하면 다음과 같은 결과가 나옵니다.

    ![06_03_SelectDifficultResult.png](image/05/05_10_SelectDifficultResult.png)  


    **예3:**  
    이전 예에서 예상대로 출력을 얻었지만 프로그램 이름이 중복되었습니다. 고유한 프로그램 이름만 원하는 경우 아래 예와 같이 결과 집합에 고유한 방법을 적용해야 합니다.

    ```cs
    using System;
    using System.Collections.Generic;
    using System.Linq;

    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Using Method Syntax
                List<string> MethodSyntax = Student.GetStudents()
                                            .SelectMany(std => std.Programming)
                                            .Distinct()
                                            .ToList();

                //Using Query Syntax
                IEnumerable<string> QuerySyntax = (from std in Student.GetStudents()
                                from program in std.Programming
                                select program).Distinct().ToList();

                //Printing the values
                foreach (string program in QuerySyntax)
                {
                    Console.WriteLine(program);
                }

                Console.ReadKey();
            }
        }
    }
    ```

    이제 프로그램을 실행하면 시퀀스에서 중복된 프로그램 이름이 제거되는 것을 볼 수 있습니다.

    **예4:**  
    이제 프로그램 언어 이름과 함께 학생 이름을 검색해야 합니다.

    ```cs
    using System;
    using System.Collections.Generic;
    using System.Linq;

    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Using Method Syntax
                var MethodSyntax = Student.GetStudents()
                                            .SelectMany(std => std.Programming,
                                                (student, program) => new
                                                {
                                                    StudentName = student.Name,
                                                    ProgramName = program
                                                }
                                                )
                                            .ToList();

                //Using Query Syntax
                var QuerySyntax = (from std in Student.GetStudents()
                                from program in std.Programming
                                select new {
                                    StudentName = std.Name,
                                    ProgramName = program
                                }).ToList();

                //Printing the values
                foreach (var item in QuerySyntax)
                {
                    Console.WriteLine(item.StudentName + " => " + item.ProgramName);
                }

                Console.ReadKey();
            }
        }
    }
    ```

    결과는 다음과 같다

    ![06_04_SelectResult2.png](image/05/05_11_SelectResult2.png)  

## <font color='dodgerblue' size="6">5) LINQ에서 Where 필터링 연산자</font>    
이 기사에서는 예제와 함께 LINQ의 Where Filtering Operators에 대해 설명합니다. 몇 가지 예와 함께 C#의 SelectMany Projection 연산자에 대해 논의한 이 기사로 진행하기 전에 이전 기사를 읽어보십시오 . 이 기사를 마치면 다음 개념을 이해하게 될 것입니다.

1. 필터링이란
2. LINQ에서 사용할 수 있는 필터링 방법은 무엇입니까?
3. "where" 연산자 및 메서드와 쿼리 구문을 모두 사용하는 예제
4. 술어란?

- ### A. 필터링이란 무엇인가?
    필터링은 데이터 소스에서 주어진 조건을 만족하는 요소만 가져오는 프로세스일 뿐입니다. 비즈니스 요구 사항에 따라 둘 이상의 조건으로 데이터 소스에서 데이터를 가져올 수도 있습니다.

    예를 들어:

        급여가 5000만원 이상인 직원.
        특정 배치에서 80% 이상의 점수를 받은 학생.
        경력 6년 이상, 부서는 IT 등

- ### B. LINQ에서 사용할 수 있는 필터링 방법은 무엇입니까?
    필터링에 사용되는 C#의 LINQ에서 제공하는 두 가지 방법이 있습니다. 그들은 다음과 같습니다

    1. Where
    2. OfType

    이 기사에서는 " Where" 연산자에 대해 자세히 설명합니다. 다음 기사에서는 몇 가지 예와 함께 OfType 연산자에 대해 설명합니다.

    LINQ의 필터링 연산자:
    표준 쿼리 연산자 " where "는 LINQ 의 필터링 연산자 범주에 있습니다.

    where 절을 사용하여 SQL에서 했던 것처럼 일부 조건을 기반으로 데이터 원본의 데이터를 필터링해야 할 때 LINQ에서 표준 쿼리 연산자를 사용해야 합니다. 따라서 간단히 말해서 일부 조건을 기반으로 데이터 소스의 데이터를 필터링하는 데 사용된다고 말할 수 있습니다.

    "where"는 항상 최소한 하나의 조건을 예상하며 술어를 사용하여 조건을 지정할 수 있습니다. 조건은 다음 기호를 사용하여 작성할 수 있습니다.

    ==, >=, <=, &&, ||, >, < 등

    두 가지 오버로드된 버전의 "where" 연산자를 사용할 수 있습니다. 그들은 다음과 같습니다
    
    ![06_05_Where.png](image/06/06_05_Where.png)  

    위의 서명에서 볼 수 있듯이 메서드는 IEnumerable<T> 인터페이스에서 확장 메서드로 구현됩니다. 메소드 술어는 매개변수로 사용됩니다. 그럼 먼저 술어가 무엇인지 알아볼까요?

- ### D. 술어란?
    술어는 주어진 조건에 대해 각각의 모든 요소를 ​​테스트하는 데 사용되는 함수일 뿐입니다. 예를 들어 이것을 이해합시다.

    아래 예에서 Lambda 표현식( num => num > 5 )은 " intList " 컬렉션 에 있는 모든 요소에 대해 실행됩니다 . 그런 다음 숫자가 5보다 큰지 여부를 확인합니다. 숫자 값이 5보다 크면 부울 값 true가 반환되고 그렇지 않으면 false가 반환됩니다.

    ```cs
    using System;
    using System.Collections.Generic;
    using System.Linq;

    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                List<int> intList = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

                //Method Syntax
                IEnumerable<int> filteredData = intList.Where(num => num > 5);

                //Query Syntax
                IEnumerable<int> filteredResult = from num in intList
                                                where num > 5
                                                select num;
                
                foreach (int number in filteredData)
                {
                    Console.WriteLine(number);
                }

                Console.ReadKey();
            }
        }
    }
    ```

    위의 프로그램을 실행하면 예상대로 다음과 같은 출력이 나옵니다.
    
    ![06_06_PredicateResult.png](image/06/06_06_PredicateResult.png)  

    위의 예에서 WHERE  확장 메서드 위로 마우스를 가져가면  Visual Studio Intelligence에 다음이 표시됩니다.
    
    ![06_07_WhereDetail.png](image/06/06_07_WhereDetail.png)  

    위 이미지에서 볼 수 있듯이 술어(Func<int, bool> 술어)는 정수 유형의 하나의 입력 매개변수를 예상하고 부울 값을 리턴합니다. Func는 하나 이상의 입력 매개 변수를 사용하고 하나의 출력 매개 변수를 반환하는 일반 대리자입니다. 마지막 매개변수는 반환 값으로 간주됩니다. 반환 유형은 필수이지만 입력 매개변수는 필수가 아닙니다.

    Generic Delegate를 처음 사용하는 경우 예제와 함께 C#의 Generic Delegate에 대해 논의한 다음 문서를 읽는 것이 좋습니다.

    https://dotnettutorials.net/lesson/generic-delegates-csharp/

    위의 예에서 확장 메서드에 전달한 람다 식은 정수 데이터 형식에서 작동하며 부울 값을 반환해야 하며 그렇지 않으면 컴파일 시간 오류가 발생합니다. 

    따라서 위의 예에서 다음 코드 줄

    IEnumerable<int> filterData = intList.where(숫자 => 숫자 > 5);

    아래와 같이 다시 작성할 수 있습니다.

    Func<int, bool> 술어 = i => i > 5;

    IEnumerable<int>filteredData = intList.Where(술어);

    예상대로 동일한 출력을 제공해야 합니다. 또한 예상대로 작동하는 별도의 기능을 아래와 같이 생성할 수도 있습니다.

    ```cs
    using System;
    using System.Collections.Generic;
    using System.Linq;

    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                List<int> intList = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

                //Method Syntax
                IEnumerable<int> filteredData = intList.Where(num => CheckNumber(num));

                foreach (int number in filteredData)
                {
                    Console.WriteLine(number);
                }

                Console.ReadKey();
            }

            public static bool CheckNumber(int number)
            {
                if (number > 5)
                {
                    return true;
                }
                else
                {
                    return false;
                }
            }
        }
    }
    ```

    **예2**  
    "where" 확장 메서드의 두 번째 오버로드된 버전에서 술어 함수의 int 매개변수는 소스 요소의 인덱스 위치를 나타냅니다.
    ```cs
    public static IEnumerable<TSource> Where<TSource>(
    this IEnumerable<TSource> source,
    Func<TSource, int, bool> predicate);
    ```
    **이것을 이해하기 위해 예를 보자.**  
    여기서 우리는 홀수, 즉 2로 나눌 수 없는 숫자만 필터링해야 합니다. 숫자와 함께 숫자의 인덱스 위치도 가져와야 합니다. 인덱스는 0을 기준으로 합니다.

    ```cs
    using System;
    using System.Collections.Generic;
    using System.Linq;

    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                List<int> intList = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

                //Method Syntax
                var OddNumbersWithIndexPosition = intList.Select((num, index) => new
                                                    {
                                                        Numbers = num,
                                                        IndexPosition = index
                                                    }).Where(x => x.Numbers % 2 != 0)
                                                    .Select(data => new
                                                    {
                                                        Number = data.Numbers,
                                                        IndexPosition = data.IndexPosition
                                                    });
                
                foreach (var item in OddNumbersWithIndexPosition)
                {
                    Console.WriteLine($"IndexPosition :{item.IndexPosition} , Value : {item.Number}");
                }

                Console.ReadKey();
            }
        }
    }
    ```
    이제 응용 프로그램을 실행하면 아래와 같이 인덱스 위치와 함께 홀수가 표시됩니다.

    ![06_08_WhereDetailResult.png](image/06/06_08_WhereDetailResult.png)  

    **쿼리 구문을 사용하여 동일한 예제를 다시 작성해 보겠습니다.**
    ```cs
    using System;
    using System.Collections.Generic;
    using System.Linq;

    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                List<int> intList = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
                
                //Query Syntax
                var OddNumbersWithIndexPosition = from number in intList.Select((num, index) => new {Numbers = num, IndexPosition = index })
                                                where number.Numbers % 2 != 0
                                                select new
                                                {
                                                    Number = number.Numbers,
                                                    IndexPosition = number.IndexPosition
                                                };
                
                foreach (var item in OddNumbersWithIndexPosition)
                {
                    Console.WriteLine($"IndexPosition :{item.IndexPosition} , Value : {item.Number}");
                }

                Console.ReadKey();
            }
        }
    }
    ```

    이제 응용 프로그램을 실행하면 메서드 구문 출력과 동일한 출력도 제공됩니다.

    **복잡한예**  
    복합형에서 where 조건을 사용하는 방법을 알아보겠습니다. 다음 Employee 클래스를 사용할 것입니다. 따라서 Employee.cs 라는 이름의 클래스 파일을 만들고 다음 코드를 복사하여 붙여넣습니다.
    ```cs
    using System.Collections.Generic;
    namespace LINQDemo
    {
        public class Employee
        {
            public int ID { get; set; }
            public string Name { get; set; }
            public string Gender { get; set; }
            public int Salary { get; set; }
            public List<string> Technology { get; set; }

            public static List<Employee> GetEmployees()
            {
                List<Employee> employees = new List<Employee>()
                {
                    new Employee {ID = 101, Name = "Preety", Gender = "Female", Salary = 60000,
                                Technology = new List<string>() {"C#", "Jave", "C++"} },
                    new Employee {ID = 102, Name = "Priyanka", Gender = "Female", Salary = 50000,
                                Technology =new List<string>() { "WCF", "SQL Server", "C#" } },
                    new Employee {ID = 103, Name = "Hina", Gender = "Female", Salary = 40000,
                                Technology =new List<string>() { "MVC", "Jave", "LINQ"}},
                    new Employee {ID = 104, Name = "Anurag", Gender = "Male", Salary = 450000},
                    new Employee {ID = 105, Name = "Sambit", Gender = "Male", Salary = 550000},
                    new Employee {ID = 106, Name = "Sushanta", Gender = "Male", Salary = 700000,
                                Technology =new List<string>() { "ADO.NET", "C#", "LINQ" }}

                };

                return employees;
            }
        }
    }
    ```

    볼 수 있듯이 ID, 이름, 성별, 급여 및 기술과 같은 5가지 속성을 사용하여 Employee 클래스를 만들었습니다. 보시다시피 데이터 소스로 이동할 모든 직원 목록을 반환하는 메서드도 하나 만들었습니다.

    **예3**  
    급여가 50000보다 큰 모든 직원을 가져와야 합니다.
    ```cs
    using System;
    using System.Collections.Generic;
    using System.Linq;

    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Query Syntax
                var QuerySyntax = from employee in Employee.GetEmployees()
                                where employee.Salary > 50000
                                select employee;
                //Method Syntax
                var MethodSyntax = Employee.GetEmployees()
                                .Where(emp => emp.Salary > 50000);
                
                foreach (var emp in QuerySyntax)
                {
                    Console.WriteLine($"Name : {emp.Name}, Salary : {emp.Salary}, Gender : {emp.Gender}");
                    if(emp.Technology != null && emp.Technology.Count() > 0)
                    {
                        Console.Write(" Technology : ");
                        foreach (var tech in emp.Technology)
                        {
                            Console.Write(tech + " ");
                        }
                        Console.WriteLine();
                    }
                    else
                    {
                        Console.WriteLine(" Technology Not Available ");
                    }
                }

                Console.ReadKey();
            }
        }
    }`
    ```

    결과  
    ![06_09_Example3lResult.png](image/06/06_09_Example3lResult.png)  

    **예4**  
    성별이 남성이고 급여가 500000보다 큰 모든 직원을 가져와야 합니다.
    ```cs
    using System;
    using System.Collections.Generic;
    using System.Linq;

    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Query Syntax
                var QuerySyntax = from employee in Employee.GetEmployees()
                                where employee.Salary > 500000 && employee.Gender == "Male"
                                select employee;
                //Method Syntax
                var MethodSyntax = Employee.GetEmployees()
                                .Where(emp => emp.Salary > 500000 && emp.Gender == "Male")
                                .ToList();
                
                foreach (var emp in MethodSyntax)
                {
                    Console.WriteLine($"Name : {emp.Name}, Salary : {emp.Salary}, Gender : {emp.Gender}");
                    if(emp.Technology != null && emp.Technology.Count() > 0)
                    {
                        Console.Write(" Technology : ");
                        foreach (var tech in emp.Technology)
                        {
                            Console.Write(tech + " ");
                        }
                        Console.WriteLine();
                    }
                    else
                    {
                        Console.WriteLine(" Technology Not Available ");
                    }
                }

                Console.ReadKey();
            }
        }
    }
    ```

    결과    
    ![06_10_Example4Result.png](image/06/06_10_Example4Result.png)  

    **예5**
    사용자 지정 작업과 데이터를 익명 형식으로 투영하는 여러 조건:

    요구 사항:

    급여가 50000 이상이고 기술이 있는 모든 직원은 null이 아니어야 합니다.

    다음 정보를 익명 유형으로 가져와야 합니다.

    이름 그대로
    있는 그대로의 성별
    월 급여 = 급여 / 12

    ```cs
    using System;
    using System.Collections.Generic;
    using System.Linq;

    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Query Syntax
                var QuerySyntax = (from employee in Employee.GetEmployees()
                                where employee.Salary >= 50000 && employee.Technology != null
                                select new {
                                    EmployeeName = employee.Name,
                                    Gender = employee.Gender,
                                    MonthlySalary = employee.Salary / 12
                                }).ToList();
                
                //Method Syntax
                var MethodSyntax = Employee.GetEmployees()
                                .Where(emp => emp.Salary >= 50000 && emp.Technology != null)
                                .Select(emp => new {
                                    EmployeeName = emp.Name,
                                    Gender = emp.Gender,
                                    MonthlySalary = emp.Salary / 12
                                })
                                .ToList();
                
                foreach (var emp in QuerySyntax)
                {
                    Console.WriteLine($"Name : {emp.EmployeeName}, Gender : {emp.Gender}, Monthly Salary : {emp.MonthlySalary}");                
                }

                Console.ReadKey();
            }
        }
    }
    ```

    결과      
    ![06_11_Example5Result.png](image/06/06_11_Example5Result.png)  

    **예6 : 인덱스 위치와 함께 요소 가져오기**
    여기서 성별이 남성이고 급여가 500000보다 큰 모든 직원을 인덱스 위치와 함께 익명 유형으로 가져와야 합니다.
    ```cs
    using System;
    using System.Collections.Generic;
    using System.Linq;

    namespace LINQDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Query Syntax
                var QuerySyntax = (from data in Employee.GetEmployees().Select((Data, index) => new { employee = Data, Index = index })
                                where data.employee.Salary >= 500000 && data.employee.Gender == "Male"
                                select new
                                {
                                    EmployeeName = data.employee.Name,
                                    Gender = data.employee.Gender,
                                    Salary = data.employee.Salary,
                                    IndexPosition = data.Index
                                }).ToList();

                //Method Syntax
                var MethodSyntax = Employee.GetEmployees().Select((Data, index) => new { employee = Data, Index = index })
                                .Where(emp => emp.employee.Salary >= 500000 && emp.employee.Gender == "Male")
                                .Select(emp => new
                                {
                                    EmployeeName = emp.employee.Name,
                                    Gender = emp.employee.Gender,
                                    Salary = emp.employee.Salary,
                                    IndexPosition = emp.Index
                                })
                                .ToList();

                foreach (var emp in QuerySyntax)
                {
                    Console.WriteLine($"Position : {emp.IndexPosition} Name : {emp.EmployeeName}, Gender : {emp.Gender}, Salary : {emp.Salary}");
                }

                Console.ReadKey();
            }
        }
    }
    ```

    결과  
    ![06_12_Example6Result.png](image/06/06_12_Example6Result.png)  

## <font color='dodgerblue' size="6">6) LINQ에서 OfType 연산자</font>    

## <font color='dodgerblue' size="6">7) LINQ에서 Set 연산자</font>    

## <font color='dodgerblue' size="6">8) C#에서 LINQ Distinct 메쏘드</font>    

## <font color='dodgerblue' size="6">8) C#에서 LINQ Except</font>    

## <font color='dodgerblue' size="6">9) C#에서 LINQ Intersect 메쏘드</font>    

## <font color='dodgerblue' size="6">10) C#에서 LINQ Union</font>    

## <font color='dodgerblue' size="6">11) C#에서 LINQ Concat 메쏘드</font>    

 
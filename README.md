# 3sem_lab5
## Homework
### Задание 1
Создайте класс MyMatrix, представляющий матрицу m на n.
Создайте конструктор, принимающий число строк и столбцов, заполняющий матрицу случайными числами в диапазоне, который пользователь вводит при запуске программы.
Создайте метод Fill, перезаполняющий матрицу случайными значениями.
Создайте метод ChangeSize, изменяющий число строк и/или столбцов с копированием значений существующей матрицы. Если новая матрица больше существующий, то метод должен дозаполнить новую матрицу случайными числами.
Создайте метод ShowPartialy, принимающий в качестве параметров начальные и конечные значения строк и столбцов, значения матрицы внутри которых нужно вывести на консоль.
Создайте метод Show, выводящий все значения матрицы на консоль.
Создайте индексатор для матрицы вида this[int index1, int index2] с аксессором и мутатором.
```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

public class MyMatrix
{
    private int[,] matrix;//двумерный массив
    private int numstroki;
    private int numstolbci;
    private static Random random = new Random();//создаём объект и выделяем память
    //общее для всех объектов

    public MyMatrix(int stroki, int stolbci, int minRandomValue, int maxRandomValue)
    {
        numstroki = stroki;
        numstolbci = stolbci;
        matrix = new int[numstroki, numstolbci];

        for (int i = 0; i < numstroki; i++)
        {
            for (int j = 0; j < numstolbci; j++)
            {
                matrix[i, j] = random.Next(minRandomValue, maxRandomValue + 1);//диапазон
            }
        }
    }
    public int this[int stroka, int stolbec]//возвращаемый тип this
    {//проход по экземпляру класса this!!! для вставки значения
        get
        {
            if (stroka >= 0 && stroka < numstroki && stolbec >= 0 && stolbec < numstolbci)
            {
                return matrix[stroka, stolbec];
            }
            else
            {
                throw new IndexOutOfRangeException("Index vne diapazona");
            }

        }
        set
        {
            if (stroka >= 0 && stroka < numstroki && stolbec >= 0 && stolbec < numstolbci)
            {
                matrix[stroka, stolbec] = value;
            }
            else
            {
                throw new IndexOutOfRangeException("Index vne diapazona");
            }
        }

    }
    public void ChangeSize(int newstroki, int newstolbci, int minRandomValue, int maxRandomValue)
    {
        if ((newstroki == numstroki) && (newstolbci == numstolbci)) return;
        //если условие ((newstroki == numstroki) && (newstolbci == numstolbci)) истинно, то оператор return просто завершает текущую функцию без возвращения какого-либо значения
        int[,] newMatrix = new int[newstroki, newstolbci];
        for (int i = 0; i < Math.Min(newstroki, numstroki); i++)
        {
            for (int j = 0; j < Math.Min(newstroki, numstolbci); j++)//копирование
            {
                newMatrix[i, j] = matrix[i, j];
            }
        }
        if (newstroki > numstroki ||  newstolbci > numstolbci)
        {
            for (int i = 0; i < newstroki; i++)
            {
                for (int j = 0; j < newstroki; j++)
                {
                    if (i>=numstroki || j >= numstolbci)
                    {
                        newMatrix[i, j] = random.Next(minRandomValue, maxRandomValue);
                    }
                }

            }
        }
        matrix = newMatrix;
        numstroki = newstroki;
        numstolbci = newstolbci;
    }
    public void PrintPartialy(int startRow, int endRow, int startColumn, int endColumn)
    {
        for (int i = startRow; i <= endRow && i < numstroki; i++)
        {
            for (int j = startColumn; j <= endColumn && j < numstolbci; j++)
            {
                Console.Write(matrix[i, j] + " ");
            }
            Console.WriteLine();
        }
    }
    public void Print()
    {
        PrintPartialy(0, numstroki - 1, 0, numstolbci - 1);
    }
}

class Program
{
    static void Main(string[] args)
    {
        Console.Write("Colichestvo strok: ");
        int numstroki = int.Parse(Console.ReadLine());

        Console.Write("Colichestvo stolbctov: ");
        int numstolbci = int.Parse(Console.ReadLine());

        Console.Write(" minimum value: ");
        int minValue = int.Parse(Console.ReadLine());

        Console.Write("maximum value: ");
        int maxValue = int.Parse(Console.ReadLine());

        MyMatrix matrix = new MyMatrix(numstroki, numstolbci, minValue, maxValue);

        Console.WriteLine("Our matrix:");
        matrix.Print();

        Console.WriteLine("\nViberite razmer matrix:");
        Console.Write("nomer numstroki: ");
        int newnumstroki = int.Parse(Console.ReadLine());

        Console.Write("nomer numstolbci: ");
        int newnumstolbci = int.Parse(Console.ReadLine());

        matrix.ChangeSize(newnumstroki, newnumstolbci, minValue, maxValue);

        Console.WriteLine("New matrix:");
        matrix.Print();

        Console.WriteLine("\nmatrix 5x5:");
        matrix.PrintPartialy(0, 4, 0, 4);

        Console.WriteLine("\nviberite element:");
        Console.Write("stroki index: ");
        int strokiIndex= int.Parse(Console.ReadLine());

        Console.Write("stolbci index: ");
        int stolbciIndex = int.Parse(Console.ReadLine());

        Console.Write("novoe znachenie: ");
        int newValue = int.Parse(Console.ReadLine());

        matrix[strokiIndex, stolbciIndex] = newValue;

        Console.WriteLine("new matrix:");
        matrix.Print();
    }
}
```
### Задание 2
Создайте класс MyList<T>.
Реализуйте в простейшем приближении возможность использования его экземпляра аналогично экземпляру класса List<T>.
Минимально требуемый интерфейс взаимодействия с экземпляром должен включать метод добавления элемента, индексатор для получения значения элемента по указанному индексу, свойство только для чтения для получения общего количества элементов и поддержку блока инициализации.
При выполнении нельзя использовать коллекции, только массивы.
```
using System;
using System.Collections;
using System.Collections.Generic;

public class MyList<T> : IEnumerable<T>//Этот интерфейс определяет метод `GetEnumerator()`, который позволяет перечислять элементы списка.
{//Интерфейс IEnumerable<T> предоставляет методы для перечисления элементов коллекции. в классе используюся т
    //т так как перебер универсальной коллекции
    private T[] items;
    private int count;//кол элементов

    public MyList()
    {
        items = new T[0];//выделяем память для 0 элементов
        count = 0;
    }

    public MyList(params T[] initialValues)//конструктор, который принимает массив `initialValues` в качестве параметра и копирует его элементы в список.
    {
        items = new T[initialValues.Length];
        Array.Copy(initialValues, items, initialValues.Length);
        count = initialValues.Length;
    }

    public MyList(IEnumerable<T> collection)//конструктор, который принимает коллекцию `collection` в качестве параметра и добавляет ее элементы в список.

    {//Перед коллекцией стоит тип `IEnumerable<T>`, чтобы указать, что параметр `collection` должен быть коллекцией, которая реализует интерфейс `IEnumerable<T>`.
        items = new T[0];
        foreach (var item in collection)
        {
            Add(item);//вставка в список
        }
    }

    public void Add(T item)//метод, который добавляет элемент `item` в список. Если список заполнен, размер массива увеличивается в два раза.
    {
        if (count == items.Length)
        {
            int newCapacity = count == 0 ? 4 : count * 2;
            //`условие ? значение_если_истина : значение_если_ложь;`
            /*Если условие верно (`count == 0`), то значение переменной `newCapacity` будет равно 4. Если условие ложно 
             * (`count != 0`), то значение переменной `newCapacity` будет равно `count * 2`.*/
            T[] newItems = new T[newCapacity];
            Array.Copy(items, newItems, count);
            items = newItems;
        }

        items[count] = item;
        count++;
    }

    public T this[int index]//индексатор, который позволяет получить элемент списка по его индексу.
    {
        get
        {
            if (index < 0 || index >= count)
                throw new IndexOutOfRangeException();

            return items[index];
        }
    }

    public int Count//свойство, которое возвращает текущее количество элементов в списке.
    {
        get { return count; }
    }

    public IEnumerator<T> GetEnumerator()//Реализация метода GetEnumerator из интерфейса IEnumerable<T>, который возвращает перечислитель для списка.
    {//тип возвращаемого элемента enumerator, так как Итераторы могут возвращать только тип IEnumerable<>
        for (int i = 0; i < count; i++)
        {
            yield return items[i];
        }
    }//возвращают итератор для перечисления элементов списка.

    IEnumerator IEnumerable.GetEnumerator()// Реализация необобщенного метода GetEnumerator из интерфейса IEnumerable, который возвращает перечислитель для списка.
    {
        return GetEnumerator();
    }
}

class Program
{
    static void Main()
    {
        MyList<int> myList = new MyList<int> { 1, 2, 3, 4, 5 };

        myList.Add(50);

        Console.WriteLine($"nashi elementi:\n");
        foreach (var item in myList)
        {
            Console.WriteLine($"Element: {item}");
        }

        Console.WriteLine($"\ncolichestvo elementov: {myList.Count}");
    }
}

```
### Задание 3
Создайте коллекцию MyDictionary<TKey,TValue>.
Реализуйте в простейшем приближении возможность использования ее экземпляра аналогично экземпляру класса Dictionary<TKey,TValue>.
Минимально требуемый интерфейс взаимодействия с экземпляром должен включать метод добавления элемента, индексатор для получения значения элемента по указанному индексу и свойство только для чтения для получения общего количества элементов.
Реализуйте возможность перебора элементов коллекции в цикле foreach. При выполнении нельзя использовать коллекции, только массивы.
```
using System;
using System.Collections;
using System.Collections.Generic;

public class MyDictionary<TKey, TValue> : IEnumerable<KeyValuePair<TKey, TValue>>//Он использует обобщенные типы TKey и TValue для представления типов ключей и значений в словаре. Он также реализует интерфейс IEnumerable<KeyValuePair<TKey, TValue>>, что позволяет объектам этого класса быть перечислимыми.
{
    private List<KeyValuePair<TKey, TValue>> items = new List<KeyValuePair<TKey, TValue>>();//десь создается приватное поле items, которое представляет собой список пар ключ-значение. Используется класс List<T> для хранения элементов.

    public void Add(TKey key, TValue value)//Этот метод добавляет новую пару ключ-значение в словарь.
    {
        items.Add(new KeyValuePair<TKey, TValue>(key, value));
    }

    public TValue this[TKey key]//Это индексатор класса, который позволяет получать значение по ключу как в словаре.
    {
        get
        {
            var item = items.Find(i => EqualityComparer<TKey>.Default.Equals(i.Key, key));
            /* использует метод `Find` списка `items` для поиска элемента с заданным ключом. В данном случае, мы 
             * передаем лямбда-выражение `i => EqualityComparer<TKey>.Default.Equals(i.Key, key)`, которое проверяет, 
             * равен ли ключ элемента `i` заданному ключу `key`. Если элемент с таким ключом найден, он присваивается 
             * переменной `item`*/
            if (item.Equals(default(KeyValuePair<TKey, TValue>)))
                /* проверяется, равен ли найденный элемент значению по умолчанию (`default(KeyValuePair<TKey, TValue>)`). 
                 * Если найденного элемента нет, это означает, что в словаре нет элемента с заданным ключом, и выбрасывается 
                 * исключение `KeyNotFoundException`.*/
            {
                throw new KeyNotFoundException($"The key '{key}' was not found in the dictionary.");
            }
            return item.Value;
        }
    }

    public int Count
    {
        get { return items.Count; }
    }

    public IEnumerator<KeyValuePair<TKey, TValue>> GetEnumerator()
    {
        return items.GetEnumerator();//Этот метод реализует интерфейс IEnumerable и возвращает объект IEnumerator, который может использоваться для перебора элементов словаря.
    }

    IEnumerator IEnumerable.GetEnumerator()
    {
        return GetEnumerator();
    }
}

class Program
{
    static void Main()
    {
        MyDictionary<string, int> myDict = new MyDictionary<string, int>
        {
            { "one", 1 },
            { "two", 2 },
            { "three", 3 }
        };


        Console.WriteLine("Count: " + myDict.Count);
        Console.WriteLine("\nznachenie dlya key 'two': " + myDict["two"]);

        Console.WriteLine("\nproxod po slovary:");
        foreach (var kvp in myDict)
        {
            Console.WriteLine($"   {kvp.Key}: {kvp.Value}");
        }
    }
}
```

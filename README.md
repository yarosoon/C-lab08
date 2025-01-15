# C#-lab08 
## Цели работы:
1. Научиться работать с механизмом сериализации средствами языка C#.
2. Научиться работать с файловой системой средствами языка C#.


## Задание №1

Используя диаграмму классов из лабораторной работы №7, реализуйте поддержку сериализации и десериализации класса Animal.

![image](https://github.com/user-attachments/assets/90272c15-5bce-44c5-955e-71e839e784cb)

Создайте экземпляр класса-наследника Animal, инициализируйте все поля и выполните его Xml-сериализацию.
Добавьте в решение новый консольный проект, в котором десериализуйте класс Animal и выведите полученный объект на консоль.

## Задание №2
Напишите приложение для поиска заданного файла во всех поддиректориях указанного пользователем пути.
Используйте FileStream для вывода содержимого файла на консоль.
Добавьте возможность сжатия найденного файла стандартными средствами .Net.

## Код ClassLibrary

    using System.Xml.Serialization;
    
    namespace lab08_cl // Объявление пространства имен, содержащего классы библиотеки.
    {
        // Атрибут для добавления комментариев к классам.
        [AttributeUsage(AttributeTargets.Class)] // Указывает, что атрибут может применяться только к классам.
        public class CommentAtt : Attribute // Определение пользовательского атрибута.
        {
            public string Comment { get; set; } // Свойство для хранения комментария.
    
            // Конструктор, принимающий строку комментария.
            public CommentAtt(string comment)
            {
                Comment = comment; // Присваивание переданного комментария свойству.
            }
        }
    
        // Перечисление для классификации животных.
        public enum eClassificationAnimal
        {
            Herbivores,  // Травоядные
            Carnivores,  // Плотоядные
            Omnivores    // Всеядные
        }
    
        // Перечисление для описания любимой еды животных.
        public enum eFavouriteFood
        {
            Meat,        // Мясо
            Plants,      // Растения
            Everything   // Все
        }
    
        // Абстрактный класс, представляющий общее описание животных.
        [CommentAtt("Абстрактный класс для объектов, представляющих животных")] // Применение пользовательского атрибута с описанием.
        [XmlInclude(typeof(Cow))]  // Указание, что класс Cow будет включен в XML-сериализацию.
        [XmlInclude(typeof(Lion))] // Указание, что класс Lion будет включен в XML-сериализацию.
        [XmlInclude(typeof(Pig))]  // Указание, что класс Pig будет включен в XML-сериализацию.
        public abstract class Animal
        {
            public string Country { get; set; }              // Свойство для хранения страны обитания животного.
            public bool HideFromOtherAnimals { get; set; }   // Свойство, указывающее, скрывается ли животное от других.
            public string Name { get; set; }                // Свойство для имени животного.
            public string WhatAnimal { get; set; }          // Свойство для типа животного (например, "млекопитающее").
    
            protected Animal() { } // Пустой конструктор, необходимый для сериализации.
    
            // Конструктор, инициализирующий свойства объекта.
            public Animal(string country, bool hideFromAnimals, string name, string whatAnimal)
            {
                // Присваивание значений свойствам с использованием кортежа.
                (Country, HideFromOtherAnimals, Name, WhatAnimal) = (country, hideFromAnimals, name, whatAnimal);
            }
    
            // Абстрактный метод для получения классификации животного. Обязателен для реализации в дочерних классах.
            public abstract eClassificationAnimal GetClassificationAnimal();
    
            // Абстрактный метод для получения любимой еды животного.
            public abstract eFavouriteFood GetFavouriteFood();
    
            // Абстрактный метод, возвращающий звуки, которые издает животное.
            public abstract string SayHello();
    
            // Метод деконструкции для упрощенного доступа к свойствам объекта.
            public void Deconstruct(out string country, out bool hideFromOtherAnimals, out string name, out string whatAnimal)
            {
                country = Country;                         // Возвращает страну обитания.
                hideFromOtherAnimals = HideFromOtherAnimals; // Возвращает информацию о скрытности.
                name = Name;                               // Возвращает имя животного.
                whatAnimal = WhatAnimal;                   // Возвращает тип животного.
            }
        }
    
        // Класс для описания коровы.
        [CommentAtt("Класс описания коровы")] // Описание класса с помощью атрибута.
        public class Cow : Animal
        {
            public Cow() { } // Пустой конструктор для сериализации.
    
            // Конструктор с параметрами, передающимися базовому классу.
            public Cow(string country, bool hideFromOtherAnimals, string name, string whatAnimal) :
                base(country, hideFromOtherAnimals, name, whatAnimal)
            { }
    
            // Реализация метода для получения любимой еды коровы.
            public override eFavouriteFood GetFavouriteFood()
            {
                return eFavouriteFood.Plants; // Коровы едят растения.
            }
    
            // Реализация метода для классификации коровы.
            public override eClassificationAnimal GetClassificationAnimal()
            {
                return eClassificationAnimal.Herbivores; // Коровы — травоядные.
            }
    
            // Реализация метода приветствия для коровы.
            public override string SayHello()
            {
                return "Moo"; // Звук коровы.
            }
        }
    
        // Класс для описания льва.
        [CommentAtt("Класс описания льва")] // Описание класса с помощью атрибута.
        public class Lion : Animal
        {
            public Lion() { } // Пустой конструктор для сериализации.
    
            // Конструктор с параметрами, передающимися базовому классу.
            public Lion(string country, bool hideFromOtherAnimals, string name, string whatAnimal) :
                base(country, hideFromOtherAnimals, name, whatAnimal)
            { }
    
            // Реализация метода для получения любимой еды льва.
            public override eFavouriteFood GetFavouriteFood()
            {
                return eFavouriteFood.Meat; // Львы едят мясо.
            }
    
            // Реализация метода для классификации льва.
            public override eClassificationAnimal GetClassificationAnimal()
            {
                return eClassificationAnimal.Omnivores; // Львы считаются всеядными.
            }
    
            // Реализация метода приветствия для льва.
            public override string SayHello()
            {
                return "Meow"; // Звук льва.
            }
        }
    
        // Класс для описания свиньи.
        [CommentAtt("Класс описания свиньи")] // Описание класса с помощью атрибута.
        public class Pig : Animal
        {
            public Pig() { } // Пустой конструктор для сериализации.
    
            // Конструктор с параметрами, передающимися базовому классу.
            public Pig(string country, bool hideFromOtherAnimals, string name, string whatAnimal) :
                base(country, hideFromOtherAnimals, name, whatAnimal)
            { }
    
            // Реализация метода для получения любимой еды свиньи.
            public override eFavouriteFood GetFavouriteFood()
            {
                return eFavouriteFood.Everything; // Свиньи едят всё.
            }
    
            // Реализация метода для классификации свиньи.
            public override eClassificationAnimal GetClassificationAnimal()
            {
                return eClassificationAnimal.Omnivores; // Свиньи являются всеядными.
            }
    
            // Реализация метода приветствия для свиньи.
            public override string SayHello()
            {
                return "Oink"; // Звук свиньи.
            }
        }
    }

## Код задания №1 (ConsoleApp)    

    using lab08_cl;
    using System.Xml.Serialization;
    
    namespace lab08_ca1 // Объявление пространства имен, в котором находится текущий класс программы.
    {
        public class Program // Определение основного класса программы.
        {
            public static void Main(string[] args) // Точка входа в программу. Метод Main вызывается при запуске приложения.
            {
                // Создание массива животных, каждое из которых является экземпляром класса Animal или его наследников.
                Animal[] animals = [
                    new Cow("Russia", false, "Borya", "Cow"),    // Создание объекта коровы с заданными параметрами.
                    new Lion("Zambia", false, "Tom", "Lion"),    // Создание объекта льва с заданными параметрами.
                    new Pig("Germany", true, "Peppa", "Pig")     // Создание объекта свиньи с заданными параметрами.
                ];
    
                // Создание объекта для сериализации массива объектов Animal в XML-формат.
                XmlSerializer xmlSerializer = new(typeof(Animal[]));
    
                // Открытие/создание файла "animals.xml" для записи данных в XML-формате.
                using (FileStream fs = new("animals.xml", FileMode.OpenOrCreate))
                {
                    // Сериализация массива объектов Animal в файл "animals.xml".
                    xmlSerializer.Serialize(fs, animals);
                }
    
                // Открытие файла "animals.xml" для чтения данных в XML-формате.
                using (FileStream fs = new("animals.xml", FileMode.OpenOrCreate))
                {
                    // Десериализация данных из XML-файла обратно в массив объектов Animal.
                    Animal?[]? newAnimals = xmlSerializer.Deserialize(fs) as Animal[];
    
                    // Проверка, что десериализация прошла успешно и массив не равен null.
                    if (newAnimals != null)
                    {
                        // Перебор всех объектов в массиве newAnimals.
                        foreach (Animal? animal in newAnimals)
                        {
                            // Вывод страны обитания животного.
                            Console.WriteLine($"Country: {animal?.Country}");
                            // Вывод информации о том, скрывается ли животное от других.
                            Console.WriteLine($"Hide from other animals: {animal?.HideFromOtherAnimals}");
                            // Вывод имени животного.
                            Console.WriteLine($"Name: {animal?.Name}");
                            // Вывод типа животного.
                            Console.WriteLine($"What animal: {animal?.WhatAnimal}");
                            // Разделительная линия между данными о животных.
                            Console.WriteLine("----------");
                        }
                    }
                }
            }
        }
    }

## Код задания №2 (ConsoleApp)

    using System.IO.Compression; // Подключение пространства имен для работы с сжатием файлов (GZip).
    
    namespace lab08_ca2 // Определение пространства имен для программы.
    {
        class Program // Объявление класса программы.
        {
            static void Main(string[] args) // Главный метод программы, точка входа.
            {
                // Вывод запроса для ввода пути к каталогу, где будет производиться поиск.
                Console.Write($"Enter the path to catalog: ");
                // Считывание пути к каталогу, введенного пользователем.
                string catalog = Console.ReadLine();
    
                // Вывод запроса для ввода имени файла, который нужно найти.
                Console.Write($"Enter the name of the file to find: ");
                // Считывание имени файла, введенного пользователем.
                string fileName = Console.ReadLine();
    
                // Перебор всех файлов в указанном каталоге и подкаталогах, соответствующих заданному имени.
                foreach (string foundFile in Directory.EnumerateFiles(
                    catalog, // Каталог, где производится поиск.
                    fileName, // Имя искомого файла.
                    SearchOption.AllDirectories // Поиск во всех подкаталогах.
                ))
                {
                    // Создание объекта FileInfo для работы с найденным файлом.
                    FileInfo fileInfo = new FileInfo(foundFile);
    
                    try // Блок для обработки возможных исключений.
                    {
                        // Вывод информации о найденном файле: путь, размер, дата создания.
                        Console.WriteLine(
                            $"Found file: {fileInfo.FullName}\n" +
                            $"Size: {fileInfo.Length} bytes\n" +
                            $"Created: {fileInfo.CreationTime}\n" +
                            $"Content: ");
    
                        // Открытие найденного файла для чтения.
                        using (FileStream fileStream = fileInfo.OpenRead())
                        {
                            // Чтение содержимого файла с использованием StreamReader.
                            using (StreamReader streamReader = new StreamReader(fileStream))
                            {
                                // Вывод содержимого файла в консоль.
                                Console.WriteLine(streamReader.ReadToEnd());
                            }
                        }
    
                        // Локальная функция для сжатия файла.
                        void CompressFile(string sourceFile, string destinationFile)
                        {
                            // Открытие исходного файла для чтения.
                            using (FileStream sourceStream = new(sourceFile, FileMode.Open, FileAccess.Read))
                            // Создание/открытие файла для записи сжатых данных.
                            using (FileStream destinationStream = new(destinationFile, FileMode.Create, FileAccess.Write))
                            // Создание потока GZip для сжатия данных.
                            using (GZipStream compressionStream = new(destinationStream, CompressionMode.Compress))
                            {
                                // Копирование содержимого исходного файла в поток сжатия.
                                sourceStream.CopyTo(compressionStream);
                            }
                        }
    
                        // Путь к исходному файлу.
                        string startPath = fileInfo.FullName;
                        // Путь к сжатому файлу.
                        string endPath = fileInfo.FullName + ".gz";
    
                        // Вызов функции сжатия файла.
                        CompressFile(startPath, endPath);
    
                        // Получение информации о сжатом файле.
                        FileInfo compressedFileInfo = new FileInfo(endPath);
    
                        // Вывод сообщения об успешном сжатии файла.
                        Console.WriteLine($"{fileInfo.Name} compressed successfully");
                        // Вывод информации о размере файла до и после сжатия.
                        Console.WriteLine($"First size: {fileInfo.Length} bytes Compressed size: {compressedFileInfo.Length} bytes");
                    }
                    catch // Обработка любых исключений, возникающих при работе с файлами.
                    {
                        continue; // Переход к следующему файлу в случае ошибки.
                    }
                }
            }
        }
    }

C# lab08

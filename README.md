```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

class Program
{
    // Константы для размеров и символов
    const int Size = 6;
    const char Wall = '#';
    const char Corridor = '.';
    const char Exit = 'E';
    const char Rod = 'R';
    const char Screen = 'S';

    static void Main()
    {
        // 1. Назначение матриц
        char[,] station = new char[Size, Size];
        int[,] radiationLevel = new int[Size, Size];
        Random rng = new Random();

        // 2. Заполнение матриц
        for (int row = 0; row < Size; row++)
        {
            for (int col = 0; col < Size; col++)
            {
                // Внешние границы — обшивка, внутри — коридоры
                if (row == 0 || row == Size - 1 || col == 0 || col == Size - 1)
                {
                    station[row, col] = Wall;
                }
                else
                {
                    station[row, col] = Corridor;
                }
        // 3. Размещение выхода
        station[1, 1] = Exit;

        // Размещение радиационных стержней в случайных внутренних ячейках
        int rodsPlaced = 0;
        while (rodsPlaced < 3)
        {
            int r = rng.Next(1, Size - 1);
            int c = rng.Next(1, Size - 1);
            if (station[r, c] == Corridor)
            {
                station[r, c] = Rod;
                rodsPlaced++;
            }
        }
                // 4. Создать параллельную целочисленную матрицу radiationLevel того же размера (см. пункт 1) и заполнить ее числами от 0 до 100 с помощью Random.
                radiationLevel[row, col] = rng.Next(0, 100);
            }
        }
        // 5. Вывод матрицы станции с выделением стержней красным цветом
        for (int row = 0; row < Size; row++)
        {
            for (int col = 0; col < Size; col++)
            {
                if (station[row, col] == Rod)
                {
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.Write(station[row, col] + " ");
                    Console.ResetColor();
                }
                else
                {
                    Console.Write(station[row, col] + " ");
                }
            }
            Console.WriteLine();
        }
        Console.WriteLine();

        // 6. Суммарный уровень радиации в каждом ряду
        Console.WriteLine("Уровень радиации по рядам:");
        for (int row = 0; row < Size; row++)
        {
            int rowSum = 0;
            for (int col = 0; col < Size; col++)
            {
                rowSum += radiationLevel[row, col];
            }
            Console.WriteLine($"Ряд {row}: {rowSum}");
        }
        Console.WriteLine();

        // 7. Координаты отсека с максимальным уровнем радиации
        int maxRadiation = radiationLevel.Cast<int>().Max();
        var maxCoords = new List<(int, int)>();
        for (int row = 0; row < Size; row++)
        {
            for (int col = 0; col < Size; col++)
            {
                if (radiationLevel[row, col] == maxRadiation)
                {
                    maxCoords.Add((row, col));
                }
            }
        }
        Console.WriteLine($"Максимальный уровень радиации: {maxRadiation}");
        Console.WriteLine("Координаты отсека (клетки) с максимальной радиацией: " +
            string.Join(", ", maxCoords.Select(c => $"[{c.Item1}, {c.Item2}]")));
        Console.WriteLine();

        // 8. Подсчет безопасных коридоров . на главной диагонали
        int safeDiag = 0;
        for (int i = 0; i < Size; i++)
        {
            if (station[i, i] == Corridor) safeDiag++;
        }
        Console.WriteLine($"Безопасных коридоров на главной диагонали: {safeDiag}");

        // 9. Запрос координат для установки свинцового экрана
        Console.WriteLine("\nУстановка свинцового экрана.");
        Console.Write("Введите номер строки (0-4): ");
        bool rowOk = int.TryParse(Console.ReadLine(), out int targetRow);
        Console.Write("Введите номер столбца (0-4): ");
        bool colOk = int.TryParse(Console.ReadLine(), out int targetCol);

        if (rowOk && colOk &&
            targetRow >= 0 && targetRow < Size &&
            targetCol >= 0 && targetCol < Size)
        {
            station[targetRow, targetCol] = Screen;
            Console.WriteLine($"Экран установлен в отсеке [{targetRow}, {targetCol}].");
        }
        else
        {
            Console.WriteLine("Ошибка: введены некорректные координаты. Экран не установлен.");
        }

        // 10. Подсчет общего числа непроходимых завалов # на всей станции
        int hullCount = 0;
        foreach (char cell in station)
        {
            if (cell == Wall) hullCount++;
        }
        Console.WriteLine($"\nОбщее число непроходимых завалов на станции: {hullCount}");

        // Финальный вид станции после установки экрана
        Console.WriteLine("\nИтоговое состояние станции:");
        for (int row = 0; row < Size; row++)
        {
            for (int col = 0; col < Size; col++)
            {
                if (station[row, col] == Rod)
                {
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.Write(station[row, col] + " ");
                    Console.ResetColor();
                }
                else if (station[row, col] == Screen)
                {
                    Console.ForegroundColor = ConsoleColor.DarkGray;
                    Console.Write(station[row, col] + " ");
                    Console.ResetColor();
                }
                else
                {
                    Console.Write(station[row, col] + " ");
                }
            }
            Console.WriteLine();
        }
    }
}
```
// ДЕЛАЛИ АРИСТОВ И БАШКАТОВА
:P

Векторы (std::vector) и строки (std::string)
Вектор и строка — базовые контейнеры стандартной библиотеки C++. Они хранят элементы в непрерывном участке памяти, обеспечивают быстрый доступ по индексу (O(1)) и эффективное добавление элементов в конец.

Контейнер std::vector
std::vector — динамический массив, который изменяет свой размер при необходимости. Для работы с ним требуется заголовочный файл <vector>.

Объявление
Все элементы вектора должны иметь один тип T, известный на этапе компиляции:

#include <string>
#include <vector>

// Вектор целых чисел
std::vector<int> numbers = {1, 2, 3};

// Пустой вектор строк
std::vector<std::string> words;

std::vector — это шаблон класса. Тип элементов указывается в угловых скобках.

Доступ к элементам
Доступ по индексу
Оператор [] не проверяет границы массива:

#include <vector>

std::vector<int> data = {10, 20, 30};

int first = data[0]; // 10

data[1] = 99;
// data: {10, 99, 30}

// data[5]; // Undefined Behavior

Безопасный доступ через at()
Метод at() проверяет индекс и выбрасывает исключение std::out_of_range, если индекс выходит за границы вектора:

#include <iostream>
#include <stdexcept>
#include <vector>

std::vector<int> data = {10, 20, 30};

try {
    std::cout << data.at(5);
} catch (const std::out_of_range& error) {
    std::cout << "Ошибка: " << error.what();
}

Основные операции
#include <vector>

std::vector<int> numbers = {1, 2, 3};

// Добавление элемента в конец
numbers.push_back(4);

// Удаление последнего элемента
numbers.pop_back();

// Размер вектора
std::size_t size = numbers.size();

// Проверка на пустоту
bool is_empty = numbers.empty();

// Очистка вектора
numbers.clear();

Резервирование памяти
Метод reserve() заранее выделяет память под указанное количество элементов. Это позволяет уменьшить количество перераспределений памяти при добавлении большого количества элементов:

#include <string>
#include <vector>

std::vector<std::string> words;

words.reserve(1000);

for (int i = 0; i < 1000; ++i) {
    words.push_back("word");
}

reserve() изменяет вместимость (capacity), но не размер (size) вектора.

Сортировка
Для сортировки используется алгоритм std::sort из заголовочного файла <algorithm>:

#include <algorithm>
#include <vector>

std::vector<int> values = {3, 1, 4, 1, 5};

// По возрастанию
std::sort(values.begin(), values.end());
// {1, 1, 3, 4, 5}

// По убыванию
std::sort(values.rbegin(), values.rend());
// {5, 4, 3, 1, 1}

В C++20 можно использовать алгоритм std::ranges::sort:

#include <algorithm>
#include <vector>

std::vector<int> values = {3, 1, 4, 1, 5};

std::ranges::sort(values);

Сравнение векторов
Векторы сравниваются лексикографически — так же, как слова в словаре:

#include <vector>

std::vector<int> first = {1, 2, 3};
std::vector<int> second = {1, 2, 4};

bool result = first < second; // true

Строки (std::string)
std::string — контейнер символов типа char со специализированными функциями для работы с текстом. Для работы со строками требуется заголовочный файл <string>.

Строку можно рассматривать как последовательность символов, похожую на std::vector<char>.

Базовые операции
#include <iostream>
#include <string>

std::string text = "Some string";

// Добавление одного символа
text += ' ';

// Добавление другой строки
text += "functions";

std::cout << text;

Преобразование числа в строку
#include <string>

int number = 42;

std::string string_number = std::to_string(number);
// "42"

Преобразование строки в число
#include <string>

std::string input = "12345";

int number = std::stoi(input);
// 12345

Полезные советы
Учитывайте беззнаковый тип size_t
Метод size() возвращает значение типа std::size_t, который является беззнаковым типом.

Выражение vec.size() - 1 для пустого вектора не даст -1. Вместо этого произойдет переполнение, и получится очень большое положительное число.

Используйте проверку empty():

if (!vec.empty() && vec.back() == value) {
    // Вектор не пуст и последний элемент равен value
}

Не используйте reserve() без необходимости
Не вызывайте reserve() без причины. Для небольших векторов стандартные аллокаторы обычно работают достаточно эффективно.

reserve() полезен, когда заранее известно приблизительное количество элементов:

std::vector<int> values;
values.reserve(10000);

Используйте указатели для тяжелых объектов
Если объект дорого копировать или перемещать, можно хранить в векторе умные указатели. Тогда при перераспределении памяти будут перемещаться только указатели, а не сами объекты:

#include <memory>
#include <vector>

struct HeavyObject {
    // Большой или сложный объект
};

std::vector<std::unique_ptr<HeavyObject>> objects;

objects.push_back(std::make_unique<HeavyObject>());

Структурная привязка в C++17
Структурную привязку удобно использовать при итерации по матрице, если каждая строка содержит ровно два элемента:

#include <vector>

std::vector<std::vector<int>> matrix = {
    {1, 2},
    {3, 4}
};

for (auto& [col1, col2] : matrix) {
    // Работа напрямую с элементами строки
    col1 *= 2;
    col2 *= 2;
}

Для матрицы произвольного размера обычно используют вложенный цикл:

#include <vector>

std::vector<std::vector<int>> matrix = {
    {1, 2},
    {3, 4}
};

for (auto& row : matrix) {
    for (auto& element : row) {
        element *= 2;
    }
}

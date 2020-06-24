# Класс CArgmaxLayer

<!-- TOC -->

- [Класс CArgmaxLayer](#класс-cargmaxlayer)
    - [Настройки](#настройки)
        - [Используемая размерность](#используемая-размерность)
    - [Обучаемые параметры](#обучаемые-параметры)
    - [Входы](#входы)
    - [Выходы](#выходы)

<!-- /TOC -->

Класс реализует слой, который возвращает координаты максимумов единственного входа вдоль некоторой размерности.

## Настройки

### Используемая размерность

```c++
void SetDimension(TBlobDim d);
```

Установка размерности блоба, вдоль которой будут найдены максимумы.

## Обучаемые параметры

Слой не имеет обучаемых параметров.

## Входы

На единственный вход подается блоб произвольного размера.

## Выходы

Единственный выход содержит блоб с данными типа `int` размера:

- `GetDimension()` равен `1`;
- остальные размерности равны соответствующим размерностям входа с координатами максимумов вдоль `GetDimension()`.
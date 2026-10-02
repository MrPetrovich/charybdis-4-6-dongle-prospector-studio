# Миграция раскладки из charybdis_zmk в charybdis-4-6-dongle-prospector-studio

Дата: 2026-10-02

## Обзор

Успешно перенесена продвинутая раскладка клавиш и конфигурация трекбола из проекта charybdis_zmk (3×6) в проект charybdis-4-6-dongle-prospector-studio (4×6).

## Выполненные изменения

### 1. Конфигурация трекбола (✅ Завершено)

Добавлены продвинутые слушатели поведения ввода:

- **trackball_listener** - автоматическое переключение на слой MOUSE при движении трекбола (time-to-live: 1000ms)
- **trackball_snipe_listener** - режим точности на слое NUMBER (делитель скорости 1/3)
- **trackball_scroll_listener** - всенаправленная прокрутка на слое FUNCTION
- **ib_wheel_scaler** - масштабирование колеса прокрутки
- **ib_toggle_layer** - автоматическое переключение слоев

### 2. Улучшенные Behaviors (✅ Завершено)

Перенесены оптимизированные поведения:

- **Home Row Mods (hml/hmr)** - с улучшенными параметрами тайминга (tapping-term-ms: 2000, require-prior-idle-ms: 250)
- **hml_s/hmr_s** - вариант с require-prior-idle-ms: 0
- **scrl_clk** - hold-tap для прокрутки/клика
- **smart_shift** - mod-morph для Shift/Caps Word
- **Макросы**:
  - `lang` - переключение языка (Win+Space)
  - `screenshoot` - скриншот (Ctrl+Shift+Print)

### 3. Combos (✅ Завершено)

Адаптированы все комбо-клавиши для 4×6 матрицы:

- **Клики мыши**: левая/правая рука (левый/правый/средний клик)
- **Навигация**: ESC, TAB, DELETE, Grave, Enter, Пробел
- **Символы**: APOS, LBKT, RBKT, MINUS, PLUS
- **Управление слоями**: переключение на MOUSE, GAME, FUNCTION, RESET, BASE
- **Кнопки мыши**: Forward, Back
- **Системные**: studio_unlock, bootloader

### 4. Раскладка клавиш (✅ Завершено)

Перенесены и адаптированы все 7 слоев:

#### BASE слой
- Home Row Mods на ASDF/JKL;
- Оптимизированная позиция thumb cluster
- Caps Lock на позиции Escape (левый)

#### MOUSE слой
- Sys Reset и Bootloader в верхнем ряду
- Модификаторы на home row левой руки
- Кнопки мыши MB1/MB2/MB3 на правой руке
- scrl_clk на thumb cluster для scroll/click

#### NUMBER слой
- F1-F12 в верхнем ряду
- Навигация (Home, End, Page Up/Down, стрелки)
- Numpad на правой стороне (7-9, 4-6, 1-3, 0)
- Макросы screenshoot и lang

#### SYMBOL слой
- Специальные символы: @, $, #, %, ^, `
- Скобки: (), [], {}
- Операторы: <, >, =, -, _, +
- Разделители: \, |, ~, &, !, ;, :, *, /

#### FUNCTION слой
- Прозрачный слой для scroll mode трекбола
- Возврат на BASE слой

#### GAME слой
- Заготовка для игровой раскладки
- Возврат на BASE слой

#### RESET слой
- Технический слой для сброса
- Возврат на BASE слой

## Технические детали

### Адаптация позиций клавиш

Матрица 3×6 (36 клавиш + 6 thumb):
```
Row 0: 0-5, 6-11
Row 1: 12-17, 18-23
Row 2: 24-29, 30-35
Thumb: 36-38, 39-41
```

Матрица 4×6 (48 клавиш + 8 thumb):
```
Row 0: 0-5, 6-11
Row 1: 12-17, 18-23
Row 2: 24-29, 30-35
Row 3: 36-41, 42-47
Thumb: 48-50, 51-53, 54-55
```

### Обновленные определения

```c
#define KEYS_L 0 1 2 3 4 5 12 13 14 15 16 17 24 25 26 27 28 29 36 37 38 39 40 41
#define KEYS_R 6 7 8 9 10 11 18 19 20 21 22 23 30 31 32 33 34 35 42 43 44 45 46 47
#define THUMBS 48 49 50 51 52 53 54 55
```

### Добавленные include

```c
#include <dt-bindings/zmk/mouse.h>
#include <behaviors/mouse_keys.dtsi>
#define ZMK_POINTING_DEFAULT_SCRL_VAL 15
```

### Настройка MSC

```c
&msc {
    delay-ms = <0>;
    time-to-max-speed-ms = <0>;
};
```

## Преимущества новой конфигурации

1. **Автоматический слой мыши** - трекбол активирует слой MOUSE автоматически
2. **Режим точности** - удержание слоя NUMBER замедляет курсор в 3 раза
3. **Прокрутка** - слой FUNCTION превращает трекбол в колесо прокрутки
4. **Улучшенные Home Row Mods** - оптимизированные тайминги для меньшего количества ложных срабатываний
5. **Расширенные combos** - больше быстрых команд без переключения слоев
6. **Макросы** - автоматизация частых действий (переключение языка, скриншоты)

## Совместимость

Конфигурация совместима с:
- ZMK firmware (latest)
- PMW3610 sensor driver (badjeff/zmk-pmw3610-driver)
- Input behavior listeners (badjeff/zmk-input-behavior-listener)
- Split peripheral input relay (badjeff/zmk-split-peripheral-input-relay)

## Следующие шаги

1. Проверить сборку прошивки через GitHub Actions
2. Прошить обе половины клавиатуры
3. Протестировать все слои и combos
4. При необходимости настроить CPI трекбола в device tree файлах
5. Адаптировать под личные предпочтения

## Файлы для дальнейшей настройки

- **CPI трекбола**: `boards/shields/charybdis/charybdis_3610.dtsi` (текущее значение: 800)
- **Тайминги модов**: `config/charybdis.keymap` (секция behaviors)
- **Combos**: `config/charybdis.keymap` (секция combos)
- **Раскладка**: `config/charybdis.keymap` (секция keymap)

## Примечания

- Все позиции клавиш пересчитаны для 4×6 матрицы
- Thumb cluster адаптирован под 8 клавиш (было 6)
- Сохранена логика и философия оригинальной раскладки
- Добавлена поддержка ZMK Studio через combo_unlock (позиции 0+4)

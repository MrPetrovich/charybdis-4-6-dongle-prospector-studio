# CLAUDE.md - Контекст работы с проектом charybdis-4-6-dongle-prospector-studio

## Текущее состояние

Раскладка и режимы трекбола из `charybdis_zmk` перенесены в эту прошивку.

Слои в `config/charybdis.keymap`: BASE, MOUSE, NUMBER, SYMBOL, FUNCTION, GAME, RESET. Home row mods стоят на A S D F / J K L ;. Комбо и макросы `lang` / `screenshoot` взяты из исходного keymap.

Трекбол остаётся на нативных обработчиках ZMK main (`split_input_common.dtsi`), не на `zmk,input-behavior-listener`. Курсор 1/2 и авто-мышь 1000 мс на слоях Base, Mouse, Symbol, Game, Reset. Точность 1/3 на Number. Прокрутка на Function: ось Y инвертирована, по горизонтали 1/3, по вертикали 1/9. CPI сенсора 600.

Тамбы переставлены под матрицу этой платы. Левые колонки совпадают с источником. Правый Return стоит на единственном оставшемся правом тамбе: в исходной матрице он был на колонке U, здесь такой тамбы нет.

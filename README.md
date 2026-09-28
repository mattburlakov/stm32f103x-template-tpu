# Введение

Шаблон проекта на STM32f103xx.

## Зависимости
| Утилита | Ссылка |
| --- | --- |
| **Компилятор** | [arm-none-eabi][1] |
| **Система сборки** | [CMake][2] |
| **Генератор makefile (для Windows)** | [MinGW Makefiles][3] |
| **Прошивка** | [OpenOCD][5] или [stlink][4] или [ST-LINK Utility][6] |
| **Отладка** | [OpenOCD][5] + gdb-multiarch (источники платформозависимы) |

На MS Windows уитлиты необходимо добавить в PATH.

## Сборка
### Linux:

```bash
cmake --preset dev
cmake --build --preset dev
```

### Windows:

```bash
cmake --preset dev-win
cmake --build --preset dev
```

## Прошивка

```bash
openocd -f interface/stlink.cfg -f target/stm32f1x.cfg -c "init" -c "reset halt" -c "stm32f1x mass_erase 0" -c "program build/dev/firmware.bin 0x8000000 verify reset exit"
```
Или
```bash
st-flash --reset write build/dev/firmware.bin 0x8000000
```

Или через графический интерфейс ST-LINK Utility.

## Отладка

В терминале 1:
```bash
openocd -f interface/stlink.cfg -f target/stm32f1x.cfg
```

В терминале 2:
```bash
gdb-multiarch firmware.elf
tar rem:3333
load
```

[1]:https://developer.arm.com/downloads/-/gnu-rm
[2]:https://cmake.org/
[3]:https://packages.msys2.org/packages/mingw-w64-x86_64-make
[4]:https://github.com/stlink-org/stlink
[5]:https://openocd.org/pages/getting-openocd.html
[6]:https://www.st.com/en/development-tools/stsw-link004.html

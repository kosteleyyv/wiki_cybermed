# Описание команд кисти

```mermaid
classDiagram
class ComportInstance {
  +baudrate int = 115200
  +bytesize = 8
  +parity = 'N' 
  +stopbits  = 1
  +timeout = 2.0
  +port 
  +last_time
  +last_delay
  +open_comport() открывает и reset in-out-буферы
  +close_comport() reset in-out-буферы и закрывает
  +write_comport(data)
  +send_com(config, power_byte, motor_byte, pwm_byte, time_int, delay) вероятно это отправка конфигурации
  +send_adc(config_byte, adc_int)
}
```

```mermaid
classDiagram
class CommandMaster{
   + time_limited_motion(config, power_byte, motor_byte, pwm_byte, time_int, delay) просто переводит из вещественного времени в целое значение, где **проверка на 100?**
   + upper_comport ComportInstance верхний контроллер (троссы наверху пальца?)
   + lower_comport ComportInstance нижний контроллер (троссы снизу пальца?)
   + upper_commands_list выполненные команды
   + lower_commands_list выполненные команды
   + set_upper_comport() - установка upper_comport
   + set_lower_comport() - установка lower_comport
   + upper_send_adc(config_byte, adc_int) - отправка команды send_adc из config_byte 00100000 (поднят (5) бит) с нужным значением adc_int, где указан номер пальцев в поднятых битах
   + get_upper_commands_list() получение upper_commands_list
   + __add_command_to_upper_list (command) добавление наверно в upper_commands_list байтов команды с их переводом в 16-ричное представление?
   + clear_upper_command_list очищает upper_commands_list
   + send_command(part, config, power_byte, motor_byte, pwm_byte, time_int, delay) - запускает выполнения команды (с вещественным временем), забивает команду с помощью __add_command_to_upper_list, part - верх/низ
   + stop_command(config, motor_byte) наверно, останавливает - но что в конфирурации то???
   + power_command(part, config, power_byte) запуск на исполнение или как?
   + send_command_bytes( part, data) - отправка команд в байтовом представлении
   + release_upper_comport_after_thread() - закрывает и открывает ComportInstance (ЧЕЕЕ?)
}
```

| название | описание | допустимые значения | бит конфигурации | аналогия |
| -- | -- | -- | -- | -- | -- |
| config | Конфигурация | - | 0-5 бит поднимаются под команды ниже (где 0 - младший бит, справа налево), 1 - присутствует параметр в запросе, 0 - отсутствует, 6,7 - не задействованы | заполнение пакета битами, количество поднятых бит == количеству байт в пакете, кроме текущего байта |
| power_byte | Исполнение команды ? | (0) | 1/0 | а если 0 - то что и зачем? |
| motor_byte | Номер двигателя и команда вращения | (1) | 0-2 бита - номер мотора 0 до 4, 3-4 бита - (0 стоп, 1 - вправо, 2 **-вправо?**, 3 - удержание) | раскручивание тросса за заданное время |
| pwm_byte | ШИМ | (2) | от 0 до 100 - какие попугаи? | скорость расручивания и закручивания троссов |
| time_int | Время работы | (3) | от 0 до 100, где 1 == 0.1 |  |
| delay | Задержка исполнения команды | (4) | от 0 до 100, где 1 == 0.1 |  |
|  adc_int  | Разрешение АЦП (5) |  | поднятые биты моторов 0 до 4 бит для каждого пальца | получение измерений на АЦП? |

ШИМ отвечает за скорость раскручивания и закручивания тросика на пальце, а номер двигателя и команда вращения выполнения действия с заданной задержкой и временем выполнения.
Время time_int включает delay или общее время - это delay+time_int?


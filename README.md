# wiki_cybermed

```mermaid
classDiagram
class ComportInstance
{
  int baudrate = 115200
  bytesize = 8
  parity = 'N'
  stopbits = 1
  timeout = 2.0
  port = comport_name
  last_time
  last_delay
  void open_comport() открывает и reset in-out-буферы
  void close_comport() reset in-out-буферы и закрывает
  void write_comport(data)
  void send_com(config, power_byte, motor_byte, pwm_byte, time_int, delay) вероятно это отправка конфигурации
  void  send_adc(config_byte, adc_int)
}
```


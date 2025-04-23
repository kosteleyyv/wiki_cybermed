# wiki_cybermed

```mermaid
classDiagram

        note right of ComportInstance
            Important information! You can write
            notes.
        end note
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


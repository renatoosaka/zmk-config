# TPS43 Azoteq na Nice!Nano direita

## Ligação na protoboard

Monte com USB e bateria desconectados. Use os nomes dos sinais no adaptador
do touchpad; a orientação de um cabo FPC pode inverter a ordem visual dos contatos.

| TPS43 | Nice!Nano / clone V1940 | Pino lógico Pro Micro |
| --- | --- | --- |
| SDA | P0.17 (017) | 2 |
| SCL | P0.20 (020) | 3 |
| RDY | P1.04 (104) | 8 |
| RST / NRST | P1.06 (106) | 9 |
| 3V3 / VDDHI | VCC, saída regulada de 3,3 V | — |
| GND | GND | — |

O RST acima é o **reset do touchpad**, ligado a um GPIO.
Não conecte o RST do touchpad ao RST da Nice!Nano.
Não alimente o TPS43 por RAW, B+ ou 5 V.

SDA e SCL precisam de pull-ups para 3,3 V: um resistor de 4,7 kΩ de SDA
a VCC e outro de SCL a VCC, se o seu adaptador não possuir esses resistores.
Use fios curtos. O barramento está configurado inicialmente em 100 kHz.

No conector FPC original do TPS43, a tabela do fabricante é:
1 RDY, 2 NRST, 3 GND, 4 VDDHI, 5 SCL, 6 SDA.
Confirme o contato 1 do seu adaptador antes de usar essa numeração.

## Primeiro teste: controladora direita por USB

1. Faça a ligação acima e conecte a Nice!Nano ao USB.
2. Entre no bootloader e copie **tps43_usb_test.uf2**.
3. Aguarde a inicialização sem encostar no touchpad e mova um dedo.
4. Teste toque com um dedo (clique esquerdo), dois dedos (clique direito),
   rolagem e pressionar/segurar para arrastar.

Esse firmware funciona sem a metade esquerda. Não precisa da PCB ou das teclas
para testar o touchpad. É temporário: não participa do split.
Se o cursor ficar invertido ou girado, registre o sentido do movimento para
ajustarmos os eixos após confirmar a comunicação.

## Uso normal no teclado dividido

Depois de validar o teste USB, grave **corne_tps43_right.uf2** na direita
e **corne_tps43_left.uf2** na esquerda. Ambos são necessários: a direita
envia os eventos do TPS43 pelo split Bluetooth e a esquerda os envia ao computador.
Conecte o USB do computador à esquerda para o teste do conjunto.

O keymap desta branch foi preservado, incluindo a camada de mouse por teclas.
A esquerda mantém o polling já validado. O OLED da direita está desativado.
Os GPIOs da matriz não foram alterados.

Não use `settings_reset.uf2` como firmware de uso normal. Ele apaga os vínculos
Bluetooth e outras configurações salvas; só use se precisarmos refazer o pareamento.

## Referências e versões

- [Driver IQS5xx](https://github.com/AYM1607/zmk-driver-azoteq-iqs5xx/tree/27321f0232b50f0af31eb27ff97d539933467ea4)
- [ZMK: dispositivo apontador em uma metade periférica](https://zmk.dev/docs/hardware-integration/pointing)
- [Azoteq: módulos e documentação](https://www.azoteq.com/product/i2c-trackpad-modules/)
- ZMK: `5b51501fead672c41b5cfb396f3dafe0894bf4e9`.
- Driver: `27321f0232b50f0af31eb27ff97d539933467ea4`.

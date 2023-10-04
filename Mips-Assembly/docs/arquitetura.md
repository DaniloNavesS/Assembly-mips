# MIPS - ASSEMBLY

## Processo de compilação

![1]("/Mips-Assembly/src/Compilacao.png")

## RISC vs CISC



## Representações das instruções

São separadoss em 3 formatos padrões:

**Tipo R** (3 Registradores)

|op | rs | rt | rd | shant|  function |
|---|----|----|----|------|-----------|
| 6 bits |  5 bits | 5 bits | 5 bits | 5 bits|  6 bits |

*op:* código da operação

*funct:* código aritmetico

*rs e rt:* registradores de operação (em ordem)

*rd:* operador de destino

*shant:* tamanho do deslocamento

|op | rs | rt | rd | shant|  function |
|---|----|----|----|------|-----------|
|Tipo R| $s1|$s2|$st0|0|add|
|0| 17|18|8|0|32|

**Tipo I** (2 Regs e 1 const )

|op | rs | rt | constante / end |
|---|----|----|-----------------|
| 6 bits |  5 bits | 5 bits | 16 bits |

*op:* código da operação

*rs e rt:* registradores de operação (em ordem)

**OBS:** No tipo I, a constante varia de -2^15 a 2^15-1

|Tipo I | $t0 | $s0 | 21 |
|---|----|----|-----------------|
| 4 |  8 | 16 | 21 |


## Deslocamento de memória

**1. Deslocamento de mémoria tipo R**

**Esquerda**
```
    sll $t0, $s0, 4
```
4 = quantidade de bits -> shamt

**Direita**
```
    srl $t0, $s0, 10
```



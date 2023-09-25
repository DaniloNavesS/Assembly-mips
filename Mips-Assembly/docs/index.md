# MIPS - ASSEMBLY

## Estrutura main

```
    .data           # Seção de dados
    .text           # Seção de código
    main:           # Rótulo
    li $v0, 10 # Encerra o codigo, parecido com return 0
    syscall
```

## Registradores

|# do Reg.| Nome       |    Descrição |
|---------|------------|--------------|
|  0      | $zero   | Retorna valor 0|
|  2~3    | $v0-$v1 |(values) separado para setar valores de syscall|
|  4~7    | $a0-$a3 | Registradores de Argumentos|
|  8~15   | $t0-$t7 | Registradores Temporários|
|  16~23  | $s0-$s7 | Registradores de Permanência|
|  24~25  | $t8-$s9 | Registradores Temporários|

## Operações

Operações possui o formato:

    *operador* $resultado, $valor1, $valor2

Operadores:


| Operação | Estrutura |  Ação |
|---------|------------|--------------|
|  Somar  | add $t0, $t1, $t2   | Soma o valor de $t1 + $t2 e o resultado vai para o $t0|

## Funções

|Call Função | Estrutura |  Ação |
|---------|------------|--------------|
|  Mover  | move $t0, $v0   | Transfere o valor de $t0 para $v0|

## Exemplo de código

```
.data
    msg: .asciiz "Hello word\n" # Criar um const com caracteres ascii

.text
main:
    li $v0, 4
    la $a0, msg
    syscall

    li $v0, 10 # Encerra o codigo, parecido com return 0
    syscall
```




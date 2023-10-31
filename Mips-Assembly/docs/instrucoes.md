## Estrutura main

```
    .data           # Seção de dados
    .text           # Seção de código
    main:           # Rótulo
    li $v0, 10 # Encerra o codigo, parecido com return 0
    syscall
```
## Tipo de dados

**.word** w1,w2,...wm -> dado de 32 bits;

**.byte** b1,b2,...bm -> dados de 8 bits;

**.asciiz** str -> cadeia de caracteres ASCII terminados pelo caracter nulo.


Exemplo:
```
x: .word 120 #Se eu colocar "x: .word 120,130,140" seria como um vetor sequencial
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
| Carregamento imediato| li $v0, 5 | Declara o valor de 5 para $v0 |
|Carregamento de endereço| la $a0, msg | Carrega o endereço de msg e armazena em $a0 |  

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
## Operações lógicas

**and, or, xor, nor**

Geralmente usadas para:

 1. Condições compostas 
 2. Máscaras

## Máscaras

**Máscara são operações para lidar com bits de uma palavra**

Ex: Extrair o bit menos significativo

$t0 = 0010 0001 1100 0011

$t9 = 0000 0000 0000 0001

$t2 = 0000 0000 0000 0001

Ex:. máscara

and $t2, $t0, $t9

## Instruções condicionais

| instrução | Estrutura | Ex | Função |
|---|----|----|-----------------|
| slt |  slt reg, r1, r2 | Set on less than  | if ( r1 < r2 ) reg = 1, else reg = 0 |

OBS: *sltu* pode retornar valores diferentes de *slt*

## Instruções de desvio condicional

| instrução | Estrutura | Ex | Função |
|---|----|----|-----------------|
| beq |  beq r1, r2, label | **B**ranch if **EQ**ual  | if ( r1 == r2 ) Desvia para *label* |
| bne |  bne r1, r2, label | **B**ranch if **N**ot **E**qual  | if ( r1 != r2 ) Desvia para *label* |


EX::. 1. Desvio para label se r1 >= r2?

```
slt $t0, r1, r2             # r1 < r2
beq $t0, $zero, label       # Se $t0 = 0, então r1 !< r2 logo, r1 >= r2
```

EX:. 2. Desviar para label r1 > r2:

```
slt $t0, r2, r1
bne $t0, $zero, label
```

**Para os exemplos 1 e 2, há duas pseudo-instruções**

| instrução | Estrutura | Ex | Função |
|---|----|----|-----------------|
| bge |  bge r1, r2, label | **B**ranch  on **G**reater than or **E**qual to   | if ( r1 >= r2 ) Desvia para *label* |
| bgt |  bgt r1, r2, label | **B**ranch on **G**reater **T**han  | if ( r1 > r2 ) Desvia para *label* |

## Desvio incondicional

```
j label     # Jump = Desvia para a label  
```

1. Deslocamento relativo ao PC

Label se transforma na qtde. de instruções a apartir da instrução sendo executada.

**O end. de destino do desvio = 4 * label + PC**

EX:. 

em C

```
if( i == j ) 
    f = g + h;
else 
    f = g - h;
```

Em assembly

f = $s0, g = $s1, j = $s4

```
main:
    beq $s3, $s4, if
    sub $s0, $s1, $s2
    j fim
    if: add $s0, $s1, $s2
```

## Laços de repetição

**Laços são combinações de desvio de condicionais**

Ex:. 

Em C

```
i = 0;
while ( i < n )  {
    A[i] = 0;
    i++;
}
```

Em assembly

i = $t0, n = $s0, A = $s1 

```
add $t0, $zero, $zero       # i = 0;

loop: slt $t2, $t0, $s0     # i < n;
      beq $t2, $zero,  exit
      sll $t1, $t0, 2
      add $t1, $t0, $t1
      sw $zero, 0($t1)
      addi $t0, $t0, 1
      j loop
exit:
```

##  Procedimentos
```
int main () {
    media( a , b );
    return 0;
}
```


**fluxo de chamada a procedimentos**

1. Coloque os argumentos nos registradores
2. Desvie a execução p/ o procedimento
3. Ajuste o armazenamento no procedimento
4. Execute o procedimento
5. Armazene o resultado nos registradores
6.  Ajuste o armazenamento
7. Retorne ao chamado

**Padrão de registradores em procedimentos**

$a0 a $a3: Passagens de argumentos.
$v0 e $v1: Retorno de procedimentos.

Ajuste de armazenamento: $s0 a $s7 devem ser preservados

## Acesso a pilha

1. Inserir

EX: Salvar $s0 e $s1 na pilha

```
add $sp, $sp, -8        # Abrir espaco

sw $s0, 0($sp)          # Salvar na pilha

sw $s1, 4($sp)          # Salvar na pilha
```

2. Remover

```
lw $s0, 0($sp)          # Restaurar valores

lw $s1, 4($sp)          # Restaurar valores

addi $sp, $sp, 8        # Restaura espaço da pilha
```

## Instruçoes para chamada de procedimentos

1. Desvia para o procedimento

```
jal procedimento        # Jump and link
```
Salva o end da próxima instrução no reg $ra(returning adress)
Faz O desvio para o rótulo procedimento

2. Retorna ao chamador
```
jr $ra                 # Jump to returning
```

EX de função:

Em C:
```
int main () {
    int a = 3;
    int b = 2;
    printf("%d\n", media(a,b));
    return 0;
}

int media(int a, int b) {
    return (a + b)/2;
}
```

Em Assembly:

```
main: 
    li $s0, 3
    li $s1, 2

    move $a0, $s0
    move $a1, $s1

    jal media 

    move $a0, $v0
    li $v0, 1
    syscall

    li $v0, 10
    syscall


media: 
    add $v0, $a0, $a1
    srl $v0, $v0, 1
    jr $ra
```

## Manipulação de caracter

1. Tabela ASCII
    printf("%d\n", '2' - '0');
    'a' - 'A' = 32;
2. Instruções de acesso a memória
    lb/lbu $t0, 0($s0)  # Load byte
    sb $t0, 0($s0)      # Store byte


## Detector de overflow - Sem sinal

```
addu $t0, $t1, $t2      # Não lança exeções

xor $t3, $t1, $t2       # Se sinal(t1) != sinal(t2), t3 < 0

slt $t3, $t3, $zero
bne $t3, $zero, sem-overflow

# Se o sinal dos op. forem iguais e o do resultado diferente, houve overflow

xor $t3, $t0, $t1
slt $t3, $t3, $zero
bne $t3, $zero, overflow
```

## Detector de overflow - Com sinal

```
addu $t0, $t1,$t2

nor $t3, $t2, $zero     # t3 = 2^32 - $t2 -1
slt $t3, $t3, $t1       # 2^32 - $t2 - 1 < t1
bne $t3, $zero, overflow

xor $t3, $t0, $t1
slt $t3, $t3, $zero
bne $t3, $zero, overflow
```
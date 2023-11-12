# Exemplos resolvidos em Assembly Mips

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

## Olá mundo
Printa olá mundo na saida.
```
.data
    msg: .asciiz "Ola mundo\n"      # Criar um const com valor "Ola mundo"

.text
main:
    li $v0, 4               # imprime uma label 
    la $a0, msg             # la = Load adress, carrega um adress da .data
    syscall

    li $v0, 10              # Encerra o codigo, parecido com return 0
    syscall
```

## Soma de inteiros

```
.data
    br: .asciiz "\n"        # Quebra de linha 
.text

main:
    li $v0, 5               # Lê uma linha de inteiros
    syscall
    move $t0, $v0           # Pega o valor digitado e adiciona no $t0

    li $v0, 5               # Lê uma linha de inteiros
    syscall
    move $t1, $v0           # Pega o valor digitado e adiciona no $t1

    add $a0, $t0, $t1       # Efetua a operacao de soma
    
    li $v0, 1               # imprime o valor de $a0, no caso, o resultado da soma
    syscall

    la $a0, br              # Carrega o "/n"
    li $v0, 4               # Imprime a break line (br)
    syscall

    li $v0, 10              # Encerra o programa
    syscall
```

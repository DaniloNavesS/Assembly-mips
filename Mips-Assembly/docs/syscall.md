# SYSCALL

**Chamadas do sistema (System Call)**

*Os 4 tipos de solicitações são:*
    
    1 - Escrita de saída padrão

    2 - Leitura da entrada padrão
    
    3 - Alocação de memória

    4 - Encerramento de processo

## Tabela Syscall

|Ação| Codigo em $v0| Argumento | Resultado |
|----|--------------|-----------|-----------|
| Printar Inteiro | 1   |$a0    |           |
| Printar Float | 2   |$f12     |           |
| Printar Double | 3   |$f12     |           |
| Printar String | 4   |$a0    |           |
| Leitura Inteiro | 5   |$a0    | Inteiro em $v0|
| Leitura Float | 6   |$a0    | float em $f0|
| Leitura Float | 7   |$a0    | float em $f0|
| Leitura String | 8   |$a0 = Adress of input buffer, $a1 = Maximum number of characters to read    | Tabela externa|
| Sbrk(Alocação de memoria na HEAP) | 9   |$a0 = Número de bytes alocados    |$v0 contém o end de memória alocada|
| Exit (Encerrar processo) | 10   |||


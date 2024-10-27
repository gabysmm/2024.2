# Implementação de tarefas

## Informações gerais

- Capítulo: [Implementação de tarefas](https://wiki.inf.ufpr.br/maziero/lib/exe/fetch.php?media=socm:socm-05.pdf)
- Disciplina: *sistemas operacionais*
- Livro: [Sistemas Operacionais: Conceitos e Mecanismos](https://wiki.inf.ufpr.br/maziero/doku.php?id=socm:start)

## Aluno

- nome: Gabrielly Stéphany
- matrícula: 20232014040012

## Respostas dos exercícios

#### Questão 1: 
Taks Control block (TCB) é uma estrutura de dados usada pelos sistemas operacionais que serve para armazenar informações sobre uma tarefa ou thread, armazenando as informações necessárias para sua execução e permitindo que o sistema realize outras operaçõe como troca de contexto por exemplo. 

Contém:

- Identificador da tarefa ou thread;
- Estado atual (pronto, executando, entre outros);
- Ponteiro para o contexto da CPU;
- Informações de agendamento;
- Ponto de execução atual;
- Outras informações específicas da tarefa (ponteiros para recursos alocados por exemplo);

#### Questão 2:
Saída do programa: 
> Valor de X: 2                                      
> Valor de X: 2                                       
> Valor de X: 2                                         
> Valor de X: 2

Duração aproximada: 10s (devido aos sleep's)

#### Questão 3:
Saída do programa: 8 letras X
> XXXXXXXX

#### Questão 4:
Threads são unidades básicas de execução dentro de um processo que compartilham o mesmo espaço de memória e recursos dentro de um processo, mas que executam independentemente.
As threas permitem dividir uma tarefa em várias partes que podem ser executadas simultaneamente, aumentando a eficiÇencia e o desempenho em aplicações de múltiplas operações. 

#### Questão 5:
- Vantagens: Maior eficiência e Uso de recursos pelo fato de compartilhar espaço de memória. 
- Desvantagens: Problemas de sicronização justamente por causa desse compartilhamento de memória, então precisa de um controle mais rigoroso pra que a sicronização aconteca, e falha de seguraças onde erros em uma thread podem comprometer todo o processo. 

#### Questão 6:
- O Cálculo de Fibonnaci Recusirvo: Cada chamada dpeende dos resultados de chamadas anteriores, de modo que paralelizar o cálculo não resulta em ganho de desempenho. 
- Processamento de uma lista de Tarefas Pequena: O tempo necessário para criar e gerenciar threas pode ser maior do que o tempo necessário para processar a lista sequencialmente

#### Questão 7: 
(a) = N:1
(b) = N:M
(c) = 1:1
(d) = N:1
(e) = N:M
(f) = N:1
(g) = 1:1
(h) = N:1
(i) = 1:1
(j) = N:M

#### Questão 8:
Duração de execução: 
- N:1 = 30s aproximadamente
- 1:1 = 12s aproximadamente
Saída:
> x: 1, y: 1                                            
> x: 1, y: 2                                              
> x: 1, y: 3


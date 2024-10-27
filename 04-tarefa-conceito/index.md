# Conceito de tarefas

## Informações gerais

- Capítulo: [Conceito de tarefas](https://wiki.inf.ufpr.br/maziero/lib/exe/fetch.php?media=socm:socm-04.pdf)
- Disciplina: *sistemas operacionais*
- Livro: [Sistemas Operacionais: Conceitos e Mecanismos](https://wiki.inf.ufpr.br/maziero/doku.php?id=socm:start)

## Aluno

- nome: Gabrielly Stéphany
- matrícula: 20232014040012

## Respostas dos exercícios

#### Questão 1: 
Time Sharing é um método utilizado em sistemas operacionais que permite o compartilhamento de tempo de um processador entre múltiplas tarefas, fazendo com que pareça que vários programas estejam sendo executado simultaneamente. Esse método funciona dividindo o tempo de processamento em pequenos intervalos chamados de quantums, e cada processo recebe um desses intervalos para ser executado. Todo esse sistema é importante para sistemas operacionais para garantir que todos os processos ativos recebam uma oportunidade justa de execução, melhorando a eficiênca e a produtividade. 

#### Questão 2: 
Um quantum é o intervalo de tempo máximo que um processo pode ocupar na CPU. Ou seja, é o tempo de processamento dividido em pequenos intervalos. Ele influencia a responsividade e a eficiência do sistema e possui crtiérios para ser escolhido como:

     -Tipo de Sistema;
     -Quantidade de Processos ativos;
     -Carga de Trabalho;
     -Overhead de Troca de contexto
     -Objetivos de Prioridade e Responsividade;

Resumidamente, Quantums menores são preferidos para que a interatividade aconteça de forma mais rápida e Quantums mais longos reudiz essa responsividade, acontece de forma mais lenta, mas favorece o monopólio da CPU.

#### Questão 3:
- Transição faltante (t6)

t6 -> Preempção - Processo em Execução é interrompido e volta ao estado de Pronto, aguardando uma nova oportunidade para execução. 

- Estados dos Processos

e1 -> Novo - Estado onde o processo é criado.
e2 -> Pronto - O processo está pronto para execução, aguardando ser escalonado.
e3 -> Executando - O processo está em execução pela CPU.
e4 -> Finalizado - O processo completou sua execução.
e5 -> Espera - O processo está aguardando algum recurso ou evento (ex.: operação de I/O).

- Transições

t1 -> Admissão - O processo passa do estado Novo (e1) para Pronto (e2).
t2 -> Término - O processo em execução (e3) conclui e vai para o estado Finalizado (e4).
t3 -> Espera por I/O - O processo em execução (e3) precisa aguardar I/O e entra no estado de Espera (e5).
t4 -> Conclui I/O - O processo que estava em espera (e5) conclui a operação e volta para Pronto (e2).
t5 -> Escalonamento - O processo em Pronto (e2) é escalonado para execução e vai para o estado Executando (e3).

#### Questão 4: 
E->P é possível. Ex: quando o quantum de tempo do processo acaba
E->S é possível. Ex: acontece quando o processo em execução precisa aguardar dados de entrada/saída
S->E é possível. Ex: ocorre quando o recurso pelo qual o processo esperava está disponível
P->N não é possivel
S->T não é possível
E->T é possível. Ex: quando o processo termina normalmente ou por erro
N->S não é possível
P->S é possível. Ex: quando o processo precisa esperar por um recurso específico. 

#### Questão 5:

[N] O código da tarefa está sendo carregado.
[P] As tarefas são ordenadas por prioridades.
[S] A tarefa sai deste estado ao solicitar uma operação de entrada/saída.
[T] Os recursos usados pela tarefa são devolvidos ao sistema.
[P] A tarefa vai a este estado ao terminar seu quantum.
[P] A tarefa só precisa do processador para poder executar.
[S] O acesso a um semáforo em uso pode levar a tarefa a este estado.
[E] A tarefa pode criar novas tarefas.
[E] Há uma tarefa neste estado para cada processador do sistema.
[S] A tarefa aguarda a ocorrência de um evento externo.



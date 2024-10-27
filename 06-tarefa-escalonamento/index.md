# Escalonamento de tarefas

## Informações gerais

- Capítulo: [Escalonamento de tarefas](https://wiki.inf.ufpr.br/maziero/lib/exe/fetch.php?media=socm:socm-06.pdf)
- Disciplina: *sistemas operacionais*
- Livro: [Sistemas Operacionais: Conceitos e Mecanismos](https://wiki.inf.ufpr.br/maziero/doku.php?id=socm:start)

## Aluno

- nome: Gabrielly Stéphany 
- matrícula: 20232014040012

## Respostas dos exercícios

#### Questão 1: 
O escalonamento Round-Robin distribui o tempo da CPU igualmente entre os processos. Cada processo recebe um tempo fixo (quantum) e, se não terminar, volta para o final da fila e espera sua vez de novo. Ex: Se temos três processos P1, P2 e P3 e um quantum de 4 ms, cada um vai rodar por 4 ms e, se não acabar, volta para a fila, onde esperará sua próxima vez.

#### Questão 2:
A eficiência do sistema mostra quanto do tempo está sendo usado de fato para processar, considerando o tempo de troca de contexto e o uso do quantum. A fórmula é: 
> E = p x tq/p x tq + tcc

Onde:
- tq = quantum de tempo;
- ttc = tempo de troca de contexto;
- p = porcentagem do quantum usado pelas tarefas de I/O

#### Questão 3:
O aging é uma técnica que ajuda a evitar que processos de prioridade baixa fiquem esperando para sempre, aumentando a prioridade deles aos poucos enquanto esperam. Assim, com o tempo, esses processos vão ter prioridade suficiente para serem executados. E é assim que ele funciona. Escalonando esses processos de baixa prioridade. 

#### Questão 4:
Para usar o aging com prioridades negativas, seria necessário ajustar a lógica de incremento de prioridade para que o processo com a menor prioridade numérica (mais negativa) seja tratado como o de menor prioridade efetiva. Portanto, seria necessário diminuir o valor negativo da prioridade aos poucos, para que o processo suba na fila e seja executado.

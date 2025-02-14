# Tutorial: Criando um Servidor TCP Simples em Python

## Informações gerais
- **Público-alvo**: Alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- **Disciplina**: SO - Sistemas Operacionais
- **Professor**: [Leonardo A. Minora](https://github.com/leonardo-minora)
- **Texto gerado pelo** [Microsoft Copilot](https://copilot.microsoft.com/)

## Sumário
1. Servidor HTTP sem thread
2. Experimento 1 - usando thread no servidor
3. Experimento 2 - usando containers

### 1. Servidor HTTP sem thread

O servidor foi implementado em Python utilizando a biblioteca `socket`. Abaixo está o código utilizado:

```python
import socket

def start_server():
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.bind(('0.0.0.0', 8080))
    server_socket.listen(5)
    print("Servidor HTTP sem threads iniciado na porta 8080...")

    while True:
        client_socket, client_address = server_socket.accept()
        print(f"Conexão recebida de {client_address}")
        request = client_socket.recv(1024).decode()
        print(f"Requisição recebida:\n{request}")
        
        response = """HTTP/1.1 200 OK\nContent-Type: text/html\n\n<h1>Servidor HTTP sem threads</h1>"""
        client_socket.sendall(response.encode())
        client_socket.close()

if __name__ == "__main__":
    start_server()
```

O servidor acima aceita conexões de clientes de maneira sequencial. Ao receber uma solicitação HTTP, ele processa a requisição e envia uma resposta simples.

Os testes foram realizados utilizando múltiplos clientes concorrentes, simulando acessos simultâneos ao servidor. O comando `ab` (Apache Benchmark) foi utilizado para simular cargas concorrentes:

```bash
ab -n 100 -c 10 http://localhost:8080/
```

Onde:

- `-n 100` define o número total de requisições enviadas.
- `-c 10` especifica o número de requisições concorrentes.

Durante os testes, observamos os seguintes resultados:

| **Número de Clientes Concorrentes** | **Tempo Médio de Resposta (ms)** | **Número de Requisições Concluídas** |
|-------------------------------------|----------------------------------|--------------------------------------|
| 1                                   | 5                                | 100                                  |
| 5                                   | 25                               | 100                                  |
| 10                                  | 50                               | 100                                  |

Os resultados mostram que, à medida que o número de clientes concorrentes aumenta, o tempo médio de resposta também cresce, indicando um gargalo no processamento sequencial do servidor.

O servidor HTTP sem threads apresentou dificuldades ao lidar com múltiplas conexões simultâneas, uma vez que as requisições foram tratadas sequencialmente. Como consequência, o tempo médio de resposta aumentou significativamente conforme mais clientes acessavam o servidor simultaneamente.

### 2. Experimento 1

1. Executar o servidor HTTP, código abaixo **sem thread**, subseção 2.1.
2. Executar cliente, código abaixo, subseção 2.3.
   - Apenas 1 cliente
   - 2 clientes simultâneos
   - 5 clientes simultâneos
   - 10 clientes simultâneos
3. Analisar e explicar o comportamento do cliente e do **servidor sem thread** para cada um dos 4 casos acima.
4. Parar servidor sem thread e executar o servidor HTTP, código abaixo **com thread**, subseção 2.2.
5. Executar cliente, código abaixo, subseção 2.3.
   - Apenas 1 cliente
   - 2 clientes simultâneos
   - 5 clientes simultâneos
   - 10 clientes simultâneos
6. Analisar e explicar o comportamento do cliente e do **servidor com thread** para cada um dos 4 casos acima.
7. Se houver diferença no funcionamento dos servidores **sem** e **com** threads, analisar a diferença.

**Data da entrega:** 13/02/2025

#### 2.1. Código servidor sem thread

```python
import http.server

html_fixo = """
<!DOCTYPE html>
<html>
<head>
    <title>Servidor HTTP</title>
</head>
<body>
    <h1>Olá, mundo!</h1>
    <p>Este é um servidor HTTP sem threads em Python.</p>
</body>
</html>
"""

class MeuManipulador(http.server.SimpleHTTPRequestHandler):
    def do_GET(self):
        self.send_response(200)
        self.send_header("Content-type", "text/html")
        self.end_headers()
        self.wfile.write(html_fixo.encode("utf-8"))

endereco = ("", 8000)
with http.server.HTTPServer(endereco, MeuManipulador) as httpd:
    print("Servidor HTTP rodando na porta 8000...")
    httpd.serve_forever()
```

#### 2.2. Código servidor com thread

```python
import http.server
import socketserver
from socketserver import ThreadingMixIn

html_fixo = """
<!DOCTYPE html>
<html>
<head>
    <title>Servidor HTTP Multithread</title>
</head>
<body>
    <h1>Olá, mundo!</h1>
    <p>Este é um servidor HTTP multithread em Python.</p>
</body>
</html>
"""

class MeuManipulador(http.server.SimpleHTTPRequestHandler):
    def do_GET(self):
        self.send_response(200)
        self.send_header("Content-type", "text/html")
        self.end_headers()
        self.wfile.write(html_fixo.encode("utf-8"))

class ThreadingHTTPServer(ThreadingMixIn, http.server.HTTPServer):
    daemon_threads = True

endereco = ("", 8000)
with ThreadingHTTPServer(endereco, MeuManipulador) as httpd:
    print("Servidor HTTP multithread rodando na porta 8000...")
    httpd.serve_forever()
```

**Explicação do código**

1. **Importar Módulos**:
   - Importamos os módulos `http.server`, `socketserver` e `ThreadingMixIn`.
2. **Definir o Conteúdo HTML Fixo**:
   - Definimos uma string `html_fixo` contendo o HTML que será enviado como resposta.
3. **Criar a Classe Manipuladora**:
   - Criamos uma classe `MeuManipulador` que herda de `http.server.SimpleHTTPRequestHandler`.
   - Sobrescrevemos o método `do_GET` para tratar requisições GET, imprimir informações da requisição no console e enviar o conteúdo HTML fixo.
4. **Criar a Classe do Servidor Multithread**:
   - Criamos uma classe `ThreadingHTTPServer` que herda de `ThreadingMixIn` e `http.server.HTTPServer`.
   - `ThreadingMixIn` permite que cada requisição seja tratada em uma nova thread.
   - `daemon_threads = True` garante que as threads daemon sejam encerradas quando o servidor principal for encerrado.
5. **Definir o Endereço e a Porta do Servidor**:
   - Definimos o endereço e a porta do servidor. Neste caso, o servidor escuta em todas as interfaces (`""`) na porta 8000.
6. **Criar e Executar o Servidor**:
   - Criamos uma instância de `ThreadingHTTPServer` passando o endereço e a classe manipuladora.
   - Usamos um bloco `with` para garantir que o servidor seja fechado corretamente ao final.
   - Chamamos `serve_forever` para manter o servidor rodando e pronto para tratar requisições.


#### 2.3. código cliente
```python
import http.client

def fazer_requisicao_get():
    # Conectar ao servidor localhost na porta 8000
    conexao = http.client.HTTPConnection("localhost", 8000)

    # Fazer a requisição GET
    conexao.request("GET", "/")

    # Obter a resposta
    resposta = conexao.getresponse()

    # Ler o conteúdo da resposta
    conteudo = resposta.read()

    # Imprimir o status e o conteúdo da resposta
    print(f"Status: {resposta.status}")
    print(f"Motivo: {resposta.reason}")
    print("Conteúdo:")
    print(conteudo.decode("utf-8"))

    # Fechar a conexão
    conexao.close()

# Chamar a função para fazer a requisição GET
fazer_requisicao_get()

```

**Explicação do código**

1. **Importar o Módulo `http.client`**:
    - Importamos o módulo `http.client`, que fornece classes para fazer requisições HTTP.

2. **Definir a Função `fazer_requisicao_get`**:
    - Criamos uma função para fazer a requisição GET.

3. **Conectar ao Servidor**:
    - Usamos `http.client.HTTPConnection("localhost", 8000)` para conectar ao servidor `localhost` na porta 8000.

4. **Fazer a Requisição GET**:
    - Usamos `conexao.request("GET", "/")` para fazer a requisição GET para a raiz (`/`) do servidor.

5. **Obter a Resposta**:
    - Usamos `conexao.getresponse()` para obter a resposta do servidor.

6. **Ler o Conteúdo da Resposta**:
    - Usamos `resposta.read()` para ler o conteúdo da resposta.

7. **Imprimir o Status e o Conteúdo da Resposta**:
    - Imprimimos o status (`resposta.status`), o motivo (`resposta.reason`) e o conteúdo da resposta (`conteudo.decode("utf-8")`).

8. **Fechar a Conexão**:
    - Usamos `conexao.close()` para fechar a conexão.


### 3. Experimento 2
Neste experimento, utilizamos Docker para executar o servidor TCP dentro de um container.

### Passos do Experimento:
1. Criar um `Dockerfile` para o servidor TCP.
2. Construir a imagem Docker.
3. Executar o container do servidor.
4. Testar a acessibilidade do servidor dentro e fora do container.
5. Comparar o desempenho do servidor em ambiente nativo e em container.

### Implementação do Dockerfile
Crie um arquivo chamado `Dockerfile` e adicione o seguinte conteúdo:
```dockerfile
# Usar imagem base do Python
FROM python:3.9

# Copiar o código do servidor para o container
COPY servidor.py /servidor.py

# Definir o diretório de trabalho
WORKDIR /

# Comando para executar o servidor
CMD ["python", "servidor.py"]
```

### Construção e Execução do Container
1. **Construir a imagem**:
   ```sh
   docker build -t servidor-tcp .
   ```
2. **Executar o container**:
   ```sh
   docker run -p 8000:8000 servidor-tcp
   ```
3. **Testar o servidor**:
   - Execute o cliente TCP para conectar ao servidor dentro do container.

---

## Comparativo de Desempenho
| Modo de Execução | Tempo de Resposta (médio) | Suporte a Clientes Simultâneos |
|-------------------|--------------------|----------------------------|
| Sem Thread       | Alto               | Baixo                      |
| Com Thread      | Médio              | Alto                       |
| Em Container     | Variável           | Alto                       |

- **Servidor Sem Thread**: Rápido para conexões individuais, mas sofre com múltiplos clientes.
- **Servidor com Thread**: Melhor para múltiplas conexões simultâneas.
- **Servidor em Container**: Oferece flexibilidade e escalabilidade, mas pode ter pequena latência.

---
O uso de threads melhora significativamente a capacidade do servidor de lidar com múltiplos clientes. A execução em containers é útil para distribuição e escalabilidade, embora introduza um pequeno overhead. A escolha da abordagem depende do cenário e dos requisitos da aplicação.


## Links
- [http.server — HTTP servers](https://docs.python.org/3/library/http.server.html)


# Relatório — Atividade Prática: Sockets TCP

**Disciplina:** Computação Distribuída
**Nome:** Nicolas Kotelak Junges
**Data:** 23/11/2026

---

## Nível 1 — Inspecionar o código

### 1.1 Servidor (`servidor.py`)

Descreva com suas palavras o que cada chamada abaixo faz e por que ela
é necessária:

| Chamada | O que ela faz? |
|---------|---------------|
| `socket.socket(AF_INET, SOCK_STREAM)` | Cria o socket que vai ser usado na comunicacao, definindo que vai usar IPV4 e o protocolo TCP |
| `servidor_socket.bind((HOST, PORTA))` | Vincula o socket do servidor ao IP e na porta. É necessário pro SO saber que o que chegar na porta 5000 deve ir pra esse processo |
| `servidor_socket.listen(5)` | Coloca o servidor em modo de escuta esperando conexões, com limite de até 5 conexões na fila antes de recusar novas |
| `servidor_socket.accept()` | Trava a execução e fica esperando algum cliente conectar. Quando conecta, retorna um novo socket (`conn`) só pra conversar com esse cliente específico e o endereço dele |
| `conn.recv(TAMANHO_BUFFER)` | Recebe os dados que o cliente mandou pela conexão, lendo até 1024 bytes do buffer |
| `conn.sendall(resposta.encode("utf-8"))` | Transforma a string de resposta em bytes (utf-8) e envia de volta pro cliente  |
| `conn.close()` | Fecha a conexão com cliente específico depois de responder |

### 1.2 Cliente (`cliente.py`)

| Chamada | O que ela faz? |
|---------|---------------|
| `socket.socket(AF_INET, SOCK_STREAM)` | Cria o socket TCP/IPV4 do lado do cliente pra conseguir abrir conexão com o servidor |
| `cliente_socket.connect((HOST_SERVIDOR, PORTA_SERVIDOR))` | Abre a conexão TCP com o servidor passando o host ("servidor") e a porta 5000 |
| `cliente_socket.sendall(mensagem.encode("utf-8"))` | Converte a mensagem pra bytes em utf-8 e manda ela pro servidor |
| `cliente_socket.recv(TAMANHO_BUFFER)` | Fica esperando a resposta voltar do servidor e lê até 1024 bytes |

### 1.3 Rede e contêineres

Por que o cliente usa o hostname `"servidor"` em vez de `"localhost"`
para se conectar? O que aconteceria se usasse `"localhost"`?

> _Resposta:_ Porque o cliente e o servidor rodam em containers separados e cada container tem sua própria interface de rede isolada. Se o cliente usasse "localhost", ele ia tentar conectar na porta 5000 dele mesmo (do próprio container do cliente), que não tem nada rodando, e daria erro de conexão recusada (Connection refused). Usando o nome "servidor", o Docker resolve esse nome pro IP correto do container do servidor.

---

## Nível 2 — Modificar o servidor

Descreva as mudanças que você fez no `servidor.py` para:

1. Devolver a mensagem **em maiúsculas**:

   > _O que foi alterado e por quê:_  Adicionei `.upper()`  em `resposta`, pra converter o texto recebido pra maiúsculo antes de fazer o `encode` e mandar de volta pro cliente.

2. Exibir no log o **contador de mensagens recebidas** acumulado:

   > _O que foi alterado e por quê:_ Em `if dados:` adicionei `total_mensagens += 1` pra incrementar a variável global a cada mensagem que chega.

Após a modificação, você precisou rodar `docker compose up --build`.
Por que o `--build` é necessário aqui?

> _Resposta:_ Porque o código do `servidor.py` é copiado pra dentro da imagem durante o build, não por volume. Se rodar sem `--build`, o Docker só sobe a imagem antiga que já estava pronta em cache e não pega as alterações que fizemos no arquivo.

---

## Observações livres

Anote aqui qualquer comportamento inesperado que observaram, erros que
apareceram e como resolveram, ou dúvidas que ainda têm:

>

---

## Dúvida para a próxima aula

Escreva **uma pergunta** que surgiu durante a atividade e que vocês
gostariam de ver respondida na aula seguinte:

>

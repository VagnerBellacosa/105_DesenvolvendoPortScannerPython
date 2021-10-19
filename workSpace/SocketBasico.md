# [SocketBasico](https://wiki.python.org.br/SocketBasico?action=fullsearch&context=180&value=linkto%3A"SocketBasico")

# Receita: SocketBasico

Esta receita é baseada em um material que preparei para uso em aulas para demostrar a programação básica para redes usando socket com a intensão de ser uma "prova de conceitos" sobre o TCP/UDP.

## Estrutura Básica de Programas para Rede



- O Python implementa a interface de rede utilizando os fundamentos da API de Socket.

- Todo socket pode estar em modo ativo ou passivo.

- Quando ele é criado ele esta no modo ativo.

- Para o servidor poder ficar “escutando” um porta é necessário colocar o socket do servidor em modo passivo.

- Seqüência no Cliente:

  

  1. Se foi fornecido um nome de hospedeiro converter em endereço IP;
  2. Se foi fornecido um nome de protocolo de transporte converter em número;
  3. Criar o socket (função socket);
  4. Conecta com o servidor (função connect);
  5. Enviar/Receber dados (permanecer nesse passo enquanto tiver dados para enviar/receber);
  6. Fechar o socket.

- Seqüência no Servidor:

  

  1. Se foi fornecido um nome de protocolo de transporte converter em número;
  2. Criar o socket (função socket);
  3. Coloca um endereço local, endereço IP e porta, no socket (função bind);
  4. Instrui o sistema operacional para colocar o socket em modo passivo (função listen);
  5. Aceita uma nova conexão (função accept);
  6. Enviar/Receber dados (permanecer nesse passo enquanto tiver dados para enviar/receber);
  7. Fechar o socket.
  8. Volta ao passo 5 para aceitar outra conexão.

- Os passos 4 e 5 do servidor são feito quando utilizamos protocolo de transporte orientado a conexão (TCP).

- O servidor tipicamente fica em laço infinito aceitando novas conexões.

- Enquanto o servidor atende uma conexão ele fica dedicado a ela. Para evitar isso é possível fazer um passo intermediário entre o 5 e o 6 para criar um novo processo ou thread para tratar da nova conexão que esta chegando. Com isso o processo/thread pai fica somente recebendo as conexões e o processo/thread filho trata das requisições dos clientes.



## Cliente e Servidor com UDP





### Cliente UDP





[Esconder número das linhas](https://wiki.python.org.br/SocketBasico#)

```
   1 import socket
   2 HOST = '192.168.1.10'  # Endereco IP do Servidor
   3 PORT = 5000            # Porta que o Servidor esta
   4 udp = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
   5 dest = (HOST, PORT)
   6 print 'Para sair use CTRL+X\n'
   7 msg = raw_input()
   8 while msg <> '\x18':
   9     udp.sendto (msg, dest)
  10     msg = raw_input()
  11 udp.close()
```





### Servidor UDP





[Esconder número das linhas](https://wiki.python.org.br/SocketBasico#)

```
   1 import socket
   2 HOST = ''              # Endereco IP do Servidor
   3 PORT = 5000            # Porta que o Servidor esta
   4 udp = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
   5 orig = (HOST, PORT)
   6 udp.bind(orig)
   7 while True:
   8     msg, cliente = udp.recvfrom(1024)
   9     print cliente, msg
  10 udp.close()
```





## Cliente e Servidor com TCP





### Cliente TCP





[Esconder número das linhas](https://wiki.python.org.br/SocketBasico#)

```
   1 import socket
   2 HOST = '127.0.0.1'     # Endereco IP do Servidor
   3 PORT = 5000            # Porta que o Servidor esta
   4 tcp = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
   5 dest = (HOST, PORT)
   6 tcp.connect(dest)
   7 print 'Para sair use CTRL+X\n'
   8 msg = raw_input()
   9 while msg <> '\x18':
  10     tcp.send (msg)
  11     msg = raw_input()
  12 tcp.close()
```





### Servidor TCP





[Esconder número das linhas](https://wiki.python.org.br/SocketBasico#)

```
   1 import socket
   2 HOST = ''              # Endereco IP do Servidor
   3 PORT = 5000            # Porta que o Servidor esta
   4 tcp = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
   5 orig = (HOST, PORT)
   6 tcp.bind(orig)
   7 tcp.listen(1)
   8 while True:
   9     con, cliente = tcp.accept()
  10     print 'Concetado por', cliente
  11     while True:
  12         msg = con.recv(1024)
  13         if not msg: break
  14         print cliente, msg
  15     print 'Finalizando conexao do cliente', cliente
  16     con.close()
```





## Servidor TCP Concorrente





[Esconder número das linhas](https://wiki.python.org.br/SocketBasico#)

```
   1 import socket
   2 import os
   3 import sys
   4 HOST = ''              # Endereco IP do Servidor
   5 PORT = 5000            # Porta que o Servidor esta
   6 tcp = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
   7 orig = (HOST, PORT)
   8 tcp.bind(orig)
   9 tcp.listen(1)
  10 while True:
  11     con, cliente = tcp.accept()
  12     pid = os.fork()
  13     if pid == 0:
  14         tcp.close()
  15         print 'Conectado por', cliente
  16         while True:
  17             msg = con.recv(1024)
  18             if not msg: break
  19             print cliente, msg
  20         print 'Finalizando conexao do cliente', cliente
  21         con.close()
  22         sys.exit(0)
  23     else:
  24         con.close()
```





## Usando Thread para Concorrência





[Esconder número das linhas](https://wiki.python.org.br/SocketBasico#)

```
   1 import socket
   2 import thread
   3 
   4 HOST = ''              # Endereco IP do Servidor
   5 PORT = 5000            # Porta que o Servidor esta
   6 
   7 def conectado(con, cliente):
   8     print 'Conectado por', cliente
   9 
  10     while True:
  11         msg = con.recv(1024)
  12         if not msg: break
  13         print cliente, msg
  14 
  15     print 'Finalizando conexao do cliente', cliente
  16     con.close()
  17     thread.exit()
  18 
  19 tcp = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
  20 
  21 orig = (HOST, PORT)
  22 
  23 tcp.bind(orig)
  24 tcp.listen(1)
  25 
  26 while True:
  27     con, cliente = tcp.accept()
  28     thread.start_new_thread(conectado, tuple([con, cliente]))
  29 
  30 tcp.close()
```



Volta para [CookBook](https://wiki.python.org.br/CookBook).
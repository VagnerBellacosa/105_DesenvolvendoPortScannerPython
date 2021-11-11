[![Blog 4Linux](https://blog.4linux.com.br/wp-content/uploads/2021/04/logo-4linux_blog.jpg)](https://blog.4linux.com.br/)

- https://blog.4linux.com.br/category/desenvolvimento/)
- Socket em Python

[![Socket em Python](https://blog.4linux.com.br/wp-content/uploads/2018/11/socket_python1-1900x1426_c.jpg)](https://blog.4linux.com.br/wp-content/uploads/2018/11/socket_python1.jpg)

 [30 de novembro de 2018](https://blog.4linux.com.br/2018/11/)[Alisson Machado](https://blog.4linux.com.br/author/alisson-machado/)11765 Views

# Socket em Python

**Sockets** são usados para enviar dados através da rede, um exemplo seria enviar um arquivo pelo [Rocket.chat](https://www.4linux.com.br/consultoria/suporte-rocket-chat-open-source) / Skype / Whats App, ou ainda podemos considerar até mesmo as próprias mensagens.

Nesse tutorial, vou criar uma aplicação estilo messenger com cliente – servidor e enviar as mensagens na rede fazendo o uso de Sockets. Um grande erro que alguns programadores cometem, é acreditar que os Sockets são da linguagem de programação, quando na realidade, são do próprio Sistema Operacional, precisamos somente saber como usá-los.

Para saber um pouco mais sobre o Socket em qualquer distribuição [**Linux**](https://www.4linux.com.br/cursos/linux), basta usar o seguinte comando:

```
 $ man socket
```

Como resultado, você terá a documentação que traz uma visão geral sobre o seu uso e suas funções, depois é só adaptar conforme a linguagem de programação que estiver usando.

### APLICAÇÃO DO LADO SERVIDOR

Primeiro, vamos criar o servidor que vai receber as mensagens da nossa aplicação Client.

```
#!/usr/bin/python 
# 
# Coded by: Alisson Machado
# Contact: alisson.copyleft@gmail.com
# servidor que recebe mensagens de aplicação client parecido com o netsend
#
import socket 
host = '' 
port = 7000 
addr = (host, port) 
serv_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
serv_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1) 
serv_socket.bind(addr) 
serv_socket.listen(10) 
print 'aguardando conexao' 
con, cliente = serv_socket.accept() 
print 'conectado' 
print "aguardando mensagem" 
recebe = con.recv(1024) 
print "mensagem recebida: "+ recebe serv_socket.close() 
```

Explicando um pouco as linhas acima:

```
serv_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
```

– Aqui, criamos o nosso mecanismo de Socket para receber a conexão, onde na função passamos 2 argumentos, AF_INET que declara a família do protocolo; se fosse um envio via **Bluetooth** por exemplo, seria: AF_BLUETOOTH, SOCKET_STREAM, indica que será **TCP/IP**.

```
serv_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
```

– Essa linha serve para zerar o TIME_WAIT do Socket, por exemplo, se o programa estiver aguardando uma conexão e você der CTRL+C para interromper, o programa será fechado, porém o Socket continua na escuta e se você for usar a mesma porta receberá a seguinte mensagem:

```
socket.error: [Errno 98] Address already in use
serv_socket.bind(addr)
```

– Esta linha define para qual **IP** e porta o servidor deve aguardar a conexão, que no nosso caso é qualquer IP, por isso o Host é ”.

```
serv_socket.listen(10)
```

– Define o limite de conexões.

```
con, cliente = serv_socket.accept()
```

– Deixa o Servidor na escuta aguardando as conexões, feita a conexão vem o seguinte comando:

```
recebe = con.recv(1024)
```

– Aguarda um dado enviado pela rede de até 1024 Bytes, a função ‘recv’ possui somente 1 argumento que é o tamanho do Buffer.

### APLICAÇÃO DO LADO CLIENTE

Agora, vamos criar a aplicação cliente para testar o nosso Servidor:

```
#!/usr/bin/python
# Coded by: Alisson Machado
# Contact: alisson.machado@responsus.com.br
#

import socket 
ip = raw_input('digite o ip de conexao: ') 
port = 7000 
addr = ((ip,port)) 
client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM) 
client_socket.connect(addr) 
mensagem = raw_input("digite uma mensagem para enviar ao servidor") 
client_socket.send(mensagem) 
print 'mensagem enviada' 
client_socket.close()
```

A maioria das linhas do nosso programa Client já foi descrita no Servidor, pois são apenas alguns ajustes para se fazer o uso do Socket, neste segundo programa, a única diferença são as linhas:

```
client_socket.connect(addr)
```

– Que faz a conexão no nosso servidor.

```
client_socket.send(mensagem)
```

– Que faz o envio do dado para servidor.

```
client_socket.close()
```

– Serve para fechar a conexão entre os dois aplicativos.

Bem, acho que um resumão sobre Socket em [**Python**](https://www.4linux.com.br/consultoria/python).

É isso aí, em breve um artigo com uso de **Threads**.

[CURSOS](https://blog.4linux.com.br/socket-em-python/#)[CONSULTORIA](https://blog.4linux.com.br/socket-em-python/#)  [CONTATO](https://blog.4linux.com.br/socket-em-python/#)

### Compartilhe este post:

[Share onTwitter](https://twitter.com/intent/tweet?text=Socket em Python&url=https%3A%2F%2Fblog.4linux.com.br%2Fsocket-em-python%2F)[Share onFacebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Fblog.4linux.com.br%2Fsocket-em-python%2F)[Share onLinkedIn](https://www.linkedin.com/shareArticle?mini=1&url=https%3A%2F%2Fblog.4linux.com.br%2Fsocket-em-python%2F&title=Socket em Python&source=https%3A%2F%2Fblog.4linux.com.br)[Share onWhatsApp](https://api.whatsapp.com/send?text=Socket em Python — https%3A%2F%2Fblog.4linux.com.br%2Fsocket-em-python%2F)[Share onEmail](mailto:?body=Eu li este post e queria compartilhá-lo com você. Aqui está o link%3A https%3A%2F%2Fblog.4linux.com.br%2Fsocket-em-python%2F&subject=Um artigo que vale a pena compartilhar%3A Socket em Python)

 Anterior[Vagas de emprego - 4Linux](https://blog.4linux.com.br/vagas-de-dba-infra-python-4linux/)

Próxima [Minikube: Kubernetes em Ambiente de Desenvolvimento](https://blog.4linux.com.br/minikube-kubernetes-em-ambiente-de-desenvolvimento/)

 Tags[Python](https://blog.4linux.com.br/tag/python/)

 Categoria[Desenvolvimento](https://blog.4linux.com.br/category/desenvolvimento/)

#### ABOUT AUTHOR

![Alisson Machado](https://secure.gravatar.com/avatar/481803b455df4d2e435d10ffbfbd60b9?s=100&d=mm&r=g





[View all posts by this author →](https://blog.4linux.com.br/author/alisson-machado/)

#### VOCÊ PODE GOSTAR TAMBÉM

[![Git: Adicione ciclos de vida aos seus arquivos](https://blog.4linux.com.br/wp-content/uploads/2017/07/Git_logo-500x500_c.png)](https://blog.4linux.com.br/git-ciclo-de-vida/)

DevOps

#### [Git: Adicione ciclos de vida aos seus arquivos](https://blog.4linux.com.br/git-ciclo-de-vida/)

Git é um versionador de código fonte fácil de usar, isso quase todos sabem, entretanto sua experiência de uso pode ser bem confusa em alguns casos. Convido-os a uma breve

[![Desenvolvimento em PHP – Como essa linguagem pode te ajudar a impulsionar a sua carreira](https://blog.4linux.com.br/wp-content/uploads/2017/07/Desenvolvimento-em-PHP-g-500x500_c.png)](https://blog.4linux.com.br/desenvolvimento-em-php/)

Desenvolvimento

#### [Desenvolvimento em PHP – Como essa linguagem pode te ajudar a impulsionar a sua carreira](https://blog.4linux.com.br/desenvolvimento-em-php/)

 Vivemos em uma época totalmente inovadora. A tecnologia desenvolveu uma linguagem própria e muito abrangente. Embora seja considerada moderna, a linguagem de TI tem um histórico longo. Já no

[![Curso MongoDB Presencial em São Paulo | 4Linux](https://blog.4linux.com.br/wp-content/uploads/2018/02/Curso-MongoDB-500x500_c.jpg)](https://blog.4linux.com.br/curso-mongodb-presencial-sao-paulo/)

Desenvolvimento

#### [Curso MongoDB Presencial em São Paulo | 4Linux](https://blog.4linux.com.br/curso-mongodb-presencial-sao-paulo/)

Bancos de dados são a base dos projetos de desenvolvimento Web. Muitos desenvolvedores estão voltando sua atenção para o MongoDB, um banco de dados sem esquema que é popular para uma


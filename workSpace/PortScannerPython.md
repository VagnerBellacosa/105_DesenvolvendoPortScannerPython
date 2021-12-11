> # Port Scanner com Python
>
> [![img](https://static.vivaolinux.com.br/imagens/fotos/111d7de2172c4.png)](https://www.vivaolinux.com.br/~diegomrodrigues)
>
> [diegomrodrigues](https://www.vivaolinux.com.br/~diegomrodrigues)
>
> Um Port Scanner é uma ferramenta que possui o objetivo de mapear as portas TCP e UDP abertas nos servidores. Existem diversos tipos de Scanners de portas, sendo que eles podem identificar o status das portas, como se estão fechadas, abertas ou apenas escutando. Neste artigo, explico melhor esse tipo de varredura além de disponibilizar um script em Python 3.x para a execução de Port Scanner.
>
> [ Hits: 2.639 ]
>
> Por: Diego Mendes Rodrigues em 19/10/2020 | Blog: https://www.linkedin.com/in/diegomendesrodrigues/
>
> [4 ](https://www.vivaolinux.com.br/artigo/Port-Scanner-com-Python#) [0 ](https://www.vivaolinux.com.br/artigo/Port-Scanner-com-Python#)
>
> [ Denuncie](https://www.vivaolinux.com.br/denuncie/index.php)  [ Favoritos](https://www.vivaolinux.com.br/addBookmark.php?tipo=artigo&codigo=17517)  [ Indicar](https://www.vivaolinux.com.br/formIndicar.php?tipo=artigo&codigo=17517)  [ Impressora](https://www.vivaolinux.com.br/artigos/impressora.php?codigo=17517)



- 
-  

- 

# INTRODUÇÃO





Um *Port Scanner*, ou scanner de porta, é uma ferramenta que possui o objetivo de mapear as portas TCP e UDP abertas em um servidor. Existem diversos tipos de scanners de portas, sendo que eles podem identificar o status das portas, como se estão fechadas, abertas ou apenas escutando conexões.

[![img](https://img.vivaolinux.com.br/imagens/artigos/comunidade/thumb_1602628127.IMAGEM_01.png)](https://img.vivaolinux.com.br/imagens/artigos/comunidade/1602628127.IMAGEM_01.png)

É comum podermos definir o intervalo das portas que a ferramenta irá escanear, com por exemplo, entre 0 e 995, analisando todas as portas 'baixas'. Uma alternativa usada muitas vezes, é podermos definir quais portas serão verificadas, atuando de forma mais assertiva, como por exemplo nas seguintes portas:

- FTP: 20, 21, 115, 989, 990
- SSH: 22
- Telnet: 23
- E-mail: 25, 109, 110, 143, 220, 465, 587, 993, 995
- WEB: 80, 443
- Banco de dados: 1433, 1434, 1521, 1522, 2483, 2484, 3306, 5432


Neste site, você encontra uma lista mais completa das principais portas utilizadas na atualidade:

- [Lista de portas dos protocolos TCP e UDP - Wikipédia, a enciclopédia livre](https://pt.wikipedia.org/wiki/Lista_de_portas_dos_protocolos_TCP_e_UDP)


Muitas vezes, os port scanners são usados por pessoas mal intencionadas, para identificar portas abertas e planejar o acesso indevido aos servidores expostos na internet. Pode também ser usado por empresas de segurança, para análise de vulnerabilidades, conhecidos como Pentest.

Os administradores de redes e de sistemas podem também utilizar os port scanners, varrendo a rede interna na busca de servidores, ou de estações de trabalho, que estejam disponibilizando serviços de forma indevida na rede da empresa, ou que estejam deixando seus equipamentos vulneráveis.

Abaixo, está um port scanner escrito na linguagem *Python*, que pode ser executado em máquinas [Linux](https://www.vivaolinux.com.br/linux/), Windows ou macOS.

Neste script, foram definidas quais portas serão varridas, além disso, o usuário pode definir um host ou um IP para que o scanner seja executado.



"""
Scanner de portas em Python 3.x
Port Scanner
\-
Diego Mendes Rodrigues
"""
import socket
import sys
from datetime import datetime

def scanner_no_ip(endereco_ip, exibir_portas_fechadas=0):
  \# Realizar a varredura nas portas de um endereço IP
  print(f'Scanner no IP {endereco_ip}')

  \# Portas que receberão uma tentativa de conexão
  portas = [20, 21, 22, 23, 42, 43, 43, 69, 80, 109, 110, 115, 118, 143,
       156, 220, 389, 443, 465, 513, 514, 530, 547, 587, 636, 873,
       989, 990, 992, 993, 995, 1433, 1521, 2049, 2081, 2083, 2086,
       3306, 3389, 5432, 5500, 5800]

  \# Escaneando
  try:
    for porta in portas:
      sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
      sock.settimeout(3)
      result = sock.connect_ex((endereco_ip, porta))
      if result == 0:
        print(f' {porta}')
      else:
        if exibir_portas_fechadas > 0:
          print(f' {porta}')
      sock.close()
  except KeyboardInterrupt:
    print('Você pressionou <Ctrl>+<C>')
    sys.exit()

  except socket.gaierror:
    print('O hostname não pode ser resolvido')
    sys.exit()

  except socket.error:
    print('Não foi possível conectar no servidor')
    sys.exit()

\# Execução do principal do script
if __name__ == '__main__':
  \# IP local da máquina
  \# ip_do_servidor = socket.gethostbyname(socket.gethostname())

  \# Buscando o IP de uma URL
  ip_do_servidor = socket.gethostbyname('vivaolinux.com.br')

  \# Data e hora inicial do escaneamento
  t1 = datetime.now()

  scanner_no_ip(ip_do_servidor)

  \# Data e hora final do escaneamento
  t2 = datetime.now()
  total = t2 - t1
  print(f'Scanner finalizado em: {total}')


A execução do script acima faz a varredura de diversas portas no IP da URL vivaolinux.com.br. Os resultados obtidos foram:

Scanner no IP 18.230.112.83
80
443
Scanner finalizado em: 0:00:38.541088

Embora existam ferramentas muito famosas para port scanner, como por exemplo o Nmap, acho muito interessante a execução de varreduras através de scripts.

Eu, particularmente, costumo executar esse tipo de análise nas redes que gerencio, nas estações de trabalho e nos servidores, sendo avisado por e-mail quando existem anomalias.

E você, realiza varreduras em suas redes?
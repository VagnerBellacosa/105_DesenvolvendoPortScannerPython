## Fundamentos do Escaneamento de Portas

Embora o Nmap tenha crescido em funcionalidade ao longo dos anos, ele começou como um eficiente scanner de portas, e essa permanece sua função principal. O simples comando **nmap \*`<alvo>`\*** escaneia mais de 1660 portas TCP no host *`<alvo>`*. Embora muitos scanner de portas tenham tradicionalmente agrupado todas as portas nos estados aberto ou fechado, o Nmap é muito mais granular. Ele divide as portas em seis estados: `aberto(open)`, `fechado(closed)`,`filtrado(filtered)`, `não-filtrado(unfiltered)`, `open|filtered`, ou `closed|filtered`.

Esses estados não são propriedades intrínsecas da porta, mas descrevem como o Nmap as vê. Por exemplo, um scan do Nmap da mesma rede como alvo pode mostrar a porta 135/tcp como aberta, enquanto um scan ao mesmo tempo com as mesmas opções, à partir da Internet poderia mostrar essa porta como `filtrada`.

**Os seis estados de porta reconhecidos pelo Nmap**

- aberto (open)

  Uma aplicação está ativamente aceitando conexões TCP ou pacotes UDP nesta porta. Encontrar esse estado é freqüentemente o objetivo principal de um escaneamento de portas. Pessoas conscientes sobre a segurança sabem que cada porta aberta é um convite para um ataque. Invasores e profissionais de avaliação de segurança querem explorar as portas abertas, enquanto os administradores tentam fechar ou proteger com firewalls sem bloquear usuários legítimos. Portas abertas são também interessantes para scans não-relacionados à segurança pois mostram os serviços disponíveis para utilização na rede.

- fechado (closed)

  Uma porta fechada está acessível (ela recebe e responde a pacotes de sondagens do Nmap), mas não há nenhuma aplicação ouvindo nela. Elas podem ser úteis para mostrar que um host está ativo em um determinado endereço IP (descoberta de hosts, ou scan usando ping), e como parte de uma deteção de SO. Pelo fato de portas fechadas serem alcançáveis, pode valer a pena escanear mais tarde no caso de alguma delas abrir. Os administradores deveriam considerar o bloqueio dessas portas com um firewall. Então elas apareceriam no estado filtrado, discutido a seguir.

- filtrado (filtered)

  O Nmap não consegue determinar se a porta está aberta porque uma filtragem de pacotes impede que as sondagens alcancem a porta. A filtragem poderia ser de um dispositivo firewall dedicado, regras de roteador, ou um software de firewall baseado em host. Essas portas frustram os atacantes pois elas fornecem poucas informações. às vezes elas respondem com mensagens de erro ICMP tais como as do tipo 3 código 13 (destino inalcançável: comunicação proibida administrativamente), mas os filtros que simplesmente descartam pacotes sem responder são bem mais comuns. Isso força o Nmap a tentar diversas vezes só para o caso de a sondagem ter sido descartada por congestionamento da rede ao invés de filtragem. Isso reduz a velocidade do scan dramaticamente.

- não-filtrado (unfiltered)

  O estado não-filtrado significa que uma porta está acessível, mas que o Nmap é incapaz de determinar se ela está aberta ou fechada. Apenas o scan ACK, que é usado para mapear conjuntos de regras de firewall, classifica portas com este estado. Escanear portas não-filtradas com outros tipos de scan, tal como scan Window, scan Syn, ou scan FIN, podem ajudar a responder se a porta está aberta.

- open|filtered

  O Nmap coloca portas neste estado quando é incapaz de determinar se uma porta está aberta ou filtrada. Isso acontece para tipos de scan onde as portas abertas não dão nenhuma resposta. A falta de resposta também pode significar que um filtro de pacotes descartou a sondagem ou qualquer resposta que ela tenha provocado. Portanto não sabe-se com certeza se a porta está aberta ou se está sendo filtrada. Os scans UDP, IP Protocol, FIN, Null, e Xmas classificam portas desta forma.

- closed|filtered

  Este estado é usado quando o Nmap é incapaz de determinar se uma porta está fechada ou filtrada. É apenas usado para o scan IPID Idle scan.
# 🌐 Projeto Mini-NET: Implementação da Pilha de Protocolos
Este projeto foi desenvolvido para a disciplina de Redes de Computadores. O objetivo central é desmistificar o funcionamento da Internet ao implementar, do zero, uma pilha de protocolos que garante a entrega de mensagens em um canal de comunicação propositalmente defeituoso.

<br>

## 🎯 O Desafio
A aplicação é um chat que utiliza UDP (protocolo não confiável) como base. Sobre o UDP, construímos camadas que resolvem os problemas clássicos de redes:

- Perda de Pacotes: Resolvido com a técnica Stop-and-Wait e Timeouts.
- Corrupção de Dados: Detectada através de cálculos de CRC32 (Checksum).
- Ordenação e Duplicatas: Controladas por Números de Sequência (bit alternante).
- Roteamento: Implementado via endereçamento virtual (VIP) e tabelas de encaminhamento.

<br>

## 🏗️ Arquitetura do Sistema
O projeto segue a abordagem Top-Down, respeitando o encapsulamento onde cada camada só conversa com a camada imediatamente inferior.

<br>

Estrutura de Encapsulamento:

- Aplicação: Mensagem em formato JSON.
- Transporte: Adiciona seq_num e lida com ACKs.
- Rede: Adiciona VIP (IP Virtual) de origem/destino e TTL.
- Enlace: Adiciona endereços MAC e o código CRC32 para integridade.

<br>

## 🛠️ Tecnologias e Requisitos

- Linguagem: Python 3.8+.
- Bibliotecas: Apenas bibliotecas padrão (socket, json, zlib, threading, etc.).

<br>

Arquivos inclusos:
- client.py: Interface do usuário e lógica de retransmissão.
- server.py: Destino das mensagens e emissor de ACKs.
- router.py: Intermediário que realiza o roteamento virtual.
- protocolo.py: Biblioteca com o simulador de ruído e estruturas de dados.

<br>

## 🚀 Como Executar
Para simular a rede completa, você precisará de três terminais abertos simultaneamente. Siga a ordem abaixo:
1. Inicie o Roteador:
- Bash
- > python router.py

2. Inicie o Servidor:
- Bash
- > python server.py

3. Inicie o Cliente:
- Bash
- > python client.py

<br>

Como testar: Digite uma mensagem no terminal do Cliente. Observe nos terminais os logs coloridos mostrando a mensagem sendo encapsulada, o risco de perda/corrupção no "meio físico" e a recuperação automática caso algo dê errado.

<br>

## 📊 Critérios de Resiliência
O sistema foi configurado no arquivo protocolo.py para operar sob as seguintes condições de estresse:

- 20% de chance de perda total do pacote.
- 20% de chance de corrupção de bits (ruído).
- Latência variável entre 0.1s e 0.5s.

<br>

A camada de transporte garante que, mesmo com essas falhas, a mensagem chegue íntegra ao destino final.

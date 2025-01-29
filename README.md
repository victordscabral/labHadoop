# PSPD_LabClusterHadoop

- **Disciplina**: FGA0244 - Programação para Sistemas Paralelos e Distribuídos (PSPD)  
- **Turma**: T01 (2024.2 - 24T23) 
- **Professor**: Fernando William Cruz
- **Atividade**: Laboratório Hadoop

## Alunos Participantes
- **Heitor Marques Simões Barbosa**: 202016462  
- **José Luís Ramos Teixeira**: 190057858  
- **Pablo Christianno Silva Guedes**: 200042416  
- **Philipe de Sousa Barros**: 170154319  
- **Victor de Souza Cabral**: 190038900  

</br>
</br>
</br>

## 1. Objetivo

O objetivo deste trabalho é implementar um laboratório utilizando o Apache Hadoop para:

- **Configurar e monitorar um cluster Hadoop básico**, composto por um nó mestre e, no mínimo, dois nós workers.
- **Testar a tolerância a falhas do Hadoop**, incluindo remoção e adição de nós no cluster, e analisar os impactos na performance das aplicações.
- **Realizar testes de desempenho com a aplicação WordCount** (paradigma MapReduce), utilizando massas de dados que garantam uma execução suficientemente longa para monitoramento adequado.

Mais detalhes podem ser encontrados no documento oficial do trabalho: [Atividade extra-classe – Laboratório Hadoop](docs/PSPD_LabClusterHadoop.pdf).

---

## 2. Instalação do Hadoop

Este projeto utiliza a versão **Hadoop 3.3.6** no sistema operacional **Ubuntu 24.04.1 LTS**. Embora o Hadoop funcione bem em versões anteriores do Ubuntu, como 20.04 e 22.04, a versão **Java 8** é obrigatória para garantir compatibilidade.

Confira o guia de instalação detalhado: [Guia de Instalação do Hadoop](docs/download-hadoop.md).

---

## 3. Estrutura do Cluster

O cluster será composto por:

- **Nó Mestre**: Computador do aluno responsável (Ubuntu 24.04).
- **Workers**: Computadores dos colegas do grupo, com configurações compatíveis com o Hadoop.
- **Rede Local**: Configuração via cabo utilizando um **Switch Fast Ethernet SF 800** para otimizar a comunicação entre os nós.

Mais detalhes sobre a configuração da rede podem ser encontrados aqui: [Configuração de Rede](docs/configs-rede.md).

---

## 4. Testes

Serão realizados os seguintes testes no cluster Hadoop:

- **Desempenho**: Medição do tempo de execução da aplicação WordCount com diferentes números de nós workers ativos.
- **Tolerância a falhas**: Simulação de cenários adversos com remoção e adição de nós durante a execução da aplicação.
- **Monitoramento**: Utilização das interfaces web do Hadoop para monitorar o estado do cluster e os serviços submetidos.

Para um roteiro detalhado de testes, acesse: [Testes no Hadoop](docs/testes-hadoop.md).  
Para testes no cluster em execução, acesse: [Testes no Cluster Hadoop](docs/testes-hadoop-cluster.md).

---

</br>
</br>
</br>

## Referências

1. **Tutorial Instalación Hadoop 3.3.6 en Ubuntu 24.04**. Gabriel Florit, [YouTube](https://www.youtube.com/watch?v=R7O3FKMg2GQ). Acesso em: 26/01/2025.
2. **Apache Hadoop 3.3.6 Installation on Ubuntu 22.04**. Abhik Dey, [Medium](https://medium.com/@abhikdey06/apache-hadoop-3-3-6-installation-on-ubuntu-22-04-14516bceec85). Acesso em: 26/01/2025.

---

Para mais detalhes técnicos e operacionais, consulte a pasta `docs/`.

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

## 1. Introdução

Este projeto tem como objetivo a implementação de um cluster básico utilizando o Apache Hadoop, com foco no processamento distribuído de dados por meio do paradigma MapReduce. A atividade envolveu a configuração de um cluster com um nó mestre e dois ou mais nós workers, além de testes de desempenho e tolerância a falhas para garantir a resiliência do sistema. O Hadoop, sendo uma ferramenta fundamental em sistemas distribuídos, foi utilizado para explorar conceitos de escalabilidade, resiliência e performance.

Neste documento encontra-se detalhes sobre as etapas da configuração, os testes realizados, os resultados observados, além das considerações sobre a instalação e o funcionamento do cluster. A metodologia adotada, os desafios enfrentados e as lições aprendidas ao longo do experimento também são discutidos. Ao final, cada integrante do grupo irá compartilhar suas conclusões e autoavaliações.

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

## 5. Conclusão

A implementação do cluster Hadoop permitiu explorar conceitos fundamentais de processamento distribuído e escalabilidade em sistemas paralelos. Durante a atividade, realizamos a instalação e configuração do ambiente, além da tentativa de execução de testes para validar o desempenho e a resiliência do cluster.

Os resultados obtidos mostraram que o Hadoop é uma ferramenta poderosa para manipulação de grandes volumes de dados, sendo capaz de distribuir o processamento de maneira eficiente entre os nós.Ao longo do projeto, enfrentamos desafios relacionados à configuração da rede, permissões do sistema e compatibilidade entre versões do software, o que exigiu um aprofundamento no entendimento do funcionamento interno do Hadoop. A experiência adquirida contribuiu significativamente para a nossa compreensão sobre sistemas distribuídos e a importância da configuração adequada para garantir o desempenho esperado.

Dessa forma, esta atividade proporcionou um aprendizado prático essencial, preparando-nos para desafios futuros no desenvolvimento e manutenção de sistemas paralelos e distribuídos.

## Referências

1. **Tutorial Instalación Hadoop 3.3.6 en Ubuntu 24.04**. Gabriel Florit, [YouTube](https://www.youtube.com/watch?v=R7O3FKMg2GQ). Acesso em: 26/01/2025.
2. **Apache Hadoop 3.3.6 Installation on Ubuntu 22.04**. Abhik Dey, [Medium](https://medium.com/@abhikdey06/apache-hadoop-3-3-6-installation-on-ubuntu-22-04-14516bceec85). Acesso em: 26/01/2025.

---

Para mais detalhes técnicos e operacionais, consulte a pasta `docs/`.

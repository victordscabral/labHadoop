# Guia de Instalação do Hadoop 3.3.6 no Ubuntu 24.04

Este guia oferece instruções resumidas e diretas para instalar e configurar o Hadoop 3.3.6 no Ubuntu 24.04, com base nas referências estudadas.

---

## 1. Pré-requisitos

- **Sistema Operacional**: Ubuntu 24.04 LTS (também compatível com 20.04 e 22.04).
- **Java Development Kit (JDK)**: Versão 8.

Certifique-se de atualizar os pacotes antes de iniciar:
```bash
sudo apt update && sudo apt upgrade -y
```

---

## 2. Instalação do Java 8

O Hadoop exige o Java 8 para funcionar corretamente.

```bash
sudo apt install openjdk-8-jdk -y
```
Verifique a instalação:
```bash
java -version
```
O comando deve exibir uma versão do Java 8.

---

## 3. Instalação do SSH

O SSH é essencial para a comunicação entre os nós do cluster Hadoop.

```bash
sudo apt install ssh -y
```
Teste a instalação do SSH:
```bash
ssh localhost
```

---

## 4. Criação do Usuário Hadoop

Crie um novo usuário para executar os serviços do Hadoop:
```bash
sudo adduser hadoop
```
Troque para o novo usuário:
```bash
su - hadoop
```

---

## 5. Configuração do SSH para o Usuário Hadoop

Configure o acesso SSH sem senha:
```bash
ssh-keygen -t rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 640 ~/.ssh/authorized_keys
ssh localhost
```

---

## 6. Download e Instalação do Hadoop

1. Baixe a versão 3.3.6 do Hadoop:
   ```bash
   wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz
   ```
2. Extraia o arquivo:
   ```bash
   tar -xvzf hadoop-3.3.6.tar.gz
   ```
3. Renomeie a pasta extraída (opcional):
   ```bash
   mv hadoop-3.3.6 hadoop
   ```

---

## 7. Configuração das Variáveis de Ambiente

Edite o arquivo `~/.bashrc` e adicione as seguintes linhas:
```bash
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export HADOOP_HOME=/home/hadoop/hadoop
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export HADOOP_YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native"
```
Carregue as configurações:
```bash
source ~/.bashrc
```

Configure o Java no arquivo `hadoop-env.sh`:
```bash
nano $HADOOP_HOME/etc/hadoop/hadoop-env.sh
```
Edite a linha:
```bash
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

---

## 8. Formatação e Inicialização do Hadoop

1. Formate o NameNode:
   ```bash
   hdfs namenode -format
   ```
2. Inicie o cluster Hadoop:
   ```bash
   start-all.sh
   ```
3. Verifique os serviços:
   ```bash
   jps
   ```

---

## 9. Acesso à Interface Web

- **NameNode**: [http://localhost:9870](http://localhost:9870)
- **Resource Manager**: [http://localhost:8088](http://localhost:8088)

---

</br>
</br>
</br>

## Referências

1. **Tutorial Instalación Hadoop 3.3.6 en Ubuntu 24.04**. Gabriel Florit, [YouTube](https://www.youtube.com/watch?v=R7O3FKMg2GQ). Acesso em: 26/11/2025.
2. **Apache Hadoop 3.3.6 Installation on Ubuntu 22.04**. Abhik Dey, [Medium](https://medium.com/@abhikdey06/apache-hadoop-3-3-6-installation-on-ubuntu-22-04-14516bceec85). Acesso em: 26/11/2025.

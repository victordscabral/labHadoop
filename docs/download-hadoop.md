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

## 3. Instalação do OpenSSH

O OpenSSH é essencial para a comunicação entre os nós do cluster Hadoop.

```bash
sudo apt install openssh-server -y
```
Teste a instalação do OpenSSH:
```bash
ssh localhost
```
```bash
sudo systemclt status ssh
```
---

## 4. Criação do Usuário Hadoop

Crie um novo usuário para executar os serviços do Hadoop, vamos padronizar **(no nosso trabalho)** para ser o nome inicial de cada aluno, definindo a senha com o nome de cada:

- **Usuários:** artur-hadoop, jose-hadoop, pablo-hadoop, phil-hadoop, victor-hadoop...
- **Senhas:**  artur, jose, pablo, phil, victor...

Pode deixar em branco (quando for solicitado) informaçoes como:
- Full Name, Room Number, Work Phone, Home Phone, Other.

Atualizar de acordo:
```bash
sudo adduser jose-hadoop
```
Troque para o novo usuário:
```bash
su - jose-hadoop
```

---

## 5. Configuração do SSH para o Usuário Hadoop

Configure o acesso SSH sem senha:
```bash
ssh-keygen -t rsa
```
- Apenas aperte `Enter` confirmando as opções em branco.
```bash
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 640 ~/.ssh/authorized_keys
```
```bash
ssh localhost
```
- Confirme o `yes` ao adicionar o fingerprint pela primeira vez.

---

## 6. Download e Instalação do Hadoop

Primeiro crie uma pasta onde será instalado o hadoop, e entre nela. Por exemplo `Downloads` (se já não tiver):
```bash
mkdir Downloads
```
```bash
cd /home/jose-hadoop/Downloads
```

Dentro dela faça a instalação do hadoop:

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

Edite o arquivo `~/.bashrc`:
```bash
nano ~/.bashrc
```
**Reajuste** as seguintes linhas, de acordo com o seu caminho:
```bash
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export HADOOP_HOME=/home/jose-hadoop/Downloads/hadoop
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export HADOOP_YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native"
```
Carregue/salve as configurações no seu ambiente:
```bash
source ~/.bashrc
```

Configure o Java no arquivo `hadoop-env.sh`:
```bash
nano $HADOOP_HOME/etc/hadoop/hadoop-env.sh
```
Procure e edite a linha `export JAVA_HOME`, colocando:
```bash
JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

---

## 8. Configuração do Hadoop

Primeiro vamos criar os diretórios para **namenode** e **datanode**:
```bash
cd hadoop/
```
```bash
mkdir -p /home/jose-hadoop/Downloads/hadoop/hadoopdata/hdfs/{namenode,datanode}
```

Agora Iremos configurar os arquivos:

- **core-site.xml**
- **hdfs-site.xml**
- **mapred-site.xml**
- **yarn-site.xml**

### 8.1. core-site.xml
```bash
nano $HADOOP_HOME/etc/hadoop/core-site.xml
```
Configure de acordo com o hostname do seu sistema:
```bash
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```
Salve e feche o arquivo.

</br>

### 8.2. hdfs-site.xml
```bash
nano $HADOOP_HOME/etc/hadoop/hdfs-site.xml
```
Configure de acordo com o hostname do seu sistema:
```bash
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:///home/jose-hadoop/Downloads/hadoop/hadoopdata/hdfs/namenode</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:///home/jose-hadoop/Downloads/hadoop/hadoopdata/hdfs/datanode</value>
    </property>
</configuration>
```
Salve e feche o arquivo.

</br>

### 8.3. mapred-site.xml
```bash
nano $HADOOP_HOME/etc/hadoop/mapred-site.xml
```
Configure de acordo com o hostname do seu sistema:
```bash
<configuration>
   <property>
      <name>yarn.app.mapreduce.am.env</name>
      <value>HADOOP_MAPRED_HOME=/home/jose-hadoop/Downloads/hadoop/bin/hadoop</value>
   </property>
   <property>
      <name>mapreduce.map.env</name>
      <value>HADOOP_MAPRED_HOME=/home/jose-hadoop/Downloads/hadoop/bin/hadoop</value>
   </property>
   <property>
      <name>mapreduce.reduce.env</name>
      <value>HADOOP_MAPRED_HOME=/home/jose-hadoop/Downloads/hadoop/bin/hadoop</value>
   </property>
</configuration>
```
Salve e feche o arquivo.

</br>

### 8.4. yarn-site.xml
```bash
nano $HADOOP_HOME/etc/hadoop/yarn-site.xml
```
Configure de acordo com o hostname do seu sistema:
```bash
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>
```
Salve e feche o arquivo.

</br>

---

## 9. Inicialização do Hadoop

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

## 10. Acesso à Interface Web

Primeiro veja o ip que está usando local:
```bash
ip a
```

Depois acesse, com o ip que está configurado:

- **NameNode**: [http://localhost:9870](http://localhost:9870)
- **Resource Manager**: [http://localhost:8088](http://localhost:8088)

---

</br>
</br>
</br>

## Referências

1. **Tutorial Instalación Hadoop 3.3.6 en Ubuntu 24.04**. Gabriel Florit, [YouTube](https://www.youtube.com/watch?v=R7O3FKMg2GQ). Acesso em: 2 dias atrás.
2. **Apache Hadoop 3.3.6 Installation on Ubuntu 22.04**. Abhik Dey, [Medium](https://medium.com/@abhikdey06/apache-hadoop-3-3-6-installation-on-ubuntu-22-04-14516bceec85). Acesso em: 2 dias atrás.

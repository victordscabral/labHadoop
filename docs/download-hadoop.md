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

## 2. Instalação do Java 8 e PDSH

O Hadoop exige o Java 8 para funcionar corretamente.

```bash
sudo apt install openjdk-8-jdk -y
```
Verifique a instalação:
```bash
java -version
```
O comando deve exibir uma versão do Java 8, como por exemplo `openjdk version "1.8.0_432"`.

O PDSH (Parallel Distributed Shell) permite executar comandos simultaneamente em vários nós do cluster, facilitando a administração do Hadoop, como iniciar ou parar serviços em múltiplas máquinas.
```bash
sudo apt install pdsh -y
```

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
sudo systemctl status ssh
```
---

## 4. Criação do Usuário Hadoop

Crie um novo usuário para executar os serviços do Hadoop, vamos padronizar **(no nosso trabalho)** para ser o nome inicial de cada aluno, definindo a senha com o nome de cada:

- **Usuários:** hadoopuser
- **Senhas:**  hadoop

Pode deixar em branco (quando for solicitado) informaçoes como:
- Full Name, Room Number, Work Phone, Home Phone, Other.

Atualizar de acordo:
```bash
sudo adduser hadoopuser
```
Troque para o novo usuário:
```bash
su - hadoopuser
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
cd /home/hadoopuser/Downloads
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
3. Renomeie a pasta extraída (opcional, mas facilita na configuração):
   ```bash
   mv hadoop-3.3.6 hadoop
   ```

---

## 7. Configuração das Variáveis de Ambiente

Edite o arquivo `~/.bashrc`:
```bash
nano ~/.bashrc
```
**Adicione no final do arquivo** e **reajuste** as seguintes linhas, de acordo com o seu caminho:
```bash
export PDSH_RCMD_TYPE=ssh
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export HADOOP_HOME=/home/hadoopuser/Downloads/hadoop
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
Procure e edite a linha `export JAVA_HOME`, descomentando ela e colocando:
```bash
JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```
Desse jeito mesmo, **SEM** o export antes do JAVA_HOME.

---

## 8. Configuração do Hadoop

Primeiro vamos criar os diretórios para **namenode** e **datanode**:
```bash
cd hadoop/
```
```bash
mkdir -p /home/hadoopuser/Downloads/hadoop/hadoopdata/hdfs/{namenode,datanode}
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
        <value>file:///home/hadoopuser/Downloads/hadoop/hadoopdata/hdfs/namenode</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:///home/hadoopuser/Downloads/hadoop/hadoopdata/hdfs/datanode</value>
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
      <value>HADOOP_MAPRED_HOME=/home/hadoopuser/Downloads/hadoop/bin/hadoop</value>
   </property>
   <property>
      <name>mapreduce.map.env</name>
      <value>HADOOP_MAPRED_HOME=/home/hadoopuser/Downloads/hadoop/bin/hadoop</value>
   </property>
   <property>
      <name>mapreduce.reduce.env</name>
      <value>HADOOP_MAPRED_HOME=/home/hadoopuser/Downloads/hadoop/bin/hadoop</value>
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

### 8.5. Editar o arquivo de Workers

1. No **master** (`hadoop-master`), abra o arquivo `workers`:
   ```bash
   sudo nano /home/hadoopuser/Downloads/hadoop/etc/hadoop/workers
   ```

2. Adicione os nomes dos workers:
   ```plaintext
   hadoop-slave1
   hadoop-slave2
   ```

3. Salve o arquivo (`CTRL + X`, `Y`, `ENTER`).

4. Certifique-se de que o arquivo `/etc/hosts` também esteja configurado corretamente em todas as máquinas.

</br>

---

## 9. Configuração de Hosts e Nomes das Máquinas

1. **Editar o arquivo `/etc/hosts` em todas as máquinas:**
   ```bash
   sudo nano /etc/hosts
   ```

   Adicione as seguintes entradas:
   ```plaintext
   192.168.0.10 hadoop-master
   192.168.0.11 hadoop-slave1
   192.168.0.12 hadoop-slave2
   ```

2. **Definir o nome de cada máquina:**
   Em cada máquina, edite o arquivo `/etc/hostname`:

   - No **master** (`hadoop-master`):  
     ```bash
     sudo nano /etc/hostname
     ```
   
   - No **slave1** (`hadoop-slave1`):  
     ```bash
     sudo nano /etc/hostname
     ```
   
   - No **slave2** (`hadoop-slave2`):  
     ```bash
     sudo nano /etc/hostname
     ```

3. **Reinicie as máquinas:**
   ```bash
   sudo reboot
   ```

---

## 10. Inicialização do Hadoop

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

## 11. Acesso à Interface Web

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
</br>
</br>

---

## 12. Verificando o Cluster Hadoop

Agora que o Hadoop está instalado e configurado, podemos verificar seu funcionamento criando diretórios no HDFS.

Crie os diretórios para teste:
```bash
hdfs dfs -mkdir /test1
hdfs dfs -mkdir /logs
```

Liste os diretórios criados para confirmar:
```bash
hdfs dfs -ls /
```

Adicione arquivos ao HDFS, por exemplo, os logs do sistema:
```bash
hdfs dfs -put /var/log/* /logs/
```

Você pode visualizar os arquivos e diretórios adicionados através da interface web do Hadoop:

1. Acesse a interface web do NameNode em [http://localhost:9870](http://localhost:9870).
2. Navegue até `Utilities` > `Browse the file system`.
3. Confirme se os diretórios `/test1` e `/logs` aparecem corretamente.


---

## 13. Desligar os serviços do Hadoop

Para desligar todos os serviços do Hadoop corretamente, utilize o seguinte comando:
```bash
stop-all.sh
```

Depois, verifique se os processos do Hadoop foram finalizados:
```bash
jps
```
Se ainda houver processos em execução, finalize-os manualmente conforme necessário:
```bash
kill <PID>
```
> Substitua `<PID>` pelo número do processo, caso queira fazer a força coloque um `-9` antes, por exemplo `kill -9 12345`

> Obs:. **NÃO** realize kill no processo `Jps`

---

</br>
</br>
</br>
</br>
</br>

---

## 14. Extras (Opcional)

Caso queira adicionar o usuário `hadoopuser` ao grupo sudo para executar comandos administrativos. Em um usuário com permissões `sudo` como o `root`, utilize:
```bash
sudo usermod -aG sudo hadoopuser
```

Verifique se o usuário foi adicionado corretamente ao grupo `sudo`:
```bash
groups hadoopuser
```
deve retornar algo como: `hadoopuser : hadoopuser sudo`

Para ver mais estudos entre na pasta [testes-hadoop.md](testes-hadoop.md), onde exploramos testes com mapper e reducer usando Python e Java.

---

</br>
</br>
</br>

## Referências

1. **Tutorial Instalación Hadoop 3.3.6 en Ubuntu 24.04**. Gabriel Florit, [YouTube](https://www.youtube.com/watch?v=R7O3FKMg2GQ). Acesso em: 26/01/2025.
2. **Apache Hadoop 3.3.6 Installation on Ubuntu 22.04**. Abhik Dey, [Medium](https://medium.com/@abhikdey06/apache-hadoop-3-3-6-installation-on-ubuntu-22-04-14516bceec85). Acesso em: 26/01/2025.

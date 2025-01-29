# Testes no Hadoop

Este documento explora como manipular o HDFS e realizar contadores de palavras utilizando Python e Java no Hadoop, adaptado ao nosso contexto de instalação.

---

## 1. Manipulando o HDFS

### Listando diretórios no HDFS

Listar o conteúdo da partição HDFS:
```bash
$HADOOP_HOME/bin/hdfs dfs -ls
```

Listar o conteúdo do diretório raiz no HDFS:
```bash
$HADOOP_HOME/bin/hdfs dfs -ls /
```

### Criando diretórios no HDFS

Criar um diretório específico no HDFS:
```bash
$HADOOP_HOME/bin/hdfs dfs -mkdir /user/jose-hadoop
```

Criar um subdiretório para entrada de arquivos:
```bash
$HADOOP_HOME/bin/hdfs dfs -mkdir /user/jose-hadoop/input
```

### Copiando arquivos para o HDFS

Copiar arquivos do sistema local para o HDFS:
```bash
$HADOOP_HOME/bin/hdfs dfs -put ~/arquivos_hadoop/poema01.txt /user/jose-hadoop/input
$HADOOP_HOME/bin/hdfs dfs -put ~/arquivos_hadoop/poema02.txt /user/jose-hadoop/input
```

### Verificando arquivos no HDFS

Listar os arquivos no diretório de entrada do HDFS:
```bash
$HADOOP_HOME/bin/hdfs dfs -ls /user/jose-hadoop/input
```

Visualizar o conteúdo de um arquivo no HDFS:
```bash
$HADOOP_HOME/bin/hdfs dfs -cat /user/jose-hadoop/input/poema02.txt
```

---

</br>
</br>
</br>
</br>
</br>

---

## 2. Contador de Palavras no Hadoop com Python

### Conferindo arquivos de entrada

Certifique-se de que o arquivo de entrada foi copiado para o HDFS:
```bash
$HADOOP_HOME/bin/hdfs dfs -cat /user/jose-hadoop/input/poema02.txt
```

### Preparando o ambiente

1. Copie o Hadoop Streaming (necessário para rodar Python no Hadoop):
   ```bash
   cp $HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-3.3.6.jar .
   ```

2. Copie os scripts `mapper.py` e `reducer.py` para a pasta local:
   ```bash
   cp ~/scripts/mapper.py .
   cp ~/scripts/reducer.py .
   ```

3. Torne os scripts executáveis:
   ```bash
   chmod +x *.py
   ls -l
   ```

### Preparando a pasta de saída

Certifique-se de que o diretório de saída no HDFS está vazio ou removido:
```bash
$HADOOP_HOME/bin/hdfs dfs -rm /user/jose-hadoop/output/*
$HADOOP_HOME/bin/hdfs dfs -rmdir /user/jose-hadoop/output
```

### Executando o WordCount com Python

Execute o Hadoop com o WordCount:
```bash
$HADOOP_HOME/bin/hadoop jar hadoop-streaming-3.3.6.jar \
-input /user/jose-hadoop/input/poema02.txt \
-output /user/jose-hadoop/output/ \
-mapper ./mapper.py \
-reducer ./reducer.py
```

### Verificando os resultados

1. Listar o conteúdo do diretório de saída no HDFS:
   ```bash
   $HADOOP_HOME/bin/hdfs dfs -ls /user/jose-hadoop/output/
   ```

2. Visualizar o arquivo de saída:
   ```bash
   $HADOOP_HOME/bin/hdfs dfs -cat /user/jose-hadoop/output/part-00000
   ```

### Copiando o resultado para o sistema local

1. Remova o arquivo local antigo (se existir):
   ```bash
   rm ~/resultados/arq_palavras.txt
   ```

2. Copie o arquivo do HDFS para o sistema local:
   ```bash
   $HADOOP_HOME/bin/hdfs dfs -copyToLocal /user/jose-hadoop/output/part-00000 ~/resultados/arq_palavras.txt
   ```

3. Verifique o conteúdo do arquivo copiado:
   ```bash
   cat ~/resultados/arq_palavras.txt
   ```

---

</br>
</br>
</br>
</br>
</br>

---

## 3. Contador de Palavras no Hadoop com Java

### Inserindo o arquivo de palavras no HDFS

Adicione o arquivo de palavras ao HDFS:
```bash
$HADOOP_HOME/bin/hdfs dfs -put ~/arquivos_hadoop/arqp.txt /user/jose-hadoop/input
```

### Preparando o ambiente Java

1. Copie o arquivo Java com as classes `map` e `reduce`:
   ```bash
   cp ~/scripts/WordCount.java .
   ```

2. Compile as classes Java:
   ```bash
   $HADOOP_HOME/bin/hadoop com.sun.tools.javac.Main WordCount.java
   ```

3. Empacote as classes Java em um arquivo JAR:
   ```bash
   jar cf wc.jar WordCount*.class
   ```

### Preparando a pasta de saída

Certifique-se de que o diretório de saída no HDFS está vazio ou removido:
```bash
$HADOOP_HOME/bin/hdfs dfs -rm /user/jose-hadoop/output/*
$HADOOP_HOME/bin/hdfs dfs -rmdir /user/jose-hadoop/output
```

### Executando o WordCount com Java

Execute o Hadoop com o WordCount:
```bash
$HADOOP_HOME/bin/hadoop jar wc.jar WordCount /user/jose-hadoop/input/arqp.txt /user/jose-hadoop/output
```

### Verificando os resultados

1. Listar o conteúdo do diretório de saída no HDFS:
   ```bash
   $HADOOP_HOME/bin/hdfs dfs -ls /user/jose-hadoop/output/
   ```

2. Visualizar o arquivo de saída:
   ```bash
   $HADOOP_HOME/bin/hdfs dfs -cat /user/jose-hadoop/output/*
   ```

### Copiando o resultado para o sistema local

1. Remova o arquivo local antigo (se existir):
   ```bash
   rm ~/resultados/arq_palavras.txt
   ```

2. Copie o arquivo do HDFS para o sistema local:
   ```bash
   $HADOOP_HOME/bin/hdfs dfs -copyToLocal /user/jose-hadoop/output/part-r-00000 ~/resultados/arq_palavras.txt
   ```

3. Verifique o conteúdo do arquivo copiado:
   ```bash
   cat ~/resultados/arq_palavras.txt
   ```

---

</br>
</br>
</br>
</br>
</br>

## Observações

- Certifique-se de ajustar os caminhos de acordo com o seu ambiente de trabalho.
- Este guia assume que os scripts `mapper.py`, `reducer.py` e `WordCount.java` estão preparados para processar os dados corretamente.


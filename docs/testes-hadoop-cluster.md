# Testes no Cluster Hadoop

Este documento servirá como roteiro para os testes do cluster Hadoop, registrando os resultados e capturas de tela durante a execução.

---

## 1. Preparação do Ambiente

1. Certificar-se de que todas as máquinas estão conectadas ao **switch** e configuradas corretamente.
2. Verificar conectividade entre as máquinas:
   ```bash
   ping master
   ping worker-1
   ping worker-2
   ```
3. Garantir que o SSH está funcionando sem senha entre os nós:
   ```bash
   ssh master
   ssh worker-1
   ssh worker-2
   ```
4. Reiniciar os serviços do Hadoop:
   ```bash
   stop-all.sh && start-all.sh
   ```
5. Verificar os nós conectados:
   ```bash
   hdfs dfsadmin -report
   ```

---

</br>
</br>
</br>

---

## 2. Teste de Execução do WordCount com Python

1. Certificar-se de que os arquivos de entrada estão no HDFS:
   ```bash
   $HADOOP_HOME/bin/hdfs dfs -ls /user/jose-hadoop/input
   ```
2. Executar o processamento WordCount com Python:
   ```bash
   $HADOOP_HOME/bin/hadoop jar hadoop-streaming-3.3.6.jar \
   -input /user/jose-hadoop/input/poema02.txt \
   -output /user/jose-hadoop/output/ \
   -mapper ./mapper.py \
   -reducer ./reducer.py
   ```
3. Verificar os resultados:
   ```bash
   $HADOOP_HOME/bin/hdfs dfs -cat /user/jose-hadoop/output/part-00000
   ```
4. Copiar o resultado para a máquina local:
   ```bash
   $HADOOP_HOME/bin/hdfs dfs -copyToLocal /user/jose-hadoop/output/part-00000 ~/resultados/arq_palavras.txt
   ```
5. Capturar tela do resultado e documentar o tempo de execução.

---

## 3. Teste de Execução do WordCount com Java

1. Compilar e empacotar a aplicação Java:
   ```bash
   $HADOOP_HOME/bin/hadoop com.sun.tools.javac.Main WordCount.java
   jar cf wc.jar WordCount*.class
   ```
2. Executar o WordCount com Java:
   ```bash
   $HADOOP_HOME/bin/hadoop jar wc.jar WordCount /user/jose-hadoop/input/arqp.txt /user/jose-hadoop/output
   ```
3. Verificar os resultados:
   ```bash
   $HADOOP_HOME/bin/hdfs dfs -cat /user/jose-hadoop/output/*
   ```
4. Copiar o resultado para a máquina local:
   ```bash
   $HADOOP_HOME/bin/hdfs dfs -copyToLocal /user/jose-hadoop/output/part-r-00000 ~/resultados/arq_palavras.txt
   ```
5. Capturar tela e anotar tempo de execução.

---

## 4. Testes de Tolerância a Falhas

1. **Remoção de um Worker:**
   - Identificar os nós ativos:
     ```bash
     hdfs dfsadmin -report
     ```
   - Desconectar um worker fisicamente ou desligá-lo.
   - Reexecutar `hdfs dfsadmin -report` e verificar a resposta do cluster.
   - Executar o WordCount novamente e comparar tempos de execução.

2. **Reinserção do Worker:**
   - Reconectar o worker e reiniciar os serviços Hadoop:
     ```bash
     start-all.sh
     ```
   - Verificar se o nó foi reintroduzido corretamente:
     ```bash
     hdfs dfsadmin -report
     ```
   - Executar novamente o WordCount e comparar o tempo com o teste anterior.

---

</br>
</br>
</br>

---

## 5. Registro dos Resultados

- Para cada teste, tirar **prints das execuções** e **documentar tempos**.
- Anotar observações sobre **impacto na performance** e **comportamento do cluster**.



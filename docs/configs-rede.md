# Configuração da Rede para o Cluster Hadoop

Este documento explica como configurar a rede para permitir a comunicação entre o **Master** e os **Workers** no cluster Hadoop, utilizando um **switch simples** e **cabos Ethernet**.

## Diagrama de Rede

A rede foi montada com o host master ao centro e ao seu lado direito e esquerdo os hosts identificados como slave1 e slave2 conectados a um switch

Imagem da Rede montada:

![Cluster](../imgs/cluster.jpg)

## 1. Equipamentos Utilizados

- **Switch 8 Portas Fast Ethernet SF 800** ([Link do produto](https://www.amazon.com.br/dp/B075JQJCPH))
- Cabos Ethernet para conexão das máquinas
- Máquinas configuradas com **Ubuntu 24.04**

Imagem do switch utilizado:

![Switch](../imgs/switch.jpeg)

---

</br>
</br>
</br>
</br>
</br>

---

## 2. Configuração da Rede

### **2.1. Conectando as Máquinas**

1. Conecte todas as máquinas ao **switch** usando **cabos Ethernet**.
2. Verifique se as conexões foram reconhecidas corretamente com:
   ```bash
   ip a
   ```

### **2.2. Definindo IPs Estáticos**

Cada máquina do cluster receberá um IP fixo na mesma sub-rede. Por exemplo:

| Máquina  | Função  | IP                |
|----------|--------|------------------|
| master   | Master | 192.168.0.10     |
| worker-1 | Worker | 192.168.0.11     |
| worker-2 | Worker | 192.168.0.12     |
| worker-3 | Worker | 192.168.0.13     |

#### **Configurando IPs via Interface do Ubuntu**

1. Conecte-se ao switch e abra as **Configurações de Rede** no Ubuntu.
2. Selecione a conexão cabeada (geralmente chamada **Wired Connection**).
3. Clique em **Configurações** e vá para a aba **IPv4**.
4. Escolha **Manual** e defina:
   - **Endereço IP** de acordo com a tabela acima.
   - **Máscara de rede**: `255.255.255.0`
   - **Gateway**: `192.168.0.1`
   - **DNS**: `8.8.8.8, 8.8.4.4`
5. Salve e reinicie a conexão.
6. Confirme a nova configuração de IP:
   ```bash
   ip a
   ```

### **2.3. Configurando SSH para Comunicação entre Máquinas**

O SSH será utilizado para permitir a comunicação entre as máquinas do cluster.

1. **Gerar chave SSH (em cada máquina):**
   ```bash
   ssh-keygen -t rsa -b 4096
   ```
   - Pressione **Enter** em todas as opções (não defina senha).

2. **Copiar a chave pública para o Master (apenas nas Workers):**
   ```bash
   ssh-copy-id usuario@192.168.0.10
   ```
   > **Substitua `usuario` pelo nome de usuário da máquina Master.**

3. **Verifique se o SSH está funcionando sem senha:**
   ```bash
   ssh usuario@192.168.0.10
   ```

### **2.4. Editando o Arquivo /etc/hosts**

Cada máquina **Worker** só precisa conhecer o **Master**. Já no **Master**, é necessário conhecer todas as Workers.

#### **Em cada Worker (`worker-1`, `worker-2`, etc.)**
1. Abra o arquivo `/etc/hosts`:
   ```bash
   sudo nano /etc/hosts
   ```
2. Adicione apenas a entrada do **Master**:
   ```plaintext
   192.168.0.10 master
   ```
3. Salve e saia (`CTRL+X`, `Y`, `ENTER`).

#### **No Master (`master`)**
1. Abra o arquivo `/etc/hosts`:
   ```bash
   sudo nano /etc/hosts
   ```
2. Adicione todas as Workers:
   ```plaintext
   192.168.0.11 worker-1
   192.168.0.12 worker-2
   192.168.0.13 worker-3
   ```
3. Salve e saia (`CTRL+X`, `Y`, `ENTER`).

4. Teste a conectividade:
   ```bash
   ping worker-1
   ping master
   ```

---

</br>
</br>
</br>
</br>
</br>

---

## 3. Configuração no Hadoop

Para que todas as máquinas reconheçam o **Master**, devemos editar o **core-site.xml**.

### **3.1. Editando o Arquivo core-site.xml**

1. Abra o arquivo de configuração do Hadoop em **todas as máquinas**:
   ```bash
   nano $HADOOP_HOME/etc/hadoop/core-site.xml
   ```

2. Substitua `localhost` pelo IP do **Master** (`192.168.0.10`):
   ```xml
   <configuration>
       <property>
           <name>fs.defaultFS</name>
           <value>hdfs://192.168.0.10:9000</value>
       </property>
   </configuration>
   ```

3. Salve (`CTRL+X`, `Y`, `ENTER`).

4. Reinicie os serviços do Hadoop:
   ```bash
   stop-all.sh && start-all.sh
   ```

5. Teste a conexão:
   ```bash
   hdfs dfsadmin -report
   ```
   O relatório deve exibir **todos os nós conectados**.

---

## 4. Conclusão

Com essas configurações, o cluster estará pronto para executar tarefas distribuídas no Hadoop. Certifique-se de que todas as máquinas estão conectadas corretamente e que o SSH funciona sem senha entre elas.

Se houver problemas, verifique:
- Conexão física (cabos e switch);
- Configuração de IPs (`ip a`);
- Firewall (`sudo ufw disable` para desativar temporariamente);
- Logs do Hadoop (`$HADOOP_HOME/logs`).


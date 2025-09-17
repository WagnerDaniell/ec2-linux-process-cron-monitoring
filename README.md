# 🖥️ Laboratório AWS: EC2 + Processos, Monitoramento e Cron Jobs

Este repositório documenta um **laboratório prático** realizado em uma instância **Amazon EC2 (Linux)**.  
O objetivo foi:

- Listar **processos em execução** e gerar logs.  
- Monitorar **uso de CPU e memória** com o comando `top`.  
- Criar e validar um **cron job** para auditoria de arquivos `.csv`.

---

## 📌 Resumo do que foi feito

- 📝 Criação do log `processes.csv` filtrando processos do root.  
- 📊 Observação de processos e desempenho com `top` e `top -hv`.  
- ⏲️ Criação de um **cron job** que gera `filteredAudit.csv`, mascarando nomes de arquivos `.csv`.  
- ✅ Todas as saídas foram validadas na pasta **SharedFolders**.

---

## 🧩 Passo a passo (com imagens)

### 1️⃣ Verificar diretório inicial
- Confirmei que estava na pasta do lab:  
  - `pwd`  
  - Saída: `/home/ec2-user/companyA`  
- Se necessário, entrei na pasta: `cd companyA`

---

### 2️⃣ Criar log de processos
- Rodei o comando para listar todos os processos, exceto os do root, e salvei em `SharedFolders/processes.csv`:  
  - `sudo ps -aux | grep -v root | sudo tee SharedFolders/processes.csv`  
- Conferi o conteúdo do arquivo:  
  - `cat SharedFolders/processes.csv`

![Arquivo processes.csv criado](images/processes-csv.jpg)

> O arquivo `processes.csv` contém todos os processos ativos, filtrando os processos do root.

---

### 3️⃣ Monitorar processos com top
- Rodei o comando `top` para observar tarefas, uso de CPU e memória.  
- Saída mostrava tarefas em **running, sleeping, stopped ou zombie**, além de uso de memória e CPU.  
- Para sair do top, pressionei **q**.  
- Também utilizei `top -hv` para ver opções e informações detalhadas.

![Saída do comando top mostrando tarefas e uso de CPU/memória](images/top-output.jpg)

---

### 4️⃣ Criar Cron Job para auditoria
- Editar crontab como root: `sudo crontab -e`  
- No editor inseri as linhas:  
  - `SHELL=/bin/bash`  
  - `PATH=/usr/bin:/bin:/usr/local/bin`  
  - `MAILTO=root`  
  - `0 * * * * ls -la $(find .) | sed -e 's/..csv/#####.csv/g' > /home/ec2-user/companyA/SharedFolders/filteredAudit.csv`  
- Salvei e fechei o editor (**ESC → :wq → ENTER**)  
- Validei a criação do cron job: `sudo crontab -l`

![Criação do cron job](images/cron.jpg)  
![Validação do cron job com crontab -l](images/installed-cron.jpg)

> O cron job roda a cada hora e gera o arquivo `filteredAudit.csv` com os nomes de todos os arquivos `.csv` mascarados.

---

## 🛠️ Comandos principais usados
- `pwd`  
- `cd companyA`  
- `sudo ps -aux | grep -v root | sudo tee SharedFolders/processes.csv`  
- `cat SharedFolders/processes.csv`  
- `top`  
- `top -hv`  
- `sudo crontab -e`  
- `sudo crontab -l`

---

## ✅ Observações finais
Este laboratório demonstra:

- Como **listar e filtrar processos** no Linux.  
- Monitoramento de **desempenho do sistema** em tempo real.  
- Criação e validação de **tarefas agendadas com cron** para auditoria.  

Todas as saídas e arquivos foram salvos e validados dentro da pasta **SharedFolders**.

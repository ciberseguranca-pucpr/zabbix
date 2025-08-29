# Monitoramento de rede e ativos com Zabbix

*Network Defense Essentials I*  

---

## Sumário
- [Objetivo](#objetivo)  
- [Especificação](#especificação)  
  - [Preparação do Ambiente](#preparação-do-ambiente)  
  - [Relatório](#relatório)  
- [Apêndice A - Configuração Agente Zabbix usando Docker](#apêndice-a--configuração-agente-zabbix-usando-docker)  
- [Apêndice B - Configuração Agente Zabbix em Máquina Nativa](#apêndice-b--configuração-agente-zabbix-em-máquina-nativa)  

---

## Objetivo
Nesta atividade vamos explorar o processo de monitoramento da rede e seus ativos.  

Para monitorar os vários parâmetros da rede, servidores e a saúde dos serviços e ativos de rede utilizaremos a ferramenta **Zabbix v7.4.2**.  

Este documento contém dois apêndices:  
- [Apêndice A](#-apêndice-a--configuração-agente-zabbix-usando-docker) — Agente via Docker  
- [Apêndice B](#-apêndice-b--configuração-agente-zabbix-em-máquina-nativa) — Agente nativo no Ubuntu  

> O repositório está baseado no **Apêndice A**.

---

### Preparação do Ambiente
Nesta seção configuraremos o **Zabbix usando Docker**.  

🔧 Ferramentas necessárias (caso não use a VM do AVA):  
- `docker`  
- `docker compose`  
- `git`  

#### Passo a passo
1. Clone o repositório:
   ```bash
   git clone https://github.com/ciberseguranca-pucpr/zabbix.git
   ```

2. Acesse o diretório:
   ```bash
   cd zabbix/
   ```

3. Construa as imagens:
   ```bash
   docker-compose build
   ```

4. Inicialize o Zabbix:
   ```bash
   docker-compose up
   ```

5. Acesse no navegador:
   ```
   http://localhost:8080
   ```

6. Autentique-se com:  
   - Usuário: `Admin`  
   - Senha: `zabbix`  

7. Reconfigure o agente do servidor:  
   - Vá em **Monitoring → Hosts → Zabbix server → Host**  
   - Altere o **Host name** para `zabbix-agent`  
   - Configure interface como **DNS = zabbix-agent**  
   - Marque **Connect to = DNS**  
   - Clique em **Update**  

8. Em poucos segundos, o ícone **ZBX** ficará verde.

---

## Apêndice A — Configuração Agente Zabbix usando Docker
1. Baixe a imagem:  
   ```bash
   docker pull zabbix/zabbix-agent
   ```

2. Execute o contêiner:  
   ```bash
   docker run -d --name agente --network zabbix_zabbix-network --hostname nde1 --ip 172.30.0.30 zabbix/zabbix-agent
   ```

---

## Apêndice B — Configuração Agente Zabbix em Máquina Nativa
Sistema: **Ubuntu 24.04 (arm64)**  
Compatível com **Debian** e **x86-64**.  

### Passo a passo
1. Abra o terminal.  

2. Baixe o binário:
   ```bash
   wget https://repo.zabbix.com/zabbix/7.4/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.4+ubuntu24.04_all.deb
   ```

3. Instale o pacote:
   ```bash
   sudo dpkg -i zabbix-release_latest_7.4+ubuntu24.04_all.deb
   ```

4. Atualize pacotes:
   ```bash
   sudo apt update
   ```

5. Instale o agente:
   ```bash
   sudo apt install zabbix-agent
   ```

6. Habilite o serviço:
   ```bash
   sudo systemctl enable zabbix-agent
   ```

7. Edite o arquivo `/etc/zabbix/zabbix_agentd.conf`:  
   ```ini
   Server=SEU_IPV4_SERVIDOR_ZABBIX
   ServerActive=SEU_IPV4_SERVIDOR_ZABBIX
   Hostname=SEU_HOSTNAME
   ListenPort=10051
   ```

   - Para obter o IPv4 (servidor):  
     ```bash
     hostname -I
     ```
   - Para obter o hostname (agente):  
     ```bash
     hostname
     ```

8. Reinicie o serviço:
   ```bash
   sudo systemctl restart zabbix-agent
   ```

9. Verifique o status:
   ```bash
   sudo systemctl status zabbix-agent
   ```

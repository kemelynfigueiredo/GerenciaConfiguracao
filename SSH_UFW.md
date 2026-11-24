# Configuração de SSH e Firewall (UFW)

## 1. Objetivo

Configurar o acesso remoto via SSH e o firewall UFW no Ubuntu 24.04 utilizando Docker.

## 2. Criar o container

Com o Docker instalado e em execução, execute no terminal do computador:

```bash
docker run -it --name ubuntu-ssh --cap-add=NET_ADMIN -p 127.0.0.1:2222:22 ubuntu:24.04 bash
```

O comando cria um container Ubuntu, permite configurar o firewall e disponibiliza a porta SSH 22 na porta 2222 do computador, apenas localmente.

## 3. Instalar SSH e UFW

Dentro do container, execute:

```bash
apt update
apt install -y openssh-server ufw
```

## 4. Configurar o SSH

Crie um usuário e defina uma senha quando solicitado:

```bash
adduser aluno
```

Inicie o serviço SSH:

```bash
service ssh start
```

## 5. Configurar o firewall

```bash
ufw default deny incoming
ufw default allow outgoing
ufw allow 22/tcp
ufw --force enable
ufw status
```

Esses comandos bloqueiam conexões de entrada por padrão, permitem conexões de saída, liberam o SSH e ativam o firewall. O resultado esperado é `Status: active`, com a porta 22 liberada.

## 6. Testar o acesso remoto

Mantenha o container aberto e, em outro terminal do computador, execute:

```bash
ssh aluno@localhost -p 2222
```

Na primeira conexão, confirme com `yes` após conferir a identidade do servidor e informe a senha criada anteriormente. Se a conexão funcionar, o terminal estará conectado ao Ubuntu via SSH.

## 7. Conclusão

Com esses passos, o Ubuntu fica configurado para receber conexões SSH e controlar o tráfego de rede por meio do UFW.

Este é um laboratório local. O UFW dentro do container não substitui o firewall do computador hospedeiro. Em produção, recomenda-se autenticação por chave SSH.

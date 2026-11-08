# Simulacao-de-Brute-Force-Medusa-Kali-Linux
Simulando um Ataque de Brute Force de Senhas com Medusa e Kali Linux

DIO

1- Introdução

Este projeto tem como objetivo estudar e demonstrar, em um ambiente controlado, como funcionam os ataques de força bruta contra diferentes serviços e aplicações.
Para realizar as simulações, são utilizadas ferramentas de segurança presentes no Kali Linux, com destaque para o Medusa, além de ambientes vulneráveis preparados para fins educacionais.
O projeto também busca compreender os riscos relacionados ao uso de senhas fracas e a importância de implementar medidas de proteção contra tentativas não autorizadas de acesso.

2- Ambiente utilizado

Sistema operacional: Kali Linux;
<br>Virtualização: Oracle VirtualBox;
<br>Máquina vulnerável: Metasploitable 2;
<br>Aplicação WEB: DVWA;
<br>Ferramenta de auditoria: Medusa, Nmap;
<br>Tipo de rede: Host-Only;


<img width="1920" height="1025" alt="Captura de tela de 2026-11-02 19-35-37" src="https://github.com/user-attachments/assets/4a1d2188-1c76-46ad-9bb2-35d95456fbdb" />


------------------------------------------------------------------------------------------------------------
Primeiro cenário - Ataque de força bruta em FTP

Objetivo:
Entender como funciona um ataque brute-force contra o serviço em questão, utilizando uma ferramente de auditoria para testar uma combinação de usuários e senhas criadas durante o curso DIO.ME

Ferramenta Utilizada:
Foi utilizada durante o teste a ferramenta medusa com auxilio no nmap.

Procedimento:
Foi utilizado o Kali para realizar tentativas de autenticação contra o serviço FTP disponibilizado no ambiente vulnerável.
Após a auditoria de portas, verificação completa com nmap, foi utilizado duas listas, uma contendo senhas e outra usuários para testar diversas combinações, tendo assim o êxito na conclusão do teste.

comandos utilizados:
<br>'nmap -sS -sV 192.168.52.106 ' - Para fazer auditoria de todas as portas abertas no ip vulnerável juntamente com suas versões.
<br>'medusa -h IP -U users.txt -P pass.txt -M ftp -t 6' - Para fazer o processo de brute-force utilizando as wordlists.

Resultados:
Como esperado, obtivemos conhecimento do usuário e da senha, necessária para ter acesso ao serviço FTP do alvo.

<img width="794" height="602" alt="Captura de tela de 2026-11-02 19-51-38" src="https://github.com/user-attachments/assets/166dc7db-4b67-46d8-93cf-fd953fd35a16" />
<img width="806" height="392" alt="Captura de tela de 2026-11-02 19-49-37" src="https://github.com/user-attachments/assets/6a2c2027-2f0b-4faa-90ce-549f4ebf74c5" />


Analise:
O serviço pode apresentar risco se mal configurado, se utilizado com senhas fracas, podendo ser substituido por uma versão mais segura (SFTP) quando aplicável

Medidas de prevenção:
Senhas fortes e exclusivas.

-----------------------------------------------------------------------------------------------------------

Segundo cenário - Ataque de força bruta em formulário Web (DVWA)

Objetivo:
Entender como ataques automatizados em formulários web com pouca segurança podem ser um problema para a empresa, colocando em cheque possíveis informações sensíveis.

Ambiente utilizado:
DVWA (Damn Vulnerable Web Application), uma aplicação desenvolvida para estudos de cybersegurança.

Procedimento:
Foi utilizado um formulário de autenticação do DVWA para simular tentativas automatizadas de login.
Esse teste permitiu observar como as ferramentas podem automatizar o envio de credenciais e identificar possíveis falhas na aplicação em questão.

Comandos utilizados:
<br>"medusa -h 192.168.56.102 -U users.txt -P pass.txt -M http \ -m PAGE:'/dvwa/login.php' \ -m FORM:'username=^USER^&password=^PASS^&Login=Login' \ -m 'FAIL=Login failed' -t 6 -v 4"

Resultado:
Após a execução do comando o Medusa iniciou o ataque, descobrindo as credenciais poucos segundos após o inicio.

<img width="791" height="686" alt="Captura de tela de 2026-11-02 20-18-25" src="https://github.com/user-attachments/assets/f3d2b8d0-5a0c-4336-96b1-f41e8257fba8" />


Análise:
Formulários sem mecanismos adequedos podem permitir que atacantes realizem várias tentativas de autenticação sem dificuldades, pondo em risco a empresa e seus usuários.

Medidas de prevenção:
Limitar as tentativas de login, adotar a politica de senhar fortes, verificação de dois fatores, monitorar as tentativas repetidas de autenticação com sistemas.

-------------------------------------------------------------------------------------------------------------
Terceiro cenário - Password Spraying em SMB

Objetivo:
Foi utilizada a técnica de Password Spraying para obter acesso ao sistema do smbclient

Conceito da técnica:
Password spraying é uma tecnica que consiste em testar a mesma senha contrá vários usuários do ip alvo.

Procedimento:
Ao iniciar o comando enum4linux, obtemos uma enumeração automatizada do alvo, fazendo uma coleta de usuários.
Após a enumeração, iniciamos o ataque utilizando o Medusa, obtendo as credenciais necessárias para realizar a autenticação através do smbclient.

<img width="802" height="37" alt="Captura de tela de 2026-11-03 20-03-00" src="https://github.com/user-attachments/assets/358ac724-9b8f-43a3-a585-d4511b342039" />

Comandos utilizados:<br>
<br>'enum4linux -a 192.168.56.102 | tee enum4_output.txt' - Realizar a enumeração e documentar no txt mencionado.
<br>'less enum4_output.txt' - Abrir o txt documentado e realizar a leitura.
<br>'grep -oP "user:[\K[^\]]+" enum4_output.txt > smb_users.txt' - Na aula não é ensinado este comando, mas surgiu uma duvida de automatizar a coleta de users e eu encontrei este comando, achei interessante em compartilhar este conhecimento.
<br>'medusa -h 192.168.56.102 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50'
<br>'smbclient -L //192.168.56.102 -U msfadmin'


Resultado obtido:
<br>Após salvar os usuarios, obtivemos uma lista completa de possíveis alvos para realizar a descoberta da senha, tento sucesso, conseguimos realizar a autenticação.

<img width="806" height="344" alt="Captura de tela de 2026-11-03 20-09-40" src="https://github.com/user-attachments/assets/3b16d9b6-7304-41a4-95f8-7b0403360e9f" />

Medidas de prevenção:
<br><br>Utilização de senhas fortes; 
<br>Monitoramento de autenticaçõe; 
<br>Politicas de bloqueio e proteção de conta; 
<br>Bloqueios de Ip's após várias tentativas de login;
<br>Firewall.

------------------------------------------------------------------------------------------------------------------

Conclusão:
Ao realizarmos esses testes, podemos compreender na prática como os ataques de força bruta e password spraying podem comprometer serviços e aplicações que não possuem mecanismos adequados de proteção.

As simulações também mostram a importancia de políticas de senhas, monitoramento de autenticação, limitação de tentativas e medidas adicionais de segurança.

O projeto contribuiu para ampliar meus conhecimentos sobre ferramentas de auditoria, vulnerabilidades de autenticação e medidas de prevenção em segurança da informação.

!!!AVISO!!!
<br>Todas as atividades deste projeto possuem finalidades (EXCLUSIVAMENTE) educacionais e são realizadas em ambiente de laboratório controlado.
<br>(Nenhum teste deve ser executado contra sistemas, serviços ou contas sem autorização!)

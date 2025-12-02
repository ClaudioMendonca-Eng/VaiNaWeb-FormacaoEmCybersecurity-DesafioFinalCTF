## 🛠️ Introdução



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

---

# VaiNaWeb-FormacaoEmCybersecurity-DesafioFinalCTF

## Relatório de Pentest - Desafio Final CTF

### Sumário Executivo

Este relatório documenta os resultados do teste de penetração realizado na aplicação web TechCorp Solutions (http://98.95.207.28) como parte do Desafio Final CTF da Formação em Cibersegurança VaiNaWeb. Durante o assessment, foram identificadas 17 vulnerabilidades críticas e de alta severidade que comprometem significativamente a postura de segurança da organização. As descobertas incluem credenciais hardcoded em múltiplos arquivos, falhas de controle de acesso permitindo acesso não autorizado a recursos sensíveis, SQL Injection que viabiliza exfiltração completa do banco de dados, Cross-Site Scripting (XSS) facilitando ataques de roubo de sessão, e vazamento massivo de informações através de arquivos expostos publicamente. A infraestrutura apresenta stack LAMP (Linux Ubuntu 20.04.6 LTS, Apache 2.4.54/2.4.65, MySQL 8.0.44, PHP 7.4.33) com múltiplas vulnerabilidades de configuração que, combinadas, permitem comprometimento total do ambiente. Este relatório detalha cada vulnerabilidade explorada com evidências técnicas, análise de impacto baseada em CVSS v3.1, mapeamento para frameworks de segurança (OWASP Top 10 2021, CWE, PCI DSS, ISO 27001), e recomendações priorizadas de remediação. A análise SWOT complementar fornece visão estratégica identificando 6 forças organizacionais, 10 fraquezas críticas, 8 oportunidades de melhoria e 10 ameaças externas, culminando em roadmap de implementação estruturado em três fases (0-30, 30-90, 90-180 dias) para transformação da postura de segurança de reativa para proativa.

### Objetivo e Escopo

O objetivo principal deste pentest foi avaliar a segurança da aplicação web TechCorp Solutions através de metodologia estruturada de Capture The Flag (CTF), identificando vulnerabilidades exploráveis, capturando flags como prova de conceito, e documentando riscos para subsidiar decisões de remediação. O escopo técnico englobou testes de segurança em camadas de aplicação web (frontend/backend), banco de dados MySQL, serviços de rede (HTTP:80, FTP:21, SSH:2222, MySQL:3306, HTTP-alt:8080), sistema operacional Linux, e análise de configurações de infraestrutura. Foram executados testes manuais e automatizados cobrindo categorias do OWASP Top 10 2021, incluindo Broken Access Control, Cryptographic Failures, Injection, Insecure Design, Security Misconfiguration, Vulnerable Components, Identification and Authentication Failures, Software and Data Integrity Failures, Security Logging and Monitoring Failures, e Server-Side Request Forgery.

O escopo do assessment foi limitado ao ambiente de teste em http://98.95.207.28 durante período autorizado entre 21 de novembro e 30 de novembro de 2025, sem impactar ambientes de produção ou dados reais de clientes. Foram excluídos do escopo testes de negação de serviço (DoS/DDoS), engenharia social contra funcionários reais, e acesso físico às instalações. As técnicas empregadas seguiram modelo de "grey-box testing" onde informações básicas sobre a infraestrutura foram fornecidas (URLs, portas, credenciais iniciais via FTP anônimo), simulando cenário realista de atacante com reconhecimento prévio limitado. Os 17 objetivos (flags) capturados representam diferentes vetores de ataque e vulnerabilidades comuns em aplicações web modernas, proporcionando avaliação holística da superfície de ataque.

### Metodologia

A metodologia adotada seguiu abordagem sistemática baseada no framework PTES (Penetration Testing Execution Standard) adaptado para contexto CTF, estruturada em cinco fases sequenciais: (1) **Reconhecimento e Enumeração** utilizando ferramentas como Nmap para port scanning, Nikto para web vulnerability scanning, curl/wget para análise de conteúdo HTTP, e técnicas manuais de inspeção de código-fonte HTML e arquivos públicos (robots.txt, .git, .env); (2) **Análise de Vulnerabilidades** envolvendo identificação de falhas de configuração, credenciais expostas, injeções SQL/XSS, broken access control, e information disclosure através de análise manual de código PHP, testes de input validation, e fuzzing de parâmetros; (3) **Exploração** executando provas de conceito para cada vulnerabilidade identificada, capturando flags como validação de exploração bem-sucedida, escalando privilégios quando possível (ex: user techcorp → root via sudo), e documentando payloads utilizados; (4) **Pós-Exploração** realizando enumeração de sistema comprometido, análise de arquivos sensíveis (.bash_history, backup scripts, git credentials), e mapeamento de vetores de persistência; (5) **Documentação e Análise** compilando evidências técnicas (screenshots, outputs de comandos, código explorado), calculando scores CVSS v3.1 para cada vulnerabilidade, mapeando para frameworks de compliance (PCI DSS, ISO 27001, NIST SP 800-53, LGPD), e gerando recomendações priorizadas de remediação com análise estratégica SWOT. Todas as atividades foram realizadas em ambiente controlado de laboratório CTF, garantindo testes éticos e sem impacto em sistemas de produção.

### Flag 1: HTML Source Code Inspection

#### Conceito: Inspeção do Código Fonte HTML

A inspeção do código fonte HTML é uma técnica fundamental em testes de segurança e pentest que consiste em examinar o código HTML de uma página web para identificar informações sensíveis, comentários de desenvolvedores, credenciais expostas, endpoints ocultos ou outras vulnerabilidades. Muitas vezes, desenvolvedores deixam comentários no código fonte durante o processo de desenvolvimento contendo informações que não deveriam estar disponíveis publicamente, como flags de CTF, notas sobre funcionalidades, ou até mesmo senhas e tokens de API. Esta técnica é considerada uma das mais básicas em reconhecimento passivo, pois não requer interação agressiva com o servidor - apenas a visualização do código que é enviado naturalmente ao navegador. Ferramentas de desenvolvedor integradas nos navegadores modernos (como Chrome DevTools, Firefox Developer Tools) ou ferramentas de linha de comando (como curl, wget) permitem essa inspeção de forma simples e eficaz.



# 🔐 Cyber Security – Desafio TSTI (UniFECAF)

> **Fortalecimento da Segurança Digital: Análise de Malware, Monitoramento de Tráfego de Rede e Varredura de Vulnerabilidades**

[![Kali Linux](https://img.shields.io/badge/OS-Kali%20Linux-557C94?style=flat-square&logo=kalilinux&logoColor=white)](https://www.kali.org)
[![Docker](https://img.shields.io/badge/Infra-Docker-2496ED?style=flat-square&logo=docker&logoColor=white)](https://www.docker.com)
[![Nmap](https://img.shields.io/badge/Tool-Nmap%207.98-4CAF50?style=flat-square)](https://nmap.org)
[![Wireshark](https://img.shields.io/badge/Tool-Wireshark-1679A7?style=flat-square&logo=wireshark&logoColor=white)](https://www.wireshark.org)
[![VirusTotal](https://img.shields.io/badge/Tool-VirusTotal-394EFF?style=flat-square)](https://www.virustotal.com)
[![License](https://img.shields.io/badge/License-Academic-orange?style=flat-square)](LICENSE)

---

## 📋 Sobre o Projeto

Este repositório documenta o projeto de conclusão da disciplina **Cyber Security** do **Centro Universitário UniFECAF**, desenvolvido pelo aluno **Eduardo Souza Mattos (R.A. 35984)**.

O desafio consiste em desenvolver um plano de segurança digital para o **Taboão da Serra Tech Institute (TSTI)** — instituição fictícia que, em expansão, enfrenta ameaças reais de malware, tráfego anômalo e portas vulneráveis. A solução técnica foi construída sobre três ferramentas pilares de análise cibernética, com ambiente laboratorial real executado em Kali Linux.

**Nota obtida:** `7,6 / 8,0` ⭐

---

## 🎯 Objetivo

Aplicar ferramentas reais de segurança da informação para:

- Detectar e analisar **malware** via triagem automatizada (VirusTotal)
- Monitorar e interpretar **tráfego de rede** em tempo real (Wireshark)
- Mapear **vulnerabilidades** e superfície de exposição (Nmap)
- Documentar evidências técnicas e propor planos de mitigação alinhados ao **NIST CSF**, **ISO/IEC 27001:2022** e **LGPD**

---

## 🏗️ Ambiente de Laboratório

| Componente | Especificação |
|---|---|
| **Hardware** | Notebook Samsung Expert 2018 |
| **RAM** | 12 GB DDR4 |
| **Armazenamento** | SSD 240 GB (externo via USB 3) |
| **Sistema Operacional** | Kali Linux (rolling) — `esm@kali-esm` |
| **Virtualização** | Docker Engine |
| **Alvo** | DVWA – Damn Vulnerable Web Application |
| **Servidor detectado** | Apache httpd 2.4.25 (Debian) |
| **Nível de segurança DVWA** | Low (máxima exposição para fins didáticos) |

> ⚠️ **Todos os testes foram realizados exclusivamente em ambiente controlado e autorizado. A replicação em sistemas de terceiros sem autorização configura crime nos termos da Lei n. 12.737/2012 (Lei Carolina Dieckmann).**

---

## 🗂️ Estrutura do Repositório

```
cyber-security-tsti-unifecaf/
│
├── README.md                          ← Este arquivo
│
├── relatorios/
│   ├── Relatorio_Tecnico_Teorico.pdf  ← Parte Teórica (25% da nota)
│   └── Relatorio_Tecnico_Pratico.pdf  ← Parte Prática (50% da nota)
│
├── evidencias/
│   ├── fig01_docker_install.jpg       ← Instalação do Docker no Kali
│   ├── fig02_virustotal_submissao.jpg ← Submissão da URL ao VirusTotal
│   ├── fig03_virustotal_resultado.jpg ← 3/90 detecções (Antiy-AVL, Cyble, CMC)
│   ├── fig04_wireshark_init.jpg       ← Inicialização do Wireshark
│   ├── fig05_wireshark_pacote99.jpg   ← Pacote POST n.99 (841 bytes)
│   ├── fig06_wireshark_payload.jpg    ← Payload XSS decodificado
│   ├── fig07_dvwa_xss_confirmado.jpg  ← alert('XSS-UniFECAF') executado
│   ├── fig08_nmap_docker_ps.jpg       ← docker ps + nmap -sV
│   └── fig09_nmap_resultado.jpg       ← Apache 2.4.25 na porta 80
│
└── video/
    └── link_youtube.txt               ← Link do vídeo pitch (4 min)
```

---

## 🛠️ Ferramentas Utilizadas

### 🔍 VirusTotal
Plataforma de aggregação de mais de 90 motores antivírus para análise de arquivos e URLs.

**Resultado no laboratório TSTI:**
- URL analisada: `http://127.0.0.1/vulnerabilities/xss_s/`
- **3/90 detecções**: Antiy-AVL (Malicious), Cyble (Malicious), CMC Threat Intelligence (Malware)
- 87 motores retornaram Clean (esperado para URL local)

| Pontos Fortes | Limitações |
|---|---|
| Inteligência coletiva global (90+ motores) | Arquivos privados tornam-se públicos — risco de vazamento de P&D |
| Análise rápida sem sandbox local | Pode falhar em ataques Zero-Day customizados |
| Histórico de análises e reputação de domínios | Não substitui antivírus local (Endpoint Protection) |

---

### 📡 Wireshark
Analisador de protocolos de rede open-source com suporte a mais de 3.000 protocolos.

**Resultado no laboratório TSTI:**
- Interface de captura: `lo` (loopback)
- Filtro aplicado: `http`
- **Pacote capturado:** POST n.99 — `/vulnerabilities/xss_s/ HTTP/1.1` (841 bytes)
- **Payload decodificado:** `mtxMessage = <script>alert('XSS-UniFECAF')</script>`
- **Confirmação:** XSS Stored persistente — alerta exibido ao recarregar a página

```
Camada 7 (Aplicação): HTTP/1.1 — payload XSS visível em texto claro
Camada 4 (Transporte): TCP — rastreio de sessão e sequência
Camada 3 (Rede):       IP 127.0.0.1 — host local (loopback)
Camada 2 (Enlace):     Ethernet (lo) — interface de captura
```

| Pontos Fortes | Limitações |
|---|---|
| Inspeção profunda (camadas 2 a 7 do modelo OSI) | Curva de aprendizado íngreme |
| Identificação de payloads maliciosos em texto claro | Ineficaz para tráfego TLS/SSL sem chaves de decodificação |
| Rastreamento pacote a pacote em tempo real | Não bloqueia ameaças — apenas analisa (sniffer, não IPS) |

---

### 🗺️ Nmap 7.98
Ferramenta padrão para descoberta de hosts, mapeamento de portas e fingerprinting de serviços.

**Resultado no laboratório TSTI:**
```bash
$ nmap -sV -p 80 localhost

Starting Nmap 7.98 at 2026-06-07 18:30 -0300
Host is up (0.000050s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.25 ((Debian))

Nmap done: 1 IP address (1 host up) scanned in 6.33 seconds
```

| Resultado | Vulnerabilidade | Criticidade | Mitigação |
|---|---|---|---|
| Apache httpd 2.4.25 | Versão 2017 com CVEs públicos | 🔴 CRÍTICO | Atualizar imediatamente |
| Porta 80/tcp open | HTTP sem criptografia | 🔴 CRÍTICO | HTTPS + TLS 1.3 |
| Banner exposto | Info do servidor visível | 🟡 ALTO | `ServerTokens Min` |
| Porta 3306 mapeada | MySQL exposto | 🔴 CRÍTICO | Remover mapeamento 3306 |

| Pontos Fortes | Limitações |
|---|---|
| Mapeamento de portas extremamente eficiente | Varreduras agressivas podem acionar IDS/IPS |
| Fingerprinting de SO e versões de serviços | Não corrige vulnerabilidades — apenas identifica |
| Scripts NSE para testes específicos (ex: XSS, SQLi) | Pode causar instabilidade em dispositivos legados |

---

## 📊 Quadro Consolidado de Vulnerabilidades

| Ferramenta | Vulnerabilidade | Criticidade | Evidência | Mitigação Principal |
|---|---|---|---|---|
| VirusTotal | URL DVWA detectada (3/90) | 🟡 ALTO | Fig. 2 e 3 | Integrar VirusTotal API ao WAF |
| VirusTotal | Ausência de antivírus nos uploads | 🟡 ALTO | Fig. 2 e 3 | ClamAV / VirusTotal API |
| Wireshark | XSS Stored — script persistente no BD | 🔴 CRÍTICO | Fig. 5, 6 e 7 | Input sanitization + CSP |
| Wireshark | Payload em texto claro via HTTP | 🔴 CRÍTICO | Fig. 5 e 6 | HTTPS com TLS 1.3 |
| Wireshark | Cabeçalhos HTTP ausentes | 🟡 ALTO | Fig. 5 | HSTS, CSP, X-Frame-Options |
| Nmap | Apache httpd 2.4.25 obsoleto | 🔴 CRÍTICO | Fig. 9 | Atualizar Apache |
| Nmap | Porta 80 sem TLS | 🔴 CRÍTICO | Fig. 9 | TLS; redirecionamento HTTP→HTTPS |
| Nmap | Porta 3306 (MySQL) exposta | 🔴 CRÍTICO | Fig. 9 | Remover mapeamento 3306 |

---

## 🔒 Frameworks e Conformidade

| Framework / Norma | Aplicação no Projeto |
|---|---|
| **NIST CSF** | Identify (Nmap) · Protect (controles) · Detect (Wireshark) · Respond (planos) · Recover |
| **ISO/IEC 27001:2022** | A.8.2 Criptografia · A.8.8 Gestão de patches · A.8.23 Filtragem Web · A.8.24 Sanitização |
| **LGPD (Lei 13.709/2018)** | Proteção de dados pessoais · Princípio da Segurança · Notificação de incidentes |
| **Cyber Kill Chain** | Reconhecimento (Nmap) · Exploração (XSS) · Persistência (XSS Stored) |
| **Lei Carolina Dieckmann** | Testes autorizados em ambiente controlado — conformidade explicitada |

---

## 🚀 Como Reproduzir o Ambiente

> Pré-requisito: Kali Linux instalado. Todos os comandos devem ser executados **somente em ambiente autorizado**.

### 1. Instalar o Docker

```bash
sudo apt update && sudo apt full-upgrade -y
sudo apt install -y docker.io build-essential
sudo usermod -aG docker $USER
newgrp docker
sudo systemctl start docker
sudo systemctl enable docker
docker run hello-world   # valida a instalação
```

### 2. Provisionar o DVWA

```bash
docker run --name dvwa -d -p 80:80 vulnerables/web-dvwa
docker ps   # confirma o contêiner ativo na porta 80/tcp
```

### 3. Configurar o DVWA no navegador

```
1. Acesse: http://localhost
2. Clique em "Create / Reset Database"
3. Login: admin / password
4. Defina o Security Level como "Low"
```

### 4. Análise com VirusTotal

```
1. Acesse https://www.virustotal.com
2. Aba "URL" → insira http://127.0.0.1/vulnerabilities/xss_s/
3. Analise os resultados (detecções, motores, histórico)
```

### 5. Monitoramento com Wireshark

```bash
sudo wireshark -k -i lo   # captura na interface loopback
# Filtro na barra de exibição: http
# Navegue pelo DVWA e submeta o payload XSS
# Inspecione o pacote POST em /vulnerabilities/xss_s/
```

### 6. Varredura com Nmap

```bash
nmap -sV -p 80 localhost                    # identificação de serviço
nmap -sV -p- localhost                      # varredura de todas as portas
nmap -O -A localhost                        # OS fingerprinting completo
nmap -p 80 localhost --script=http-stored-xss   # teste NSE para XSS
```

---

## 📈 Resultados e Conclusões

Os testes práticos documentaram as seguintes descobertas críticas em ambiente controlado:

- **VirusTotal:** 3 de 90 motores classificaram a URL do DVWA como maliciosa, demonstrando a eficácia da triagem coletiva mesmo para URLs locais.
- **Wireshark:** O payload `alert('XSS-UniFECAF')` foi capturado em texto claro no pacote POST n.99 (841 bytes), confirmando um ataque XSS Stored persistente — violação direta do pilar de **Integridade** e **Confidencialidade** da tríade CID.
- **Nmap:** O Apache httpd 2.4.25 (versão de 2017 com CVEs públicos) foi identificado na porta 80 em 6,33 segundos, evidenciando uma superfície de ataque imediata por ausência de patching.

A integração das três ferramentas — seguindo a metodologia do **Cyber Kill Chain** — demonstra como um analista pode identificar, documentar e mitigar vetores de ataque de forma estruturada, sustentada pelos frameworks NIST CSF, ISO/IEC 27001:2022 e pela LGPD.

---

## 📚 Referências

- NAKAMURA, Emilio Tissato. **O papel da segurança cibernética no universo digital: a importância do fator humano**. In: KUBOTA, Luis Claudio (Org.). Digitalização e tecnologias da informação e comunicação. Rio de Janeiro: Ipea, 2024. Cap. 9.
- HUTCHINS, Eric M.; CLOPPERT, Michael J.; AMIN, Rohan M. **Intelligence-Driven Computer Network Defense**. Leading Issues in Information Warfare, v. 1, n. 1, p. 80, 2011.
- LYON, Gordon. **Nmap Network Scanning**. Nmap Software LLC, 2008.
- ISO/IEC 27001:2022. **Information security management systems**. Geneva: ISO, 2022.
- NIST. **Cybersecurity Framework**. 2018. Disponível em: https://www.nist.gov/cyberframework
- BRASIL. **Lei n. 13.709, de 14 de agosto de 2018** (LGPD). Disponível em: https://www.planalto.gov.br
- BRASIL. **Lei n. 12.737, de 30 de novembro de 2012** (Lei Carolina Dieckmann).
- RIBEIRO, G. A. M.; CORDEIRO, P. I. R. V.; FUMACH, D. M. **O malware como meio de obtenção de prova**. Revista Brasileira de Direito Processual Penal, v. 8, n. 3, 2022.
- VIRUSTOTAL. Documentation Hub. Disponível em: https://virustotal.readme.io
- WIRESHARK FOUNDATION. User's Guide. Disponível em: https://www.wireshark.org/docs
- DIGININJA. DVWA. Disponível em: https://github.com/digininja/DVWA

---

## 👤 Autor

**Eduardo Souza Mattos**
R.A. 35984 · Centro Universitário UniFECAF
Disciplina: Cyber Security · 2026

---

## ⚖️ Aviso Legal

Este projeto foi desenvolvido exclusivamente para fins acadêmicos em ambiente controlado e autorizado. Nenhuma técnica aqui documentada deve ser replicada em sistemas de terceiros sem autorização expressa. O uso não autorizado das ferramentas descritas pode configurar crime nos termos da **Lei n. 12.737/2012 (Lei Carolina Dieckmann)** e do **Marco Civil da Internet (Lei n. 12.965/2014)**.

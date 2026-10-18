# NRA Sentinel & OSINT EDL
**Inteligência de Ameaças, Ocultação de Borda e Sustentação Consultiva MSSP para Ecossistemas Fortinet.**

<p>
  <img src="https://img.shields.io/badge/CDN-Cloudflare_Edge-00e5ff?style=for-the-badge&logo=cloudflare&logoColor=white" alt="CDN Cloudflare">
  <img src="https://img.shields.io/badge/Status-Operacional-10b981?style=for-the-badge" alt="Status Operacional">
  <img src="https://img.shields.io/badge/Ecosystem-NetworkRA_MSSP_%26_Community-black?style=for-the-badge" alt="NetworkRA MSSP & Community">
</p>

Motor tático de Threat Intelligence desenvolvido pela **NetworkRA** para auxiliar empresas, arquitetos e analistas de SOC na blindagem de perímetros contra **0-days, Agentes de IA Autônomos, Scanners Massivos, Botnets, Malware e Ransomware**.

---

### ❯ Topologia da Arquitetura

O ecossistema automatiza a coleta de telemetria global, sanitiza os dados brutos e entrega indicadores de comprometimento (IoCs) prontos para consumo nativo nos **External Connectors**.

<p><sub><i><strong>[ NRA Sentinel ]</strong> Workflow tático: Da coleta de inteligência primária à sanitização no perímetro via External Connectors.</i></sub></p>
<img src="https://raw.githubusercontent.com/networkra/nra-sentinel-community/main/image_4cc9b12.png" width="900" alt="NRA Sentinel Workflow Infographic">

<p><sub><i><strong>[ NRA OSINT EDL ]</strong> Workflow tático: Da compilação de bases OSINT Globais à entrega via External Connectors.</i></sub></p>
<img src="https://raw.githubusercontent.com/networkra/nra-sentinel-community/main/image_4cc9b10.png" width="900" alt="NRA OSINT EDL Workflow Infographic">

<p><sub><i><strong>[ Deploy Integrado ]</strong> Exemplo de Operação dos Conectores NRA Sentinel e OSINT no Ambiente FortiOS.</i></sub></p>
<img src="https://raw.githubusercontent.com/networkra/nra-sentinel-community/main/image_4cc9b14.png" width="900" alt="FortiGate Integration Deploy">

---

### ❯ Overview Operacional

- 🎯 **NRA Sentinel (Sniper):** Motor tático desenhado para mitigação de **0-days, Botnets, Malware e Ransomware**. Automatiza a coleta de Threat Intel Global, sanitiza dados brutos e os entrega limpos ao seu *External Resource*. Opera sob rígida Tripla Validação: **Safelist DNS**, Score de Reputação **(AbuseIPDB)** e **(urlscan.io)**, entregues em **Tiers escaláveis (FIFO)** para evitar esgotamento de memória RAM (*Conserve Mode*).
- 🌐 **NRA OSINT EDL:** Defesa ativa fundamentada em bases OSINT Globais. Extração, sumarização CIDR e distribuição oficial de players como **Spamhaus DROP, Firehol e CISA**. Mitigação autônoma de **Scanners Massivos** (Shodan, Censys, Modat, Cortex Xpanse...), **IPs Maliciosos** (Botnet-C&C, Phishing...) e **Agentes de IA**, blindada por um Circuit Breaker limitador de 150.000 IoCs.

#### 🔬 Engenharia de Curadoria Ativa: Expurgo NXDOMAIN & Check-up VirusTotal
Diferente de listas públicas estáticas que acumulam registros obsoletos e ruído na memória do equipamento, o ecossistema **NetworkRA** executa um pipeline contínuo de higienização e validação cruzada antes de publicar qualquer indicador na borda:

- **`>_` Varredura Ativa NXDOMAIN (Dead-Domain Purge):** Resolução DNS automatizada que identifica e elimina domínios mortos, expirados ou não publicados (*NXDOMAIN*), removendo indicadores inativos e poupando memória RAM da tabela de *DNS Filter* do FortiGate.
- **`>_` Check-up Multi-Engine via VirusTotal:** Auditoria de reputação cruzada com a inteligência do **VirusTotal** e bases globais, garantindo a permanência apenas de ameaças ativas confirmadas e reduzindo drasticamente a taxa de falsos positivos.

---

### ❯ Fontes de Telemetria (Data Sources)

| Player / Fonte | Módulo | Função Tática |
| :--- | :--- | :--- |
| **VirusTotal** | Sentinel | Auditoria multi-engine de reputação para confirmação de ameaças ativas. |
| **AlienVault (LevelBlue)** | Sentinel | Inteligência estratégica sobre campanhas de Ransomware e 0-days. |
| **MalwareBazaar** | Sentinel | Assinaturas de arquivos (Hashes) validadas pela comunidade. |
| **URLHaus** | Sentinel | Infraestruturas ativas distribuindo payload/malware. |
| **AbuseIPDB** | Sentinel | Validação cruzada de reputação de IPs (Prevenção de Falsos Positivos). |
| **urlscan.io** | Sentinel | Análise histórica e estrutural de domínios e URLs. |
| **DNS Shadow / NXDOMAIN** | Sentinel | Varredura ativa para expurgo automático de domínios mortos/expirados. |
| **Bases OSINT Globais** | OSINT EDL | Compilação de Scanners massivos, Agentes de IA e IPs maliciosos. |

---

### ❯ Rotação e Performance (Multi-Tier FIFO)

Sob a premissa de ser um motor **Sniper** (precisão cirúrgica no lugar de volume irracional), o ecossistema utiliza rotação **FIFO** segmentada para proteger a integridade do **WAD/IPS Engine** do seu Firewall:

| Arquitetura | Target Hardware | Capacidade Máxima |
| :--- | :--- | :--- |
| **Tier 1 (Entry-Level)** | Linha E / 40F a 80F (2GB-3GB RAM) | 35.000 IoCs por categoria |
| **Tier 2 (Mid-Range)** | 100F a 600F (4GB-8GB RAM) | 150.000 IoCs por categoria |
| **Tier 3 (High-End)** | Data Centers / Clusters | 300.000 IoCs por categoria |
| **OSINT EDL (Universal)** | Qualquer Appliance | 150.000 IoCs *(Circuit Breaker)* |

> 💡 **Dimensionamento Correto:** Sempre alinhe o Tier à RAM do equipamento. Para verificar o uso atual de RAM no FortiOS via CLI utilize: `diagnose hardware sysinfo conserve`.

---

### ⚠️ Política de Acesso, Rate Limit & Whitelist PRO

Para garantir a alta disponibilidade da nossa infraestrutura e a entrega contínua de inteligência curada, nossa borda (Cloudflare Edge) opera sob três modalidades de acesso:

1. **Tier Free (Degustação / Lab):** Sincronização limitada a **1 requisição a cada 7 dias (168 horas)** por IP público. Tentativas de download em intervalo menor retornarão o erro `HTTP 429 (Too Many Requests)` no log do Firewall.
2. **Membro YouTube (Nível Sentinel — Uso Pessoal/Lab):** Direito à liberação de **01 (um) único IP público fixo** na **Whitelist PRO** (sincronização a cada **60 minutos** sem bloqueios), exclusivo para uso em laboratório/ambiente próprio mediante validação via formulário na aba Comunidade.
3. **Plano Business & Sustentação MSSP (Empresas e Consultores Parceiros):** Licenciamento corporativo para **múltiplos IPs / múltiplas caixas (SD-WAN)** em tempo real (60m), com emissão de **Relatório Executivo Mensal** e **PoC Gratuita de 7 dias**.

🔓 **Ativação Rápida:**
- 👉 **[1 IP Lab: Assinar Nível Sentinel no YouTube](https://www.youtube.com/channel/UCs8isxhuF4phuQXimE52tOg/join)**
- 👉 **[Multi-IPs / Empresas: Solicitar PoC Corporativa de 7 Dias](https://networkra.seg.br/#Corporate)**

---

### ❯ External Connectors URLs

> ⚠️ **Update interval recomendado no FortiGate:** `7 dias / 10080 min` (Free) ou `60 minutos` (Whitelist PRO / Business).

**<mark>&nbsp;`TIER 1: ENTRY-LEVEL [ HW: Linha E / 40F a 80F | MAX: 35k IoCs ]`&nbsp;</mark>**

```text
https://nra-sentinel-feeds.networkra.seg.br/nra-ips-critical-1.txt
```

```text
https://nra-sentinel-feeds.networkra.seg.br/nra-dom-critical-1.txt
```

```text
https://nra-sentinel-feeds.networkra.seg.br/nra-hash-critical-1.txt
```

**<mark>&nbsp;`TIER 2: MIDRANGE [ HW: 100F a 600F | MAX: 150k IoCs ]`&nbsp;</mark>**

```text
https://nra-sentinel-feeds.networkra.seg.br/nra-ips-mid-critical-1.txt
```

```text
https://nra-sentinel-feeds.networkra.seg.br/nra-dom-mid-critical-1.txt
```

```text
https://nra-sentinel-feeds.networkra.seg.br/nra-hash-mid-critical-1.txt
```

**<mark>&nbsp;`TIER 3: HIGH-END [ HW: Data Centers | MAX: 300k IoCs ]`&nbsp;</mark>**

```text
https://nra-sentinel-feeds.networkra.seg.br/nra-ips-high-critical-1.txt
```

```text
https://nra-sentinel-feeds.networkra.seg.br/nra-dom-high-critical-1.txt
```

```text
https://nra-sentinel-feeds.networkra.seg.br/nra-hash-high-critical-1.txt
```

**<mark>&nbsp;`MODULE: OSINT EDL [ HW: Universal ]`&nbsp;</mark>**

```text
https://nra-osint-edl.networkra.seg.br/osint_reputation_ips.txt
```

```text
https://nra-osint-edl.networkra.seg.br/osint_scanner_ips.txt
```

---

### ❯ Corporate Plans & MSSP Solutions

A **NetworkRA** oferece duas modalidades corporativas para organizações e consultores parceiros que desejam blindar seus perímetros Fortinet em produção:

#### 📦 1. Licenciamento NRA Sentinel PRO (Modalidade Produto — Sem Consultoria)
Desenhado para empresas e consultores de TI que já possuem administração técnica própria dos seus appliances FortiGate e desejam blindar a borda em tempo real contra scanners autônomos, Agentes de IA, botnets, malwares e ataques 0-day.
- **`>_` Whitelist PRO Multi-IPs:** Sincronização contínua a cada 60 minutos para múltiplas caixas ou links redundantes (SD-WAN) sem restrições de rate limit.
- **`>_` Curadoria 24/7 Anti-Crash (FIFO):** Listas calibradas por capacidade de hardware (Tier 1, 2 ou 3) com 0% de falsos positivos em ASNs corporativos críticos e proteção contra *Conserve Mode*.
- **`>_` Relatório Executivo Mensal:** Emissão mensal de dossiê executivo em PDF consolidando o volume de bloqueios, IPs atacantes retidos e origens geográficas para apresentação à diretoria.

#### 🛡️ 2. Sustentação Consultiva MSSP Full (Engenharia Fortinet + Threat Intel)
Gestão contínua da segurança de borda combinando o motor proprietário NRA Sentinel & OSINT com suporte consultivo especializado Nível 3, maximizando a segurança da infraestrutura atual sem necessidade de troca de hardware.
- **`>_` Whitelist PRO Multi-IPs:** Sincronização contínua a cada 60 minutos para múltiplas caixas ou links redundantes (SD-WAN) sem restrições de rate limit.
- **`>_` Onboarding & Adequação Tática Inicial:** Estudo da topologia de borda e injeção guiada dos conectores em *Local-In Policy* seguindo as melhores práticas do fabricante e blindagem com Drop Silencioso.
- **`>_` Governança Executiva Contínua:** Extração de dados, auditoria de logs e entrega mensal do Relatório Executivo de Mitigação (2h fixas alocadas da franquia mensal) para apresentação à diretoria.
- **`>_` Franquia de 10 Horas Mensais Globais:** Inclui 8h livres mensais de engenharia Nível 3 para acionamentos de suporte, administração de regras, VPNs, SD-WAN, BGP e troubleshooting avançado.

#### 🚀 Solicitar PoC Gratuita de 7 Dias ou Proposta Comercial
Valide a eficácia do **NRA Sentinel & OSINT** no seu próprio ambiente corporativo por **7 dias sem compromisso**, ou solicite uma proposta comercial / parceria enviando um e-mail para **`contato@networkra.seg.br`** com o assunto `[POC / PROPOSTA] NRA Sentinel PRO - NOME_DA_EMPRESA` e os dados abaixo:

```text
>_ Empresa / Consultoria: 
>_ Modalidade de Interesse (1-Somente Produto PRO | 2-Sustentação MSSP): 
>_ Quantidade e Modelos dos FortiGates (ex: 02x 60F, 01x 100F): 
>_ IP(s) Público(s) de Borda (caso deseje iniciar a PoC de 7 dias): 
>_ Telefone / WhatsApp para Contato: 
```

---

### ❯ Incident Response & Whitelisting

A heurística automatizada do NRA Sentinel opera sob rigidez tática. Contudo, em cenários de compartilhamento de infraestruturas cloud (ASNs) ou reatribuição de IP, um indicador pode gerar alertas falsos positivos. Caso o seu tráfego legítimo sofra um bloqueio indevido, utilize a via oficial abaixo para solicitar **Whitelisting** imediato.

> **SLA DE RESPOSTA:** Até 24 Horas Úteis.

**1. Destinatário do SOC**
```text
soc@networkra.seg.br
```

**2. Assunto (Obrigatório)**
```text
[FP-REPORT] INSIRA_O_IP_OU_DOMINIO_AQUI
```

**3. Corpo do E-mail (Template)**
```text
>_ IP/Domínio afetado: 
>_ Lista de Bloqueio (ex: NRA-OSINT, Scanners): 
>_ Breve Justificação / Evidência de Tráfego Legítimo: 
```

---

### ❯ Comunidade & Laboratório

Para estudantes e profissionais que desejam liberar **01 IP fixo de laboratório** em tempo real (60m) e apoiar a criação de conteúdo técnico na comunidade brasileira de Cibersegurança, torne-se membro do canal no nível **Sentinel**:
👉 **[Assinar Nível Sentinel no YouTube](https://www.youtube.com/channel/UCs8isxhuF4phuQXimE52tOg/join)**

---

### ❯ Hall da Fama & Reconhecimento Técnico

O NRA Sentinel ganha robustez através do engajamento e feedback da comunidade. Nosso agradecimento aos especialistas que colaboraram diretamente com a evolução desta arquitetura:

- **[@faustocaldeira](https://github.com/faustocaldeira):** Mapeamento e estruturação da Safelist (AdGuard), crucial na mitigação de Falsos Positivos.
- **@RodrigoAssinger:** Insight estratégico do algoritmo Multi-Tier, permitindo a implantação das listas por camadas de hardware (Entry-Level, Midrange e High-End).
- **@MsAbreu000:** Correção e troubleshooting no fluxo de autenticação cifrada via PBKDF2.
- **[@lcs1337](https://www.linkedin.com/in/lcs1337/):** Melhorias na safelist incluindo proteções por RFC. Exemplo: RFC_6598 Carrier-Grade NAT (CGNAT) - 100.64.0.0/10.

---

### 👨‍💻 NetworkRA — Consultoria em Cibersegurança & MSSP
**Robert Alexandrino**  
*Especialista em Segurança de Redes & Arquiteto MSSP*

© 2026 NetworkRA. Todos os direitos reservados.  

🌐 **[Portal Oficial (networkra.seg.br)](https://networkra.seg.br)** | ✉️ **[contato@networkra.seg.br](mailto:contato@networkra.seg.br)** | 🔗 **[LinkedIn](https://www.linkedin.com/in/networkra/)** | 📺 **[YouTube](https://www.youtube.com/@NetworkRA)** | 💬 **[Telegram](https://t.me/+jHlbAlp-7Xg0MTJh)**

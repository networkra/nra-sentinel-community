## 🛡️ NRA Sentinel & OSINT EDL - Threat Intelligence Ecosystem (V33.5)

O **NRA Sentinel** é um motor tático de Threat Intelligence desenvolvido para auxiliar arquitetos e analistas de SOC na proteção de perímetros contra **0-days, Botnets, Malware e Ransomware**. O ecossistema automatiza a coleta de telemetria global, sanitiza os dados brutos e entrega indicadores de comprometimento (IoCs) prontos para consumo nativo nos **External Connectors**. Não é uma "bala de prata", mas sim uma camada de defesa ativa projetada para operar em conjunto com a sua arquitetura de segurança atual.

<img src="https://raw.githubusercontent.com/networkra/nra-sentinel-community/main/image_4cc9b12.png" width="900" alt="NRA Sentinel Workflow Infographic">
<p align="left"><sub><i><strong>[ NRA Sentinel ]</strong> Workflow tático: Da coleta de inteligência primária à sanitização no perímetro via External Connectors.</i></sub></p>

---

O módulo **NRA OSINT EDL (Community Edition)** é uma arquitetura de defesa ativa desenvolvida com o objetivo de democratizar a segurança de borda para profissionais, empresas e provedores (MSSPs) que operam appliances desassistidos ou sem licenciamento ativo. Nossa engenharia extrai, sumariza (via CIDR) e compila bases OSINT globais consolidadas (como Spamhaus DROP, Firehol, CISA, entre outros). Toda a inteligência é filtrada por nossa Safelist e entregue pronta para bloqueio incondicional (drop).

Esta infraestrutura atua como um braço de apoio técnico à comunidade, garantindo que restrições orçamentárias não se tornem pontos cegos na segurança das redes brasileiras.

<img src="https://raw.githubusercontent.com/networkra/nra-sentinel-community/main/image_4cc9b10.png" width="900" alt="NRA OSINT EDL Workflow Infographic">
<p align="left"><sub><i><strong>[ NRA OSINT EDL ]</strong> Workflow tático: Da compilação de bases OSINT Globais à entrega via External Connectors.</i></sub></p>

---

<img src="https://raw.githubusercontent.com/networkra/nra-sentinel-community/main/image_4cc9b14.png" width="900" alt="FortiGate Integration Deploy">
<p align="left"><sub><i><strong>[ Deploy Integrado ]</strong> Exemplo de Operação dos Conectores NRA Sentinel e OSINT no Ambiente FortiOS.</i></sub></p>

---

> **[ STATUS: OPERACIONAL ]** O motor está provisionado através de CDN Global (Cloudflare Edge). 
> **Domínio Oficial:** `networkra.seg.br`
> *Alta disponibilidade, latência otimizada e tolerância a falhas para operações de SOC.*

---

### 🧠 Fontes de Telemetria (Data Sources)
O ecossistema consolida informações de players renomados, submetendo os dados a um rigoroso processo de sanitização antes da entrega:

| Player / Fonte | Módulo | Função Tática |
| :--- | :---: | :--- |
| **AlienVault (LevelBlue)** | ![Sentinel](https://img.shields.io/badge/NRA-Sentinel-0055ff?style=flat-square&logo=shield&logoColor=white) | Fornece inteligência estratégica sobre campanhas de Ransomware e 0-days. |
| **MalwareBazaar (abuse.ch)** | ![Sentinel](https://img.shields.io/badge/NRA-Sentinel-0055ff?style=flat-square&logo=shield&logoColor=white) | Entrega assinaturas de arquivos (Hashes) validadas pela comunidade cibernética. |
| **URLHaus (abuse.ch)** | ![Sentinel](https://img.shields.io/badge/NRA-Sentinel-0055ff?style=flat-square&logo=shield&logoColor=white) | Monitora infraestruturas ativas distribuindo payload/malware em tempo real. |
| **AbuseIPDB** | ![Sentinel](https://img.shields.io/badge/NRA-Sentinel-0055ff?style=flat-square&logo=shield&logoColor=white) | Validação cruzada de reputação de IPs, atuando na contenção de falsos positivos. |
| **urlscan.io** | ![Sentinel](https://img.shields.io/badge/NRA-Sentinel-0055ff?style=flat-square&logo=shield&logoColor=white) | Análise histórica e estrutural de domínios e URLs processadas pelo motor. |
| **Bases OSINT** | ![EDL](https://img.shields.io/badge/NRA-OSINT_EDL-ff8800?style=flat-square&logo=shield&logoColor=white) | Compilação de Scanners massivos e IPs maliciosos para proteção de appliances sem licença. |

---

### 🛡️ Motor de Prevenção de Falsos Positivos
Para garantir que tráfego legítimo não sofra *drop* acidental, o NRA Sentinel emprega uma esteira de dupla validação (Blindagem de Falsos Positivos):

1. **Safelist (Exceção Absoluta):** Todo IP processado sofre um *match* contra a `nra-safelist.txt`. Caso pertença a infraestruturas conhecidas, o IoC é imediatamente descartado.
2. **Validação Heurística (AbuseIPDB):** Se não estiver na Safelist, o IP passa por verificação em tempo real. Só avança para a blocklist final se possuir um *Abuse Confidence Score* $\ge$ **20%**. 
*(Exceção: Ameaças críticas classificadas como **0-day** têm by-pass no score, mas continuam submissas à Safelist de DNS).*

**Origem da Safelist:** Curada e atualizada com base na documentação oficial de resolvedores globais do **[AdGuard DNS Providers](https://adguard-dns.io/kb/pt-BR/general/dns-providers/)**.

---

### ⚙️ Overview Operacional

* **Atualização (Sentinel):** Extração, validação e compilação de novos IoCs ocorrem automaticamente a cada **8 horas**.
* **Atualização (OSINT EDL):** A esteira OSINT é executada **1 vez ao dia**, cadência ideal para sumarização de CIDRs sem gerar *overhead* nas fontes primárias.
* **Persistência & Rotação:** A memória do motor obedece a uma regra estrita de FIFO (First-In, First-Out), garantindo rotatividade de ameaças obsoletas.
* **Sanitização (Clean-up):** O código limpa protocolos (http/https), remove portas e query strings, entregando o artefato no formato exato exigido pelo daemon do FortiOS.

---

### 🛡️ Rotação e Performance (Multi-Tier)

Sob a premissa de ser um motor **Sniper** (precisão cirúrgica no lugar de volume irracional), o ecossistema é segmentado para proteger a integridade do **WAD/IPS Engine** do seu Firewall, prevenindo cenários de *Conserve Mode*:

| Arquitetura | Target Hardware | Capacidade Máxima |
| :--- | :--- | :--- |
| 🛡️ **Sentinel (Entry-Level)** | 40F, 60F, 80F (2GB-3GB RAM) | 35.000 IoCs por categoria |
| 🛡️ **Sentinel (Mid-Range)** | 100F a 600F (4GB-8GB RAM) | 150.000 IoCs por categoria |
| 🛡️ **Sentinel (High-End)** | Data Centers / Clusters | 300.000 IoCs por categoria |
| 🌐 **OSINT EDL (Universal)** | Qualquer Appliance (C/ ou S/ Licença) | 150.000 IoCs *(Circuit Breaker)* |

> [!NOTE]
> **A Engenharia do Circuit Breaker (150k Limit)**
> A sumarização diária de CIDRs OSINT flutua entre 60.000 e 100.000 blocos. O motor Python possui uma trava dura (*Circuit Breaker*) em **150.000 linhas**. Em caso de anomalia global de roteamento (BGP Hijack) ou corrupção na fonte original, o script aborta a atualização e mantém a lista íntegra do dia anterior. Isso blinda Firewalls de entrada (como o 40F) contra esgotamento súbito de RAM.

> [!TIP]
> **Dimensionamento Correto:** Sempre alinhe o Tier à RAM do equipamento. Caixas com 2GB (Entry) devem rodar as listas `critical`. Caixas a partir de 4GB podem escalar para `mid-critical` ou `high-critical`. Para verificar o uso atual de RAM no FortiOS via CLI: `diagnose hardware sysinfo conserve`.

---

### 🌐 NRA OSINT EDL (Community Edition)

O desenvolvimento deste módulo prova que a defesa de perímetro não precisa ser um privilégio comercial. É uma solução tática audaciosa que preenche a lacuna entre a "cegueira operacional" e a proteção corporativa.

> [!IMPORTANT]
> **DEMOCRATIZAÇÃO DA SEGURANÇA (100% FREE)**
> A segurança da sua rede não pode ficar desamparada por restrições de *budget*. Este módulo consolida intel OSINT de IPs maliciosos e scanners, entregando proteção gratuita, validada e sumarizada para infraestruturas sem licenciamento ativo.

### 🛡️ O que estamos entregando para o seu Firewall?

Nossa arquitetura compila e sanitiza indicadores globais, transformando dados brutos em **inteligência acionável**. Ao plugar este feed, seu perímetro passa a bloquear proativamente duas grandes frentes de ataque:

###  1. Ameaças Ativas & Reputação
* 🚨 **Botnet-C&C:** Servidores de Comando e Controle de Botnets globais.
* 🛑 **Malicious:** Hosts catalogados em ataques ativos e drop de malwares.
* 🎣 **Phishing:** Infraestruturas conhecidas por hospedagem de páginas de Phishing.
* ⛏️ **Blockchain-Crypto.Mining:** Pools de mineração não autorizada (Cryptojacking).
* 🧅 **Tor Nodes (Exit, Relay, Tor):** Nós da rede TOR frequentemente utilizados para anonimizar invasões.
* 📧 **Spam:** IPs e servidores identificados em campanhas massivas de e-mails indesejados e maliciosos.
* 🕵️ **Proxy & Anonymous VPN:** Serviços de mascaramento de IP usados para burlar perímetros.

###  2. Motores de Reconhecimento (Anti-Scanning)
Para impedir que atacantes mapeiem a topologia ou vulnerabilidades (CVEs) da sua rede, compilamos a base completa de *Scanners*. O feed mitiga milhares de prefixos voltados a varredura massiva:

* 👁️ **Global Scanners:** Shodan, Censys, Rapid7, Shadowserver, BinaryEdge, LeakIX.
* 🔍 **Corporate & Gov Scanners:** Palo Alto Cortex Xpanse, Internet Census Group, UK NCSC, NetScout.
* 🕷️ **Mass Crawlers & Recon:** Stretchoid, CriminalIP, Hadrian, ONYPHE, entre dezenas de outros *bots* de enumeração autônoma.

---

### <mark>&nbsp;🚀 Guia de Integração (External Connectors)&nbsp;</mark>
> [!WARNING]
> 🚨 URLs antigas apontando para `raw.githubusercontent.com` ou baseadas na nomenclatura antiga **foram descontinuadas**. Atualize seus conectores imediatamente para a nova CDN da Cloudflare abaixo.

### 🔗 Endpoints Oficiais (Produção)

Selecione a camada compatível com o hardware do seu SOC. *Refresh Rate* recomendado: **60 Minutos**.

### 🟢 1. NRA Sentinel - Entry-Level
Focado no core de ameaças ativas (Botnets, C2, Malwares críticos).
* **IPs:** `https://nra-sentinel-feeds.networkra.seg.br/nra-ips-critical-1.txt`
* **Domains:** `https://nra-sentinel-feeds.networkra.seg.br/nra-dom-critical-1.txt`
* **Hashes (Malware):** `https://nra-sentinel-feeds.networkra.seg.br/nra-hash-critical-1.txt`

### 🟡 2. NRA Sentinel - Midrange
Base *Critical* + IoCs adicionais com tempo de vida (TTL) expandido.
* **IPs:** `https://nra-sentinel-feeds.networkra.seg.br/nra-ips-mid-critical-1.txt`
* **Domains:** `https://nra-sentinel-feeds.networkra.seg.br/nra-dom-mid-critical-1.txt`
* **Hashes (Malware):** `https://nra-sentinel-feeds.networkra.seg.br/nra-hash-mid-critical-1.txt`

### 🔴 3. NRA Sentinel - High-End
Telemetria total com amplo histórico e profundidade em detecção.
* **IPs:** `https://nra-sentinel-feeds.networkra.seg.br/nra-ips-high-critical-1.txt`
* **Domains:** `https://nra-sentinel-feeds.networkra.seg.br/nra-dom-high-critical-1.txt`
* **Hashes (Malware):** `https://nra-sentinel-feeds.networkra.seg.br/nra-hash-high-critical-1.txt`

---

### 🔵 4. NRA OSINT EDL (Community)
Proteção autônoma contra Scanners Globais e IPs de baixa reputação.
* **IP Reputation:** `https://nra-osint-edl.networkra.seg.br/osint_reputation_ips.txt`
* **Scanners / Scrapers:** `https://nra-osint-edl.networkra.seg.br/osint_scanner_ips.txt`

---

### ⚙️ Como configurar no FortiGate (GUI)

1. Acesse o seu FortiGate via interface web.
2. Navegue até **Security Fabric** -> **External Connectors**.
3. Clique em **Create New** e selecione a categoria (IP Address, Domain Name, ou Malware Hash).
4. Insira um nome descritivo (ex: `NRA_Sentinel_Critical_IP`).
5. Cole o **Endpoint Oficial** desejado no campo URL.
6. Defina o **Refresh Rate** para **60 minutos**.
7. Valide se o *Status* está "Enable" e aplique.
8. Referencie esses objetos dinâmicos em suas **Firewall Policies**, **AntiVirus Profiles** ou **Web-Filters** (Ação: Block/Drop).

> **WARNING - Nota de Responsabilidade**:
> Toda inteligência cibernética opera sob contexto e probabilidade. A ação final de bloqueio e a curadoria dos logs gerados são de responsabilidade do analista/arquiteto encarregado pelo ambiente. Proceda com monitoramento contínuo.

---

### 💎 Apoie a Manutenção do Código

O ecossistema **NRA Sentinel & OSINT EDL** é mantido **100% gratuito e open-source**. Nosso propósito é elevar o nível de segurança da comunidade brasileira.

🔗 **Alertas Táticos (Telegram):** [NRA Sentinel & OSINT Alerts](https://t.me/+jHlbAlp-7Xg0MTJh)

🤖 **Incentive o Desenvolvimento:** Se esta infraestrutura gerou valor para o seu SOC, mitigou incidentes ou economizou tempo da sua equipe MSSP, considere apoiar a manutenção dos servidores Cloudflare tornando-se membro institucional:
👉 **[Apoie o Projeto via YouTube (Nível Sentinel / MSSP)](https://www.youtube.com/channel/UCs8isxhuF4phuQXimE52tOg/join)**

---

### 🏆 Hall da Fama: Parceiros MSSP

Reconhecimento técnico aos Arquitetos e provedores MSSP que viabilizam o funcionamento ininterrupto desta arquitetura.

| 🛡️ Analista / MSSP | 🔗 Perfil Profissional | 📅 Deploy Status |
| :--- | :--- | :--- |
| *@RodrigoAssinger* | *LinkedIn / Site* | *Ativo (Desde Ago/2026)* |
| *[ Slot Disponível ]* | - | - |
| *[ Slot Disponível ]* | - | - |

---

### 🤝 Intel Contributors

O motor ganha robustez através dos *feedbacks* de engenharia dos nossos pares:

*   **[@faustocaldeira](https://github.com/faustocaldeira/):** Mapeamento e estruturação da Safelist (AdGuard), crucial na mitigação de Falsos Positivos (DNS Providers).
*   **@RodrigoAssinger:** Design do algoritmo Multi-Tier, permitindo compatibilidade estável em clusters e caixas Entry-Level simultaneamente.
*   **@MsAbreu000:** Correção e *troubleshooting* no fluxo de autenticação cifrada via PBKDF2.

---

### 👨‍💻 Desenvolvedor & Arquiteto

**Robert Alexandrino (NetworkRA)** 
*Network Security Engineer & MSSP Solutions Architect*

---

### ⚖️ Licenciamento & EULA

Arquitetura desenvolvida e mantida por **Robert Alexandrino (NetworkRA)**.
© 2026 NetworkRA. All rights reserved.

A distribuição e integração deste *Threat Feed* são **100% livres e abertas** para uso acadêmico, laboratorial e corporativo. 

🔗 **[LinkedIn - Robert Alexandrino](https://www.linkedin.com/in/networkra/)** | 📺 **[YouTube - NetworkRA](https://www.youtube.com/@NetworkRA)** | 🔗 **[Telegram - SOC Comunnity](https://t.me/+jHlbAlp-7Xg0MTJh)**

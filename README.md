# NRA Sentinel & OSINT EDL
**Feeds OSINT Automatizados para Mitigação de Ameaças e Defesa de Perímetro em Tempo Real.**

<p>
  <img src="https://img.shields.io/badge/CDN-Cloudflare_Edge-00e5ff?style=for-the-badge&logo=cloudflare&logoColor=white" alt="CDN Cloudflare">
  <img src="https://img.shields.io/badge/Status-Operacional-10b981?style=for-the-badge" alt="Status Operacional">
  <img src="https://img.shields.io/badge/Licence-100%25_Community-black?style=for-the-badge" alt="100% Community">
</p>

Motor tático de Threat Intelligence desenvolvido para auxiliar arquitetos e analistas de SOC na proteção de perímetros contra **0-days, Botnets, Malware e Ransomware**.

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

**NRA Sentinel** - 
Opera sob rígida Tripla Validação: Safelist DNS e Score de Reputação (AbuseIPDB), entregues em Tiers escaláveis para evitar esgotamento de memória no Firewall.

**NRA OSINT EDL** - Mitigação autônoma de Scanners Massivos (Shodan, Censys, Rapid7...) e IPs Maliciosos (Botnet-C&C, Phishing...), blindada por um Circuit Breaker limitador de 150.000 IoCs para proteção de appliances sem licença.

---

### ❯ Fontes de Telemetria (Data Sources)

| Player / Fonte | Módulo | Função Tática |
| :--- | :--- | :--- |
| **AlienVault (LevelBlue)** | Sentinel | Inteligência estratégica sobre campanhas de Ransomware e 0-days. |
| **MalwareBazaar** | Sentinel | Assinaturas de arquivos (Hashes) validadas pela comunidade. |
| **URLHaus** | Sentinel | Infraestruturas ativas distribuindo payload/malware. |
| **AbuseIPDB** | Sentinel | Validação cruzada de reputação de IPs (Prevenção de FP). |
| **urlscan.io** | Sentinel | Análise histórica e estrutural de domínios e URLs. |
| **Bases OSINT** | OSINT EDL | Compilação de Scanners massivos e IPs maliciosos. |

---

### ❯ Rotação e Performance (Multi-Tier)

Sob a premissa de ser um motor **Sniper** (precisão cirúrgica no lugar de volume irracional), o ecossistema é segmentado para proteger a integridade do **WAD/IPS Engine** do seu Firewall:

| Arquitetura | Target Hardware | Capacidade Máxima |
| :--- | :--- | :--- |
| **Tier 1 (Entry-Level)** | 40F, 60F, 80F (2GB-3GB RAM) | 35.000 IoCs por categoria |
| **Tier 2 (Mid-Range)** | 100F a 600F (4GB-8GB RAM) | 150.000 IoCs por categoria |
| **Tier 3 (High-End)** | Data Centers / Clusters | 300.000 IoCs por categoria |
| **OSINT EDL (Universal)** | Qualquer Appliance | 150.000 IoCs *(Circuit Breaker)* |

> 💡 **Dimensionamento Correto:** Sempre alinhe o Tier à RAM do equipamento. Para verificar o uso atual de RAM no FortiOS via CLI utilize: `diagnose hardware sysinfo conserve`.

---

### ❯ External Connectors URLs

> ⚠️ **Update interval recomendado no Firewall:** `60 minutos`.

**<mark>&nbsp;`TIER 1: ENTRY-LEVEL [ HW: 40F/60F/80F | MAX: 35k ]`&nbsp;</mark>**

```text
https://nra-sentinel-feeds.networkra.seg.br/nra-ips-critical-1.txt
```

```text
https://nra-sentinel-feeds.networkra.seg.br/nra-dom-critical-1.txt
```

```text
https://nra-sentinel-feeds.networkra.seg.br/nra-hash-critical-1.txt
```

**<mark>&nbsp;`TIER 2: MIDRANGE [ HW: 100F a 600F | MAX: 150k ]`&nbsp;</mark>**

```text
https://nra-sentinel-feeds.networkra.seg.br/nra-ips-mid-critical-1.txt
```

```text
https://nra-sentinel-feeds.networkra.seg.br/nra-dom-mid-critical-1.txt
```

```text
https://nra-sentinel-feeds.networkra.seg.br/nra-hash-mid-critical-1.txt
```

**<mark>&nbsp;`TIER 3: HIGH-END [ HW: Data Centers | MAX: 300k ]`&nbsp;</mark>**

```text
https://nra-sentinel-feeds.networkra.seg.br/nra-ips-high-critical-1.txt
```

```text
https://nra-sentinel-feeds.networkra.seg.br/nra-dom-high-critical-1.txt
```

```text
https://nra-sentinel-feeds.networkra.seg.br/nra-hash-high-critical-1.txt
```

**<mark>&nbsp;`MODULE: OSINT EDL (COMMUNITY) [ HW: Universal ]`&nbsp;</mark>**

```text
https://nra-osint-edl.networkra.seg.br/osint_reputation_ips.txt
```

```text
https://nra-osint-edl.networkra.seg.br/osint_scanner_ips.txt
```

---

### ❯ Incident Response & Whitelisting

A heurística automatizada do NRA Sentinel opera sob rigidez tática. Contudo, em cenários de compartilhamento de infraestruturas cloud (ASNs) ou reatribuição de IP, um indicador pode gerar alertas falsos positivos. Caso o seu tráfego legítimo sofra um bloqueio indevido, utilize a via oficial abaixo para solicitar **Whitelisting** imediato.

> **SLA DE RESPOSTA:** 24 Horas Úteis.

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

### ❯ Apoie a Manutenção do Código

O ecossistema **NRA Sentinel & OSINT EDL** é mantido **100% gratuito e open-source**. Nosso propósito é elevar o nível de segurança da comunidade brasileira.

🤖 **Incentive o Desenvolvimento:** Se esta infraestrutura gerou valor para o seu SOC ou economizou tempo da sua equipa MSSP, considere apoiar a manutenção dos servidores tornando-se membro institucional:
👉 **[Apoie o Projeto via YouTube (Nível Sentinel / MSSP)](https://www.youtube.com/channel/UCs8isxhuF4phuQXimE52tOg/join)**

---

### ❯ Hall da Fama & Contributors

O motor ganha robustez através dos feedbacks de engenharia dos nossos pares. Reconhecimento técnico aos Arquitetos que viabilizam esta arquitetura:

*   **[@faustocaldeira](https://github.com/faustocaldeira/):** Mapeamento e estruturação da Safelist (AdGuard), crucial na mitigação de Falsos Positivos.
*   **@RodrigoAssinger:** Design do algoritmo Multi-Tier, permitindo compatibilidade estável em clusters e caixas Entry-Level simultaneamente.
*   **@MsAbreu000:** Correção e troubleshooting no fluxo de autenticação cifrada via PBKDF2.

---

### 👨‍💻 Desenvolvedor & Arquiteto
**Robert Alexandrino (NetworkRA)** 
*Network Security Engineer & MSSP Solutions Architect*

© 2026 NetworkRA. All rights reserved. A distribuição e integração deste *Threat Feed* são **100% livres e abertas** para uso académico, laboratorial e corporativo. 

🔗 **[LinkedIn](https://www.linkedin.com/in/networkra/)** | 📺 **[YouTube](https://www.youtube.com/@NetworkRA)** | 💬 **[Telegram - Alertas Táticos](https://t.me/+jHlbAlp-7Xg0MTJh)**

## 🛡️ NRA Sentinel - Inteligência de Ameaças (V33.5)

O **NRA Sentinel** é um projeto desenvolvido com o objetivo de auxiliar profissionais de segurança e redes na proteção de suas infraestruturas contra 0-days, Botnets, Malware e Ransomware. Ele automatiza a coleta e a organização de dados de novas ameaças globais, entregando listas limpas e prontas para uso nos **External Connectors** do FortiGate. Este motor não busca ser uma "solução milagrosa", mas sim uma ferramenta de apoio que soma forças aos recursos que você já utiliza no dia a dia, ideal para quem busca reforçar ainda mais a segurança.

<img src="image_4cc9b8.png" width="900" alt="NRA Sentinel Workflow Infographic">
<p align="left"><sub><i>Workflow detalhado: Da coleta em fontes globais à entrega sanitizada no perímetro via FortiOS.</i></sub></p>

---

<img src="image_4cc9b7.png" width="800" alt="NRA Sentinel FortiGate Integration">
<p align="left"><sub><i>Exemplo da integração dos conectores NRA Sentinel operando em ambiente FortiOS.</i></sub></p>

---

### 🧠 Fontes de Dados

O motor busca informações em fontes respeitadas mundialmente, garantindo que o que chega ao seu firewall tenha passado por um processo de filtragem:

| Player | Função |
| :--- | :--- |
| **AlienVault (LevelBlue)** | Fornece inteligência estratégica sobre campanhas de Ransomware e 0-days. |
| **MalwareBazaar (abuse.ch)** | Entrega assinaturas de arquivos (Hashes) validadas pela comunidade. |
| **URLHaus (abuse.ch)** | Monitora links que estão distribuindo malware no exato momento. |
| **AbuseIPDB** | Ajuda a validar a reputação dos IPs, evitando falsos positivos. |
| **urlscan.io** | Verifica o histórico de segurança dos domínios e URLs processadas. |

---

### 🛡️ Prevenção de Falsos Positivos
Para garantir que infraestruturas legítimas não sejam bloqueadas acidentalmente, o NRA Sentinel conta com uma esteira de dupla validação antes de aprovar qualquer bloqueio:

1. **Safelist (Exceção Absoluta):** Todo IP extraído dos feeds é cruzado com o arquivo local `nra-safelist.txt`. Se o IP constar nesta lista, ele é imediatamente descartado, protegendo a sua infraestrutura.
2. **Validação de Reputação (AbuseIPDB):** Caso o IP não esteja na Safelist, ele passa por uma checagem em tempo real. O artefato só é incluído no conector final do FortiGate se atingir um *Abuse Confidence Score* igual ou superior a **20%**. 
*(Nota: Indicadores classificados como ameaças críticas de **0-day** recebem prioridade máxima de bloqueio, mas ainda assim são obrigados a respeitar a Safelist).*

**Origem dos Dados (Safelist):** A nossa lista base de provedores DNS globais foi extraída e curada a partir da documentação oficial do **[AdGuard DNS Providers](https://adguard-dns.io/kb/pt-BR/general/dns-providers/)**. 

---

### ⚙️ Detalhes do Funcionamento

* **Atualização:** Os feeds são processados a cada 4 horas automaticamente.
* **Persistência:** O motor mantém o histórico acumulado (não apaga registros antigos validados).
* **Otimização:** Arquivos organizados com até 35.000 linhas para garantir performance no FortiOS.
* **Limpeza:** Dados sanitizados (sem protocolos ou portas), prontos para leitura nativa do firewall.

---

### 🛡️ Rotação

No cenário de ameaças cibernéticas, o volume de hashes de malware cresce exponencialmente. Para manter a filosofia **Sniper** (precisão sobre volume) e garantir que o **NRA Sentinel** não sobrecarregue o hardware do seu FortiGate (especialmente modelos de entrada como 40F, 60F e 80F), implementamos uma lógica de **Rolling Buffer**:

* **Capacidade Inteligente:** O feed de hashes é limitado automaticamente aos **35.000 registros mais recentes**.
* **Performance Garantida:** Mantemos a lista bem abaixo do limite técnico de 50.000 entradas do FortiOS, garantindo que o consumo de memória (**WAD/IPS Engine**) permaneça estável e o sistema opere fora do *Conserve Mode*.
* **Foco no que é Ativo:** Malwares de campanhas obsoletas são "podados" automaticamente, garantindo que sua proteção esteja sempre focada em ameaças ativas e variantes de **0-day**.

> [!IMPORTANT]
> **Complemento, não Substituição:** O NRA Sentinel **não tem o objetivo de substituir a base de dados do FortiGuard**. O FortiGuard é a sua infantaria global e essencial. O Sentinel atua como um **Sniper de elite**: uma camada extra de inteligência cirúrgica, focada em indicadores de alta fidelidade e ameaças emergentes que acabaram de ser detectadas nos laboratórios.

*A eficiência de um feed de Threat Intel não é medida por quantos itens ele contém, mas pela relevância do que ele bloqueia hoje.*

---

### 🤝 De Projeto Fechado para Open Source

O NRA Sentinel nasceu como um projeto exclusivo para os membros do canal NetworkRA. Essa fase restrita foi essencial como um período de homologação, onde validamos a estabilidade do código, a precisão da Safelist e a performance do motor no FortiOS.

Hoje, com o motor maduro e validado (incluindo *0-days* e infraestruturas maliciosas antes mesmo das assinaturas oficiais), decidi **abrir 100% do projeto para a comunidade**. Acredito que a defesa cibernética se faz em conjunto e que proteger infraestruturas críticas não deve ter barreiras. 

O tempo dedicado a este projeto é a minha contribuição para fortalecer o nosso ecossistema.

Você tem à disposição os seguintes conectores para o seu firewall:

* **IP Threat Feed:** Lista de endereços IPs validados para políticas de bloqueio (Firewall Policy).
* **Domain Threat / FortiGuard Category Feed:** FQDNs e URLs para proteção de DNS (DNS Filter) ou URL (Web Filter).
* **Malware Hash Feed:** Assinaturas de arquivos para reforço do motor de Antivírus com a nossa lógica de *Rolling Buffer* (AV Profile).

---

### 💎 Como Acessar e Acompanhar

Siga os passos abaixo para proteger o seu ambiente:

1. **Implementação Direta:** Vá até o final desta página, abra o **Guia de Configuração Rápida (FortiOS CLI)**, copie os scripts e aplique diretamente no seu equipamento.
2. **Acompanhe ao Vivo (O Pulse do Sentinel):** Criei um canal exclusivo no Telegram onde a nossa API dispara alertas em tempo real sempre que um novo *0-day*, IP malicioso ou hash entra na lista de bloqueio.
🔗 **Entre no grupo e acompanhe os bloqueios:** [Telegram - NRA Sentinel Alerts](https://t.me/+jHlbAlp-7Xg0MTJh)
3. **Apoie o Canal (Opcional):** O uso do Sentinel é 100% gratuito. No entanto, se o projeto agregar valor à sua operação corporativa e você quiser apoiar a evolução da ferramenta e de mais recursos, considere se tornar membro do [Canal NetworkRA no YouTube](https://www.youtube.com/channel/UCs8isxhuF4phuQXimE52tOg/join). Além de apoiar o projeto, você ganha acesso a Laboratórios Práticos (Hands-on): Tudo o que você precisa para montar seu lab do zero. Imagens, arquivos VMware e topologias .unl prontas para importar no EVE-NG, simulando as estruturas MSSP mais procuradas do mercado. Automação & Gestão: Automações em Python, rotinas de backup e Study Guides completos. Inteligência para FortiAnalyzer: Templates de relatórios (Reports) avançados, Handlers e Correlation Handlers prontos para implementar. Agentes de IA (GEMS Pro): Acesso direto aos nossos agentes personalizados do Gemini Pro, altamente treinados e especializados em arquitetura e troubleshooting de produtos Fortinet.

---

### 🚀 Changelog: NRA Sentinel V33.5 (28/06/2026)
**Foco da Atualização: Sanitização Avançada de IoCs e Estabilidade (FortiOS)**

Nesta atualização, aplicamos um patch crítico na engenharia de higienização de dados (Data Sanitization) para garantir 100% de compatibilidade com o motor de leitura do FortiGate:

* 🧹 **Filtro de Query Strings (URL Clean-up):** Aprimoramento da função `clean_indicator` no Python para identificar e remover automaticamente parâmetros de busca web e caracteres especiais (como `?` e `&`) de domínios reportados "sujos" pelas fontes globais. O motor agora extrai estritamente o FQDN (Fully Qualified Domain Name).
* 🛡️ **Prevenção de Falhas no External Connector:** Esse tratamento resolve erros pontuais de *parsing* (falha de leitura de sintaxe) que faziam o *daemon* do FortiOS rejeitar linhas inválidas. Isso garante que o firewall absorva a lista de ameaças na íntegra, sem abortar a sincronização.

### 🚀 Changelog: NRA Sentinel V33.5 (24/05/2026)
**Foco da Atualização: Zero Falsos Positivos e Auditoria de Memória**

Nesta versão, a arquitetura do Sentinel recebeu aprimoramentos críticos voltados para operações de missão crítica em ambientes MSSP:

* 🛡️ **Motor de Exceção Híbrida (Safelist):** Introduzido o arquivo `nra-safelist.txt` na raiz do repositório. Atuando como *Single Source of Truth*, o motor cruza IPs táticos identificados contra uma base de provedores DNS globais legítimos antes da aplicação do bloqueio. Isso mitiga riscos de indisponibilidade acidental em redes de produção.
* ⚙️ **Engenharia de Memória FIFO Absoluta:** O motor de armazenamento local foi reescrito (migração de `sets` para `dicts`) para garantir ordem cronológica perfeita (First-In, First-Out). Novos IoCs (IPs, Domínios e Hashes) entram estritamente no fim da fila, mantendo a integridade temporal da esteira de ameaças.
* 📊 **Auditoria de Rotação (Churn Visibility):** Visibilidade total sobre o descarte de artefatos antigos. Ao atingir o limite de proteção de RAM dos equipamentos (35.000 indicadores por categoria), o pipeline do GitHub Actions agora registra no console o cálculo exato do excedente e exibe os artefatos antigos que estão sendo removidos para dar lugar aos novos *0-days*.

---

### <mark>&nbsp;🚀 Guia de Configuração Rápida (FortiOS CLI)&nbsp;</mark>

<details>
<summary><b>👉 Clique aqui para expandir o Script de Configuração</b></summary>

<br>

#### --- CONFIGURAÇÃO DOS RECURSOS EXTERNOS (LISTAS GLOBAIS) ---
```
config system external-resource
    edit "OpenDBL_blocklist.de"
        set type address
        set resource "https://opendbl.net/lists/blocklistde-all.list"
        set refresh-rate 1440
    next
    edit "OpenDBL_BruteForce"
        set type address
        set resource "https://opendbl.net/lists/bruteforce.list"
        set refresh-rate 1450
    next
    edit "OpenDBL_TOR"
        set type address
        set resource "https://opendbl.net/lists/tor-exit.list"
        set refresh-rate 1460
    next
    edit "OpenDBL_Threats"
        set type address
        set resource "https://opendbl.net/lists/etknown.list"
        set refresh-rate 1470
    next
    edit "OpenDBL_IPSum"
        set type address
        set resource "https://opendbl.net/lists/ipsum.list"
        set refresh-rate 1490
    next
    edit "blocklist.de"
        set type address
        set resource "https://blocklist.de/lists/all.txt"
        set refresh-rate 1440
    next
    edit "cinsscore"
        set type address
        set resource "http://cinsscore.com/list/ci-badguys.txt"
        set refresh-rate 1480
    next
    edit "Serpro"
        set type address
        set resource "https://s3.i02.estaleiro.serpro.gov.br/blocklist/blocklist.txt"
        set refresh-rate 1490
    next
end
```
#### --- CONFIGURAÇÃO DO NRA SENTINEL ---
```
config system external-resource
    edit "NRA_Sentinel_IPs"
        set type address
        set username "networkra"
        set password ENC weGSO5BLSuVszyE4uZR3Ch/6rVXkC9IRunTQm9QlA5xLErpSM6Ihs4HObBNz5OatXT/Yi/9Ja7xH32mvy0hh2MUxW3T7PaxkMZNdDWCwayrUJwBd4F53SewLaHfQljZoYaYtUHXTsYev9uvDFxX+ofz/CMs/55Na24wLxCW/PUIsS5j9mAphzUVXBwRgfHNVy2RlZ1lmMjY3dkVA
        set resource "https://raw.githubusercontent.com/networkra/nra-sentinel-feeds/refs/heads/main/nra-ips-critical-1.txt"
        set refresh-rate 60
    next
    edit "NRA_Sentinel_Domain-WF"
        set category 193
        set username "networkra"
        set password ENC z002m8awkQtezNKjUdnrdmezsHqmgNLINfP9mkJELYn4p0V4R9zqkdqYhusisbpFL56baGW+k7GGov6jKI8B084mLcp+p7qWwbA6VsNrmPlp9+YrG9pKo+EIAYdx7X2qq9GF9sXdxkQoPxvaLnMzr17Bh6iEyke5SwalpgrhKixaa4P4LMZQZGrkH1prnsrCeY3Xc1lmMjY3dkVA
        set resource "https://raw.githubusercontent.com/networkra/nra-sentinel-feeds/refs/heads/main/nra-dom-critical-1.txt"
        set refresh-rate 60
    next
    edit "NRA_Sentinel_Malware-Hash"
        set type malware
        set username "networkra"
        set password ENC ajetWcf3r0vpYq/CgQbCUzN4RU1mGo2edJxKZlAHseAoPJVZ/u72nsScyIFRTRf3GJAwlX43cYcdkd2gCbG7Bo2kR4Li9YSM7mOPl+iKNlmK5ONSG0W6VrTT5Frq8Wo3csdc3Xj3cEgd/3BoRdZbP2id8xi+FNxWJenoMW/+7v4RvlB2POobmFyRpFWfHgVXdhgNcFlmMjY3dkVA
        set resource "https://raw.githubusercontent.com/networkra/nra-sentinel-feeds/refs/heads/main/nra-hash-critical-1.txt"
        set refresh-rate 60
    next
end
```
#### --- CRIAÇÃO DA POLÍTICA DE DENY (TOP OF POLICY) ---
#### Nota: Neste exemplo estruturamos a Policy com o ID 816598, considerando interface Lan e virtual-wan-link.
```
config firewall policy
    edit 816598
        set name "Deny_Threat-Intel"
        set srcintf "Lan"
        set dstintf "virtual-wan-link"
        set srcaddr "all"
        set dstaddr "OpenDBL_blocklist.de" "OpenDBL_BruteForce" "OpenDBL_TOR" "OpenDBL_Threats" "OpenDBL_IPSum" "blocklist.de" "cinsscore" "Serpro" "NRA_Sentinel_IPs"
        set schedule "always"
        set service "ALL"
        set action deny
        set logtraffic all
    next
end
```
#### Mover a regra para o topo (substitua 'ID_DA_PRIMEIRA_REGRA' pelo ID real)
```
config firewall policy
    move 816598 before ID_DA_PRIMEIRA_REGRA
end
```
#### --- APLICAÇÃO NOS PROFILES DE SEGURANÇA ---
Além dos IPs, a inteligência do NRA Sentinel atua nas camadas de inspeção. Aplique os conectores criados aos profiles de segurança que você já utiliza nas suas regras de acesso (Accept).

**1. Proteção contra Malware**
Aplica a lista de hashes bloqueados globalmente pelo Sentinel.
```
config antivirus profile
    edit "SEU_PROFILE_AV"
        set external-blocklist "NRA_Sentinel_Malware-Hash"
    next
end
```
**2. Bloqueio de Domínios Maliciosos**
O feed nra-dom-critical-1.txt pode ser usado tanto no DNS Filter (recomendado para pegar ataques na raiz/resolução) quanto no Web Filter (para inspeção de URL/SNI). Você pode aplicar em um deles ou em ambos, dependendo da arquitetura do seu ambiente. Por experiência, recomendamos o uso no profile de Web Filter.

Opção: Aplicação via Web Filter (Categoria 193)
```
config webfilter profile
    edit "SEU_PROFILE_WF"
        config ftgd-wf
            unset options
            config filters
                edit 0
                    set category 193
                    set action block
                next
            end
        end
    next
end
```
</details>

---

### 🚀 O Foco do Canal

O canal **NetworkRA** é especializado em **Arquitetura MSSP e Segurança de Redes**, focado em desmistificar cenários reais de infraestrutura através de laboratórios práticos (**Hands-on**). Nosso objetivo é transformar teoria complexa em implementações funcionais e resilientes.

* **SD-WAN Expert:** Especialização em estruturas de *Self-Healing* utilizando BGP e *SLA-based steering* (Lowest Cost, Preferência de Network), além de técnicas de **Hardening** para proteger o plano de controle.
* **VPN & ADVPN Profissional:** Domínio completo de topologias *Hub-and-Spoke*, ADVPN (Single/Multiple Hub), integrações com OSPF (HUBs) Regionais e Hardening proposto com IKEv2 e local-in-policy.
* **Remote Access & Autenticação:** Implementações robustas de VPN Client com **IKEv2 + EAP**, integração com **RADIUS** (Multi-group membership) e autenticação by DC.
* **Automação & Gestão:** Desenvolvimento de ferramentas de automação (Python Scripts) como o *SD-WAN Builder* e gestão centralizada via FortiManager/FortiAnalyzer.
* **Metodologia Hands-on:** Todo o conteúdo é validado em cenários reais utilizando o **EVE-NG**, com arquivos de laboratório exclusivos para membros no Google Drive.
* **Acessibilidade Global:** Vídeos produzidos em Português com legendas profissionais revisadas em **Inglês** e **Espanhol**.

Embora o idioma principal do canal seja o **Português**, acreditamos na democratização do conhecimento técnico:

* **Legendas Profissionais:** Todos os vídeos possuem legendas revisadas manualmente em **Inglês** e **Espanhol**.
* **Comunidade Global:** Profissionais à nível global já utilizam as nossas arquiteturas como referência.

---

### 👨‍💻 Sobre o Autor

**Robert Alexandrino (NetworkRA)** - *Especialista em Arquiteturas MSSP (SD-WAN) & Network Security Engineer*

Acredito que o compartilhamento técnico deve caminhar junto com a valorização do tempo e do esforço. O tempo é o nosso recurso mais escasso; valorizá-lo é respeitar a sua própria jornada.

#### 🎓 Certificações Fortinet

| Certificação | Tecnologia | Status |
| :--- | :--- | :--- |
| **FCSS** | Enterprise Firewall 7.6 Administrator | Pass (2026) |
| **FCSS** | Enterprise Firewall 7.4 Administrator | Pass (2025) |
| **FCSS** | Network Security 7.4 Support Engineer | Pass (2025) |
| **NSE 7** | SD-WAN 7.2 | Pass (2024) |
| **NSE 7** | Enterprise Firewall 7.0 | Pass (2023) |
| **NSE 5** | FortiAnalyzer 6.4 | Pass (2022) |
| **NSE 5** | FortiManager 6.4 | Pass (2022) |
| **NSE 4** | FortiOS 6.4 | Pass (2021) |

---

* [![LinkedIn](https://img.shields.io/badge/LinkedIn-NetworkRA-blue?style=flat&logo=linkedin)](https://www.linkedin.com/in/networkra/)
* [![YouTube](https://img.shields.io/badge/YouTube-NetworkRA-red?style=flat&logo=youtube)](https://www.youtube.com/@NetworkRA)
---

> **Nota de Responsabilidade**:
> A inteligência do Sentinel é baseada em fontes de terceiros. Embora o esforço para minimizar erros seja constante, a decisão final de bloqueio e o monitoramento do tráfego são de responsabilidade do administrador da rede. Vamos sempre trabalhar com cautela e monitoramento.

---

### ⚖️ Licença e Copyright

Este projeto é desenvolvido e mantido por **Robert Alexandrino (NetworkRA)**.

© 2026 NetworkRA. Todos os direitos reservados.
O uso deste feed é exclusivo para os membros do canal NetworkRA.

🔗 **[Acompanhe no LinkedIn](https://www.linkedin.com/in/networkra/)** | 📺 **[Inscreva-se no Canal NetworkRA](https://www.youtube.com/@NetworkRA)**
🔗 **Entre no grupo e acompanhe os bloqueios:** [Telegram - NRA Sentinel Alerts](https://t.me/+jHlbAlp-7Xg0MTJh)

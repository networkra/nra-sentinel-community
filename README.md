## 🛡️ NRA Sentinel - Inteligência de Ameaças (V33.5)

O **NRA Sentinel** é um projeto desenvolvido com o objetivo de auxiliar profissionais de segurança e redes na proteção de suas infraestruturas. Ele automatiza a coleta e a organização de dados de ameaças globais, entregando listas limpas e prontas para uso nos **External Connectors** do FortiGate. Este motor não busca ser uma "solução milagrosa", mas sim uma ferramenta de apoio que soma forças aos recursos que você já utiliza no dia a dia.

---

### 🤝 Valorização e Colaboração

Gostaria de compartilhar com vocês o racional por trás deste projeto:

Eu adoraria poder distribuir essa infraestrutura de forma totalmente aberta para toda a comunidade. Porém, o desenvolvimento do NRA Sentinel exige muitas horas de estudo, programação e tempo dedicado para manter o projeto ativo.

A decisão de manter o acesso exclusivo para os membros do canal **não visa o enriquecimento próprio**, mas sim:

1. **Ser justo com quem apoia meu trabalho:** É uma forma de retribuir aos amigos que acreditam e contribuem no canal NetworkRA.
2. **Sustentabilidade do Projeto:** O apoio dos membros é sempre muito importante e me motiva a continuar evoluindo a ferramenta para todos nós.
3. **Valorização Profissional:** Acredito que o compartilhamento técnico deve andar junto com a valorização do tempo e do esforço do profissional.

---

### 🧠 Fontes de Dados

O motor busca informações em fontes respeitadas mundialmente, garantindo que o que chega ao seu firewall tenha passado por um processo de filtragem:

| Player | O que ele faz no projeto? |
| :--- | :--- |
| **AlienVault (LevelBlue)** | Fornece inteligência estratégica sobre campanhas de Ransomware e 0-days. |
| **MalwareBazaar (abuse.ch)** | Entrega assinaturas de arquivos (Hashes) validadas pela comunidade. |
| **URLHaus (abuse.ch)** | Monitora links que estão distribuindo malware no exato momento. |
| **AbuseIPDB** | Ajuda a validar a reputação dos IPs, evitando falsos positivos. |
| **urlscan.io** | Verifica o histórico de segurança dos domínios e URLs processadas. |

---

### 📦 Feeds Disponíveis para Membros:
* **IP Threat Feed:** Lista de endereços IPs validados para políticas de bloqueio (Firewall Policy).
* **Domain Threat Feed:** FQDNs e URLs para proteção de DNS, Web Filter ou políticas de bloqueio (Firewall Policy).
* **Malware Hash Feed:** Assinaturas de arquivos para reforço do motor de Antivírus (AV Profile).

---

### ⚙️ Detalhes do Funcionamento

* **Atualização:** Os feeds são processados a cada 4 horas.
* **Persistência:** O motor mantém o histórico, não apagando o que já foi validado anteriormente.
* **Otimização:** Arquivos organizados com até 49.999 linhas para garantir performance no FortiOS.
* **Limpeza:** Todos os dados passam por uma sanitização para remover protocolos e caminhos, mantendo apenas o que o firewall precisa ler.

---

### 💎 Como solicitar seu acesso

Se você deseja utilizar esses feeds no seu ambiente e apoiar o canal, o fluxo é simples:

1. **Seja Membro:** Torne-se um membro do [Canal NetworkRA no YouTube](https://www.youtube.com/channel/UCs8isxhuF4phuQXimE52tOg/join).
2. **Aguarde 7 dias:** Por questões de segurança, o acesso é liberado após a primeira semana de assinatura.
3. **Fale comigo no LinkedIn:** Envie uma mensagem no meu **chat privado do LinkedIn** (link abaixo) informando seu usuário de membro.
4. **Liberação:** Eu mesmo farei a validação e enviarei seu **Token Pessoal** de acesso para configuração do External Connector.

---

### 👨‍💻 Sobre o Autor

**Robert Alexandrino (NetworkRA)** - Analista de Redes e Segurança.

* [![LinkedIn](https://img.shields.io/badge/LinkedIn-NetworkRA-blue?style=flat&logo=linkedin)](https://www.linkedin.com/in/networkra/)
* [![YouTube](https://img.shields.io/badge/YouTube-NetworkRA-red?style=flat&logo=youtube)](https://www.youtube.com/@NetworkRA)

---

> [!CAUTION]
> **Nota de Responsabilidade:**
> A inteligência do Sentinel é baseada em fontes de terceiros. Embora o esforço para minimizar erros seja constante, a decisão final de bloqueio e o monitoramento do tráfego são de responsabilidade do administrador da rede. Vamos sempre trabalhar com cautela e monitoramento.

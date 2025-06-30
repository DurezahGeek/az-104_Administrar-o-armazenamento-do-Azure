# ✅ Administração do Armazenamento no Azure – AZ-104

## ☁️ 1. Conta de Armazenamento (Storage Account)

Serviço base para armazenar blobs, arquivos, tabelas, filas e mais.

### Configurações ao criar:

- Tipo: General Purpose v2, BlobStorage, etc.
- Desempenho: Standard ou Premium
- Localização: Região Azure
- Replicação: LRS, ZRS, GRS, RA-GRS, GZRS, RA-GZRS

❗ **Importante**: tipo e desempenho **não podem ser alterados** após a criação.

---

## 📦 2. Tipos de Armazenamento

| Tipo   | Finalidade |
|--------|------------|
| Blob   | Arquivos não estruturados (imagens, vídeos, backups) |
| File   | Compartilhamento via SMB |
| Table  | Dados NoSQL em chave-atributo |
| Queue  | Mensagens assíncronas para apps distribuídos |

---

## 🧱 3. Replicação de Dados

| Modelo     | Localização          | Cópias | Acesso Secundário | Observações |
|------------|----------------------|--------|--------------------|-------------|
| **LRS**    | Mesma região         | 3      | ❌                 | Baixa tolerância a falhas |
| **ZRS**    | 3 zonas da mesma região | 3  | ❌                 | Alta disponibilidade regional |
| **GRS**    | Região secundária     | 6      | ❌                 | Proteção contra desastre |
| **RA-GRS** | Região secundária     | 6      | ✅                 | Leitura secundária |
| **GZRS**   | Zonas + região secundária | 6  | ❌              | Proteção máxima |
| **RA-GZRS**| Zonas + região secundária | 6  | ✅              | Proteção máxima + leitura |

---

## 🔐 4. Ponto de Extremidade (Endpoint)

Cada serviço de armazenamento possui uma URL única:

| Serviço | Formato do Endpoint |
|---------|---------------------|
| Blob    | `https://<conta>.blob.core.windows.net` |
| File    | `https://<conta>.file.core.windows.net` |
| Table   | `https://<conta>.table.core.windows.net` |
| Queue   | `https://<conta>.queue.core.windows.net` |

### 🔒 Acesso Seguro aos Endpoints:

- **Firewalls e redes virtuais (VNets)**
- **Private Endpoints** com IP privado
- **Domínio personalizado** via CNAME

---

## 📊 5. Tipos de Conta

- **General Purpose v2**: mais versátil, recomendado.
- **BlobStorage/Premium**: foco em desempenho e uso específico.

---

## 🧊 6. Camadas de Acesso (Blob Storage)

| Camada   | Frequência | Custo Armazenamento | Uso Ideal |
|----------|------------|---------------------|-----------|
| Hot      | Frequente  | Alto                | Sites e apps ativos |
| Cool     | Ocasional  | Médio               | Backups e logs |
| Archive  | Raro       | Muito baixo         | Arquivamento de longo prazo |
| Inferido | Automática | Variável            | Sem definição explícita |

---

## 🔁 7. Gerenciamento do Ciclo de Vida

- Mover automaticamente entre camadas (ex: Hot → Archive)
- Regras com base em:
  - Data de modificação
  - Prefixo, tipo, tags
  - Apenas para **GPv2 ou Blob Storage**

---

## 🌍 8. Replicação de Objetos do Blob

- Copia blobs entre contas e regiões diferentes
- Requisitos:
  - Habilitar versionamento
  - Contas GPv2
- **Unidirecional** e **assíncrona**

---

## 🔐 Estratégias de Segurança

### 1️⃣ Criptografia

- **Em repouso**: AES-256, ativada por padrão
  - Gerenciada pela Microsoft (default)
  - Gerenciada pelo cliente (Key Vault)

- **Em trânsito**:
  - HTTPS obrigatório
  - SMB 3.0 com criptografia
  - Criptografia no cliente (opcional)

### 2️⃣ Autenticação com Azure AD + RBAC

- Controle de acesso baseado em função
- Reduz o uso de chaves da conta

### 3️⃣ SAS Token (Shared Access Signature)

- Acesso temporário e controlado
- Tipos:
  - SAS de Conta
  - SAS de Serviço

- Boas práticas:
  - HTTPS sempre
  - Tempo e permissões mínimas
  - Monitoramento com Storage Analytics

### 4️⃣ Acesso Anônimo

- Permitido apenas se necessário
- ⚠️ **Desencorajado em produção**

---

## 📁 Gerenciamento de Arquivos (Azure Files)

### 🔹 Cotas

- Definir limite de armazenamento por compartilhamento

### 🖥️ Montagem

- **Windows**: porta 445 deve estar aberta
- **Linux/macOS**: usar `mount` ou `mount_smbfs`

### 🧊 Snapshots

- Cópias somente leitura do momento
- Permitem restauração e proteção contra exclusões acidentais

---

## 🛠️ Ferramentas e Utilitários

### 🔍 Azure Storage Explorer

- Interface gráfica para:
  - Blobs, Files, Queues, Tables, Cosmos DB
- Geração de tokens SAS
- Multiplataforma

### 📦 Azure Import/Export

- Envio físico de discos criptografados para grandes volumes

### ⚙️ AzCopy (CLI)

- Copiar dados entre local ↔ Azure
- Suporte a autenticação via:
  - Azure AD
  - SAS Token

---

## 📘 Tabela de Perguntas e Respostas – Armazenamento no Azure

## 📘 Tabela de Perguntas e Respostas – Armazenamento no Azure

| Pergunta | Resposta Correta |
|---------|------------------|
| Qual tipo de armazenamento permite navegação via File Explorer, uso de SMB 3.0 e suporte a cotas? | Azure Files |
| Qual opção de replicação mantém 6 cópias e replica para região secundária por padrão? | GRS (Geo-Redundant Storage) |
| Como sincronizar arquivos locais com o Azure Files de forma bidirecional? | Implantar a Sincronização de Arquivos do Azure (Azure File Sync) |
| Ferramenta para mover dados entre contas de armazenamento? | AzCopy |
| Diferença entre Snapshot e Backup no Azure Files? | Snapshot é instantâneo e somente leitura; backup é regular e gerenciável |
| Ferramenta gráfica (fora do portal) para gerenciar Azure Storage? | Azure Storage Explorer |
| Vantagem principal do Azure Files? | Compartilhamento de arquivos na nuvem sem servidor físico |
| Quando usar Azure Files em vez de Blob? | Para compartilhamento com mapeamento de unidade |
| Ferramenta CLI para transferências pequenas para o Storage? | AzCopy |
| Função do Azure Storage Explorer? | Gerenciar recursos de armazenamento de forma visual |
| Problema ao mapear Azure Files localmente? | Verificar se a porta 445 está liberada |
| Pode mudar de Premium para Standard após criar a Storage Account? | Não. É necessário criar uma nova conta |
| Qual replicação oferece maior disponibilidade entre regiões? | RA-GZRS |
| Forma mais segura de conceder acesso temporário? | SAS Token (Shared Access Signature) |
| Como restaurar blob excluído acidentalmente? | Soft Delete (Exclusão reversível) |
| Qual modelo copia dados para outra região em datacenters distantes? | GRS |
| Limitação do LRS? | Replicação apenas em um único datacenter |
| Diferença do RA-GRS em relação ao GRS? | Permite leitura da região secundária |
| Quando usar GRS? | Para proteção contra desastres regionais |
| Nomenclatura correta de Storage Account? | 3-24 caracteres, único globalmente, sem maiúsculas ou especiais |
| Benefício do Hierarchical Namespace? | Diretórios e controle de ACL (útil para big data) |
| Diferença entre Hot e Cool? | Hot = acesso frequente; Cool = custo menor para dados raros |
| Cobrança do modelo Premium? | Por espaço reservado, mesmo se não for usado |
| Para que serve o Soft Delete? | Recuperar dados excluídos dentro de um período |
| Solução para alta performance de logs críticos? | Blob Storage com blobs anexados (Append Blobs) |
| Armazenamento ideal: REST API, global, privado, não estruturado? | Azure Blob Storage com RA-GZRS |
| Melhor estratégia para vídeos críticos e não críticos? | Duas Storage Accounts: GRS para críticos, LRS para não críticos |
| Mudança imediata entre camadas? | Hot para Cool |
| Camada ideal para acesso raro e recuperação demorada? | Archive |
| Regra de ciclo de vida permite...? | Mover/excluir blobs com base em data de modificação ou acesso |
| O que ocorre ao mover blob para Archive? | Fica inacessível até reidratação (horas) |
| Objetivo das camadas de acesso? | Otimizar custo conforme frequência de acesso |
| O que se pode configurar ao enviar um blob? | Camada de acesso (Hot, Cool, Archive) |
| Risco de basear regra só na última modificação? | Arquivos acessados podem ser indevidamente movidos/excluídos |
| Função do SAS Token? | Acesso temporário e controlado a blobs |
| Relação entre container e blob? | Blobs estão dentro de containers em uma Storage Account |
| Regra de rede padrão da Storage Account? | Permitir todas as conexões de todas as redes |
| Acesso automatizado temporário para não-produção? | SAS Token |
| Acesso temporário somente leitura ao container “mídia”? | Gerar um SAS Token com permissões mínimas |
| Blob Storage permite alternância de camadas? | Sim, entre Hot, Cool e Archive |
| Responsabilidade do cliente sobre segurança? | Configurar criptografia, acesso e proteção dos dados |
| Função da chave gerenciada pelo cliente? | Controle sobre criptografia e rotação da chave |
| Solução simples para criptografar dados locais antes de subir ao Azure? | Criptografia do Serviço de Armazenamento do Azure |
| O que fazer se a aplicação estiver exposta à internet? | Permitir leitura e restringir modificação |
| O que é um SAS Token? | URL temporária com acesso controlado a recursos |
| O que ocorre com “expiração de chave” ativada? | A chave de acesso expira automaticamente após tempo definido |
| Como o Azure protege dados em trânsito? | Criptografia automática via HTTPS/SMB 3.0 |
| Risco de usar chave de conta em vez de SAS? | Acesso total a todos os dados, sem controle granular |


---

## 📚 Fontes Oficiais

- [🔗 Delegate access with SAS (Microsoft Docs)](https://learn.microsoft.com/pt-br/rest/api/storageservices/delegate-access-with-shared-access-signature)
- [🔗 Criptografia do Serviço de Armazenamento](https://learn.microsoft.com/azure/storage/common/storage-service-encryption)
- [🔗 Guia AZ-104 - Ex. 11 - Microsoft CloudLabs](https://mslabs.cloudguides.com/en-us/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2011)
- [🔗 Redundância de Armazenamento](https://learn.microsoft.com/pt-br/azure/storage/common/storage-redundancy)
- [🔗 Guia AZ-900 - Exercício 5 - Microsoft CloudLabs](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%205)

---


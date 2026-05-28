# OrbitaAlerta — Master Doc

> **Projeto Global Solution 2026 — FIAP**
> Tema: Espaço · Subtema: Economia espacial downstream aplicada a desastres territoriais
> Versão: 1.0 · Data: 27/05/2026

---

## 0. Como usar este documento

Este Master Doc é o **artefato vivo** que coordena o trabalho dos 5 integrantes do grupo nas 7 disciplinas exigidas pela Global Solution 2026. Ele consolida o descritivo de produto, a arquitetura TOGAF/ArchiMate exigida pela disciplina **Testing, Compliance & Quality Assurance** (peso total 10), o plano Agile de execução, a divisão de papéis, as métricas de QA e o roteiro do vídeo-pitch.

A entrega oficial da disciplina principal é descrita na seção 7. Os diagramas ArchiMate prontos para o app Archi acompanham este Master Doc em arquivo separado (`OrbitaAlerta_Archimate.xml`). Um resumo curto, entregável avulso, está em `OrbitaAlerta_Resumo_Breve.md`.

---

## 1. Identidade do projeto

**Nome:** OrbitaAlerta
**Tagline:** *Da órbita ao talhão em minutos.*
**Categoria:** Plataforma space-tech downstream para resposta rápida a queimadas e desastres territoriais
**Hipótese central (acadêmica):** combinar foco satelital, validação por visão computacional, risco meteorológico, contexto territorial e operação mobile offline reduz o tempo entre detecção e primeira ação e melhora a confiabilidade operacional em comparação ao uso isolado de mapas de foco.

OrbitaAlerta não tenta construir satélite nem competir com fornecedores de imagens. Ele se posiciona na camada de **decisão operacional**: recebe sinais públicos do INPE e da NASA FIRMS, valida focos com visão computacional sobre imagens Sentinel, enriquece com risco meteorológico e telemetria IoT local, calcula um *score* de prioridade com Machine Learning, e despacha a ação certa para a equipe certa por meio de um app mobile que funciona mesmo sem sinal.

---

## 2. Descritivo breve do projeto *(peso 1 da disciplina principal)*

OrbitaAlerta é uma plataforma space-tech brasileira de resposta rápida a queimadas e incêndios em áreas rurais e periurbanas. O sistema recebe focos de calor em tempo quase real do INPE e da NASA FIRMS, valida cada foco com um módulo de visão computacional que cruza imagens Sentinel-2 e Sentinel-1, enriquece o sinal com risco meteorológico e contexto territorial (propriedades, talhões, ativos críticos, rotas de brigada) e atribui um *score* de prioridade calculado por um modelo de Machine Learning. O resultado é entregue ao usuário final por meio de um painel web para coordenação e de um app mobile offline-first para brigadas e equipes de campo, que registra evidências fotográficas, gera laudo pós-evento e sincroniza com o servidor assim que houver conectividade. Uma estação IoT microclimática complementa o contexto local enviando dados de temperatura, umidade e vento por MQTT.

O escopo do projeto contempla quatro frentes integradas: ingestão de dados satelitais e meteorológicos públicos, validação automatizada por visão computacional, motor de priorização baseado em Machine Learning e operação de campo offline. O público-alvo primário são **cooperativas agropecuárias, produtores médios organizados, prefeituras e consórcios municipais e empresas com metas ESG ou ativos territoriais**. O projeto se enquadra no segmento *downstream* da economia espacial — capturar valor a partir da infraestrutura orbital já existente — e responde diretamente à dor de fragmentação de fontes, ruído de detecção bruta e baixa conectividade rural diagnosticada pelo INPE, Ibama, Anatel, Cetic.br e CEPAL.

---

## 3. Contexto e justificativa

A economia espacial global é projetada para atingir US$ 1,8 trilhão até 2035, segundo o World Economic Forum e a McKinsey, partindo de US$ 630 bilhões em 2023, e o maior crescimento está nas camadas downstream — software, analytics e serviços derivados de observação da Terra. O Brasil reúne três condições que tornam o tema queimadas particularmente relevante para uma solução acadêmica forte: (i) abundância de dados públicos — o INPE publica focos a cada 10 minutos, eventos de fogo de hora em hora e risco de fogo observado e previsto para três dias, e a NASA FIRMS oferece API aberta de hotspots; (ii) magnitude do problema — o MapBiomas Fogo registrou 30 milhões de hectares queimados em 2024, 62% acima da média histórica, com a Amazônia respondendo por 52% da área e o Cerrado por 35%; (iii) gargalo estrutural de conectividade — 66% das localidades rurais ainda não têm cobertura 4G, segundo a Anatel, e 54% da população rural está na pior faixa de conectividade significativa, segundo o Cetic.br.

A dor real, portanto, não é "ausência de dado". É a fragmentação entre fontes, o ruído da detecção bruta (o Ibama documenta erro médio de cerca de 400 m, desvio padrão de cerca de 3 km, 80% das detecções num raio de 1 km, além de falsos positivos por brilho solar, água brilhante, solo claro e bordas costeiras), e a impossibilidade de transformar dado orbital em ação local sem uma camada de software que opere em baixa conectividade. OrbitaAlerta endereça exatamente essa lacuna.

---

## 4. Stakeholders, drivers, objetivos e requisitos *(peso 3 — Visão da arquitetura)*

### 4.1 Stakeholders

| Stakeholder | Interesse | Influência |
|---|---|---|
| Cooperativa agropecuária | Reduzir perdas por fogo, dar previsibilidade a cotistas | Alta — cliente pagante |
| Produtor médio organizado | Receber alerta priorizado por talhão e propriedade | Alta — usuário final |
| Prefeitura / Defesa Civil municipal | Painel único de decisão e evidência pós-evento | Alta — cliente B2G |
| Brigada de campo / corpo de bombeiros voluntário | App de campo objetivo e offline | Alta — operador |
| Empresa com metas ESG / certificação carbono | Laudo digital, rastreabilidade, base para auditoria e seguro | Média-alta — pagante |
| INPE / NASA / ESA | Fonte de dados públicos | Baixa — fornecedor externo |
| Operadora de conectividade (incl. satelital) | Parceiro de cobertura rural híbrida | Média — parceiro |
| Universidade (FIAP) e corpo docente | Avaliação acadêmica e P&D | Alta — avaliação |
| Equipe interna do produto (5 integrantes) | Executar entregas dentro do prazo | Alta — entrega |

### 4.2 Drivers de projeto

| Driver | Descrição |
|---|---|
| D1 — Risco climático crescente | Seca 2023-2024 atingiu 59% do território; hotspots se intensificam. |
| D2 — Fragmentação de fontes | INPE, NASA FIRMS, ALARMES, Brasil MAIS, Sentinel — informação dispersa. |
| D3 — Ruído da detecção bruta | Falsos positivos e omissões por nuvem, canopy, espectro e duração. |
| D4 — Conectividade rural deficiente | 66% das localidades rurais sem 4G; sync intermitente é regra, não exceção. |
| D5 — Pressão regulatória ESG / seguro | Necessidade de comprovar incidente, área afetada e reação. |
| D6 — Custo de soluções enterprise | Soluções existentes excluem produtor médio e prefeitura pequena. |

### 4.3 Objetivos (goals) e metas (outcomes)

| Goal | Outcome mensurável |
|---|---|
| G1 — Encurtar o ciclo detecção → primeira ação | Tempo médio entre alerta priorizado e aceite ≤ 8 minutos |
| G2 — Aumentar a confiabilidade do alerta | Falso-positivo percebido pelo usuário ≤ 15% |
| G3 — Operar em baixa conectividade | ≥ 80% das ocorrências fechadas sem conexão contínua |
| G4 — Gerar evidência auditável | 100% das ocorrências com laudo PDF assinado digitalmente |
| G5 — Integrar com fontes públicas e privadas | INPE + NASA FIRMS + Sentinel ingeridos com ≥ 99% de uptime |
| G6 — Viabilizar uso por produtor médio | Onboarding de uma cooperativa em ≤ 1 dia |

### 4.4 Requisitos funcionais (RF)

| ID | Requisito | Disciplina(s) primária(s) |
|---|---|---|
| RF-01 | Ingestão automática de focos do INPE a cada 10 minutos e da NASA FIRMS sob demanda | SoA/Webservices, ML |
| RF-02 | Validação de cada foco por visão computacional sobre tile Sentinel-2/1 | Physical Computing IoT & IoB, ML |
| RF-03 | Cálculo de score de prioridade (0–100) por foco usando ML | Machine Learning |
| RF-04 | Geofencing por propriedade, talhão e ativo crítico | SoA/Webservices |
| RF-05 | Painel web para coordenação e despacho | SoA/Webservices, C# |
| RF-06 | App mobile React Native offline com mapa, checklist, evidência foto | Mobile & IoT |
| RF-07 | Estação IoT microclimática (temperatura, umidade, vento) via MQTT | Physical Computing IoT & IoB |
| RF-08 | Geração de laudo PDF pós-evento com linha do tempo e área afetada | C# Software Development |
| RF-09 | Autenticação JWT, RBAC (admin, coordenador, brigadista, auditor) | SoA/Webservices, Cibersegurança |
| RF-10 | Sincronização incremental delta entre app e backend | Mobile & IoT, SoA/Webservices |
| RF-11 | Dashboard de KPIs (tempo de resposta, falso-positivo, cobertura) | C# Software Development |
| RF-12 | Logs auditáveis e trilha de evidência criptografada | Cibersegurança |

### 4.5 Requisitos não-funcionais e constraints

| ID | Tipo | Especificação |
|---|---|---|
| RNF-01 | Performance | API responde 95% das chamadas em ≤ 300 ms |
| RNF-02 | Disponibilidade | Núcleo de ingestão com SLA 99,5% |
| RNF-03 | Escalabilidade | Suportar 10k focos/dia sem degradação |
| RNF-04 | Resiliência offline | App funcional sem rede por até 72h |
| RNF-05 | Segurança | TLS 1.3 em trânsito; AES-256 em repouso para evidências |
| RNF-06 | Compliance LGPD | Dados pessoais minimizados, consentimento explícito, direito ao esquecimento |
| RNF-07 | Observabilidade | Logs estruturados, métricas Prometheus, tracing OpenTelemetry |
| C-01 | Constraint | Não construir satélite nem operar imagem proprietária |
| C-02 | Constraint | Usar exclusivamente dados públicos ou acordos abertos |
| C-03 | Constraint | Operar em áreas com 2G/EDGE como pior caso; Starlink opcional |
| C-04 | Constraint | Backend núcleo em Java (disciplina SoA); serviço C# auxiliar; CV em Python (disciplina IoT/IoB); app em React Native (disciplina Mobile) |
| C-05 | Constraint | Compatível com Windows Server (disciplina OS) e Linux |

---

## 5. Arquitetura de negócio *(peso 2)*

### 5.1 Atores e papéis

| Ator | Papel | Localidade |
|---|---|---|
| Coordenador de Cooperativa | Triador de alertas, configura áreas monitoradas | Sede da cooperativa |
| Brigadista | Operador de campo, aceita ocorrência, registra evidência | Talhão / área de risco |
| Agente da Defesa Civil | Gestor territorial municipal | Centro de operações da prefeitura |
| Auditor ESG / Seguro | Consumidor de laudo digital | Sede corporativa do cliente |
| Operador OrbitaAlerta (interno) | Suporte, parametrização inicial, treinamento | Centro de operações cloud |
| Administrador de sistema | Gestão de usuários, permissões, integrações | Centro de operações cloud |

### 5.2 Funções de negócio

A operação se organiza em quatro funções de negócio: **Monitoramento Territorial** (ingestão, validação e enriquecimento de focos); **Despacho e Resposta** (priorização, atribuição de ocorrência, comunicação com brigada); **Auditoria e Compliance** (geração de laudo, trilha de evidência, exportação ESG/seguro); e **Suporte e Onboarding** (parametrização de áreas, treinamento, sustentação).

### 5.3 Processos de negócio

#### Processo 1 — Ingestão e priorização de focos
1. Scheduler dispara coleta INPE/NASA FIRMS.
2. Worker geoespacial deduplica e cruza com cadastro territorial.
3. Módulo CV valida foco com imagem Sentinel mais recente.
4. Modelo ML calcula score combinando foco + risco meteorológico + telemetria IoT + contexto histórico.
5. Alerta é classificado (Crítico / Alto / Médio / Baixo) e gravado.

#### Processo 2 — Despacho de brigada
1. Coordenador recebe alerta priorizado no painel web.
2. Coordenador atribui ocorrência a um brigadista.
3. App mobile do brigadista notifica e baixa mapa offline da área.
4. Brigadista aceita ocorrência e segue rota.
5. Brigadista registra evidência fotográfica e checklist no campo.
6. Sistema sincroniza ao reconectar.

#### Processo 3 — Encerramento e laudo
1. Brigadista marca ocorrência como controlada/encerrada.
2. Serviço C# gera laudo PDF com linha do tempo, área afetada e evidências.
3. Laudo é assinado digitalmente e arquivado em object storage.
4. Auditor ESG / seguro recebe link com expiração.

#### Processo 4 — Telemetria IoT contínua
1. Estação microclimática lê sensores a cada 30 s.
2. Dados publicados em tópico MQTT.
3. Broker repassa para serviço de enriquecimento.
4. Dados alimentam re-cálculo de risco e dashboard meteorológico local.

### 5.4 Serviços de negócio entregues

Alerta priorizado por impacto territorial; despacho rastreável de brigada; laudo digital pós-evento; painel territorial unificado; integração ESG/seguro; suporte e onboarding territorial.

---

## 6. Arquitetura de sistema *(peso 2)*

### 6.1 Camadas e componentes

A arquitetura é organizada em **cinco camadas lógicas**, cada uma com componentes claros e contratos de comunicação bem definidos. A escolha por uma stack heterogênea é deliberada: ela atende ao requisito acadêmico de exercitar Java, C#, Python e React Native dentro de um único produto coerente.

**Camada de Apresentação**

| Componente | Tecnologia | Responsabilidade | Disciplina |
|---|---|---|---|
| App Mobile Brigada | React Native (Expo) | Mapa offline, checklist, evidência, sync | Mobile & IoT |
| Painel Web Coordenação | C# ASP.NET Core MVC + Razor / Blazor | Dashboards, gestão de áreas, despacho, KPIs | C# Software Development |

**Camada de API (núcleo SoA)**

| Componente | Tecnologia | Responsabilidade | Disciplina |
|---|---|---|---|
| API Core OrbitaAlerta | Java 21 + Spring Boot 3 | REST principal: usuários, áreas, alertas, ocorrências, laudos, telemetria | SoA/Webservices |
| API Gateway | Spring Cloud Gateway | Roteamento, rate-limit, JWT, observabilidade | SoA/Webservices, Cibersegurança |

**Camada de Domínio / Workers**

| Componente | Tecnologia | Responsabilidade | Disciplina |
|---|---|---|---|
| Worker de Ingestão Satelital | Python (FastAPI + APScheduler) | Pull INPE/FIRMS, deduplicação, normalização | Physical Computing IoT & IoB |
| Módulo de Visão Computacional | Python (OpenCV + PyTorch) | Recorte Sentinel, detecção de cicatriz/fumaça, score de confiança visual | Physical Computing IoT & IoB (obrigatório), ML |
| Serviço de Scoring ML | Python (scikit-learn + XGBoost) | Modelo de priorização com features de foco, meteorologia, território, histórico | Machine Learning |
| Gerador de Laudos | C# .NET 8 (worker service) | Render PDF (QuestPDF), assinatura digital, push para storage | C# Software Development |
| Broker IoT | Eclipse Mosquitto (MQTT 5) | Telemetria de estações, retained messages para últimos estados | Physical Computing IoT & IoB |

**Camada de Dados**

| Componente | Tecnologia | Responsabilidade | Disciplina |
|---|---|---|---|
| Banco Operacional | PostgreSQL 16 + PostGIS 3.4 | Áreas, focos, ocorrências, usuários, telemetria | SoA/Webservices |
| Cache | Redis 7 | Sessões, alertas em janela, locks de fila | SoA/Webservices |
| Object Storage | MinIO (on-prem) ou Azure Blob | Evidências fotográficas, laudos PDF, imagens Sentinel cacheadas | Operating Systems, Cibersegurança |
| Data Lake / Histórico ML | Parquet em object storage | Datasets de treino, replay para retreinamento | Machine Learning |

**Camada de Integração Externa**

| Componente | Tecnologia | Responsabilidade |
|---|---|---|
| Adaptador INPE BDQueimadas | HTTP cliente | Coleta de focos quase em tempo real |
| Adaptador NASA FIRMS | HTTP cliente (API key) | Hotspots por área e país |
| Adaptador Sentinel | Google Earth Engine (Python) | Recorte de imagens para CV |
| Adaptador Meteorológico | INPE risco de fogo + OpenWeather | Enriquecimento contextual |
| Webhook ESG/Seguro | REST out | Envio de laudo assinado |

### 6.2 Padrões de comunicação

- **REST/JSON** entre clientes (mobile, painel web) e API Core, via API Gateway.
- **gRPC interno** entre API Core e workers Python/C# para chamadas síncronas de baixo overhead (validação CV, scoring ML, render PDF).
- **MQTT** entre dispositivos IoT e Broker; o Broker republica eventos relevantes para a API via bridge MQTT→REST.
- **Fila de mensagens** (RabbitMQ ou Redis Streams) para jobs assíncronos: ingestão, retreinamento, geração de laudo.
- **SQL via JDBC/Npgsql** entre API/serviço C# e PostgreSQL.

### 6.3 Componentes de dados principais

`area_monitorada` (geometria PostGIS), `talhao` (FK área), `ativo_critico`, `foco` (lat/long, satélite, timestamp, score CV, score ML, prioridade), `ocorrencia` (foco → brigadista → status), `evidencia` (FK ocorrência, blob ref), `estacao_iot` (device_id, localização), `telemetria` (FK estação, timestamp, sensores), `usuario` + `papel` + `permissao`, `laudo` (FK ocorrência, blob PDF, hash).

---

## 7. Arquitetura de tecnologia *(peso 2)*

### 7.1 Topologia de servidores

A topologia foi pensada para **demonstrar a disciplina de Operating Systems** sem inflar custo: usa máquinas virtuais em Hyper-V (laboratório) com possibilidade de réplica idêntica em Azure (produção simulada). O ambiente é segmentado em três zonas: **DMZ pública** (apenas gateway), **zona de aplicação** (APIs e workers) e **zona de dados** (banco e storage).

| VM | SO | Recursos sugeridos | Componentes hospedados | Zona |
|---|---|---|---|---|
| VM-EDGE | Ubuntu Server 22.04 LTS | 2 vCPU / 4 GB / 40 GB | NGINX + Spring Cloud Gateway + Certbot | DMZ |
| VM-APP-JAVA | Ubuntu Server 22.04 LTS | 4 vCPU / 8 GB / 80 GB | API Java Spring Boot, RabbitMQ | App |
| VM-APP-CS | Windows Server 2022 | 4 vCPU / 8 GB / 80 GB | Painel Web C#, Gerador de Laudos C#, IIS | App |
| VM-WORKERS-PY | Ubuntu Server 22.04 LTS | 4 vCPU / 8 GB / 80 GB (GPU opcional T4) | Worker ingestão, módulo CV, serviço ML | App |
| VM-IOT | Ubuntu Server 22.04 LTS | 2 vCPU / 4 GB / 40 GB | Mosquitto MQTT, bridge para API | App |
| VM-DB | Ubuntu Server 22.04 LTS | 4 vCPU / 16 GB / 200 GB | PostgreSQL 16 + PostGIS, Redis | Dados |
| VM-STORAGE | Ubuntu Server 22.04 LTS | 2 vCPU / 4 GB / 500 GB | MinIO (S3-compatível) | Dados |

**Domínio Windows:** a VM-APP-CS é joined a um Active Directory minimalista (`orbitaalerta.local`) hospedado em uma VM Windows Server adicional para o lab da disciplina de OS, com GPO básica de auditoria e contas de serviço para o IIS.

### 7.2 Topologia de rede

```
                          Internet pública
                                  │
                          [Cloudflare / WAF]
                                  │
                          ┌───────┴───────┐
                          │   VM-EDGE     │  (NGINX + API Gateway, TLS 1.3)
                          └───────┬───────┘
                                  │ (VLAN 10 — DMZ)
            ┌─────────────────────┼─────────────────────┐
            │                     │                     │
    ┌───────┴───────┐    ┌────────┴───────┐    ┌────────┴───────┐
    │  VM-APP-JAVA  │    │   VM-APP-CS    │    │ VM-WORKERS-PY  │
    │   API Core    │◄──►│  Painel + PDF  │    │  CV / ML / Ing │
    └───────┬───────┘    └────────┬───────┘    └────────┬───────┘
            │                     │                     │
            │              (VLAN 20 — Aplicação)        │
            └─────────────────────┼─────────────────────┘
                                  │
                          ┌───────┴───────┐
                          │     VM-IOT    │  (MQTT 8883 TLS)
                          └───────┬───────┘
                                  │
                                  │ (VLAN 30 — Dados, sem rota pública)
                          ┌───────┴───────┐
                ┌─────────┴────┐    ┌─────┴────────┐
                │    VM-DB     │    │  VM-STORAGE  │
                │  Postgres    │    │    MinIO     │
                └──────────────┘    └──────────────┘

  Campo:  [Estação IoT ESP32] ──MQTT/TLS──► VM-IOT
          [App RN Brigadista] ──HTTPS────► VM-EDGE
```

### 7.3 Dispositivos físicos

- **Estação IoT microclimática** — ESP32-DevKitC + sensor DHT22 (temperatura/umidade) + anemômetro reed-switch + opcionalmente MQ-2 (gás/fumaça). Firmware em C++/Arduino com cliente MQTT (PubSubClient) e cliente Wi-Fi com fallback para LoRa em uma versão estendida.
- **Câmera de campo (opcional, fase 2)** — Raspberry Pi 4 + Camera Module v3 + módulo 4G. Executa visão computacional Python local para detecção de fumaça em tempo real e publica eventos no broker.
- **Dispositivo mobile do brigadista** — qualquer Android 9+ ou iOS 14+ (BYOD).

### 7.4 Padrões de conectividade

- **Backbone do datacenter:** Gigabit Ethernet entre VMs, segmentação por VLAN.
- **Borda → Internet:** HTTPS (TLS 1.3) apenas na VM-EDGE.
- **Campo → Backend:** HTTPS sobre 4G/3G/EDGE; MQTT sobre TLS na porta 8883.
- **Fallback de campo:** sincronização local cache no app; sync ao reconectar.
- **Roadmap:** conectividade satelital (Starlink Roam ou similar) para áreas críticas sem cobertura celular.

---

## 8. Stack técnica consolidada e mapeamento de disciplinas

| Disciplina | Entregável no projeto | Responsável principal |
|---|---|---|
| Testing, Compliance & Quality Assurance | Este Master Doc + ArchiMate + plano QA + matriz de testes | Integrante 5 |
| Operating Systems | Lab de VMs Hyper-V/Azure, Windows Server 2022 com AD/GPO, Ubuntu Server hardening | Integrante 4 |
| Cibersegurança | Relatório de threat model (STRIDE), análise OWASP API Top 10, plano de criptografia e LGPD | Integrante 5 |
| Physical Computing IoT & IoB | Estação IoT ESP32 + módulo de Visão Computacional em Python (OpenCV/PyTorch) — **mandatório CV em Python** | Integrante 3 |
| SoA e Webservices | API Java Spring Boot, Gateway, contratos OpenAPI 3, testes de contrato | Integrante 1 |
| C# Software Development | Painel Web ASP.NET Core + Gerador de Laudos PDF (worker C#) | Integrante 4 |
| Mobile Development & IoT | App React Native offline-first com integração MQTT/REST | Integrante 2 |
| Machine Learning | Modelo de scoring de prioridade (XGBoost), pipeline de retreinamento, notebook de avaliação | Integrante 3 + 5 |

**Disciplinas não-críticas para o produto** (caso seja necessário "improvisar para garantir nota"): a disciplina de C# pode entregar apenas o painel web mesmo que ele seja simplificado em demo; a disciplina de Operating Systems pode ser cumprida com print/relatório do lab de VMs sem precisar estar publicamente acessível durante o pitch.

---

## 9. Divisão de papéis dos 5 integrantes

| # | Papel principal | Disciplinas titulares | Disciplinas de apoio | Entregáveis-chave |
|---|---|---|---|---|
| 1 | **Tech Lead / Backend Java (SoA)** | SoA e Webservices | Cibersegurança, ML (integração) | API Java, contratos OpenAPI, integração com workers |
| 2 | **Mobile Engineer / IoT App** | Mobile Development & IoT | SoA (consumo da API) | App React Native, sync offline, integração MQTT |
| 3 | **IoT & Computer Vision Engineer** | Physical Computing IoT & IoB, Machine Learning (co-titular) | SoA (worker Python) | Firmware ESP32, módulo CV Python, modelo ML |
| 4 | **C# Dev / DevOps / OS Engineer** | C# Software Development, Operating Systems | Cibersegurança (hardening) | Painel C#, gerador PDF, infra VMs/Windows Server |
| 5 | **Scrum Master / QA / ML Co-lead** | Testing, Compliance & Quality Assurance, Cibersegurança | Machine Learning (avaliação) | Master Doc, ArchiMate, plano QA, relatório de cibersegurança, retreinamento ML |

O integrante 5 acumula o papel de **Scrum Master** rotativo e Product Owner sombra para manter o foco da disciplina principal Agile/QA, e é responsável por consolidar a documentação final e o vídeo-pitch.

---

## 10. Plano Agile (Scrum) — disciplina Testing, Compliance & QA

### 10.1 Cerimônias e cadência

Sprints de **1 semana** durante 5 semanas (calendário Global Solution 2026). Cerimônias enxutas dado o tamanho do time:

- **Sprint Planning** — segunda, 30 min, presencial ou call.
- **Daily** — assíncrono via canal de chat do grupo (formato: o que fiz, o que vou fazer, bloqueios), com sync rápido de 10 min se houver bloqueio.
- **Sprint Review** — sexta, 45 min, demo do incremento.
- **Sprint Retrospective** — sexta após Review, 20 min, "start/stop/continue".

### 10.2 Backlog inicial (épicos)

| Épico | Disciplinas envolvidas | Sprint alvo |
|---|---|---|
| E1 — Ingestão satelital e cadastro territorial | SoA, ML | Sprint 1-2 |
| E2 — Visão computacional para validação de foco | IoT/IoB, ML | Sprint 2-3 |
| E3 — Motor de scoring ML | ML | Sprint 2-3 |
| E4 — App mobile offline brigada | Mobile, SoA | Sprint 2-4 |
| E5 — Painel web coordenação e KPIs | C#, SoA | Sprint 2-4 |
| E6 — Estação IoT microclimática | IoT/IoB | Sprint 1-3 |
| E7 — Gerador de laudo PDF e auditoria | C#, Cibersegurança | Sprint 3-4 |
| E8 — Infra VMs + Windows Server + hardening | OS, Cibersegurança | Sprint 1-2 |
| E9 — Testes, QA, threat model e ArchiMate final | QA, Cibersegurança | Todas |
| E10 — Vídeo-pitch e apresentação final | Todos | Sprint 5 |

### 10.3 Definition of Ready (DoR)

Um item entra no sprint quando: tem critério de aceite escrito, dependências externas resolvidas, mockup/wireframe se aplicável, estimativa em pontos (Fibonacci 1-13), e responsável claro.

### 10.4 Definition of Done (DoD)

Um item é considerado pronto quando: código revisado em pull request, testes unitários ≥ 70% nas classes de domínio, lint sem erros, deploy em ambiente de demo executado, documentação OpenAPI/README atualizada, smoke test manual passou, e o entregável foi visto pelo Scrum Master.

### 10.5 Matriz de testes e QA

| Camada | Tipo de teste | Ferramenta sugerida |
|---|---|---|
| API Java | Unitário + contrato OpenAPI | JUnit 5, Rest Assured, Pact |
| Worker Python | Unitário + integração | pytest, hypothesis |
| Modelo ML | Avaliação offline (precision/recall, AUC) e teste de regressão | scikit-learn metrics, MLflow |
| Painel C# | Unitário + UI | xUnit, Playwright |
| App RN | Unitário + UI | Jest, Detox |
| IoT firmware | Hardware-in-the-loop manual + simulação | PlatformIO, Wokwi |
| End-to-end | Smoke da jornada brigadista | Postman + Playwright |

### 10.6 Plano de releases

- **Release 0** (fim sprint 1) — esqueleto: API "hello", app navegável, VMs no ar, ingestão mock.
- **Release 1** (fim sprint 3) — fluxo principal: ingestão real, CV validando, scoring básico, app aceita ocorrência, painel mostra alertas.
- **Release 2** (fim sprint 4) — laudo PDF, estação IoT integrada, dashboards KPI, hardening de cibersegurança.
- **Release Final** (sprint 5) — bugfix, polimento, demo gravada, vídeo-pitch.

---

## 11. Plano de Cibersegurança (entregável da disciplina)

O relatório de cibersegurança da Global Solution tem como espinha dorsal um **threat model STRIDE** aplicado a OrbitaAlerta, complementado por uma análise OWASP API Security Top 10 e por um plano de tratamento de dados pessoais sob a LGPD.

Os ativos críticos a proteger são: (a) credenciais de usuários e tokens JWT; (b) evidências fotográficas e laudos PDF assinados; (c) telemetria IoT (uso defensivo); (d) chaves de API externas (INPE/NASA/Sentinel); (e) banco geoespacial com localização de ativos críticos de clientes. As principais ameaças identificadas via STRIDE incluem spoofing de estação IoT (mitigação: certificado por device e mutual TLS no MQTT), tampering em evidência fotográfica (mitigação: hash SHA-256 da evidência registrado on-chain interno ou em log imutável), repúdio de laudo (mitigação: assinatura digital do PDF + carimbo de tempo), information disclosure de localização de ativos (mitigação: criptografia em repouso + RBAC granular), DoS na API pública (mitigação: rate-limit no gateway + Cloudflare na borda) e elevation of privilege via JWT mal validado (mitigação: validação de claims + chaves rotacionadas).

Para a LGPD, OrbitaAlerta minimiza dados pessoais — coleta apenas nome, e-mail, papel e geolocalização do dispositivo durante turno operacional —, aplica anonimização nos datasets de retreinamento ML, registra base legal (execução de contrato e legítimo interesse com balanceamento documentado) e oferece endpoint para exercício dos direitos do titular.

---

## 12. Plano de Machine Learning

O modelo central de OrbitaAlerta é um **classificador de prioridade** que recebe um foco já validado por visão computacional e atribui um score de 0 a 100 com classe associada (Crítico/Alto/Médio/Baixo). As features de entrada combinam atributos do foco (satélite, confiança bruta, brilho, FRP), atributos meteorológicos (temperatura, umidade, vento, índice de risco do INPE), atributos territoriais (distância a ativo crítico, tipo de cobertura, declividade, histórico de fogo na área) e atributos temporais (hora do dia, mês, estiagem acumulada).

O algoritmo de baseline é **Gradient Boosting (XGBoost)** por sua robustez com dados tabulares heterogêneos e por permitir explicabilidade via SHAP, o que é particularmente útil para um produto que precisa justificar prioridade ao usuário final ("este foco é Crítico porque está a 800 m de talhão de soja, com vento sul de 18 km/h e umidade de 22%"). Um modelo secundário, baseado em CNN leve (MobileNetV3) treinada com transfer learning sobre tiles Sentinel-2 cortados em 64x64 px, classifica visualmente cada foco como "provável-fogo", "provável-falso" ou "inconclusivo" — este é exatamente o módulo que cumpre o requisito de Visão Computacional em Python obrigatório da disciplina Physical Computing IoT & IoB.

A avaliação adota validação temporal (treino com janelas anteriores, validação na janela mais recente) para evitar leakage. Métricas alvo iniciais: precision ≥ 0,75 e recall ≥ 0,80 na classe Crítico. O pipeline de retreinamento é executado mensalmente com dados rotulados das ocorrências encerradas — sim/não foi fogo confirmado, validando a hipótese acadêmica de melhoria com feedback humano.

---

## 13. Métricas de validação do produto

| Métrica | Meta inicial | Forma de medição |
|---|---|---|
| Tempo entre alerta e aceite | ≤ 8 min mediana | Telemetria do app |
| Falso-positivo percebido | ≤ 15% | Marcação do brigadista no fechamento |
| % ocorrências fechadas sem conexão contínua | ≥ 80% | Flag no payload de sync |
| Tempo de geração de laudo | ≤ 60 s | Telemetria do serviço C# |
| Uptime ingestão INPE/FIRMS | ≥ 99% | Monitor de jobs |
| Adoção (cooperativa piloto) | ≥ 5 usuários ativos/semana | Log de sessão |

---

## 14. Riscos e mitigações

| Risco | Probabilidade | Impacto | Mitigação |
|---|---|---|---|
| Incerteza do dado bruto de fogo | Alta | Alto | Score de confiança, validação CV cruzada, buffers geográficos, feedback humano |
| Conectividade rural inviabilizar demo | Média | Alto | App offline-first nativo; demo com modo avião forçado |
| Stack heterogênea atrasar integração | Média | Médio | Contratos OpenAPI estáveis desde sprint 1; gateway uniforme |
| Falta de GPU para treinar CV | Média | Médio | Transfer learning + Google Colab gratuito; modelo pequeno (MobileNetV3) |
| Integrante sobrecarregado | Alta | Médio | Scrum Master rotativo; pair programming entre disciplinas correlatas |
| Falha em entrega de uma disciplina secundária | Baixa | Médio | "Improviso" permitido: entrega mínima viável para garantir nota |
| Política da NASA/INPE mudar API | Baixa | Alto | Adaptadores isolados em camada anticorrupção |

---

## 15. Roteiro do vídeo-pitch (3-5 minutos)

**0:00–0:30 — Hook.** Imagens da seca 2024, manchete sobre 30 milhões de hectares queimados, "e se o tempo entre detectar e agir caísse de horas para minutos?"

**0:30–1:15 — Problema.** Brasil já tem dado orbital. O que falta é decisão operacional. Fragmentação, ruído, baixa conectividade. Dor para cooperativa, defesa civil e produtor médio.

**1:15–2:30 — Solução.** Demo curta: foco aparece no painel, validado por CV, scoring alto, brigadista recebe no app mesmo offline, aceita, navega, registra evidência, sincroniza, laudo PDF é gerado.

**2:30–3:30 — Arquitetura.** Diagrama do ArchiMate aparecendo: satélites → ingestão → CV → ML → API → app + IoT. Mostrar Java, C#, Python, React Native, MQTT, PostGIS conectados.

**3:30–4:15 — Mercado e modelo.** Downstream space economy. Cooperativas, defesa civil, ESG. SaaS por área monitorada + kit IoT opcional.

**4:15–5:00 — Time e fechamento.** 5 integrantes, 7 disciplinas, uma solução. Tagline: *"Da órbita ao talhão em minutos."*

---

## 16. Diagramas ArchiMate

Os quatro diagramas exigidos pela disciplina principal estão consolidados no arquivo `OrbitaAlerta_Archimate.xml`, no formato **ArchiMate Open Exchange File Format 3.1**, que é importável diretamente pelo aplicativo Archi (menu *File → Import → Open Exchange File*).

O arquivo contém:

1. **View 1 — Motivation & Strategy (Visão da arquitetura, peso 3):** stakeholders, drivers, goals, outcomes, requirements e constraints.
2. **View 2 — Business (Arquitetura de negócio, peso 2):** atores, papéis, funções, processos, serviços de negócio e localidades.
3. **View 3 — Application & Data (Arquitetura de sistema, peso 2):** componentes por camada, serviços de aplicação, objetos de dados e fluxos.
4. **View 4 — Technology (Arquitetura de tecnologia, peso 2):** nodes (VMs/servidores), devices (ESP32, RPi), system software, redes e caminhos de comunicação.

Após abrir o XML no Archi, qualquer integrante pode editar elementos, reposicionar e exportar para PNG ou PDF para inclusão em entregas individuais.

---

## 17. Entregáveis consolidados

| # | Arquivo / artefato | Disciplina | Responsável |
|---|---|---|---|
| 1 | `OrbitaAlerta_MasterDoc.md` (este doc) | Testing, Compliance & QA | Integrante 5 |
| 2 | `OrbitaAlerta_Archimate.xml` | Testing, Compliance & QA | Integrante 5 |
| 3 | `OrbitaAlerta_Resumo_Breve.md` | Entrega curta separada | Integrante 5 |
| 4 | Repositório `orbitaalerta-api-java` | SoA/Webservices | Integrante 1 |
| 5 | Repositório `orbitaalerta-app-rn` | Mobile & IoT | Integrante 2 |
| 6 | Repositório `orbitaalerta-iot-cv` | Physical Computing IoT & IoB + ML | Integrante 3 |
| 7 | Repositório `orbitaalerta-painel-cs` + worker laudos | C# Software Development | Integrante 4 |
| 8 | `OrbitaAlerta_LabOS.pdf` (prints VMs, AD, GPO) | Operating Systems | Integrante 4 |
| 9 | `OrbitaAlerta_Cibersec_Report.pdf` (STRIDE, OWASP, LGPD) | Cibersegurança | Integrante 5 |
| 10 | `OrbitaAlerta_ML_Notebook.ipynb` | Machine Learning | Integrante 3 + 5 |
| 11 | `OrbitaAlerta_Pitch.mp4` (3-5 min) | Todos | Todos |

---

## 18. Referências de fundamentação

- World Economic Forum / McKinsey (2024) — Space Economy outlook US$ 1,8 tri até 2035.
- OCDE — Space Economy and AI in Earth Observation.
- INPE — Programa Queimadas e BDQueimadas (focos quase em tempo real, risco de fogo).
- NASA FIRMS — Fire Information for Resource Management System (API hotspots).
- ESA — Sentinel-1 e Sentinel-2 missions (Copernicus).
- MapBiomas Fogo — Relatório anual 2024 (30 Mha queimados; +62% sobre média).
- Estratégia Nacional de Adaptação à Mudança do Clima (Brasil, 2024).
- Cemaden — Boletim de alertas e ocorrências 2024.
- Anatel — Cobertura 4G em áreas rurais brasileiras.
- Cetic.br — TIC Domicílios e Conectividade Significativa em zona rural.
- CEPAL — Agricultura digital e barreiras de adoção na América Latina.
- Ibama — Documentação técnica BDQueimadas (erros de localização, falsos positivos).
- The Open Group — TOGAF 9.2 / ArchiMate 3.1 specification.

---

> **Próxima ação do grupo:** validar este Master Doc em reunião de kickoff, confirmar a alocação dos papéis, abrir os 4 repositórios no GitHub do grupo, importar o `OrbitaAlerta_Archimate.xml` no Archi e iniciar a Sprint 1 conforme seção 10.

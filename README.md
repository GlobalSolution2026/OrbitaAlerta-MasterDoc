# OrbitaAlerta — Resumo Breve do Projeto

> Documento de apoio à entrega da Global Solution 2026 — FIAP.
> *Este é o "breve descritivo" referenciado como peso 1 da disciplina Testing, Compliance & Quality Assurance.*
> Para mais informações, refira-se a *OrbitaAlerta_MasterDoc.md*

---

## Diagrama Archimate
<img width="1120" height="760" alt="View 1 - Motivation   Strategy" src="https://github.com/user-attachments/assets/8b8f5f1c-8d8a-4c06-a08a-6fec4999907f" />

<img width="1120" height="740" alt="View 2 - Business Architecture" src="https://github.com/user-attachments/assets/48a71ae1-579b-4b18-b5ba-899793d2232b" />

<img width="1100" height="620" alt="View 3 - Application   Data" src="https://github.com/user-attachments/assets/60ac854e-7cd6-4611-962e-4465a84b47c6" />

<img width="1100" height="840" alt="View 4 - Technology" src="https://github.com/user-attachments/assets/eff876ae-4d70-4627-9aa0-8a4806d89cc8" />



## O que é o OrbitaAlerta

OrbitaAlerta é uma plataforma space-tech brasileira de resposta rápida a queimadas e incêndios em áreas rurais e periurbanas. O sistema transforma dado orbital em decisão operacional: recebe focos de calor em tempo quase real do INPE e da NASA FIRMS, valida cada foco com visão computacional sobre imagens Sentinel-2 e Sentinel-1, enriquece o sinal com risco meteorológico e telemetria IoT local de uma estação microclimática, e atribui um score de prioridade calculado por um modelo de Machine Learning. O resultado é entregue ao usuário final em um painel web de coordenação e em um app mobile offline-first que orienta o brigadista do alerta até a evidência fotográfica em campo, mesmo sem sinal de celular.

## Por que o projeto importa

O Brasil queimou 30 milhões de hectares em 2024 — 62% acima da média histórica, segundo o MapBiomas Fogo —, e o problema central não é mais a ausência de dado orbital: o INPE atualiza focos a cada 10 minutos e a NASA FIRMS oferece API aberta. O problema é a fragmentação entre fontes, o ruído estrutural da detecção bruta (o Ibama documenta erro médio de 400 m e desvio padrão de 3 km) e a baixa conectividade rural (66% das localidades rurais ainda não têm 4G, segundo a Anatel). OrbitaAlerta ataca exatamente esse gargalo: a camada de decisão operacional entre dado público bruto e ação local confiável.

## Escopo

O escopo do MVP contempla quatro frentes integradas: (i) ingestão automatizada de dados satelitais e meteorológicos públicos; (ii) validação automatizada por visão computacional para reduzir falso-positivo; (iii) motor de priorização por Machine Learning com explicabilidade SHAP; e (iv) operação de campo em modo offline com sincronização posterior. O público-alvo primário inclui cooperativas agropecuárias, produtores médios organizados, prefeituras e consórcios municipais, e empresas com metas ESG ou ativos territoriais.

## Stack tecnológica

A arquitetura é deliberadamente heterogênea para cobrir as disciplinas obrigatórias da Global Solution sem perda de coerência técnica: API núcleo em **Java Spring Boot** (SoA), serviço auxiliar em **C# ASP.NET Core** (painel web e gerador de laudo PDF), workers em **Python** para ingestão satelital, visão computacional (OpenCV + PyTorch) e scoring (XGBoost), app de campo em **React Native** com persistência offline, **PostgreSQL com PostGIS** para dados geoespaciais, **MQTT (Mosquitto)** para telemetria IoT vinda de uma estação microclimática baseada em **ESP32**, e infraestrutura em VMs com **Windows Server 2022** e **Ubuntu Server** para a disciplina de Operating Systems.

## Posicionamento

OrbitaAlerta se posiciona na camada *downstream* da economia espacial — não constrói satélite, não opera imagem proprietária, e não compete com fornecedores enterprise como ALARMES, Brasil MAIS ou umgrauemeio. Captura valor a partir da infraestrutura orbital já existente e da abundância de dados públicos, entregando o que falta no mercado brasileiro: uma plataforma acessível, offline-first e orientada ao segmento intermediário entre o dado público bruto e a suíte enterprise completa.

A tagline resume a proposta: **da órbita ao talhão em minutos**.

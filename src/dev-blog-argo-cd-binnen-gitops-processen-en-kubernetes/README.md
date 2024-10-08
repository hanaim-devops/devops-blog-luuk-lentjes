# Argo CD binnen GitOps-processen en Kubernetes
*[Luuk Lentjes, oktober 2024.](https://github.com/hanaim-devops/devops-blog-GoobyTheBOI/tree/main/src/dev-blog-name-in-kebab-case)*
<hr/>

## Introductie Argo CD
Argo CD is een declaratieve GitOps-tool die continue delivery binnen Kubernetes ondersteunt. Je beschrijft de 
gewenste eindtoestand van je applicatie en infrastructuur, zonder dat je stap-voor-stap- handelingen uitvoert om
die te bereiken. Argo CD automatiseert, controleert en vereenvoudigt applicatiedeployments en lifecycle management, 
waardoor het eenvoudig blijft om te gebruiken. (Argo CD - Declarative GitOps CD For Kubernetes, z.d.)

Op 13 maart 2018 releasete (Argoproj, z.d.) het team de tool met als doel de complexiteit van applicatiedeployments op Kubernetes te 
verminderen. Het project richtte zich op het automatiseren van workflows en het verbeteren van de 
ontwikkelaarservaring. Door functies zoals GitOps en verschillende implementatiestrategieën te bieden, zorgt Argo 
CD ervoor dat teams de zichtbaarheid en controle tijdens het deployproces vergroten, wat hen helpt efficiënter te 
werken. (Wang, 2023)

<img src="./plaatjes/argocd-ui.gif"> <i>Figuur 1: Argo CD UI https://argo-cd.readthedocs.io/en/stable</i>
<br />

### Functionaliteiten
Argo CD haalt automatisch de nieuwste versie van je Git-repository op en zet deze direct om naar 
Kubernetes-resources, in tegenstelling tot andere CD-tools die push-based deployments gebruiken. Dit vereenvoudigt 
het beheer van infrastructuurconfiguraties en applicatie-updates voor ontwikkelaars in één systeem. (Team, 2024)

Team (2024) beschrijft de volgende kernfunctionaliteiten van Argo CD:

- Je kunt applicaties handmatig of automatisch deployen naar een Kubernetes-cluster.
- Argo CD synchroniseert de applicatiestatus automatisch met de declaratieve configuratie.
- De tool biedt zowel een webinterface als een command-line interface (CLI).
- Het visualiseert deploymentproblemen en detecteert en herstelt configuration drift.
- Role-based access control (RBAC) maakt multi-cluster management mogelijk.
- Met Single sign-on (SSO) verbind je met providers zoals GitLab, GitHub, Microsoft, OAuth2, OIDC, LinkedIn, LDAP en 
  SAML 2.0.
- Webhooks ondersteunen acties die triggeren in GitLab, GitHub en BitBucket.

## Integratie met Kubernetes
Figuur 2 toont de architectuur van Argo CD. 

<img src="./plaatjes/argo-cd-architecture.png"> 
<br /> 
<i>Figuur 2: Argo CD architectuur https://argo-cd.readthedocs.io/en/stable/operator-manual/architecture/ </i> 
<br />

In het gedeelte met de blauwe rand zie je de Argo CD-omgeving. De applicatiecontroller functioneert als een 
Kubernetes-controller die continu de status van draaiende applicaties bewaakt. Deze vergelijkt de huidige status 
van applicaties met de gewenste status, die in een Git-repository is opgeslagen. Wanneer de applicatiestatus afwijkt
van de configuratie in Git, markeert de controller dit als OutOfSync. Vervolgens synchroniseert de controller 
automatisch de configuraties om de applicatie weer in lijn te brengen met de gewenste status. (Architectural 
Overview - Argo CD - Declarative GitOps CD For Kubernetes, z.d.)

De API server functioneert als de [gRPC/REST-interface](https://document360.com/blog/grpc-vs-rest/) die de Web UI, CLI 
en CI/CD-systemen gebruikt. Deze server beheert belangrijke taken zoals applicatiebeheer, statusrapportage en het 
uitvoeren van applicatie-operaties, 
waaronder synchronisaties en rollbacks. De API server beheert ook de repository- en clustercredentials, die als 
Kubernetes-secrets worden opgeslagen, en zorgt voor authenticatie en autorisatie via externe identiteitsproviders 
zoals Single Sign-On (SSO). Het handhaaft rolgebaseerde toegangscontrole (RBAC) om de toegang tot de systemen te 
beveiligen. (Architectural Overview - Argo CD - Declarative GitOps CD For Kubernetes, z.d.)

De repositoryserver is een interne service die een lokale cache bijhoudt van de Git-repository waar de 
applicatiemanifests worden opgeslagen. Deze server genereert de Kubernetes-manifests op basis van inputs zoals de 
repository URL, revisie (commit, tag of branch), het applicatiepad en template-specifieke instellingen, zoals Helm 
values.yaml. Het retourneert vervolgens de manifesten die nodig zijn om de gewenste Kubernetes-resources te 
definiëren. (Architectural Overview - Argo CD - Declarative GitOps CD For Kubernetes, z.d.)

Dankzij deze componenten integreert Argo CD moeiteloos met Kubernetes. Zodra een ontwikkelaar een nieuwe versie van 
een applicatie pusht naar Git, detecteert Argo CD de wijziging, vergelijkt het de huidige toestand van de 
Kubernetes-resources met de bijgewerkte configuratie in Git, en synchroniseert het om de gewenste status te 
herstellen. Deze GitOps-benadering zorgt voor gestroomlijnde en geautomatiseerde deployments, terwijl controle en 
zichtbaarheid behouden blijven.

## Implementatie







# Argo CD binnen GitOps-processen en Kubernetes
*[Luuk Lentjes, oktober 2024.](https://github.com/hanaim-devops/devops-blog-GoobyTheBOI/tree/main/src/dev-blog-name-in-kebab-case)*
<hr/>

## Introductie Argo CD
Argo CD is een declaratieve GitOps-tool voor continue delivery binnen Kubernetes. 
Dit betekent dat je de gewenste eindtoestand van je applicatie en infrastructuur beschrijft, in plaats van 
stap-voor-stap- handelingen uit te voeren om die eindtoestand te bereiken. Argo CD is ontworpen om  
applicatiedeployments en lifecycle management automatisch, controleerbaar en
gebruiksvriendelijk te maken. (Argo CD - Declarative GitOps CD For Kubernetes, z.d.)

De tool is begonnen met releasen om 13 maart 2018 (Argoproj, z.d.) en was bedoeld om de complexiteit van 
applicatiedeployments op Kubernetes te verminderen. Het project richtte zich op het automatiseren van workflows en 
het verbeteren van de ontwikkelaarservaring, met functies zoals GitOps en diverse implementatiestrategieën. Door 
deze functies streeft Argo ernaar om de zichtbaarheid en controle tijdens het deployproces te verbeteren, waardoor 
teams efficiënter kunnen werken. (Wang, 2023)

<img src="./plaatjes/argocd-ui.gif">
<i>Figuur 1: Argo CD UI https://argo-cd.readthedocs.io/en/stable</i>
<br />
<br />

In tegenstelling tot andere CD-tools, die voornamelijk gebruikmaken van push-based deployments, haalt Argo CD de 
nieuwste versie van je Git-repository op en zet deze direct in als Kubernetes-resources. 
Hierdoor wordt het voor ontwikkelaars eenvoudiger om zowel de infrastructuurconfiguratie als applicatie-updates in 
één systeem te beheren. (Team, 2024)

Volgens Team (2024) biedt Argo CD de volgende kernfunctionaliteiten:

- Handmatige of automatische deployment van applicaties naar een Kubernetes-cluster.
- Automatische synchronisatie van de applicatiestatus met de huidige versie van de declaratieve configuratie.
- Een webinterface en een command-line interface (CLI).
- Mogelijkheid om deploymentproblemen te visualiseren, configuration drift te detecteren en te herstellen.
- Role-based access control (RBAC) waarmee multi-cluster management mogelijk is.
- Single sign-on (SSO) met providers zoals GitLab, GitHub, Microsoft, OAuth2, OIDC, LinkedIn, LDAP en SAML 2.0.
- Ondersteuning voor webhooks die acties triggeren in GitLab, GitHub en BitBucket.

## Integratie met Kubernetes
Hoe de architectuur van Argo CD eruit ziet, zie je in figuur 2.
<img src="./plaatjes/argo-cd-architecture.png">
<br />
<i>Figuur 2: Argo CD architectuur https://argo-cd.readthedocs.io/en/stable/operator-manual/architecture/</i> 
<br />

In figuur 2 zie je een gedeelte met een blauwe rand, wat de Argo CD-omgeving weergeeft. De applicatiecontroller 
draait als een Kubernetes-controller en bewaakt continu de status van draaiende applicaties. Het vergelijkt de 
huidige status van de applicaties met de gewenste status die is opgeslagen in een Git-repository. Als de 
applicatiestatus afwijkt van de configuratie in Git, wordt dit gemarkeerd als een OutOfSync-status. Vervolgens 
onderneemt de controller automatisch corrigerende maatregelen door de configuraties te synchroniseren, zodat de 
applicatie weer overeenkomt met de gewenste status. (Architectural Overview - Argo CD - Declarative GitOps CD For 
Kubernetes, z.d.)

De API server dienen als de gRPC/REST-interface die wordt gebruikt door de Web UI, CLI en CI/CD-systemen. Het is 
verantwoordelijk voor verschillende essentiële taken, zoals applicatiebeheer, statusrapportage, en het uitvoeren 
van applicatie-operaties zoals synchronisaties en rollbacks. Daarnaast beheert de API server repository- en 
clustercredentials (opgeslagen als Kubernetes-secrets) en zorgt het voor authenticatie en autorisatie door externe 
identiteitsproviders te integreren, bijvoorbeeld via Single Sign-On (SSO). Het handhaaft rolgebaseerde 
toegangscontrole (RBAC) voor beveiligd gebruik. (Architectural Overview - Argo CD - Declarative GitOps CD For 
Kubernetes, z.d.)

Daarnaast is er de repositoryserver, een interne service die een lokale cache bijhoudt van de Git-repository waar 
de applicatiemanifests worden opgeslagen. Deze server speelt een cruciale rol bij het genereren van 
Kubernetes-manifests. Op basis van de repository URL, de revisie (commit, tag of branch), het applicatiepad en 
eventuele template-specifieke instellingen (zoals Helm values.yaml) kan de repositoryserver de benodigde manifesten 
retourneren om de gewenste Kubernetes resources te definiëren. (Architectural Overview - Argo CD - Declarative 
GitOps CD For Kubernetes, z.d.)

Door deze componenten werkt Argo CD naadloos samen met Kubernetes. Wanneer een ontwikkelaar bijvoorbeeld een nieuwe 
versie van een applicatie pusht naar Git, detecteert Argo CD de wijziging, vergelijkt het de huidige toestand van 
de Kubernetes-resources met de bijgewerkte configuratie in Git en synchroniseert het automatisch om de gewenste 
status te herstellen. Deze GitOps-benadering maakt de integratie met Kubernetes krachtig, omdat het zorgt voor 
gestroomlijnde en geautomatiseerde deployments, met behoud van controle en zichtbaarheid.






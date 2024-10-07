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
<br>
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








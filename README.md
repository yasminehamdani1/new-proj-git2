<h1>1.1 Qu’est-ce que le CI/CD ?</h1>

CI/CD signifie Intégration Continue (Continuous Integration) et Déploiement Continu (Continuous Deployment) ou Livraison Continue (Continuous Delivery).

C’est un ensemble de pratiques qui automatisent l’intégration des modifications de code provenant de plusieurs contributeurs dans un même projet logiciel, et qui automatisent également le processus de déploiement de ces applications en production.
	•	Intégration Continue (CI) : c’est la pratique qui consiste à tester et à fusionner automatiquement le nouveau code dans la branche principale, de manière fréquente — souvent plusieurs fois par jour.
🎯 Objectif : détecter les bogues et les problèmes d’intégration le plus tôt possible.
	•	Déploiement/Livraison Continue (CD) : garantit qu’une fois le code testé et fusionné, il est automatiquement (ou facilement) déployé vers les environnements de préproduction ou de production.
🎯 Objectif : rendre les déploiements fiables, rapides et routiniers.

Ensemble, le CI/CD aide les équipes à :
	•	Livrer des fonctionnalités plus rapidement
	•	Réduire les erreurs manuelles
	•	Améliorer la qualité du logiciel
	•	Créer une boucle de retour plus rapide pour les développeurs

⸻

1.2 Pourquoi l’évolutivité est importante dans le CI/CD

Dans les petits projets, des pipelines CI/CD simples peuvent suffire.
Mais à mesure que le projet grandit (plus de contributeurs, plus de code, plus d’environnements), l’évolutivité devient essentielle.

👉 Pourquoi l’évolutivité est importante :
	•	Forte parallélisation : pour exécuter les tests et les compilations plus rapidement, il faut pouvoir exécuter plusieurs workflows simultanément.
Déploiement multi-environnements : les environnements de développement, de préproduction (staging) et de production peuvent avoir des configurations et des processus de validation différents.

Résilience : des pipelines évolutifs peuvent se rétablir rapidement après une défaillance et gérer les pics de charge.

Maintenabilité : les grands projets ont besoin de workflows modulaires et réutilisables afin d’éviter les duplications et le désordre.

⸻

👉 Si votre CI/CD n’est pas évolutif, vous risquez de rencontrer :
	•	Des temps d’attente longs pour les builds
	•	Une augmentation des coûts due à des workflows inefficaces
	•	Des échecs fréquents de déploiement
  
1.3 Pourquoi choisir GitHub Actions ?

GitHub Actions est devenu l’une des principales solutions CI/CD, car :
	•	Il est intégré directement à GitHub, ce qui réduit les dépendances externes.
	•	Il permet de créer des workflows puissants grâce à une simple configuration en YAML.
	•	Il propose d’innombrables Actions communautaires disponibles sur le GitHub Marketplace.
	•	Il peut s’adapter à tous les niveaux de projet, du plus petit au niveau entreprise, grâce à des fonctionnalités telles que :
	•	Les builds en matrice (matrix builds)
	•	Les runners auto-hébergés (self-hosted runners)
	•	Le caching
	•	Les règles de protection d’environnement
	•	La gestion des secrets

Autres avantages :
	•	Gratuit pour les dépôts publics, avec une utilisation gratuite généreuse pour les dépôts privés.


2. Fondamentaux de GitHub Actions

⸻

2.1 Comprendre les Workflows, Jobs et Steps

Dans GitHub Actions, tout commence par un workflow.
Un workflow est un processus automatisé composé de jobs, et chaque job contient plusieurs steps (étapes).

Décomposons-les :

Terme	Signification
Workflow	Une procédure automatisée que vous définissez dans un fichier YAML, déclenchée par un événement (comme un push, une pull request, ou une tâche planifiée cron).
Job	Un ensemble d’étapes exécutées sur le même runner (machine). Les jobs peuvent s’exécuter séquentiellement ou en parallèle.
Step	Une tâche unique, comme exécuter une commande (npm install) ou appeler une action (actions/checkout).

Hiérarchie rapide :

Workflow
 ├── Job 1
 │    ├── Step 1
 │    ├── Step 2
 │    └── Step 3
 └── Job 2
      ├── Step 1
      └── Step 2


⸻

2.2 Concepts clés : Événements, Runners et Artifacts

⚙ Événements (Events)

Un événement est ce qui déclenche l’exécution d’un workflow.

Exemples d’événements :
	•	push sur une branche
	•	pull_request ouverte ou fusionnée
	•	schedule (tâche planifiée, par ex. tous les jours)
	•	release publiée
	•	Déclenchement manuel (workflow_dispatch)

Exemple de déclencheur sur push :

on:
  push:
    branches:
      - main


⸻

💻 Runners

Un runner est le serveur qui exécute vos workflows.
	•	GitHub-hosted runners : fournis par GitHub (Ubuntu, Windows, macOS disponibles).
	•	Self-hosted runners : vos propres machines, utiles pour des environnements personnalisés ou pour gérer la montée en charge.

Par défaut, les jobs s’exécutent sur les GitHub-hosted runners, sauf si vous en spécifiez un autre.

Exemple :

runs-on: ubuntu-latest


⸻

📦 Artifacts (Artefacts)

Les artifacts sont des fichiers créés pendant l’exécution d’un workflow, que vous pouvez sauvegarder ou partager entre plusieurs jobs.

Utilisations courantes :
	•	Sauvegarder les fichiers compilés (build outputs)
	•	Partager les résultats de tests ou les journaux (logs)

Exemple :

- name: Upload build artifact
  uses: actions/upload-artifact@v3
  with:
    name: build-files
    path: dist/

Vous pourrez ensuite télécharger ou réutiliser ces artifacts dans d’autres jobs.

⸻

2.3 Les bases de la syntaxe YAML pour les Workflows

Les workflows GitHub Actions sont définis dans des fichiers YAML placés dans le dossier :

.github/workflows/

Exemple de workflow basique :

name: CI Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test

Points clés à retenir dans YAML :
	•	L’indentation est cruciale (utilisez des espaces, pas de tabulations).
	•	name : donne un nom lisible pour chaque étape ou workflow.
	•	on : définit les événements déclencheurs.
	•	jobs : contient les tâches principales du workflow.
	•	steps : chaque étape décrit une action ou une commande à exécuter.

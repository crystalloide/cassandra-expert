#### Lancement de l'expert IA Cassandra en local :
##### Un plugin prévu pour Claude Code est fourni ici : https://github.com/rustyrazorblade/skills
##### L'idée est de porter ce plugin pour un usage moins coûteux avec Google Gemini : 
##### Comme Gemini CLI a été conçu pour être compatible avec l'écosystème de Claude, la transition est très simple.
##### Voici la marche à suivre pas à pas pour utiliser l'expert IA via  Gemini.

#####  Étape 1 : Lancement Gemini CLI
1°) Lancer Ubuntu sur la machine windows (Ubuntu 24.4.1 dans cet exemple) 
```bash
ubuntu
```

2°) Prendre le user souhaité :
```bash
su - user
```

3°) Regarder où nous sommes :
```bash
pwd
```

##### affichage en retour :  
```bash
/home/user
```

4°) Regarder si le dépôt claude-skills est déjà présent : 
```bash
ls
```

##### affichage en retour :  
```bash
claude-skills 
```

```bash
sudo rm -Rf claude-skills 
```

#####  Si vous ne l'avez pas encore, installez l'outil officiel de Google. Il nécessite Node.js (version 18+).

```bash
sudo npm install -g @google/gemini-cli@preview
```

```bash
sudo npm install -g npm@11.13.0
```

#####  Étape 2 : Connexion (Authentification gratuite)

#####  Lancez la commande suivante pour lier votre compte Google. Une fenêtre de navigateur s'ouvrira.

```bash
gemini login
```
     => choisir le choix 1 pour "truster" le répertoire en cours

#####  Gemini CLI redémarre :
     => choisir un mot de passe robuste : xxxxxxxx

     => choisir le choix 1 pour se logger avec son compte Google

     => taper 'r' pour redémarrer Gemini cli
     
#####  Note : Cela utilise le tier gratuit (1 000 requêtes/jour).



#####  Étape 3 : Cloner le dépôt de Skills

##### Si besoin lancer une autre session Ubuntu pendant que gemini finit son installation ou sa mise à jour
1°) Lancer Ubuntu sur la machine windows (Ubuntu 24.4.1 dans cet exemple) 
```bash
ubuntu
```

2°) Prendre le user souhaité :
```bash
su - user
```

#####  Nous allons nous appuyer sur le projet et les compétences  de Jon Haddad (rustyrazorblade), qui est un expert renommé de Cassandra.
```bash
git clone https://github.com/rustyrazorblade/skills.git ~/claude-skills
```



#####  Étape 4 : Lier les Skills à Gemini

#####  Gemini cherche ses compétences dans ~/.gemini/skills. 
#####  Nous allons créer un lien symbolique pour que Gemini "voit" les dossiers du dépôt que vous venez de cloner.

#####  Créer le dossier parent pour Gemini
```bash
mkdir -p ~/.gemini/skills
```

#####  Créer un lien symbolique du skill Cassandra vers le dossier Gemini
```bash
rm ~/.gemini/skills/cassandra-expert
```

```bash
ls ~/.gemini/skills/
```

```bash
rm ~/.gemini/skills/expert
rm ~/.gemini/skills/cassandra-expert

```

```bash
ls ~/claude-skills/plugins/cassandra-expert/
```

```bash
ln -s ~/claude-skills/plugins/cassandra-expert ~/.gemini/skills/cassandra-expert
```


```bash
ls ~/claude-skills/plugins/cassandra-expert/skills/expert
```

     Gemini va lire le fichier qu'on lui fournira et utiliser les instructions de ~/claude-skills/cassandra-expert/SKILL.md
	 
#####  Étape 5 : Activer les Skills dans Gemini
```bash
gemini skills link ~/claude-skills/plugins/cassandra-expert/skills/data-model
gemini skills link ~/claude-skills/plugins/cassandra-expert/skills/diagnose
gemini skills link ~/claude-skills/plugins/cassandra-expert/skills/expert
gemini skills link ~/claude-skills/plugins/cassandra-expert/skills/optimize
gemini skills link ~/claude-skills/plugins/cassandra-expert/skills/token-skew
gemini skills link ~/claude-skills/plugins/cassandra-expert/skills/training
```
     
     Si demandé, saisir le mot de passe xxxxxx
     
	répondre "Y"
	
#####  Étape 6 : On vérifie que le nouveau skills est visible dans Gemini		
```bash	
gemini skills list	
```	

##### Affichage en retour :
 
```text
		
	Loaded cached credentials.
	Discovered Agent Skills:

	data-model [Enabled]
	  Description: Data modeling and schema design for Apache Cassandra. Use when designing tables, choosing partition keys, modeling time-series data, or reviewing existing schemas.
	  Location:    /home/user/.gemini/skills/data-model/SKILL.md

	diagnose [Enabled]
	  Description: Systematic troubleshooting for Apache Cassandra clusters. Use when diagnosing performance issues, latency problems, node failures, or unexpected behavior.
	  Location:    /home/user/.gemini/skills/diagnose/SKILL.md

	expert [Enabled]
	  Description: General Apache Cassandra expertise for questions, CQL analysis, best practices, vnodes, and guidance. Use for general Cassandra questions that don't fit diagnose, optimize, or data-model.
	  Location:    /home/user/.gemini/skills/expert/SKILL.md

	optimize [Enabled]
	  Description: Performance optimization for Apache Cassandra clusters. Use when tuning configuration, improving throughput, reducing latency, optimizing resource usage, or configuring vnodes/num_tokens.
	  Location:    /home/user/.gemini/skills/optimize/SKILL.md

	token-skew [Enabled]
	  Description: Analyze token distribution and data ownership skew across Cassandra nodes and racks. Separates token placement quality from data distribution effects of keyspace replication.
	  Location:    /home/user/.gemini/skills/token-skew/SKILL.md

	training [Enabled]
	  Description: Interactive Cassandra training with guided curriculum. Covers fundamentals through advanced topics with hands-on CQL and code examples. Use when a user wants to learn Cassandra, not just get answers.
	  Location:    /home/user/.gemini/skills/training/SKILL.md
 
```

#####  Cas pratique : Analyse de votre fichier exemple : cassandra.yaml

     Maintenant que le skill "Cassandra Expert" est installé, 
     nous allons l'utiliser sur le fichier que vous avez fourni. 

     Placez-vous dans le dossier où se trouve votre fichier cassandra.yaml

     Lancez la commande suivante :

```bash
cd /endroit du fichier à analyser
```

##### Exemple : on se positionne sur le répertoire où les éléments à analyser ont été déposés :

##### Attention à dézipper préalablement et supprimer les fichiers zippés avant de lancer l'analyse. Ca fait gagner du temps ...

```bash
cd /mnt/b/a/logs-cassandra03-04_du_06_au_12_avril_2026/LOG_APP_PROXY_20260413
```

##### Exemple : Remarque : il faudra "truster" le répertoire indiqué avant que gemini puisse lancer l'analyse et autoriser les commandes "ls" etc

```bash
 > analyse les logs des 2 noeuds cassandra03 et cassandra04. Identifie les points majeurs à contrôler pour identifier
   des anomalies conduisants à des pics de consommation CPU et de mémoire
```

##### Autre exemple : 

```bash
gemini "Analyse mon fichier cassandra.yaml. Est-ce que la configuration des tokens et du stockage est optimale pour un cluster de production ?"
```

#####  Ce qui va se passer :

     - Détection : Gemini va scanner ses "Skills" et voir que cassandra-expert correspond au sujet.

     - Activation : Il va charger les instructions du fichier SKILL.md (qui contient des conseils sur les num_tokens, la stratégie de compactage, etc.).

     - Analyse : Il lira votre fichier (notamment votre num_tokens: 16) et vous donnera un avis d'expert.

##### Résultat attendu (Exemple) :

#####   Gemini devrait vous répondre quelque chose comme :

     "En tant qu'expert Cassandra, je vois que vous utilisez num_tokens: 16. 
     C'est une excellente valeur pour les versions récentes (3.0+), 
     car cela réduit l'overhead par rapport aux anciens 256 tokens. 
     Cependant, je remarque que..."

#####  Une astuce de pro 💡 :

     Le dépôt que vous avez cloné contient aussi des "slash commands". 

     Vous pouvez essayer de taper directement ceci dans le terminal Gemini :

```bash
/cql Génère moi une table pour stocker des logs de température
```

     Puisque le skill définit la commande /cql, 
     Gemini l'interprétera immédiatement avec les bonnes pratiques d'expert.


##### Exemple de résultat fourni par l'expert Cassandra IA : 

```text
✦ L'analyse de votre fichier cassandra.yaml montre des points positifs, mais aussi plusieurs points critiques qui pourraient compromettre la stabilité et la
  sécurité d'un cluster en production.

  Voici mon évaluation détaillée :


  1. Configuration des Tokens (Vnodes) - Optimale
   * num_tokens: 16 : C'est un excellent choix pour les versions récentes de Cassandra (3.x/4.x). Cela permet un bon équilibrage de la charge sans surcharger
     le CPU lors des réparations (repairs) ou des compactions.
   * allocate_tokens_for_local_replication_factor: 3 : C'est un paramètre crucial souvent oublié. Il optimise la répartition des tokens en fonction de votre
     facteur de réplication (RF=3), ce qui garantit une distribution de données plus uniforme dès le départ.


  2. Configuration du Stockage - Améliorable
   * Politiques de panne (disk_failure_policy: stop) : C'est correct, mais en production, die est souvent préféré si vous utilisez un orchestrateur (comme
     Kubernetes) ou un gestionnaire de services, afin que le processus soit redémarré proprement au lieu de rester "zombie".
   * Chemins par défaut : Les répertoires (data_file_directories, commitlog_directory) sont commentés, utilisant donc les chemins par défaut. Pour la
     production :
       * Assurez-vous que le CommitLog et les Data Files sont sur des disques physiques séparés (ou au moins des partitions distinctes avec des SSD
         performants) pour éviter les contentions d'I/O.
       * Utilisez impérativement des disques SSD ou NVMe.
   * Débit de compaction (compaction_throughput_mb_per_sec: 64) : Cette valeur est un peu basse pour des disques modernes. Sur des SSD/NVMe, on monte souvent
     à 128, 256, voire on désactive la limite (0) si les ressources le permettent.

  3. Points Critiques et Risques (Production)

  ⚠️ Timeouts excessifs (Danger)
  C'est le point le plus problématique de votre fichier :


   1 read_request_timeout_in_ms: 3600000  # 1 heure !
   2 range_request_timeout_in_ms: 3600000
   3 write_request_timeout_in_ms: 3600000
   4 request_timeout_in_ms: 3600000
  Pourquoi est-ce dangereux ? En production, si un nœud ralentit ou ne répond pas, le coordinateur attendra 1 heure avant d'abandonner. Cela va saturer les
  threads de lecture/écriture (concurrent_reads/writes) et peut faire tomber tout le cluster par effet domino.
   * Recommandation : Revenez à des valeurs raisonnables (ex: 5000 à 10000 ms pour les lectures/écritures, et peut-être plus pour les range_request si
     nécessaire, mais jamais 1 heure).


  ⚠️ Sécurité inexistante
   1 authenticator: AllowAllAuthenticator
   2 authorizer: AllowAllAuthorizer
  N'importe qui ayant accès au réseau peut lire, modifier ou supprimer toutes vos données.
   * Recommandation : Changez pour PasswordAuthenticator et CassandraAuthorizer.


  4. Performance et Réglages Fins
   * concurrent_compactors: 8 : Vous l'avez augmenté, ce qui est bien si vous avez beaucoup de cœurs et des SSD.
   * memtable_allocation_type: heap_buffers : Pour des heaps importants (> 8 Go), envisagez offheap_objects pour réduire la pression sur le Garbage Collector
     (GC).
   * concurrent_reads / writes : Ils sont à 32. Sur des serveurs avec beaucoup de cœurs et des disques NVMe, vous pourriez augmenter ces valeurs (ex: 64 ou
     128) après tests de charge.


  Résumé de mon avis :
  Votre configuration des tokens est très bonne. Cependant, la configuration globale n'est pas prête pour la production à cause des timeouts extrêmes et de
  l'absence de sécurité.

```

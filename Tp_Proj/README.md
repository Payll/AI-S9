# Projet
## Description
Ce projet vise à créer un système de reconnaissance de la langue à partir d'audio,
en utilisant des technique d'apprentissage automatique.
L'idée centrale est de convertir les signaux audio en représentations textuelles sous forme de phonèmes,
puis d'utiliser ces représentations pour prédire la langue parlée.
Notre approche consiste à combiner un modèle de transformation audio-texte,
tel que Wav2Vec, avec un réseau de neurones récurrents (RNN) pour traiter et classifier les phonèmes extraits.

## Chargement des Données
Les données audio pour ce projet proviennent de l'ensemble de données [Common Voice](https://huggingface.co/datasets/common_language/blob/main/data/CommonLanguage.zip) de Mozilla,
qui est une vaste collection de fichiers audio dans diverses langues, accompagnés de transcriptions textuelles.
Ces fichiers sont ensuite convertis en vecteur, qui servent d'entrée pour le modèle Wav2Vec.
Le traitement des données comprend également la segmentation en phrases ou unités parlées pour faciliter la correspondance avec les transcriptions textuelles.

## Tokenisation et Padding
Pour la tokenisation, nous utilisons un tokenizer personnalisé qui mappe chaque phonème à un identifiant unique.
Ce processus est crucial car il convertit le texte en une forme que le modèle RNN peut traiter.
Nous avons soigneusement sélectionné un ensemble de phonèmes qui couvre toutes les langues de notre ensemble de données,
en veillant à inclure les sons spécifiques à certaines langues.
Pour gérer les entrées de différentes longueurs, nous appliquons un padding aux séquences.
Cela signifie ajouter des tokens spéciaux 'PAD' à la fin des séquences plus courtes pour atteindre une longueur uniforme.
Ce padding est essentiel pour s'assurer que le réseau de neurones reçoit des entrées de taille fixe.


## Modèle pré-entraîné Wav2Vec
Nous avons choisi le modèle facebook/wav2vec2-xlsr-53-espeak-cv-ft,
un modèle multilingue, basé sur le notebook [Fine-Tune Wav2Vec2 for English ASR Notebook](https://colab.research.google.com/drive/1FjTsqbYKphl9kL-eILgUc-bl4zVThL8F?usp=sharing).
Des modifications ont été apportées pour traiter des fichiers audio dans plusieurs langues.
Toutefois, ce modèle étant principalement entraîné sur des locuteurs anglophones,
il rencontre des difficultés à extraire correctement les phonèmes des fichiers audio en français,
ce qui affecte la performance du modèle RNN subséquent.

## Modèle RNN
Notre modèle RNN est construit avec des couches LSTM (Long Short-Term Memory) bidirectionnelles.
Cette architecture est choisie pour sa capacité à retenir des informations sur de longues périodes,
ce qui est essentiel pour comprendre le contexte linguistique dans les séquences de phonèmes.
Le modèle bidirectionnel traite les séquences à la fois dans le sens avant et arrière,
ce qui permet une meilleure compréhension du contexte.
Nous avons expérimenté avec différentes tailles de couches cachées et des configurations de dropouts pour optimiser les performances.
Le modèle RNN dépend étroitement de la qualité des phonèmes extraits par Wav2Vec.
La difficulté de Wav2Vec à traiter le français impacte négativement la capacité du RNN à prédire correctement la langue.
L'accuracy de 0.5 suggère que le modèle fonctionne au niveau du hasard, indiquant une marge d'amélioration significative.


## Problèmes Rencontrés
Lors de la mise en œuvre de notre projet de reconnaissance de la langue,
nous avons été confrontés à plusieurs défis majeurs qui ont influencé notre stratégie et nos résultats.

### Restriction de la Portée Linguistique
Nous avions initialement l'ambition de développer un système capable de détecter une multitude de langues.
Cependant, des contraintes de ressources nous ont obligés à limiter notre champ d'action.
Nous avons dû concentrer nos efforts sur deux langues principales, le français et l'anglais.
Cette limitation était principalement due aux exigences matérielles nécessaires pour traiter un plus grand nombre de langues,
ce qui aurait demandé des ressources bien supérieures à celles dont nous disposions.

### Défis Techniques avec Wav2Vec
Le modèle Wav2Vec, bien que puissant, présente des défis significatifs en termes de ressources et de temps de traitement.
Nous avons utilisé un GPU A100 avec 40 GB de mémoire et 50 GB de RAM,
ce qui représente une configuration matérielle assez conséquente. Malgré cela,
l'entraînement et le traitement avec Wav2Vec restaient gourmands en ressources et en temps.

### Extraction de Phonèmes en Français
Une difficulté particulière a été observée avec le modèle Wav2Vec lors de
l'extraction des phonèmes des fichiers audio en français.
Cette lacune est probablement due à la structure intrinsèque du modèle, initialement
plus orienté vers l'anglais. Cette limitation a affecté la précision de notre système lorsqu'il
s'agissait de traiter des données en français.

### Biais du Dataset
Un autre problème notable concerne le biais inhérent à notre dataset.
Il y avait une disproportion marquée dans la quantité de données disponibles,
avec une prédominance de l'anglais par rapport au français.
Ce déséquilibre a eu pour effet de biaiser notre modèle en faveur de l'anglais,
rendant la reconnaissance du français moins précise et moins efficace.

## Pistes d'amélioration
### Enrichissement du Dataset
L'amélioration cruciale réside dans l'expansion de notre dataset.
L'intégration de davantage d'échantillons pour chaque langue visée,
en particulier celles sous-représentées, renforcerait la capacité du modèle à généraliser.
Il est également primordial de se concentrer sur la diversité des accents.
Un dataset qui réduit le biais d'accent, comme des enregistrements de locuteurs non-natifs
(par exemple, des anglophones parlant français),
pourrait améliorer considérablement la reconnaissance des langues parlées avec différents
accents. Ceci permettrait de rendre notre système plus robuste et précis dans des
situations du monde réel où les accents varient largement.

### Optimisation du Modèle Wav2Vec
Pour améliorer l'efficacité de l'extraction des phonèmes,
il serait bénéfique d'explorer différentes versions ou configurations de Wav2Vec.
Une attention particulière doit être accordée aux langues moins représentées ou
aux dialectes spécifiques. L'ajustement des paramètres de Wav2Vec,
ou même l'utilisation de versions plus récentes ou spécialisées,
pourrait offrir une extraction plus précise des caractéristiques
linguistiques essentielles, en particulier pour les langues avec des structures
phonétiques complexes ou uniques.

### Exploration de Modèles Alternatifs
L'adoption de modèles alternatifs pour la reconnaissance des phonèmes
est une autre piste prometteuse. Bien que Wav2Vec ait démontré son efficacité,
il existe une tendance à mieux performer avec des données majoritairement anglophones.
Explorer d'autres architectures, peut-être celles conçues spécifiquement pour des langues
non anglophones ou des modèles qui ont été entraînés avec un ensemble de données
plus diversifié, pourrait réduire le biais linguistique.
Des modèles comme BERT ou des variantes de réseaux neuronaux convolutifs pourraient
offrir des perspectives intéressantes dans ce domaine.

### Nettoyage des Données
Un processus de nettoyage approfondi des données audio est essentiel.
Chaque fichier devrait être systématiquement traité pour normaliser le volume et
minimiser le bruit de fond. Des techniques avancées de traitement du signal,
telles que la normalisation RMS et le filtrage passe-bas, sont indispensables.
En outre, une révision manuelle des fichiers audio pour identifier et corriger
les anomalies ou les erreurs de transcription pourrait grandement améliorer la
qualité des données. Cette étape manuelle assure que le modèle n'apprend pas à
partir de données erronées ou de mauvaise qualité, améliorant ainsi la précision
globale du système.

Ces améliorations visent à renforcer la précision,
la robustesse et la fiabilité du système dans la reconnaissance de la
langue à partir de fichiers audio, en s'attaquant aux problèmes de diversité des
données, de biais de modèle, et de qualité de l'ensemble de données.
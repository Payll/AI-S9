# Projet
## Description
Le projet vise à identifier la langue parlée à partir de fichiers audio. Nous utilisons un modèle pré-entraîné pour extraire les phonèmes des enregistrements audio, puis un modèle RNN pour prédire la langue.

## Modèle pré-entraîné Wav2Vec
Nous avons choisi le modèle facebook/wav2vec2-xlsr-53-espeak-cv-ft, un modèle multilingue, basé sur le notebook [Fine-Tune Wav2Vec2 for English ASR Notebook](https://colab.research.google.com/drive/1FjTsqbYKphl9kL-eILgUc-bl4zVThL8F?usp=sharing). Des modifications ont été apportées pour traiter des fichiers audio dans plusieurs langues. Toutefois, ce modèle étant principalement entraîné sur des locuteurs anglophones, il rencontre des difficultés à extraire correctement les phonèmes des fichiers audio en français, ce qui affecte la performance du modèle RNN subséquent.

## Modèle RNN
Le modèle RNN dépend étroitement de la qualité des phonèmes extraits par Wav2Vec. La difficulté de Wav2Vec à traiter le français impacte négativement la capacité du RNN à prédire correctement la langue. L'accuracy de 0.5 suggère que le modèle fonctionne au niveau du hasard, indiquant une marge d'amélioration significative.

## Dataset utilisé
Le dataset [Common Voice](https://huggingface.co/datasets/common_language/blob/main/data/CommonLanguage.zip) a été utilisé pour l'entraînement. Il serait bénéfique d'envisager des datasets supplémentaires pour améliorer la diversité linguistique et la représentation des langues moins courantes.

## Pistes d'amélioration
Enrichissement du Dataset
Inclure des datasets avec une plus grande variété linguistique pourrait améliorer la capacité du modèle à reconnaître des langues autres que l'anglais, en particulier le français.

### Optimisation du Modèle Wav2Vec
Explorer d'autres versions ou configurations de Wav2Vec pourrait aider à mieux extraire les phonèmes des langues moins représentées.

### Exploration de Modèles Alternatifs
Envisager l'utilisation de modèles différents pour la reconnaissance des phonèmes pourrait offrir de meilleures performances, en particulier pour les langues non anglophones.


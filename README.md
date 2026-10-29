# Miniature Sonore — Espace Composite
Auteur : Pouria AKBARHESARAKI
## Présentation
La notion de « miniature sonore » est peu connue et peut recevoir plusieurs interprétations. Ce projet explore l’idée d’un dispositif interactif capable de résumer un son entendu dans son environnement en une note musicale : une forme de « miniaturisation » sonore.
Cette version expérimentale prend le son au moyen du microphone, estime sa fréquence fondamentale, puis la restitue sous la forme d’un son synthétique.
## Fonctionnement
1. Cliquer sur « Commencer l’écoute (3s) » et autoriser l’accès au microphone.
2. Chanter ou jouer un son pendant les trois secondes d’écoute.
3. Le programme analyse un court tampon audio et affiche la fréquence estimée en hertz.
4. Si une fréquence exploitable est détectée, le navigateur la restitue avec un oscillateur en triangle.
5. Le traitement utilise une estimation de hauteur fondée sur la corrélation du signal dans le domaine temporel, plutôt qu’une détection de hauteur fondée sur les pics du spectre FFT.
## Technologies
- HTML et CSS
- JavaScript (https://strudel.cc)
- Web Audio API
- Accès au microphone via `getUserMedia()`
- Analyse temporelle du signal et synthèse sonore par oscillateur
## Lancer le projet
Ouvrir `ecouteur.html` dans un navigateur compatible. Autoriser l’accès au microphone lorsque le navigateur le demande.
Suivre les instructions et expérimenter.
## Limites de cette version
Il s’agit d’un prototype expérimental. L’estimation peut être imprécise avec des sons complexes, du bruit ambiant ou plusieurs sources sonores simultanées. Cette version mesure une fréquence sur un court extrait ; elle ne réalise pas une analyse complète et continue de l’environnement sonore.
## Contexte du projet
Le projet s’inscrit dans une réflexion autour de la miniature sonore et du traitement informatique du son, en lien avec l’espace composite, la programmation web, le Web Audio et l’informatique musicale.

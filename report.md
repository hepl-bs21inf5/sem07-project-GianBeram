# Projet

https://hepl-bs21inf5.github.io/sem07-project-GianBeram/

## Semaine 1

| Activité  | Estimation | Temps passé | Commentaire                                                                                                                                                                                                                                                                                                                                                |
| --------- | ---------- | ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Vu.js     | 10 minutes | 10 min      | Préparation du projet (installation et setup).                                                                                                                                                                                                                                                                                                             |
| Bootstrap | 10 minutes | 15 min      | Encore de la préparation.                                                                                                                                                                                                                                                                                                                                  |
| Quiz      | 2h         | 2h30        | Pour rajouter des questions, j'ai copier coller le "squelette de la question qui était présente de base dans QuizForm.vue. Pour la fonction reset, je change la valeur de ce qui est séléctionner à "null", le problème de cette fonction est que si je souhaite rajouter des fonctions, je dois rajouter une nouvelle valeur qui doit être égal à "null". |

### Question rapport

| Fichier         | Rôle                                                                                                                               |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| main.ts         | Fichier utilisant TypeScript permettant d'initialiser l'application Vue.                                                           |
| main.css        | C'est le "corps" du site, c'est ce qui va permettre de personalisé le site en changeant la couleur du texte, des boutons, etc... . |
| App.vue         | App.vue va fournir à toute les pages du site la navbar qui nous permet de naviguer dans le site.                                   |
| router/index.ts | C'est le fichier qui gère la navigation entre les différente page de l'application vue.                                            |
| AboutView.vue   | C'est une page qui affiche un contenu.                                                                                             |
| HomeView.vue    | C'est la page d'acceuil du site qui contient le contenu de QuizForm.vue.                                                           |
| QuizForm.vue    | C'est le fichier qui contient toutes les questions qui seront afficher sur le site.                                                |

&nbsp;

## Semaine 2

| Activité          | Estimation | Temps passé | Commentaire                                                                                                                                                                           |
| ----------------- | ---------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Question radio    | 30 min     | 30 min      |                                                                                                                                                                                       |
| Question Text     | 10 min     | 1h          | ça semble rapide mais il y a des éléments à rajouter dans le code donné et il faut savoir quoi raouter ce qui porend du temps. Pour rendre placeholder optionelle, rajouter un props. |
| API               | 1h         | 30 min      |                                                                                                                                                                                       |
| Question Checkbox | 30 min     |             |                                                                                                                                                                                       |

### Question rapport

|Question|Réponse|
|--------|-------|
|Quelle est la différence entre un prop et un modèle (v-model) ?| Un prop permet de transférer des données d'un composant parent à un document enfant, ça va uniquement de ce sens parent --> enfant. Tandis que le v-model lie une valeur entre un composant enfant et son parent, ça va dans les deux sens parent <---> enfant.|
|Comment rendre la propriété placeholder optionnelle ?| Dans la définition des props, il faudrait rendre cette propriété optionelle. On peut utilisé la valeur par défaut qui est une chîne caractère vide. Dans le document parent, on peut définir nos question avec ou sans la propriété placeholder.|

&nbsp;

## Semaine 3

|Activité| Estimation| Temps passé| Commentaire|
|--------|-----------|------------|------------| 
|Réponse| 1h| 40 min| je pensais que il faillait chercher ce qu'il fallait mettre mais tout était deja à disposition|
|Score| 30 min| 15 min| Aucune difficulté|

### Question rapport

|Question|Réponse|
|--------|-------|
|À quoi sert l'option immediate: true dans le watch ? Que se passe-t-il si on l'enlève ou si on met immediate: false ?|L'immediate true permet de traiter les valeurs directement en leur donnant une valeur de base même si celle-ci n'as pas été touché, pour nos questions, dans le debug mode on a false partout pour nous dire qu'on a répondu à aucune question. Avec un immediate false, on traite les valeurs uniquements si elles ont été touché, dans le debug mode on aura du vide et une fois qu'on répond à une question on aura un True qui indique qu'on a répondu à la question.|
|Proposer une autre manière de calculer le score (réécrire la fonction du computed) et comparer les deux méthodes.|La manière utilisé parcourt les valeurs de correctAnswers et utilise filter pour conserver uniquement les valeurs justes. On peut aussi utiliser un "reduce" (chatgpt) qui parcourt correctAnswers et à chaque fois qu'on tombe sur une bonne réponses on ajoute 1 au score. <br/> const score = computed<number>(() => <br/> correctAnswers.value.reduce((sum, value) => sum + (value ? 1 : 0), 0) <br/> );|


&nbsp;

## Semaine 4

|Activité| Estimation| Temps passé |
|--------|-----------|-------------|
|États| 2h| 1h30| 
|Boutons| 1h| 1h| 
|Réponses| 5 min| 5 min|

### Question rapport

|Question|Réponse|
|--------|-------|
|Comment pourrait-on réécrire la ligne suivante sans l'opérateur ternaire (avec des if et else) ? <br/> model.value = <br/> value.value === props.answer ? QuestionState.Correct : QuestionState.Wrong;|if (value.value === props.answer) { <br/> model.value = QuestionState.Correct; <br/> } else { <br/> model.value = QuestionState.Wrong; <br/>} |
|Comment pourrait-on réécrire autrement la logique du watch sur value ?|On pourrait utilisé un opérateur ternaire comme sur le watch model. On aurait : <br/> model.value = newValue === null ? QuestionState.Empty : QuestionState.Fill;|

&nbsp;

## Semaine 5

| Activité | Estimation | Temps passé | Commentaire |
|----------|------------|-------------|-------------|
| Réponse détaillé | 10 min | 15 min |  |
| Style | 5 min | 2 min |  |

### Question rapport

**Ajouter ce computed dans QuestionRadio.vue :**

**const answerText = computed<string>(**
  **() =>**
    **props.options.find((option) => option.value === props.answer)?.text ??**
    **props.answer,**
**);**

**Remplacer {{ props.answer }} par {{ answerText }} dans le template.**

**Expliquer pourquoi on a fait ce changement ainsi que le code du computed.**

Dans le code de base avec props.answer lorsque la réponse sera affiché, ce qui sera affiché est ce qui est contenu dans value. Par exemple dans mon code pour la première question la réponses afficher sera "waluigi" la valeur dans value au lieu de "Waluigi". Le answerText prend la valeur text associé à la valeur et affichera donc "Waluigi".

**Que se passe-t-il lorsqu'on ne met pas de valeur à answer-detail ? Est-ce satisfaisant ? Si ce n'est pas le cas, proposer une amélioration.**

Il n'y a pas de détail pour la réponse et il reste le tiret comme si il y avait du texte, ce qui peut être dérangeant. On peut mettre un message par défaut de la même manière que pour le placeholder dans QuestionText.

## Amélioration

**Trivia :**

J'ai choisi d'adapter Trivia car mes questions sont sur des sujets jeux vidéos et sur des jeux qui font partis de mes jeux favoris et ce n'est pas forcément adapter pour tous le monde et Trivia apporte pas mal de question sur différents sujets.

Pour adapter trivia, j'ai ajouté tous ce qui était présent dans quizform qui n'était pas dans trivia. C'est-à-dire toutes les varibles, fonctions et boutons et dans le question radio de trivia j'ai rajouté les attributs restant, c'est-à-dire : ":option", ":answer", etc...

Pour aller plus loin je pourrai faire en sorte que l'ordre des réponses pour chaque question soit aléatoire.

**QuestionCheckbox :**

J'ai choisi Checkbox pour diversifié les types de questions présents sur le site.

J'ai pris Questionradio/text comme base puis je l'ai changé par rapport au modéle de checkbox qu'on avait dans la semaine 2. Pour adapté le model je me suis aidé de chatgpt sinon le reste est très similaire à questiontext/radio. Voire les commentaires sur le code pour plus de détails.

Pour amélioré QuestionCheckbox je pourrai rajouter le "answerText" mais je ne trouve pas comment le faire. J'ai essayé de faire comme pour questionRadio mais ça ne fonctionne pas.
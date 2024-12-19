# Projet

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
|À quoi sert l'option immediate: true dans le watch ? Que se passe-t-il si on l'enlève ou si on met immediate: false ?|a|
|Proposer une autre manière de calculer le score (réécrire la fonction du computed) et comparer les deux méthodes.|a|

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
|Comment pourrait-on réécrire la ligne suivante sans l'opérateur ternaire (avec des if et else) ? <br/> model.value = <br/> value.value === props.answer ? QuestionState.Correct : QuestionState.Wrong;| |
|Comment pourrait-on réécrire autrement la logique du watch sur value ?| |

&nbsp;

## Semaine 5

| Activité | Estimation | Temps passé | Commentaire |
|----------|------------|-------------|-------------|
| Réponse détaillé | 10 min | 15 min |  |
| Style | 5 min | 2 min |  |

### Question rapport

Ajouter ce computed dans QuestionRadio.vue :

const answerText = computed<string>(
  () =>
    props.options.find((option) => option.value === props.answer)?.text ??
    props.answer,
);

Remplacer {{ props.answer }} par {{ answerText }} dans le template.

Expliquer pourquoi on a fait ce changement ainsi que le code du computed.

Que se passe-t-il lorsqu'on ne met pas de valeur à answer-detail ? Est-ce satisfaisant ? Si ce n'est pas le cas, proposer une amélioration.

## Amélioration

Adapté Trivia

QuestionCheckBox
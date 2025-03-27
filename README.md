# Data Battle IA PAU

**Nom de l'équipe :** IA-Mates  
**Auteurs :** [À compléter avec vos noms ou pseudos GitHub]

Ce projet est un chatbot juridique interactif alimenté par l'intelligence artificielle, destiné à aider les candidats à se préparer à l'examen européen de qualification (EQE) et à l'EPAC. Il propose des questions à choix multiples (QCM), des questions ouvertes, et des questions vrai/faux, basées sur des sujets d'examen passés ou simulés. Les réponses de l'utilisateur sont corrigées instantanément, soit à partir des réponses enregistrées, soit via une vérification par IA avec récupération de contexte (RAG) en cas d'absence de réponse.

---

## Fonctionnalités

- QCM classiques avec options A/B/C/D
- Questions vrai/faux multi-énoncés (ex : 10.1, 10.2, ...)
- Questions ouvertes évaluées par une IA
- Vérification des réponses à partir des explications du dataset
- Fallback IA avec RAG via Mistral si aucune réponse disponible
- Interface multilingue (FR / EN / DE) via Google Translate
- Suivi de l'empreinte carbone avec CodeCarbon
- Interface utilisateur épurée avec retour immédiat et questions aléatoires

---

## Technologies utilisées

- Python (Flask pour le backend)
- HTML / CSS / JavaScript (frontend)
- API Mistral (modèle de langage)
- LangChain + ChromaDB pour le RAG
- Fichier JSONL pour les questions et réponses
- CodeCarbon pour l'estimation des émissions

---

## Lancer l'application depuis un notebook `.ipynb`

1. Ouvrir le fichier `chatbot.ipynb` (dans Google Colab)
2. Exécuter toutes les cellules (`Runtime > Run all` ou `Exécuter > Tout exécuter`)
3. Attendre que le lien **ngrok** s'affiche (exemple : `https://...ngrok.io`)
4. Cliquer sur ce lien pour accéder à l'application dans le navigateur

---

## Routes principales

- `/` : Page d'accueil (chat)
- `/menu/<source>/<qtype>` : Chargement des questions par type (QCM, OPEN, VRAI/FAUX)
- `/api/check` : Vérification des QCM
- `/api/check_tf` : Vérification des questions vrai/faux
- `/api/open` : Vérification des réponses ouvertes via l'IA
- `/api/clarify` : Structuration d'une question QCM pour affichage
- `/api/clarify_tf` : Structuration d'une question VRAI/FAUX pour affichage

---

## Format des questions (`.jsonl`)

```json
{
  "question_number": "5",
  "question": "Selon la CBE, qui peut agir comme mandataire devant l'OEB ?",
  "type": "qcm",
  "source": "EPAC_2023",
  "answer": "The answer is correct. The correct answer is A. Un mandataire agréé...",
  "chunk_id": "EPAC_2023_5"
}

---

## Fonctionnement du fallback IA (RAG)

Si aucune réponse n'est fournie ou identifiable dans le dataset :

Le système recherche les sources juridiques pertinentes via ChromaDB

Il construit un prompt incluant la question et des extraits de loi

Il envoie cette requête au modèle de langage Mistral via l'API

Il extrait une réponse structurée au format JSON :

"correct" : true/false

"correct_letter" : A, B, C, D, etc.

"explanation" : explication juridique courte et fiable

Cette réponse est ensuite utilisée pour vérifier l'exactitude de la réponse de l'utilisateur.


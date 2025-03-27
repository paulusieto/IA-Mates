# Data Battle IA PAU 2025 - Chatbot Juridique

**Nom de l'équipe :** IA-Mates  
**Auteurs :** Usieto Paul, Courthial Mathis, Fazille Tara, Bisbau Maxime, Cros Clément

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

- Python (exécuté dans un notebook Jupyter ou Google Colab)
- Flask (serveur web)
- HTML / CSS / JavaScript (frontend)
- API Mistral (modèle de langage)
- LangChain + ChromaDB pour le RAG
- Fichier JSONL pour les questions et réponses
- CodeCarbon pour l'estimation des émissions

---

## Lancer l'application depuis un notebook `.ipynb`

1. Ouvrir le fichier `chatbot.ipynb` (dans Google Colab ou VS Code)
2. Exécuter toutes les cellules (`Runtime > Run all` ou `Exécuter > Tout exécuter`)
3. Attendre que le lien **ngrok** s'affiche (exemple : `https://...ngrok.io`)
4. Cliquer sur ce lien pour accéder à l'application dans le navigateur

---

## Structure du projet

```
IA-Mates/
├── chatbot.ipynb             # Notebook principal à exécuter
├── src/
│   ├── static/
│   │   └── style.css
│   ├── templates/
│   │   ├── chat.html
│   │   └── questions.html
│   └── merged_dataset.jsonl  # Données des questions
```

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
```

---

## Fonctionnement du fallback IA (RAG)

Si aucune réponse n'est fournie ou identifiable dans le dataset :

1. Le système recherche les sources juridiques pertinentes via ChromaDB
2. Il construit un prompt incluant la question et des extraits de loi
3. Il envoie cette requête au modèle de langage Mistral via l'API
4. Il extrait une réponse structurée au format JSON :
   - `"correct"` : true/false
   - `"correct_letter"` : A, B, C, D, etc.
   - `"explanation"` : explication juridique courte et fiable

Cette réponse est ensuite utilisée pour vérifier l'exactitude de la réponse de l'utilisateur.

---

## Interface multilingue

L'interface permet de basculer dynamiquement entre les langues suivantes grâce au bouton "globe" dans le coin supérieur :
- Français
- Anglais
- Allemand

---

## Configuration

Dans le notebook `chatbot.ipynb`, définissez votre clé API Mistral dans la cellule correspondante :

```python
MISTRAL_API_KEY = "votre_clé_api"
```

---

## À faire (améliorations futures)

- [ ] Ajout du suivi de score par utilisateur
- [ ] Export des résultats en CSV
- [ ] Filtrage par thème ou chapitre
- [ ] Suivi de la progression personnelle
- [ ] Historique des tentatives

---

## Licence

Ce projet est librement réutilisable pour un usage éducatif ou personnel. Aucune garantie n’est fournie.

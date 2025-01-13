# Workshop 1 : Analyse de Journaux Système avec un Modèle NVIDIA LLM

## Utilisation des Modèles LLM pour la Détection d'Anomalies en Cybersécurité

## Introduction
Ce TP a pour objectif de vous initier à l’utilisation des modèles de langage (LLM) open-source dans le domaine de la cybersécurité. Nous allons utiliser le modèle **LLaMA-3.1-Nemotron-70B-Instruct** fourni par NVIDIA pour analyser des journaux système, détecter des anomalies et générer des recommandations de sécurité.

---

## Objectifs
1. Comprendre comment utiliser l’API NVIDIA pour exploiter un modèle de langage avancé.
2. Apprendre à analyser des journaux système avec un modèle LLM.
3. Automatiser la génération de rapports de sécurité.

---

## Prérequis
1. **Compte NVIDIA NGC** : Créez un compte [ici](https://www.nvidia.com/ngc).
2. **Clé API NVIDIA** : Générez une clé API dans votre tableau de bord NVIDIA NGC.
3. **Google Colab** : Utilisez une instance Colab pour exécuter ce TP.
4. **Documentation de l'API NVIDIA** : Consultez [ce lien](https://build.nvidia.com/nvidia/llama-3_1-nemotron-70b-instruct).

---

## Étape 1 : Configuration de l’API NVIDIA

### 1.1 Installation des Bibliothèques
Exécutez la cellule suivante dans Google Colab pour installer les dépendances :

```python
!pip install openai
```

### 1.2 Configuration de l’API
Créez un client pour interagir avec l’API NVIDIA.

```python
from openai import OpenAI

# Configurer le client NVIDIA
client = OpenAI(
    base_url="https://integrate.api.nvidia.com/v1",
    api_key="VOTRE_API_KEY"  # Remplacez par votre clé API NVIDIA
)
print("API NVIDIA configurée avec succès.")
```

**Remarque** : Remplacez `VOTRE_API_KEY` par votre clé API NVIDIA.

---

## Étape 2 : Chargement des Journaux Système

### 2.1 Simulation de Journaux
Créez une liste contenant des journaux système simulés :

```python
# Journaux système simulés
logs = [
    "2025-01-10 12:00:01 User 'admin' logged in from IP 192.168.1.10",
    "2025-01-10 12:15:23 Failed login attempt for user 'root' from IP 10.0.0.15",
    "2025-01-10 12:20:45 User 'john' downloaded sensitive file 'confidential.pdf'",
    "2025-01-10 12:35:50 Unusual traffic detected from IP 10.0.0.20",
    "2025-01-10 12:50:10 User 'admin' performed a password reset for user 'guest'"
]

print("Journaux système chargés :")
for log in logs:
    print(log)
```

---

## Étape 3 : Analyse des Journaux avec NVIDIA LLM

### 3.1 Préparer le Prompt
Le prompt est une instruction que nous envoyons au modèle pour effectuer une analyse.

```python
# Créer un prompt pour le modèle
prompt = f"""
Voici une liste de journaux système :
{chr(10).join(logs)}

1. Identifiez les événements suspects ou inhabituels.
2. Expliquez pourquoi ces événements sont problématiques.
3. Proposez des recommandations pour éviter ces problèmes à l'avenir.
"""

print("Prompt créé pour le modèle.")
```

### 3.2 Envoyer le Prompt à l’API
Utilisez l’API NVIDIA pour analyser les journaux.

```python
# Envoyer la requête à l’API NVIDIA
completion = client.chat.completions.create(
    model="nvidia/llama-3.1-nemotron-70b-instruct",
    messages=[{"role": "user", "content": prompt}],
    temperature=0.5,
    top_p=1,
    max_tokens=1024,
    stream=True
)

# Afficher la réponse en continu
print("\n### Résultat d'analyse ###")
for chunk in completion:
    if chunk.choices[0].delta.content is not None:
        print(chunk.choices[0].delta.content, end="")
```

---

## Étape 4 : Sauvegarder le Rapport

### 4.1 Enregistrer le Rapport
Les résultats générés par le modèle peuvent être sauvegardés pour une analyse ultérieure.

```python
# Sauvegarder le rapport généré
rapport = ""
for chunk in completion:
    if chunk.choices[0].delta.content is not None:
        rapport += chunk.choices[0].delta.content

with open("rapport_securite.txt", "w") as file:
    file.write(rapport)

print("\nLe rapport a été sauvegardé sous 'rapport_securite.txt'.")
```

---

## Discussions et Questions

### Questions pour les étudiants
1. Quels événements ont été identifiés comme suspects ?
2. Les recommandations sont-elles pertinentes ?
3. Comment pourrait-on améliorer l’analyse pour des systèmes réels ?

### Suggestions d’amélioration
1. Ajouter des journaux système réels pour tester la robustesse.
2. Personnaliser les modèles avec des données d’entraînement spécifiques.
3. Automatiser la classification des événements par gravite : critique, moyenne, faible.

---

## Conclusion

Ce TP vous a permis de :
- Configurer un modèle LLM NVIDIA.
- Analyser des journaux système et détecter des anomalies.
- Générer un rapport automatisé de sécurité.

Si vous avez des questions ou souhaitez explorer d’autres cas d’usage, n’hésitez pas à les poser à la fin du workshop.

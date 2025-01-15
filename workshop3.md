# Workshop 3 : Intelligence Artificielle et Législation Européenne (AI Act)

---

## **Introduction**

L'AI Act, proposé par l'Union Européenne, est le premier cadre législatif global visant à réglementer l'intelligence artificielle. Il établit des normes claires pour le développement et l'utilisation des systèmes d'IA tout en garantissant la protection des droits fondamentaux des citoyens européens.

Dans ce workshop, nous examinerons :
- Les principes fondamentaux de l'AI Act.
- Comment évaluer la conformité d'un projet d'IA avec ce cadre réglementaire.
- Les bonnes pratiques pour minimiser les risques juridiques.

---

## **Objectifs**
1. Comprendre les catégories de risque définies par l'AI Act.
2. Étudier les implications du cadre législatif pour les systèmes d'IA, notamment les LLM (Large Language Models).
3. Créer un plan de conformité pour un projet d'IA en se basant sur les exigences clés.

---

## **Prérequis**
- Connaissance de base sur l’intelligence artificielle.
- Familiarité avec les concepts éthiques en IA.
- Environnement de développement comme Google Colab ou Jupyter Notebook pour tester des outils de validation de conformité.

---

## **Plan du Workshop**

### **Partie 1 : Comprendre l'AI Act**

#### **1.1 Catégories de Risque**
L'AI Act classe les systèmes d'IA en quatre catégories de risque :
1. **Risque inacceptable** :
   - Exemples : Reconnaissance biométrique en temps réel dans des lieux publics, manipulation subliminale.
   - Interdiction complète.

2. **Risque élevé** :
   - Exemples : Diagnostic médical assisté, IA dans les processus de recrutement.
   - Obligations strictes, comme la transparence et la gestion des données.

3. **Risque limité** :
   - Exemples : Chatbots, systèmes de recommandation.
   - Obligations légères, comme informer les utilisateurs qu’ils interagissent avec une IA.

4. **Risque minimal** :
   - Exemples : Jeux vidéo, filtres photo.
   - Aucune obligation spécifique.

---

### **Partie 2 : Analyse de la Conformité d’un Projet d’IA**

#### **2.1 Cas Pratique : Analyse d’un Chatbot**
Prenons un exemple de chatbot intégré dans une plateforme de service client.

#### Étape 1 : Identifier le type de risque
- **Usage** : Fournir des réponses aux clients.
- **Impact potentiel** : Le chatbot peut influencer des décisions financières.
- **Catégorie de risque** : **Risque limité**.

#### Étape 2 : Évaluation des Obligations
- **Transparence** : Informer les utilisateurs qu'ils interagissent avec une IA.
- **Accessibilité des données** : Garantir que les données personnelles sont collectées conformément au RGPD.

---

### **Partie 3 : Implémentation Technique**
Dans cette section, nous utiliserons Python pour vérifier si un modèle d'IA répond aux critères de transparence.

#### Exemple de vérification de transparence
```python
def check_transparency(prompt):
    """
    Vérifie si le modèle informe l'utilisateur qu'il interagit avec une IA.
    """
    message = f"Vous parlez avec un assistant IA : {prompt}"
    return message

# Test
user_input = "Pouvez-vous m'aider à trouver un crédit adapté ?"
response = check_transparency(user_input)
print(response)
```

#### Résultat attendu
```
Vous parlez avec un assistant IA : Pouvez-vous m'aider à trouver un crédit adapté ?
```

---

### **Partie 4 : Évaluation des Données**
Vérifiez que le système ne traite pas de données sensibles sans consentement explicite.

```python
import re

def validate_data_input(user_data):
    """
    Vérifie que les données sensibles (comme les informations financières) sont anonymisées.
    """
    sensitive_data_pattern = r"(numéro de carte|compte bancaire|adresse complète)"
    if re.search(sensitive_data_pattern, user_data, re.IGNORECASE):
        return "Attention : données sensibles détectées ! Anonymisez vos informations."
    return "Les données semblent conformes."

# Test
user_data = "Mon numéro de carte est 1234-5678-9012-3456."
print(validate_data_input(user_data))
```

---

### **Partie 5 : Plan de Conformité**

#### Étapes Clés :
1. **Identification des Risques** :
   - Catégorisez votre système selon l'AI Act.
2. **Transparence** :
   - Ajoutez des messages clairs pour informer les utilisateurs.
3. **Validation des Données** :
   - Implémentez des outils d'anonymisation automatique.
4. **Documentation** :
   - Maintenez un registre détaillé des décisions liées à l'IA.

---

## **Conclusion**

Dans ce workshop, nous avons exploré :
1. Les principes fondamentaux de l'AI Act.
2. La catégorisation des risques pour les systèmes d'IA.
3. Des approches pratiques pour vérifier la conformité de vos projets.

L'AI Act est une opportunité pour développer des systèmes d'IA transparents, responsables et respectueux des droits fondamentaux. Si vous avez des questions ou des cas spécifiques à discuter, n’hésitez pas !
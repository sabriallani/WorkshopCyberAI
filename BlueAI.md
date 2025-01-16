# Tutoriel : Créer une application Streamlit pour détecter des anomalies dans les fichiers réseau avec l'API NVIDIA

## Prérequis
Avant de commencer, assurez-vous que :

1. **Python** est installé sur votre système.
2. Vous avez installé les bibliothèques suivantes :
    - `streamlit`
    - `openai` (compatible NVIDIA API)
3. Vous disposez d'une clé API NVIDIA valide.

Installez les bibliothèques nécessaires avec :
```bash
pip install streamlit openai
```

---

## Objectif
Nous allons créer une application Streamlit qui :

1. Permet de télécharger un fichier réseau (e.g., fichier texte ou JSON contenant des logs).
2. Analyse ce fichier à l'aide de l'API NVIDIA.
3. Indique s'il y a une anomalie ou non.

---

## Code de l'application
Créez un fichier Python nommé `app.py` et collez le code suivant :

```python
import streamlit as st
from openai import OpenAI
import json

# Configurer le client NVIDIA
client = OpenAI(
    base_url="https://integrate.api.nvidia.com/v1",
    api_key="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"  # Remplacez par votre clé API NVIDIA
)

st.title("Détection d'anomalies dans les fichiers réseau")
st.write("Cette application utilise l'API NVIDIA pour analyser les fichiers réseau et détecter d'éventuelles anomalies.")

# Upload du fichier
uploaded_file = st.file_uploader("Téléchargez un fichier réseau (format JSON ou texte)", type=["txt", "json"])

if uploaded_file is not None:
    # Lire le contenu du fichier
    try:
        content = uploaded_file.read().decode("utf-8")
        st.write("Fichier chargé avec succès :")
        st.text(content[:500])  # Afficher les 500 premiers caractères

        # Appel à l'API NVIDIA
        response = client.completions(
            model="network-anomaly-detector",
            prompt=f"Analyse ce fichier réseau et détecte s'il y a des anomalies : {content}",
            max_tokens=200
        )

        # Analyse de la réponse
        if response and "choices" in response:
            result = response["choices"][0]["text"].strip()
            st.success("Résultat de l'analyse :")
            st.write(result)
        else:
            st.error("Une erreur s'est produite lors de l'analyse. Veuillez réessayer.")

    except Exception as e:
        st.error(f"Erreur lors du traitement du fichier : {e}")
```

---

## Instructions pour l'exécution
1. Sauvegardez le fichier sous le nom `app.py`.
2. Exécutez l'application Streamlit avec la commande suivante :
   ```bash
   streamlit run app.py
   ```
3. Ouvrez l'URL générée par Streamlit (généralement `http://localhost:8501`).
4. Téléchargez un fichier réseau pour détecter des anomalies.

---

## Exemple de fichier réseau
Créez un fichier nommé `sample_network.json` avec le contenu suivant pour tester l'application :

```json
{
    "timestamp": "2025-01-16T10:00:00Z",
    "source_ip": "192.168.1.10",
    "destination_ip": "192.168.1.20",
    "protocol": "TCP",
    "packet_size": 512
}
```

---

## Points importants
- Remplacez la clé API dans le code par une clé valide et personnelle.
- Cette application est un point de départ. Vous pouvez l'enrichir pour gérer différents types de fichiers ou fournir des visualisations plus avancées.

---

## Conclusion
Félicitations ! Vous avez créé une application Streamlit pour analyser les fichiers réseau à l'aide de l'API NVIDIA. Continuez à l’améliorer en explorant d'autres modèles ou en ajoutant des fonctionnalités.

# Tutoriel : Créer une application web pour analyser des journaux système avec l'API NVIDIA

## Prérequis
Avant de commencer, assurez-vous que :

1. **Python** est installé sur votre système.
2. Vous avez installé les bibliothèques suivantes :
    - `streamlit`
    - `openai`
    - `json` (intégré dans Python, aucune installation nécessaire)
3. Vous disposez d'une clé API NVIDIA valide.

Installez les bibliothèques nécessaires avec :
```bash
pip install streamlit openai
```

---

## Objectif
Nous allons créer une application Streamlit qui :

1. Permet de télécharger un fichier de journaux système (format texte).
2. Analyse ce fichier à l'aide de l'API NVIDIA.
3. Interprète les journaux pour identifier des événements suspects ou critiques.

---

## Code de l'application
Créez un fichier Python nommé `app.py` et collez le code suivant :

```python
import streamlit as st
import requests
import json  # Assurez-vous que json est importé

# Configurer le client NVIDIA
API_KEY = "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"
BASE_URL = "https://integrate.api.nvidia.com/v1"

def analyze_logs(content):
    headers = {"Authorization": f"Bearer {API_KEY}"}
    prompt = f"""
    Voici une liste de journaux système :
    {content}

    1. Identifiez les événements suspects ou inhabituels.
    2. Expliquez pourquoi ces événements sont problématiques.
    3. Proposez des recommandations pour éviter ces problèmes à l'avenir.
    """
    payload = {
        "model": "nvidia/llama-3.1-nemotron-70b-instruct",
        "messages": [{"role": "user", "content": prompt}],
        "temperature": 0.5,
        "top_p": 1,
        "max_tokens": 1024,
        "stream": True,
    }
    response = requests.post(f"{BASE_URL}/chat/completions", headers=headers, json=payload, stream=True)
    if response.status_code == 200:
        return response
    else:
        st.error(f"Erreur API NVIDIA : {response.status_code} - {response.text}")
        return None

# Application Streamlit
st.title("Analyse des journaux système avec NVIDIA LLM")
st.write("Cette application analyse les journaux système pour identifier des événements critiques ou suspects.")

# Upload du fichier
uploaded_file = st.file_uploader("Téléchargez un fichier de journaux système (format texte)", type=["txt"])

if uploaded_file is not None:
    try:
        # Lire le contenu du fichier
        content = uploaded_file.read().decode("utf-8")
        st.write("Fichier chargé avec succès :")
        st.text(content[:500])  # Afficher les 500 premiers caractères

        # Appel à l'API NVIDIA
        response = analyze_logs(content)
        if response:
            st.success("Résultat de l'analyse :")
            result = ""
            for line in response.iter_lines():
                if line:
                    try:
                        chunk = line.decode("utf-8")
                        data = json.loads(chunk.replace("data: ", ""))
                        if "choices" in data and data["choices"][0]["delta"].get("content"):
                            result += data["choices"][0]["delta"]["content"]
                    except json.JSONDecodeError:
                        st.error("Erreur lors du décodage JSON : chunk mal formé.")
                    except Exception as e:
                        st.error(f"Erreur dans le traitement du chunk : {e}")
            # Afficher le rapport d'analyse avec un formatage clair
            st.markdown("### Rapport d'analyse")
            for line in result.split("\n"):
                if line.strip():
                    st.write(f"- {line.strip()}")

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
4. Téléchargez un fichier contenant les journaux système pour les analyser.

---

## Exemple de fichier de journaux système
Créez un fichier nommé `journaux_systeme.txt` avec le contenu suivant pour tester l'application :

```
2025-01-10 12:00:01 User 'admin' logged in from IP 192.168.1.10
2025-01-10 12:15:23 Failed login attempt for user 'root' from IP 10.0.0.15
2025-01-10 12:20:45 User 'john' downloaded sensitive file 'confidential.pdf'
2025-01-10 12:35:50 Unusual traffic detected from IP 10.0.0.20
2025-01-10 12:50:10 User 'admin' performed a password reset for user 'guest'
```

---

## Points importants
- Remplacez la clé API dans le code par une clé valide et personnelle.
- Cette application est un point de départ. Vous pouvez l'enrichir pour fournir des visualisations ou des recommandations plus avancées.

---

## Conclusion
Félicitations ! Vous avez créé une application Streamlit pour analyser des journaux système à l'aide de l'API NVIDIA. Continuez à l’améliorer en explorant d'autres modèles ou en ajoutant des fonctionnalités.

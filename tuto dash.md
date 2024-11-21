## **Tuto 1 sur Dash**
Dash, un framework Python permettant de créer des dashboards interactifs. 
Dash repose sur Flask pour le backend, Plotly pour les graphiques, et React.js pour les composants interactifs.

#### 1. Installation de Dash
Avant de commencer, installez Dash et ses dépendances en exécutant la commande suivante :
```bash
pip install dash
```

#### 2. Structure de base d’une application Dash

Une application Dash repose principalement sur deux éléments :

``layout`` : Définit la structure visuelle de l'application.
``callback`` : Permet de rendre l'application interactive.
Voici un exemple minimaliste :

```bash
import dash
from dash import dcc, html
from dash.dependencies import Input, Output

# Créer l'application Dash
app = dash.Dash(__name__)

# Layout de l'application
app.layout = html.Div([
    html.H1("Mon premier Dashboard"),
    dcc.Input(id='input-text', type='text', placeholder='Tapez quelque chose...'),
    html.Div(id='output-text')
])

# Callback pour interagir dynamiquement avec l'entrée utilisateur
@app.callback(
    Output('output-text', 'children'),
    Input('input-text', 'value')
)
def update_output(value):
    if value:
        return f'Vous avez tapé : {value}'
    return "Entrez du texte ci-dessus."

# Exécuter l'application
if __name__ == '__main__':
    app.run_server(debug=True)
```

#### Ce que fait cet exemple :

``html.Div`` : Crée une division HTML contenant les éléments.
``dcc.Input`` : Champ de saisie utilisateur.
``html.H1`` et ``html.Div`` : Balises HTML pour le texte et la présentation.
``Callback`` : Relie le champ de texte (``input-text``) au contenu de l'élément output-text.

#### 3. Ajout de graphiques interactifs avec Plotly
Vous pouvez facilement intégrer des graphiques avec Dash via la bibliothèque Plotly.

#### Exemple avec un graphique simple :
```bash
import dash
from dash import dcc, html
import plotly.express as px

# Données pour le graphique
df = px.data.gapminder().query("year == 2007")

# Créer le graphique
fig = px.scatter(df, x="gdpPercap", y="lifeExp", 
                 size="pop", color="continent", 
                 hover_name="country", log_x=True, size_max=60)

# Initialisation de l'application
app = dash.Dash(__name__)

# Layout de l'application
app.layout = html.Div([
    html.H1("Graphique interactif avec Dash"),
    dcc.Graph(figure=fig)
])

# Lancer l'application
if __name__ == '__main__':
    app.run_server(debug=True)
```

#### Ce que fait cet exemple :
``plotly.express.scatter`` : Crée un graphique interactif de type scatter.
``dcc.Graph`` : Composant Dash pour afficher le graphique.

#### 4. Création d’interactions complexes avec des callbacks
Exemple : Mettre à jour un graphique en fonction de l’entrée utilisateur :

```bash
import dash
from dash import dcc, html
from dash.dependencies import Input, Output
import plotly.express as px
import pandas as pd

# Données pour le graphique
df = px.data.gapminder()

# Initialisation de l'application
app = dash.Dash(__name__)

# Layout de l'application
app.layout = html.Div([
    html.H1("Sélectionner une année pour afficher les données"),
    dcc.Slider(
        id='year-slider',
        min=df['year'].min(),
        max=df['year'].max(),
        step=5,
        marks={str(year): str(year) for year in df['year'].unique()},
        value=df['year'].min()
    ),
    dcc.Graph(id='graph-output')
])

# Callback pour mettre à jour le graphique
@app.callback(
    Output('graph-output', 'figure'),
    Input('year-slider', 'value')
)
def update_graph(selected_year):
    filtered_df = df[df['year'] == selected_year]
    fig = px.scatter(filtered_df, x="gdpPercap", y="lifeExp",
                     size="pop", color="continent", 
                     hover_name="country", log_x=True, size_max=60)
    return fig

if __name__ == '__main__':
    app.run_server(debug=True)
```

#### Ce que fait cet exemple :
``dcc.Slider`` : Composant interactif pour sélectionner une valeur (une année ici).
``Callback`` : Met à jour dynamiquement le graphique lorsque la valeur du slider change.

#### 5. Organisation d’un projet Dash (Structure avancée)
Pour des projets plus complexes, il est recommandé d’organiser votre code. Voici une structure suggérée :

```bash
my_dash_app/
├── app.py           # Point d'entrée principal
├── layout.py        # Définition du layout
├── callbacks.py     # Définition des callbacks
└── assets/
    ├── style.css    # Fichiers CSS personnalisés
    └── script.js    # Scripts JS personnalisés
```
#### 6. Ajout de CSS pour styliser votre Dashboard
Dash permet de personnaliser facilement votre dashboard avec des fichiers CSS.

Exemple avec CSS :
1. Créez un fichier ``assets/style.css`` :

```bash
body {
    font-family: Arial, sans-serif;
    background-color: #f8f9fa;
}

h1 {
    color: #333;
    text-align: center;
}

div {
    margin: 20px;
}
```
1. Dash chargera automatiquement ce fichier CSS.
notre dashboard sera stylisé avec les règles définies dans le fichier.

#### 7. Hébergement de l’application Dash
Méthode simple avec Heroku :
1. Installer Heroku CLI : Suivez les instructions sur Heroku.
2. Créer un fichier ``Procfile`` dans notre projet :

``web: gunicorn app:server``

3. Créer un fichier ``requirements.txt``:
4. Déployer sur Heroku:

```bash
git init
git add .
git commit -m "Initial commit"
heroku create
git push heroku main
```

#### 8. Ressources utiles pour approfondir
1. Documentation officielle : Dash Documentation
2. Galerie d'exemples : Dash Gallery

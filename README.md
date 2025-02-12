<h1>Semantic Search and Evaluation Across Multi-Domain Text and Image Datasets</h1>

<p>Ce projet implémente une <strong>recherche sémantique avancée</strong> basée sur <strong>Sentence Transformers</strong> et la <strong>similarité cosinus</strong> pour identifier les documents et images les plus pertinents selon une requête donnée.</p>
<p>Les performances sont évaluées à l’aide de <strong>métriques de récupération d’information</strong>.</p>

<h2>Table des matières</h2>
<ul>
    <li><a href="#introduction">Introduction</a></li>
    <li><a href="#dependances">Dépendances</a></li>
    <li><a href="#datasets">Utilisation des jeux de données</a>
        <ul>
            <li><a href="#ntcir">NTCIR-18 AEOLLM</a></li>
            <li><a href="#trec">TREC-COVID</a></li>
            <li><a href="#imageclef">ImageCLEF</a></li>
        </ul>
    </li>
    <li><a href="#workflow">Flux de travail du code</a></li>
    <li><a href="#evaluation">Évaluation des performances</a></li>
    <li><a href="#execution">Exécution du code</a></li>
    <li><a href="#contributions">Contributions</a></li>
</ul>

<h2 id="introduction">1. Introduction</h2>
<p>Ce projet applique des techniques de <strong>traitement du langage naturel (NLP)</strong> et d’<strong>apprentissage automatique</strong> pour analyser et rechercher des informations dans trois jeux de données distincts :</p>
<ul>
    <li><strong>NTCIR-18 AEOLLM</strong> : Un corpus de <strong>questions-réponses annotées</strong>, utilisé pour évaluer des modèles de <strong>retrieval QA</strong>.</li>
    <li><strong>TREC-COVID</strong> : Un corpus de <strong>documents scientifiques liés au COVID-19</strong>, utilisé pour récupérer les études pertinentes en fonction d’une requête.</li>
    <li><strong>ImageCLEF</strong> : Un ensemble de <strong>descriptions textuelles d’images</strong>, utilisé pour la <strong>recherche sémantique d’images</strong>.</li>
</ul>

<h2 id="dependances">2. Dépendances</h2>
<p>Avant d’exécuter le code, installez les bibliothèques nécessaires :</p>
<pre><code>pip install datasets pandas sentence-transformers sklearn lxml tensorflow-datasets</code></pre>
<p>Le projet utilise également <strong>Google Drive</strong> pour stocker et accéder aux fichiers.</p>

<h2 id="datasets">3. Utilisation des jeux de données</h2>

<h3 id="ntcir">NTCIR-18 AEOLLM</h3>
<p>Ce dataset contient des <strong>paires de questions-réponses annotées</strong> et est structuré comme suit :</p>
<ul>
    <li><strong>questionId</strong> : Identifiant de la question.</li>
    <li><strong>question</strong> : Texte de la question.</li>
    <li><strong>answer</strong> : Texte de la réponse associée.</li>
    <li><strong>score</strong> : Score de pertinence.</li>
</ul>
<p><strong>Chargement des données :</strong></p>
<pre><code>from datasets import load_dataset
dataset = load_dataset("THUIR/AEOLLM", split='train')
df = pd.DataFrame(dataset)
df.head()</code></pre>

<h3 id="trec">TREC-COVID</h3>
<p>Ce dataset est une collection de <strong>documents scientifiques</strong> sur le COVID-19. Il comprend :</p>
<ul>
    <li><strong>queries.csv</strong> : Contient les requêtes des utilisateurs.</li>
    <li><strong>corpus.csv</strong> : Contient les articles scientifiques à indexer.</li>
    <li><strong>relevance_data_test.csv</strong> : Contient les jugements de pertinence des articles par rapport aux requêtes.</li>
</ul>
<pre><code>queries = pd.read_csv('/content/drive/MyDrive/queries.csv')
corpus = pd.read_csv('/content/drive/MyDrive/corp.csv')
test = pd.read_csv('/content/drive/MyDrive/relevance_data_test.csv')</code></pre>

<h3 id="imageclef">ImageCLEF</h3>
<p>Ce dataset contient des <em>annotations textuelles d’images</em> extraites de fichiers XML :</p>
<ul>
    <li><strong>Titre</strong> (TITLE)</li>
    <li><strong>Description</strong> (DESCRIPTION)</li>
</ul>
<pre><code>!wget http://www-i6.informatik.rwth-aachen.de/imageclef/resources/iaprtc12.tgz -O iaprtc12.tgz
!mkdir -p /content/drive/MyDrive/dataset
!tar -xvzf iaprtc12.tgz -C /content/drive/MyDrive/dataset/</code></pre>

<h2 id="workflow">4. Flux de travail du code</h2>
<h3>NTCIR-18 AEOLLM</h3>
<ul>
    <li>Chargement des données.</li>
    <li>Nettoyage et regroupement des questions par questionId.</li>
    <li>Encodage des questions et réponses via Sentence Transformers.</li>
    <li>Recherche basée sur la similarité cosinus.</li>
    <li>Évaluation : Precision@30, Recall@30, F1-Score, MAP.</li>
</ul>
<h3>TREC-COVID</h3>
<ul>
    <li>Chargement des fichiers CSV.</li>
    <li>Encodage des documents et requêtes via Sentence Transformers.</li>
    <li>Recherche et évaluation des résultats.</li>
</ul>
<h3>ImageCLEF</h3>
<ul>
    <li>Téléchargement des fichiers XML.</li>
    <li>Extraction des titres et descriptions des images.</li>
    <li>Génération des embeddings et recherche basée sur la similarité cosinus.</li>
</ul>

<h2 id="evaluation">5. Évaluation des performances</h2>

<h3>Précision à K (Precision@K)</h3>
<pre><code>Precision@K = (Documents pertinents retrouvés dans les K premiers résultats) / K</code></pre>

<h3>Rappel à K (Recall@K)</h3>
<pre><code>Recall@K = (Documents pertinents retrouvés) / (Nombre total de documents pertinents)</code></pre>

<h3>F1-Score</h3>
<pre><code>F1@K = 2 × (Precision@K × Recall@K) / (Precision@K + Recall@K)</code></pre>

<h3>MAP (Mean Average Precision)</h3>
<pre><code>MAP = (1 / |Q|) Σ AP(q)</code></pre>

<h2 id="execution">6. Exécution du Code</h2>
<ul>
    <li><strong>NTCIR-18 AEOLLM</strong> :
        <pre><code>python proj_ntcir.py</code></pre>
    </li>
    <li><strong>TREC-COVID</strong> :
        <pre><code>python proj_trec_trials.py</code></pre>
    </li>
    <li><strong>ImageCLEF</strong> :
        <pre><code>python proj_imageclef.py</code></pre>
    </li>
</ul>

<h2 id="contributions">7. Contributions</h2>
<p>Ce projet explore l’efficacité des modèles d’embedding de phrases pour la récupération d’information dans des domaines variés :</p>
<ul>
    <li>Les essais cliniques (<strong>TREC-COVID</strong>).</li>
    <li>Les bases de <strong>Questions-Réponses</strong> (<strong>NTCIR-18 AEOLLM</strong>).</li>
    <li>La recherche d’images par annotation (<strong>ImageCLEF</strong>).</li>
</ul>

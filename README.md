# Comparativa_de_Embeddings
En esta carpeta se recogen las evaluaciones realizadas a diferentes embeddings, 24 evaluaciones en total. Estos embedding se basan en dos grafos de 
conocimiento: WordNet+Gloss y WordNet+Gloss+SyntagNet. Los grafos de conocimiento han sido extrapolados al corpus sintético mediante el random walk the UKB de
Goikoetxea et al. 2015 y Deepwalk, también implementado en UKB (http://ixa2.si.ehu.es/ukb/). El entrenamiento ha sido realizado por el word2vec implementado por
Goikoetxea (https://github.com/JosuGoiko/word2vec_constraints) y la evaluación de la similitud semántica de palabras mediante el evaluate_similarity.py implementado
por Iker García et al. 2020 (https://github.com/ikergarcia1996/MVM-Embeddings/blob/master/evaluate_similarity.py). Tanto los embeddings como los corpus son muy pesados
y no es posible añadirlos en el repositorio. Además, esta incluido el grafo WordNet+Gloss+SyntagNet en formato UKB para acceso público. Para obtener los embeddings
utilizados para esta evaluación es necesario seguir los siguientes pasos:

-Clonar o descargar tanto la colección de programas de UKB, el repositorio word2_constraints de Josu Goikoetxea y el repositorio MVM-Embeddings de Iker García.
-Dentro de la carpeta bin de UKB hay un README con instrucciones sobre como compilar el grafo de UKB a una serialización binaria. Para poder trabajar con el grafo
de este repositorio es necesario tener localmente el wn+gloss+syn.txt y ejecutar el siguiente comando en \bin:

./compile_kb -o wn_gloss_syn.csr64 wn+gloss+syn.txt

-Para formar el corpus con el random walk, en \bin también ejecutar el siguiente comando:

./ukb_walkandprint --srand 5555 num_contextos -K  wn_gloss_syn.csr64 -D wnet30_dict.txt > fichero_de_corpus

-Para el corpus generado por deepwalk:

./ukb_walkandprint --srand 5555 --deepwalk --deepwalk_walks num_caminos --deepwalk_length longitud_camino -K  wn_gloss_syn.csr64 -D wnet30_dict.txt > fichero_de_corpus

-Para entrenar el embedding con el corpus en \word2vec_costraints (los hiperparámetros son los utilizados por Goikoetxea et al. 2015):

./word2vec_constraints -train fichero_corpus -output fichero_embedding -size 300 -window 5 -negative 5 -cbow 0 -lambdasim 0.01

-Para evaluación en la tarea de similitud semántica de palabras dentro de \MVM-Embeddings:

python3 evaluate_similarity.py -i fichero_embedding -lg idioma_del_conocimiento


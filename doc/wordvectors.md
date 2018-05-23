# Word vectors do INFOSIGA

*Word vectors* são [representações multidimensionais de palavras](https://en.wikipedia.org/wiki/Word_embedding), usadas no sistema de triagem automática de boletins de ocorrência, provenientes da Polícia Civil, que alimentam o [INFOSIGA](http://www.infosiga.sp.gov.br/).

## Download
[Link para download dos word vectors treinados](https://github.com/INFOSIGA/TriagemAutom/raw/master/data/vectors/BOvectors.vec)

## Utilização
Usando a biblioteca [gensim](https://radimrehurek.com/gensim/), em ambiente Python >=2.6

### Carregando os *wordvectors* do arquivo ```BOvectors.vec```
```python
from gensim.models import KeyedVectors
emb = KeyedVectors.load('./BOvectors.vec')
```
O arquivo ```BOvectors.vec``` segue o padrão de um cabeçalho com dois números inteiros indicando o tamanho do vocabulário e a dimensão dos vetores. As próximas linhas seguem o padrão de uma palavra por linha, seguida por 300 números (dimensão).

```
41000 300
...
onibus 0.002 0.088 0.013 ...
carro 0.039  0.071  0.075 ...
...
```

### Exemplo de utilização

**Busca as palavras mais similares a um termo**

```python
emb.most_similar('batalhao')
```
```
[('cia', 0.6652517318725586),
 ('pelotao', 0.6569888591766357),
 ('bpmi', 0.6214360594749451),
 ('bpmm', 0.608372151851654),
 ('bpm', 0.6041714549064636),
 ('comando', 0.601152777671814),
 ('departamento', 0.5995941162109375),
 ('bprv', 0.5948061943054199),
 ('cgp', 0.5768635869026184),
 ('policiamento', 0.5643672943115234)]
```

```python
emb.most_similar('morumbi')
```
```
[('iguatemi', 0.7357641458511353),
 ('bras', 0.7071871161460876),
 ('jaragua', 0.7064776420593262),
 ('mooca', 0.7063295841217041),
 ('pinheiros', 0.7012538909912109),
 ('morrinhos', 0.6828417778015137),
 ('aricanduva', 0.6767406463623047),
 ('interlagos', 0.6755187511444092),
 ('butanta', 0.6722168922424316),
 ('pq', 0.6712494492530823)]
```

**Busca as palavras mais similares a combinação de duas palavras**
```python
model.most_similar(['carro', 'caco'])
```
```
[('vidro', 0.5585413575172424),
 ('amassado', 0.5544250011444092),
 ('batente', 0.5441012978553772),
 ('macaco', 0.5432937145233154),
 ('compartimento', 0.5312308669090271),
 ('paramas', 0.5132814049720764),
 ('painel', 0.5076980590820312),
 ('lama', 0.5064489841461182),
 ('pedaço', 0.5054812431335449),
 ('portamalas', 0.4995840787887573)]
 ```

## Sobre a Metodologia

### WordVectors
O modelo utilizou 84 mil Boletins de ocorrência pré-processados para remover números e sequências de caracteres com números. 
As palavras também foram alteradas, removendo-se acentuação de palavras.

**Exemplo:** A palavra 'associação' após a transformação ficaria como 'associaçao'. 

Para o treinamento, o modelo utilizado foi o [word2vec](https://arxiv.org/abs/1301.3781), usando a metodologia *Continuous Bag-of-Words* (CBOW).

### Utilização em Modelos de Classificação
Pela a biblioteca [gensim](https://radimrehurek.com/gensim/), é possível criar facilmente uma camada do tipo [embeddings](https://keras.io/layers/embeddings/) com os vetores pré-inicializados para o uso em modelos de redes neurais usando a biblioteca [Keras](https://keras.io/).

```python
emb.wv.get_keras_embedding(train_embeddings=False)
```

## Referências

[1] [Tomas Mikolov,  Efficient Estimation of Word Representations in Vector Space, 2013](https://arxiv.org/abs/1301.3781)

[2] [Stéphane Mallat. Understanding deep convolutional networks. arXiv preprint arXiv:1601.04920, 2016](https://arxiv.org/pdf/1601.04920.pdf)
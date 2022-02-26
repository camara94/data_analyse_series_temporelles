# Data Analyse Series Temporelles

Dans ce tutoriel, nous allons répondre aux questions suivantes: 
1. Lire les données Microsoft à l'aide du package **Pandas Data reader** 
2. Obtenez le **prix maximum** de l'action de **2017 à 2022** 
3. Quelle est la **date du cours le plus élevé** de l'action ?
4. Quelle est la **date du cours le plus bas** de l'action ?

## Installation du Package pandas_reader

<pre>
<code>
    !pip install pandas-datareader
</code>
</pre>

## Importation des Package

<pre>
<code>
	import pandas_datareader as pdr
	import matplotlib.pyplot as plt
	import numpy as np
	plt.style.use('ggplot')
	%matplotlib inline
</code>
</pre>

## Site Yahoo Finance

Nous allons récupérer la référence de Microsoft sur le **site de yahoo finances** à travers ce lien ci-dessous:

[microsoft share price](https://www.google.com/search?q=microsoft+share+price&rlz=1C1PNJJ_frTN929TN929&oq=microsoft+share&aqs=chrome.3.69i57j0i512l3j0i20i263i512j0i512l5.16901j0j4&sourceid=chrome&ie=UTF-8)

## Créer le Dataset des Actions de microsoft

<pre>
<code>
	df_microsoft = pdr.get_data_yahoo('MSFT')
</code>
</pre>

## Répresantation de la Colonne Close

<pre>
<code>
	plt.figure(figsize=(16,6))
	plt.plot(df_microsoft.loc['2017','Close'])
</code>
</pre>
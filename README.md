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

## Quelques Aggrégations

<pre>
<code>
	les_max = df_microsoft.High.resample('W').agg(['max', 'mean'])
	les_min = df_microsoft.High.resample('W').agg(['min'])
</code>
</pre>

## Répresentation Personnalisée

<pre>
<code>
	plt.figure(figsize=(16,6))
	les_max['2018']['mean'].plot(c='green')
	plt.fill_between(
    		les_max['2018'].index,
    		les_max['2018']['max'],
    		les_min['2018']['min'],
    		alpha=0.3
	)
</code>
</pre>

## Les Prix Minimum et Maximum de 2017 à 2022

<pre>
<code>
	price_max_from_2017_to_2022 = df_microsoft.High.agg(['max'])
	price_min_from_2017_to_2022 = df_microsoft.Low.agg(['min'])
</code>
</pre>

## Affichage du Prix Minimum et Maximum

<pre>
<code>
	index_max = df_microsoft[df_microsoft['High'] == price_max_from_2017_to_2022['max']].index
	index_min = df_microsoft[df_microsoft['Low'] == price_min_from_2017_to_2022['min']].index
	plt.figure(figsize=(16, 8))
	plt.plot(df_microsoft.index, df_microsoft.High, c='g')
	plt.scatter(np.array([index_max]), price_max_from_2017_to_2022['max'], lw=13, c="b", label=f'Le {index_max[0].strftime("%d/%m%Y")}')
	plt.scatter(np.array([index_min]), price_min_from_2017_to_2022['min'], lw=13, c="r" , label=f'Le {index_min[0].strftime("%d/%m/%Y")}')
	plt.legend()
</code>
</pre>

## Définition d'une periode_fr

Cette fonction permet de retourner la periode en français

<pre>
<code>
	def periode_fr(periode):
    		p = ''
    		if periode == 'Q':
        		p = 'Trimestre'
    		elif periode == 'M':
        		p = 'Mois'
    		elif periode == 'D':
        		p = 'Jour'
    		elif periode == 'W':
        		p = 'Semaine'
        
    		return p
</code>
</pre>

## Répresentation Personnalisée des Colonnes

<pre>
<code>
	def graphiphique_perso(df_microsoft, periode, annee, col, fun_agg, c='g'):
    		data = df_microsoft.resample(periode).agg(['max', 'mean', 'min']);
    		plt.figure(figsize=(16, 5))
    		plt.plot(data[annee][col][fun_agg], label=f'{fun_agg}', c=c)
    		plt.fill_between(
        		data[annee][col].index, 
        		data[annee][col]['min'], 
        		data[annee][col]['max'],
        		alpha=0.3)
    		plt.title(f'L\'analyse de la colonne {col} par {periode_fr(periode)}')
    		plt.legend();
</code>
</pre>
# Analysis and Clustring of My Favorite Spotify Songs: Project Overview


I am a huge music fan and, in my opinion, there is no obvious pattern what types of songs I like. Therefore, I decided to look  a little bit deeper into it, perform analysis and try to distinguish some groups.

The aim of this project is to analyze and cluster my favorite Spotify songs. This project consist of the following steps:
* Data gathering 
* Data preparation and exploratory analysis of my streaming history.
* Spotify API: Adjusting class from https://github.com/codingforentrepreneurs/. Adding extra methods for my case. 
* K-Means clustering. Finding an accurate number of clusters using the elbow method.
* Cluster visualisation using TSNE.


## Technologies Used:

*	Python version 3.8.3
*	Pandas
*	Numpy
*	Matplotlib
*	Seaborn 
* Spotify API
*	JSON
*	Requests
*	urlib.parse
* base64
* K-means clustering
* TSNE


## Data Sources and Preparation:
1.  My streaming history.
- I requested songs which I have listened to during the last 365 days  from Spotify website. Then I kept only songs which were played more than 5 times.
2. Spotify API
- I used it to get details about each song, audio features like danceability, loudness or tempo.
Explanations of all audio features can be found at Spotify's developers page https://developer.spotify.com/discover/.
For this I used code from https://github.com/codingforentrepreneurs/
I modified this code and added methods to the class aproperate to my case.



## EDA
I looked at the general info and distribution of the data. Here are some highlights:
### Distribution of audio features
<p align="center">
  <img src="https://github.com/azebryk/My_spotify_songs/blob/master/images/histograms.png" width=900>
</p>
Summary:
- I'd rather like energetic electronic/rock/pop music, so the acousticness and speechiness of my songs is close to 0 and energy and danceability close to 1.

- The mode in the tempo distribution is close to 126 BPM - most of EDM songs have such a tempo

- Instrumentalness looks strange. For better understanding, I plotted cumulative histogram.
### Distribution of Instrumentalness
<p align="center">
  <img src="https://github.com/azebryk/My_spotify_songs/blob/master/images/instrumentalness.png" width=700>
</p>
- As we can see, only 10 % of my songs have instrumentalness above 0.15, but 75% have them below 0.002883, so on the histogram we have a high peak close to 0.

### Correlations
<p align="center">
  <img src="https://github.com/azebryk/My_spotify_songs/blob/master/images/correlations.jpg" width=700>
</p>

- We can see a positive correlation between loudness and energy, which makes sense, and between danceability and valence (musical positiveness), so it's also correct, so you always want to dance to positive sounds :)
- Acousticness is negatively correlated with most of the other features, especially with energy.



## Clustering
As it was shown in the distribution above, features have different ranges. To perform correct clustering using the K-Means algorithm, all features have been standardized using StandardScaler.
### K-Means Clustering - Number of clusters
There are several methods to determine the best number of clusters. In this project I used 2 of those.
1. Elbow method - this method is based on the plot of Distortion Score for a different number of clusters. We should select the point at which we have "elbow". This visualisation was made using *yellowbrick* library.

Selecting the number of clusters using elbow method:
<p align="center">
  <img src="https://github.com/azebryk/My_spotify_songs/blob/master/images/elbow_full2.png" width=500>
</p>
Number of cluster based on elbow method: **6**.

2. The silhouette method -  computes silhouette coefficients of each point that measure how much a point is similar to its own cluster compared to other clusters. by providing a succinct graphical representation of how well each object has been classified.
<p align="center">
  <img src="https://github.com/azebryk/My_spotify_songs/blob/master/images/silhouette.png" width=500>
</p>

## Cluster visualisation using TSNE.
Visualisation of 6 clusters using TSNE:
<p align="center">
  <img src="https://github.com/azebryk/My_spotify_songs/blob/master/images/tsne_6.png" width=600>
</p>

## Model Evaluation
In unsupervised clustering we do not have target features and labels to easily score our model. I will evaluate the model using EDA.

1. Distribution of the clusters.
<p align="center">
  <img src="https://github.com/azebryk/My_spotify_songs/blob/master/images/cluster_dist.png" width=600>
</p>
We can see imbalance in different clusters.

2. Detailed investigation of the clusters.
- Energy
<p align="center">
  <img src="https://github.com/azebryk/My_spotify_songs/blob/master/images/cluster_energy.png" width=600>
</p>

- Tempo
<p align="center">
  <img src="https://github.com/azebryk/My_spotify_songs/blob/master/images/cluster_tempo.png" width=600>
</p>

- Acousticness
<p align="center">
  <img src="https://github.com/azebryk/My_spotify_songs/blob/master/images/cluster_acoust.png" width=600>
</p>

- Loudness
<p align="center">
  <img src="https://github.com/azebryk/My_spotify_songs/blob/master/images/cluster_loud.png" width=600>
</p>

## Clusters explanation
Based on model evaluation and investigation, I will focus on 2 clusters that stand out: cluster nr 5 and 4.

### Cluster nr 5 - EDM songs
I will start with cluster nr 5, since it was clearly separated from all other songs on the t-SNE visualisation. Based on model evaluation, it has:
- the highest energy
- the lowest acousticness
- the narrowest IQR in the tempo box plot
Most of the songs in that cluster have a tempo close to 126 BPM, which is typical tempo for EDM songs. Looking at the artists and titles of the songs this is definitely true. Top artists in that cluster are Oliver Heldens, Tiesto and Dillon Francis.

### Cluster nr 4
This cluster is the opposite of cluster nr 5. Based on model evaluation, it has:
- the lowest energy and loudness
- the highest acousticness
It looks like this cluster is formed by slower songs that I have streamed. The songs that have landed here are from Muse, my fav rock band that, apart of energetic rock songs, has also beautiful ballads, but also from rapers Jay-Z and Post Malone.


## Code and Resources Used 
**Python Version:** 3.8.3  
**Packages:** pandas, numpy, sklearn, matplotlib, seaborn, requests, BeautifulSoup

**Spotify API, Source:** https://github.com/codingforentrepreneurs/


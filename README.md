# Inferring Passengers’ Interactive Choices on Public Transits via MA-AL: Multi-Agent Apprenticeship Learning

Abstract
----
Public transports, such as subway lines and buses, offer affordable ride-sharing services and reduce the road network traffic. Extracting
people’s preferences from their public transit choices is non-trivial. When people travel by public transits, they make sequences of transit choices, and their rewards are usually influenced by the other people’s choices, so this process can be seen as a Markov Game
(MG). In this paper, we make the first effort to model travelers’ preferences of making transit choices using MGs. Based on the discovery that passengers usually never change their policies, we propose novel algorithms to extract the reward functions from the observed, deterministic equilibrium joint policy of all agents in a general-sum MG to infer travelers’ preferences. First, we assume we have the access to the entire joint policy. We characterize the set of all reward functions for which the given joint policy is a Nash equilibrium policy. In order to remove the degeneracy of the solution, we then attempt to pick reward functions so as to maximize the deviation from the the observed policy to the suboptimal policy of each agent. This result in a skillfully solvable linear programming algorithm of the multi-agent inverse reinforcement learning (MA-IRL) problem. Then, we deal with the case where we have access to the equilibrium joint policy through an actual trajectory. We propose an iterative algorithm inspired by singleagent apprenticeship learning algorithms and the cyclic coordinate descent approach. Then, we validate our algorithms using a simple discrete problem. Finally, under the assumption that the actual joint policy is Nash equilibrium and the passengers’ reward functions are linear with the decision-making features, we use the proposed algorithms on a unique real-world dataset (from Shenzhen, China) to extract passengers’ preferences.

Solution Framework
----
![](https://github.com/Trenchant-ymz/WWW2020-MAAL/blob/master/images/solution%20framework.png)
<br> Fig.1 Solution Framework

Data Description
----
In [data_download](https://github.com/Trenchant-ymz/WWW2020-MAAL/tree/master/data_download), we release our dataset. The released dataset was collected and processed by following steps.

### Data Collection: Public Transport Trajectory Data, Transport Graph Data and Bus GPS Data
In Shenzhen, there are automatic fare collection (AFC) systems in all buses and subway stations. Bus passengers tap their smart cards at AFC devices to get aboard, while subway passengers need to tap their cards both when they enter and leave a subway station. We collected the passenger transaction data at AFC devices from buses and subway stations. Each record contains six attributes: card ID, transaction type, cost, record time, station name and transit mode. The transaction type indicates if the record is an event of getting aboard of a bus, or entering/leaving a subway station. The transit mode presents which transportation the passenger takes (e.g., subway line #2). Then, Taking the advantage of the Google Geocoding API [1], we used a bounding square to represent Shenzhen. The square was defined by latitude from 22.42◦ to 22.8◦, while longitude from 113.75◦ to 114.68◦. It covers most of the Shenzhen urban area. We also have access to the bus GPS data which contains each bus's plate number, bus line, and the geographic coordinates corresponding to time slot.

<br>[1] Google GeoCoding. 2016. Road map data. Google. https://developers.google.com/maps/documentation/geocoding/

### Map Griding and Trip Aggregation
In Shenzhen, China, there are about five thousand bus stops and more than one hundred subway stations. Many of these stations are located closely, especially in downtown areas. Usually, stations within a certain walking distance (e.g., 500m) are considered to be close. Hence, we divide the urban area into small regions. For the ease of implementation, we partition the urban area into equal side-length grids using the griding based methods. Fig.2 shows the result, highlighting (in white color) those 1018 grids covered by roads and transit network in Shenzhen, China. Then, we aggregate stations in the transport graph into the grid level. Stations in the same grid are seen as a aggregated station. 
![](https://github.com/Trenchant-ymz/WWW2020-MAAL/blob/master/images/Map%20griding.png)
<br> Fig.2 Map Griding

### Anonymization
To protect the privacy of passengers, we anonymize the card IDs as well as the plate numbers.

### Example
Taking the following small piece of data as an example:

| grid_id | key | route | time | type |	card | stage |
| ------ | ------ | ------ |------ |------ |------ |------ |
|1452 |	cbb639ab400b04e522cfc47584a3702f |	清湖 | 2016-09-06 07:04:24 |	subway_on |	8b688eabf26d1086d3db8a996ee9ee09 |	0 |
|1534 |	17568993a93397afd36b3f042133b2ee |	上梅林 |	2016-09-06 07:26:30 |	subway_off |	8b688eabf26d1086d3db8a996ee9ee09 |	1 |

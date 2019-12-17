# Inferring Passengers’ Interactive Choices on Public Transits via MA-AL: Multi-Agent Apprenticeship Learning

Abstract
----
Public transports, such as subway lines and buses, offer affordable ride-sharing services and reduce the road network traffic. Extracting
people’s preferences from their public transit choices is non-trivial. When people travel by public transits, they make sequences of transit choices, and their rewards are usually influenced by the other people’s choices, so this process can be seen as a Markov Game
(MG). In this paper, we make the first effort to model travelers’ preferences of making transit choices using MGs. Based on the discovery that passengers usually never change their policies, we propose novel algorithms to extract the reward functions from the observed, deterministic equilibrium joint policy of all agents in a general-sum MG to infer travelers’ preferences. First, we assume we have the access to the entire joint policy. We characterize the set of all reward functions for which the given joint policy is a Nash equilibrium policy. In order to remove the degeneracy of the solution, we then attempt to pick reward functions so as to maximize the deviation from the the observed policy to the suboptimal policy of each agent. This result in a skillfully solvable linear programming algorithm of the multi-agent inverse reinforcement learning (MA-IRL) problem. Then, we deal with the case where we have access to the equilibrium joint policy through an actual trajectory. We propose an iterative algorithm inspired by singleagent apprenticeship learning algorithms and the cyclic coordinate descent approach. Then, we validate our algorithms using a simple discrete problem. Finally, under the assumption that the actual joint policy is Nash equilibrium and the passengers’ reward functions are linear with the decision-making features, we use the proposed algorithms on a unique real-world dataset (from Shenzhen, China) to extract passengers’ preferences.

Solution Framework
----

Data Description
----
We use two datasets in our study, including public transit trajectory data (buses and subway lines), and transit graph data. All these
datasets are adjust to the same time period: 06/2016–12/2016.

### Public transport trajectory data. 
We collected the passenger transaction data at AFC devices from buses and subway stations.Each record contains six attributes: card ID, transaction type, cost, record time, station name and transit mode. The transaction type indicates if the record is an event of getting aboard of a bus, or entering/leaving a subway station. The transit mode presents which transportation the passenger takes (e.g., subway line #2).

###  Transport Graph Data. 
Taking the advantage of the Google Geocoding API [1], we used a bounding square to represent Shenzhen. The square was defined by latitude from 22.42◦ to 22.8◦, while longitude from 113.75◦ to 114.68◦. It covers most of the Shenzhen urban area. Within this square, we obtain Shenzhen transport graph containing 892 bus routes and 8 subway lines. This transport graph data serves for feature extracting and can provide the information about buses and subway lines.
[1] Google GeoCoding. 2016. Road map data. Google. https://developers.google.com/maps/documentation/geocoding/

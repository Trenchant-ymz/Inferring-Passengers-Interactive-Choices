# Inferring Passengers’ Interactive Choices on Public Transits via MA-AL: Multi-Agent Apprenticeship Learning

Abstract
----
Public transports, such as subway lines and buses, offer affordable ride-sharing services and reduce the road network traffic. Extracting
people’s preferences from their public transit choices is non-trivial. When people travel by public transits, they make sequences of transit choices, and their rewards are usually influenced by the other people’s choices, so this process can be seen as a Markov Game
(MG). In this paper, we make the first effort to model travelers’ preferences of making transit choices using MGs. Based on the discovery that passengers usually never change their policies, we propose novel algorithms to extract the reward functions from the observed, deterministic equilibrium joint policy of all agents in a general-sum MG to infer travelers’ preferences. First, we assume we have the access to the entire joint policy. We characterize the set of all reward functions for which the given joint policy is a Nash equilibrium policy. In order to remove the degeneracy of the solution, we then attempt to pick reward functions so as to maximize the deviation from the the observed policy to the suboptimal policy of each agent. This result in a skillfully solvable linear programming algorithm of the multi-agent inverse reinforcement learning (MA-IRL) problem. Then, we deal with the case where we have access to the equilibrium joint policy through an actual trajectory. We propose an iterative algorithm inspired by singleagent apprenticeship learning algorithms and the cyclic coordinate descent approach. Then, we validate our algorithms using a simple discrete problem. Finally, under the assumption that the actual joint policy is Nash equilibrium and the passengers’ reward functions are linear with the decision-making features, we use the proposed algorithms on a unique real-world dataset (from Shenzhen, China) to extract passengers’ preferences.

Data Description
----
In [Download_Data](https://anonymous.4open.science/repository/4fe4a551-b4df-43cf-8cb7-47dff24fe608/Download_Data/), <font color=red> Please refer to the link on the top of this page to download the released datase if the link is not available </font>
, we release our dataset. The released dataset was collected and processed by following steps.

### Data Collection: Public Transport Trajectory Data, Transport Graph Data and Bus GPS Data
In Shenzhen, there are automatic fare collection (AFC) systems in all buses and subway stations. Bus passengers tap their smart cards at AFC devices to get aboard, while subway passengers need to tap their cards both when they enter and leave a subway station. We collected the passenger transaction data at AFC devices from buses and subway stations. Each record contains six attributes: card ID, transaction type, cost, record time, station name and transit mode. The transaction type indicates if the record is an event of getting aboard of a bus, or entering/leaving a subway station. The transit mode presents which transportation the passenger takes (e.g., subway line #2). Then, Taking the advantage of the Google Geocoding API [1], we used a bounding square to represent Shenzhen. The square was defined by latitude from 22.42◦ to 22.8◦, while longitude from 113.75◦ to 114.68◦. It covers most of the Shenzhen urban area. We also have access to the bus GPS data which contains each bus's plate number, bus line, and the geographic coordinates corresponding to time slot.

<br>[1] Google GeoCoding. 2016. Road map data. Google. https://developers.google.com/maps/documentation/geocoding/

### Map Griding and Trip Aggregation
In Shenzhen, China, there are about five thousand bus stops and more than one hundred subway stations. Many of these stations are located closely, especially in downtown areas. Usually, stations within a certain walking distance (e.g., 500m) are considered to be close. Hence, we divide the urban area into small regions. For the ease of implementation, we partition the urban area into equal side-length grids using the griding based methods. Then, we aggregate stations in the transport graph into the grid level. Stations in the same grid are seen as a aggregated station. 


### Anonymization
To protect the privacy of passengers, we anonymize the card IDs as well as the plate numbers.

### An Example
Based on the starting point and the destination, we divide data into 295 groups. All the passengers in the same group travel from the same grid to a same destination. Then in each group, we classify the data based on the time. Trajectories on the same day are put in a .csv file, while records on the same month are put in a folder. The specific data are as follows：

| grid_id | key | route | time | type |	card | stage |
| ------ | ------ | ------ |------ |------ |------ |------ |
|1814 |	cf98d760e6328e46e508021a8aca74fb |	379 |	2016-09-05 07:13:05	| bus_on |	ae77849a6dacb5e609625acd782a0833 |	0 |
|1856 |	cf98d760e6328e46e508021a8aca74fb |	379 |	2016-09-05 07:47:35 |	bus_off |	ae77849a6dacb5e609625acd782a0833 |	1 |

There are seven attributes: *grid_id* indicates the grid coordinates; *key* represents the anonymized plate numbers of buses (when the transit mode is subway, it represents a subway station); *route* indicates a subway station or a but routes; *time* is the time slot the passenger taps the card; *type* indicates if the record is an event of getting aboard of a bus, or entering/leaving a subway station; *card* is the anonymized card_ID; *stage* represents the stage in the journey (starts from 0). This example data represents that on grid 1814 a passenger whose card_ID is 'ae77849a6dacb5e609625acd782a0833' got on the bus line 379 at 2016-09-05 07:13:05, and the plat number of the bus was 'cf98d760e6328e46e508021a8aca74fb'. Then the passenger got off the bus at 2016-09-05 07:47:35 on grid 1856.



Change Log
-----

### 2019/12/16
Due to the time, we only released the data for two weeks. We will update the complete six-month data, as well as the code soon.

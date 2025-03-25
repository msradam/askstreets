# 🛣️ AskStreets: Query and Visualizing Street Networks using OpenStreetMap, ArangoDB, and LangGraph
Author: Adam Munawar Rahman, March 2025

🏆 AskStreets is the Second Place Winner of ArangoDB and Nvidia's "Building the Next-Gen Agentic App with GraphRAG & NVIDIA cuGraph" Hackathon! See the project submission at https://devpost.com/software/askstreets-querying-and-visualizing-street-networks

Using powerful open-source libraries like OSMnx, we can retrieve geographic features and street network datasets from OpenStreetMap and persist them as graphs and collections in ArangoDB. Then, via a  LangGraph ReAct agent, we feed natural language queries to LLM-based tools to execute complex lookups, run GPU-backed graph algorithms, and visualize geospatial coordinates. This agentic app enables meaningful insights into the network properties of the desired geographic location, and empowers us to address real-world infrastructure challenges.

## Requirements
- Python 3.10
- An ArangoDB instance, e.g. a [local Docker container](https://arangodb.com/download-major/docker/)
- (for GPU acceleration) Nvidia CUDA toolkit, a compatible NVIDIA GPU, and RAPIDS (follow the install guide [here](https://docs.rapids.ai/install/))
- A `credentials.yml` file with API keys for your desired AI platform (see sample file in repo)

## Running the Code

Clone the repo, then in the `askstreets` directory:
```
python3.10 -m venv env
source env/bin/activate 
pip3 install -r requirements.txt
```

Ensure your ArangoDB instance is running, e.g. to startup the local Docker container:
```
docker run -e ARANGO_ROOT_PASSWORD=<root_password> -p 8529:8529 arangodb/arangodb
```

Then startup the Jupyter server:
```
jupyter-notebook
```

There are two versions of the AskStreets notebook:
- `askstreets-gpu.ipynb` which is the hackathon submission with GPU acceleration and fully answered query samples
- `askstreets.ipynb` which has cleared cell output and does not have GPU acceleration

`askstreets-gpu.ipynb` also contains output for additional queries that were not demo'd in the video submission.

## Inspiration
During my internship at UNICEF Innovation in NYC, I used OpenStreetMap, OSMnx, and NetworkX to calculate distances between schools and health facilities in UNICEF programme countries using national street and road networks. By writing code to execute graph algorithms, I was able to generate data to address a real-world issue.

I would like to generalize this challenge towards a broader use case: if you're a city planner, business owner, architect, etc., you may wish to ask questions about street networks (e.g. accessibility of services, routed distances between landmarks, crowded intersections, etc.) without needing to code lengthy algorithms. This is where AskStreets comes in: by providing the ability to translate natural language queries into street network analysis.

## What it does
The Jupyter Notebook submission contains the proof-of-concept for AskStreets. The agentic app uses a LangGraph ReAct agent that accepts user queries and calls multiple tools that can geocode, generate and run AQL, generate and run OSMnx/NetworkX code, and visualize points on a Folium map.

By passing a query to the `query_street_network` function, it will invoke the ReAct agent to run these tools to interpret the query, execute code, and provide a natural language response or map visualization for the specific question.

## How we built it
I used OSMnx to download street network graphs and geographic features for certain locations, prepared this data to load into ArangoDB, then wrote the LLM-based tools and ReAct agent code for the agentic app. 

## Challenges we ran into
Prompt engineering was a large aspect of this project; I needed to revise the prompts per tool multiple times as I continuously tested and analyzed the output of the ReAct agent, so that I could provide the AI models with more context to accurately interpret user queries and assemble the correct code. Modifying the tools to work in tandem with each other to generate the correct intermediate queries to pass between them was also challenging, but ultimately rewarding.

## Accomplishments that we're proud of
I particularly enjoyed seeing the outcome of these tools, which are able to accurately interpret user queries about street networks and correctly identify the geographic attributes to filter or run algorithms against. I especially appreciated that by loading additional datasets like OpenStreetMap features and health facility data into ArangoDB, along with the graph network, the AQL tool was able to pull data across these sources to answer user queries with a higher degree of specificity. 

## What we learned
I learned how to prepare OSMnx data to load into ArangoDB, how to invoke LLMs and prompt engineer to improve answers to queries, and how to work with the LangChain and LangGraph libraries.

## Citations
Boeing, G. (2024). Modeling and Analyzing Urban Networks and Amenities with OSMnx. Working paper. https://geoffboeing.com/publications/osmnx-paper/

## What's next for AskStreets: Querying and Visualizing Street Networks
Working with geospatial data has so much potential! In the submission notebook, I demonstrate at the end how health facility data can be overlaid with the street network graph and OpenStreetMap features data so that it can also be queried by AQL. By overlaying and combining even more data sets, we can enhance the app's ability to answer many different types of queries. 

The AI tools can also be enhanced to be more precise, for example, different models can be deployed for each tool for their particular use case. More tools can also be written - for example, to dynamically pull in more data from OpenStreetMap - or the visualization tool can be updated to draw paths between points. The prompts can also be further tuned to avoid redundant operations like repeating coordinate lookups.

This app can be further enhanced with a UI that can not only accept queries, but also automatically download OSM road networks and features from a user-specified location. It can even be integrated with a sophisticated mapping tool like kepler.gl that offers more visualization options for large geospatial datasets.

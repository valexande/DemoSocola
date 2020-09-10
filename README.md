# DemoSocola
My components for the first demo of Socola


Currently this repo contains only the components of Alexandros Vassiliades


## (A) Component Name: Subgraph Creator

**Brief Description:** This component will receive input the label of an Object and an Action (potentially also the label the pro and meta state), and will create a subgraph for each label using information from ConceptNet.

**Input:** The label of an Object and an Action

**Output:** A subgraph for the Object, and a subgraph for the Action, created from the knowledge graph of ConceptNet

**Process:**
The implementation of this component is straight forward, it will receive as input the label of an object and the label of an action (potentially it will receive the label of some states), and will create a subgraph of ConceptNet for each one. The component searches if the entity exists in the ConceptNet knowledge graph, and extracts all the nodes that are connected with it at depth 1 and 2. The component searches for nodes at depth 1 and 2 only based only on specific ConceptNet properties, which we can define as we desire. We keep it at depth 2 for now, because otherwise each subgraph needsf hours to be created, but if something like this is needed we can extend the component to do it. Finally, the subgraphs are exported into .json file for each pair of object-action, and they have the form:
{object:{N1:{property1:[....], property2:[...]}, N2:{property1:[....], property2:[...]}}, action:{N1:{property1:[....], property2:[...]}, N2:{property1:[....], property2:[...]}}}.



## (B) Component Name: Graph Similarity

**Brief Description:** This component will receive as input two graphs and will infer how much they are semantically related.

**Ιnput:** Two graphs.

**Output:** A score on how much the two graphs are semantically connected

        (1) the number of identical entities 
        
        (2) similarity based on DBpedia comment boxes
        
        (3) WUP similarity for the given entities
        
        (4) the number of different paths (without loops)
        
        (5) the existence or frequency of a given relation we value as intuitive (e.g., usedFor) that is found in paths
        
**Process:**
This component will receive as input two graphs that were produced by the Subgraph Creator Component, and it will indicate if the two graphs are semantically connected and in which ways. Basically, is a knowledge graph parser, over small knowledge graphs where it will search for common nodes, and the variety of paths in which we could reach the centroid of one graph to the other. The centroid of each graph shall be considered as the node for which the graph was created. Therefore, if the parser finds common nodes, which are part of the english language,  it will infer that the two centroids are semantically connected. Moreover, if there are more paths than common nodes, from which we could reach one centroid to the other, the conclusion that the two centroids are semantically connected will be further backed up.

Also, consider including ontological knowledge when comparing graphs and calculating path-based or relation-based metrics. For example, you may be given the term knife; apart from what is found in ConceptNet, you can also include information found maybe in DBPedia that knife is a type of tool used in the kitchen. In that case, you may add in the graph information about such tools and calculate metrics including such information.



## (C) Upper Level Ontology and Knowledge Graph Generator

**Brief Description:** This component generates an RDF schema for the ULO

**Ιnput:** List of Triplets

**Output:** Upper Level Ontology + Domain Specific part of Ontology

**Process:**
This component populates the upper level ontology with information returned by the computer vision module. The upper level ontology was created based on other relevant studies about household robotics, and it contains the most important features needed in the knowledge representation of a cognitive robotic system which acts in a household environment. This component inserts information in the upper level ontology about Object and Actions Hierarchies, Object-Action relations, and Object-State relations. Basically, this component takes as input triplets from the vision module, and tries to infer what kind of information it is, in order to insert it in the appropriate part of the upper level ontology. The component receives triplets as input and based on the similarity of the Graph Similarity component decides if this information should exist in the KG of the framework.
For example, if we have an object and an action returned by the vision module and the Graph Similarity component shows that the subgraph of these two are adequately related, it will insert the information (action, performedBy, object).

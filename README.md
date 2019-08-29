# Linked Data Browser prototype for NEON vocabularies.

This repository contains a brief demonstrator of publishing NEON vocabularies as linked open data using the [Zazuko Trifid](https://github.com/zazuko/trifid) Lightweight Linked Data Server and Proxy as a browser platform. The demo is build from two sub-repositories, [one containing a forked version of Trifid](https://github.com/cicoe/trifid-neon) and [second sub-repository](https://github.com/cicoe/fuseki-geosparql-docker) the has a version of [Apache Fuseki](https://jena.apache.org/documentation/fuseki2/) with GeoSPARQL support. An [RDF knowledge graph](https://github.com/cicoe/fuseki-geosparql-docker/blob/master/data/geosparql_test.rdf) replaces the default Knowledge Graph in the fuseki-geosparql-docker container with one that is based on the NEON site Abby Road. The compose file also replaces the default Trifid configuration JSON file with one that is configured to use the containerized version Apache Fueski as a SPARQL endpoint. 

Note: Since the URI namespace prefix is @prefix neon: <http://ld.neonscience.org>, an entry in the local /etc/hosts/ file aliasing ld.neonscience.org to localhost for the demo to resolve properly. 
```
127.0.0.1 ld.neonscience.org 
```

To execute the demo:
```
docker-compose up
```

The demo uses the localhost port 80 by default. A SPARQL endpoint is proxied to [http://ld.neonscience.org/query](http://ld.neonscience.org/query) and a YASGUI interface is available at [http://ld.neonscience.org/sparql](http://ld.neonscience.org/sparql).

## Sparql query #1:
```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX schema: <http://schema.org/>
SELECT * WHERE {
  ?sub ?pred schema:Place .
} 
LIMIT 10
```
Returns the first 10 entities in the knowledge graph that are schema:Place which is automatically populated because geoschemas:ResearchSite is a subclassOf schema:Place in the knowledge graph.

## SPARQL query #2
```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX hydra: <http://www.w3.org/ns/hydra/core#>
PREFIX sosa-ext: <http://www.w3.org/ns/ssn/ext#>
SELECT * WHERE {
  ?sub ?pred sosa-ext:ObservationCollection .
} 
LIMIT 10
```
Returns the first 10 of the Observation Collections in the knowledge graph.

## SPARQL query #3
```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT * WHERE {
  <http://coe1.crc.nd.edu/v1.0/Datastream> ?pred ?obj .
} 
LIMIT 10
```

Returns links to the SensorThings Datastream URL for a observation collection.

## Notes:
There is currently an issue with Trifid and the YASGUI interface that returns a '405' error if a http GET is not made on the query endpoint first. Execute from the command line. 
```
curl http://localhost/query
```

YASGUI SPARQL queries work normally after this operation.
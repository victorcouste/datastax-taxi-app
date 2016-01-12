Transaction Search Demo
========================

This demo traces moving vehicles as they pass through geohash tiles. It also keeps track of a vehicle movements on a day to day basis. Similar to a vessel tracking or taxi application.  

The application 

1. Allows the user to track a vehicles movements per day.

2. Find all vehicles per tile. Tiles have 2 sizes. Tile1 is large, Tile2 is small. 

3. Find all vehicles within a given radius of any vehicle

 
To create the schema, run the following

	mvn clean compile exec:java -Dexec.mainClass="com.datastax.demo.SchemaSetup"
	
To create the solr core, run 

	dsetool create_core datastax_taxi_app.current_location reindex=true coreOptions=src/main/resources/solr/rt.yaml schema=src/main/resources/solr/geo.xml solrconfig=src/main/resources/solr/solrconfig.xml
	
To continuously update the locations of the vehicles run 
	
	mvn clean compile exec:java -Dexec.mainClass="com.datastax.taxi.Main"
	
To find all movements of a vehicle 

	http://localhost:8080/datastax-taxi-app/rest/getmovements/1/20160112


To find all vehicle movement, use the rest command

	http://localhost:8080/datastax-taxi-app/rest/getvehicles/gcrf

or 

	CQL - select * from current_location where solr_query = '{"q": "tile1:gcrf"}';


To find all vehicles within a certain distance of a latitude and longitude. 

	http://localhost:8080/datastax-taxi-app/rest/search/52.53956077140064/-0.20225833920426117/5
 	
To remove the tables and the schema, run the following.

    mvn clean compile exec:java -Dexec.mainClass="com.datastax.demo.SchemaTeardown"
    
    

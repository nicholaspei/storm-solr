Storm Solr Integration
===========================

Integrates [Storm](https://github.com/nathanmarz/storm/) and  [Apache Solr](http://lucene.apache.org/solr/) by providing a generic and configurable `backtype.storm.Bolt` 
implementation that create Solr indexes based on your Storm `Tuple` objects.

### Using as a Maven dependency

**Building from Source**

		$ mvn install
		
**Declare it in your pom.xml**

	<dependency>
		<groupId>storm</groupId>
		<artifactId>storm-solr</artifactId>
		<version>1.0-SNAPSHOT</version>
	</dependency>		

### Usage

**Basic Usage**

The `SimpleSolrBolt` class provides a convenience constructor that takes only the URL where Solr is running as argument:

		IRichBolt solrBolt = new SimpleSolrBolt("http://localhost:8983/solr");

The above constructor will create a `SolrBolt` that writes to the "`http://localhost:8983/solr`" Solr instance.
The idea here is to convert a Tuple into a SolrInputDocument.

For each field in the `backtype.storm.Tuple` received, the `SimpleSolrBolt` will write a SolrInputField into a SolrInputDocument.
So keep in mind that you must have your tuple fields defined in your solr schema.xml.

For example, given a tuple value of:

		{id: 12345, username: "joaopedro", full_name: "João Pedro"}

Your Solr schema.xml could be something like this :
	
	 <fields>   
	  <field name="id"      	type="long"   	indexed="true"  stored="true"  multiValued="false" required="true"/>
	  <field name="full_name"   type="string"   indexed="true"  stored="true"  multiValued="false" /> 
	  <field name="username"    type="string"   indexed="true"  stored="true"  multiValued="false" /> 
	 </fields> 

**Important note**

For performance reasons SolrBolt is not responsible for committing. 
I highly recommend leaving this job for Apache Solr. Take a look at [autoCommit](http://wiki.apache.org/solr/SolrConfigXml#Update_Handler_Section) feature.
	






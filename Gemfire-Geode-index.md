If you are creating multiple indexes on the same region, first define your indexes and then create the indexes all at once to avoid iterating over the region multiple times. See Creating Multiple Indexes at Once for details.


**Tips for Writing Queries that Use Indexes**

* In general an index will improve query performance if the FROM clauses of the query and index match exactly.

* The query evaluation engine does not have a sophisticated cost-based optimizer. It has a simple optimizer which selects best index (one) or multiple indexes based on the index size and the operator that is being evaluated.

* For AND operators, you may get better results if the conditions that use indexes and conditions that are more selective come before other conditions in the query.

* Indexes are not used in expressions that contain NOT, so in a WHERE clause of a query, qty >= 10 could have an index on qty applied for efficiency. However, NOT(qty < 10) could not have the same index applied.

* Whenever possible, provide a hint to allow the query engine to prefer a specific index. See Using Query Index Hints

-------------------------------------------------------------------------------

**Create Index Example :**

Using gfsh:


    gfsh> create index --name=myIndex --expression=status --region=/exampleRegion
    gfsh> create index --name=myKeyIndex --type=key --expression=id --region=/exampleRegion
    gfsh> create index --name=myHashIndex --type=hash --expression=mktValue --region=/exampleRegion




Using Java API:

     QueryService qs = cache.getQueryService();
     qs.createIndex("myIndex", "status", "/exampleRegion");
     qs.createKeyIndex("myKeyIndex", "id", "/exampleRegion");
     qs.createHashIndex("myHashIndex", "mktValue", "/exampleRegion");


Using cache.xml:

    <region name=exampleRegion>
        <region-attributes . . . >
    </region-attributes>
      <index name="myIndex" from-clause="/exampleRegion" expression="status"/>
      <index name="myKeyIndex" from-clause="/exampleRegion" expression="id" key-index="true"/>
      <index name="myHashIndex" from-clause="/exampleRegion p" expression="p.mktValue" type="hash"/>
      ...
     </region>

Spring : 

    <gfe:index id="myIndex" expression="someField" from="/someRegion"/>



**Listing and remove Indexes**

      gfsh> list indexes
      gfsh> list indexes --with-stats

      gfsh> destroy index
      gfsh> destroy index --name=myIndex
      gfsh> destroy index --region=/exampleRegion

-------------------------------------------------------------------------------
**Creating Key Indexes **

When data is partitioned using a key or a field value

    create index --name=myKeyIndex --expression=id --region=/exampleRegion

xml:

    <region name=exampleRegion>
      <region-attributes . . . >
    </region-attributes>
     <index name="myKeyIndex" from-clause="/exampleRegion" expression="id" key-index="true"/>
    ...
    </region>


Tere are two issues to note with key indexes:

The key index is not sorted. Without sorting, you can only do equality tests. Other comparisons are not possible. To obtain a sorted index on your primary keys, create a functional index on the attribute used as the primary key.
The query service is not automatically aware of the relationship between the region values and keys. For this, you must create the key index.




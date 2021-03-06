Neo4jPHP
========
Author: Josh Adell <josh.adell@gmail.com>  
Copyright (c) 2011  

PHP Wrapper for the Neo4j graph database REST interface

Install
-------
Copy or symlink the `lib/Everyman` directory into your `include_path` or autoloader path.


Examples
--------
Coming soon.


API
---

### Transport

    __construct(string $host='localhost', integer $port=7474)
Create a new REST transport.

### Client

    __construct(Transport $transport)
Create a new Neo4j client.

    getLastError() : integer
Return any error code from the last operation.  Returns null if no error occurred.

### Node

    __construct(Client $client)
Create a new Node instance.  Nodes are not saved to the server until `save` is called.

    delete() : boolean
Delete this node from the server.  Returns true on success, false otherwise.

    findPathsTo(Node $to, string $type=null, string $dir=null) : PathFinder
Return a PathFinder that can be used to get paths from the node to $to.  $dir must be one of `Relationship::DirectionAll`, `Relationship::DirectionIn`, `Relationship::DirectionOut`, or null.  If $dir is specified, $type must also be specified.

    getId() : integer
Return this node's unique identifier.

    getProperties() : array
Return an array of all properties set on this node, indexed by key.

    getProperty(string $property) : string
Return the value of the named property.  Returns null if the named property is not set.

    getRelationships($types=array(), $dir=null) : array
Return an array of Relationships that match the given criteria.  $dir must be one of `Relationship::DirectionAll`, `Relationship::DirectionIn`, `Relationship::DirectionOut`, or null.

    hasId() : boolean
Returns true if the node is identified, false otherwise.

    load() : boolean
Load this node's data from the server.  Returns true on success, false otherwise.

    relateTo(Node $to, string $type) : Relationship
Create a relationship to $to.  Note that the Relationship is not saved to the server until its `save` method is called.

    removeProperty(string $property) : Node
Removes the named property from the node.  Returns the node.

    save() : boolean
Save this node to the server.  Returns true on success, false otherwise.

    setId(integer $id) : Node
Set the id of this node before calling `load` to retrieve the node's data from the server.  Returns the node.

    setProperties(array $properties) : Node
Set multiple properties, indexed by key.  Returns the node.

    setProperty(string $property, mixed $value) : Node
Set the named property to the given value.  $value must be scalar.  Returns the node.

### Relationship

    __construct(Client $client)
Create a new Relationship instance.  Relationships are not saved to the server until `save` is called.

    delete() : boolean
Delete this relationship from the server.  Returns true on success, false otherwise.

    getId() : integer
Return this relationship's unique identifier.

    getEndNode() : Node
Return the node on the incoming end of this relationship.  Returns null of none is set.

    getProperties() : array
Return an array of all properties set on this relationship, indexed by key.

    getProperty(string $property) : string
Return the value of the named property.  Returns null if the named property is not set.

    getStartNode() : Node
Return the node on the outgoing end of this relationship.  Returns null of none is set.

    getType() : string
Return the type of relationship.

    hasId() : boolean
Returns true if the relationship is identified, false otherwise.

    load() : boolean
Load this relationships's data from the server.  Returns true on success, false otherwise.

    removeProperty(string $property) : Relationship
Removes the named property from the relationship.  Returns the relationship.

    save() : boolean
Save this relationship to the server.  Returns true on success, false otherwise.

    setEndNode(Node $end) : Relationship
Set the relationship's incoming node.  Returns the relationship.

    setId(integer $id) : Relationship
Set the id of this relationship before calling `load` to retrieve the relationship's data from the server.  Returns the relationship.

    setProperties(array $properties) : Relationship
Set multiple properties, indexed by key.  Returns the relationship.

    setProperty(string $property, mixed $value) : Relationship
Set the named property to the given value.  $value must be scalar.  Returns the relationship.

    setStartNode(Node $start) : Relationship
Set the relationship's outgoing node. Returns the relationship.

    setType($type) : Relationship
Set the type of relationship.  Returns the relationship.

### Index

    __construct(Client $client, string $type, string $name)
Create a new index.  Indexes are not saved to the server until `save` or `add` are called.  $type must be one of `Index::TypeNode` or `Index::TypeRelationship`

    add(mixed $entity, string $key, mixed $value) : boolean
Add a Node or Relationship to the index.  $value must be scalar.  If the index did not exist on the server previously, it is created.  Returns true on success, false otherwise.

    delete() : boolean
Remove the index from the server.  Returns true on success, false otherwise.

    find(string $key, mixed $value) : array
Find Nodes or Relationships where the named property has the given value.  $value must be scalar.

    findOne(string $key, mixed $value) : mixed
Find the first Node or Relationship where the named property has the given value.  $value must be scalar.  Returns null if no match is found.

    getName() : string
Return the index name.

    getType() : string
Return the index type, one of `Index::TypeNode` or `Index::TypeRelationship`.

    remove(mixed $entity, string $key=null, mixed $value=null) : boolean
Remove the given Node or Relationship from the index.  If given, $value must be scalar.

    save() : boolean
Save this index to the server.  Return true on success, false otherwise.

### Path

    __construct()
Create a new Path.  The default context for a Path is nodes.

    count() : integer
If current context is `Path::ContextNode`, returns the number of nodes in the Path.  Otherwise, returns the number of relationships in the Path.

Path implements the `Countable` interface, which means it can be used as `count($path)`.

    getContext() : string
Returns the current context, one of `Path::ContextNode` or `Path::ContextRelationship`.
	
    getEndNode() : Node
Return the Node at the end of the path.  Returns null if there are no nodes.

    getLength() : integer
Alias for `count`

    getIterator() : ArrayIterator
If current context is `Path::ContextNode`, returns the nodes in an ArrayIterator.  Otherwise, returns the relationships in an ArrayIterator.

Path implement the `IteratorAggregate` interface, which means it can be looped over in `foreach` loops.  If the current context is nodes, `foreach` will loop over the nodes in the path, otherwise it will loop over the relationships.

    getNodes() : array
Return the ordered array of Node objects that make up this path.

    getRelationships() : array
Return the ordered array of Relationship objects that make up this path.

    getStartNode() : Node
Return the Node at the beginning of the path.  Returns null if there are no nodes.
	
    setContext($context) : Path
Set whether `count` and `foreach` will refer to the nodes or relationships of this path.  $context should be one of `Path::ContextNode` or `Path::ContextRelationship`.  Returns the Path.

### PathFinder

    __construct(Client $client)
Create a new PathFinder object.

    getAlgorithm() : string
Return the current path search algorithm.  One of `PathFinder::AlgoShortest` (default), `PathFinder::AlgoAll`, `PathFinder::AlgoAllSimple` or `PathFinder::AlgoDijkstra`.

    getCostProperty() : string
Return the current relationship property to use to determine path cost.  Only used when `PathFinder::AlgoDijkstra` is the search algorithm.

    getDefaultCost() : numeric
Return the default cost to assign relationships without a cost property.  Only used when `PathFinder::AlgoDijkstra` is the search algorithm.

    getDirection() : string
Return the current path finding direction, one of the `Relationship::Direction` constants.

    getEndNode() : Node
Return the Node to find paths to.

    getMaxDepth() : integer
Return the current maximum length for found paths.

    getPaths() : array
Return an array of Path objects matching the search criteria.

    getSinglePath() : Path
Return the first Path matching the search criteria.

    getStartNode() : Node
Return the Node to find paths from.

    getType() : string
Return the current relationship type to which path relationships will be limited.

    setAlgorithm(string $algorithm) : PathFinder
Set the path search algorithm.  One of `PathFinder::AlgoShortest` (default), `PathFinder::AlgoAll`, `PathFinder::AlgoAllSimple` or `PathFinder::AlgoDijkstra`.  Returns the PathFinder.

    setCostProperty(string $property) : PathFinder
Set the relationship property to use to determine path cost.  Only used when `PathFinder::AlgoDijkstra` is the search algorithm.  Returns the PathFinder.

    setDefaultCost(numeric $cost) : PathFinder
Set the default cost to assign relationships without a cost property.  Only used when `PathFinder::AlgoDijkstra` is the search algorithm.  Returns the PathFinder.

    setDirection($dir) : PathFinder
Set the path finding direction, one of the `Relationship::Direction` constants.  Returns the PathFinder.

    setEndNode(Node $end) : PathFinder
Set the Node to find paths to.  Returns the PathFinder.

    setMaxDepth($max) : PathFinder
Set the maximum length for found paths.  Returns the PathFinder.

    setStartNode(Node $start) : PathFinder
Set the Node to find paths from.  Returns the PathFinder.

    setType($type) : PathFinder
Set the relationship type to which path relationships will be limited.  Returns the PathFinder.




To Do
-----
* Querying an index
* Traversal
* Batch/transaction support? (experimental)
* Caching



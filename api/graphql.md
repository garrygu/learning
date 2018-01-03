> GraphQL forces your hand into the one option that it allows: high customization and no [network] cachability.

- [GraphQL vs REST: Overview (01/24/2017)]https://philsturgeon.uk/api/2017/01/24/graphql-vs-rest-overview/  
If your REST API is following good practices, like allowing careful evolution instead of global versioning, serializing data instead of returning directly from data store, implementing sparse fieldsets to allow slimming down response sizes, GZiping contents, outlining data structures with JSON Schema, offering binary alternatives to JSON like Protobuff or BSON, etc., then the advertised advantages of GraphQL seem to fall a bit short.

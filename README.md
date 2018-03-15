# LongId - better than Snowflake / Snowizard

Simple Java UUID generator.

Replacement for auto-incrementing ID, especially in multi-server multi-datacenter environment.

Created in response to complexity of Snowflake and Snowizard, it's only a single class.

**Advantages:**

- SQL inserts will always be at bottom of table when used as primary key
  - ID always larger than previous
  - algorithm: currentTimeMillis & intraMillCounter & serverId

- thread-safe
  
- Single class

- 8byte Java 'long' result, perfect for primary key

- Single or Multiple Servers 

  - Does not require server coordination

  - Instantiate with serverId (0-4095) for 100% assurance of uniqueness

- 256,000 unique IDs per second per server

- Good from 1970 to 2557


**Installation**

- copy the source into your code
- or
- compile 'com.communicate:longid:1.0'

**Use Example: Prototype**
```
  int serverId = 0;  
  LongId.getNewId(serverId);
```

**Use Example: Instantiated**
```
  int serverId = 0;
  LongId idGenerator = new LongId(serverId);
  
  idGenerator.getNewId();
```

**Disadvantages:**

- Not technically a UUID generator as it's not Universal, that would require 128bits/16bytes or more
  - Uniqueness is guaranteed within a single server and multi-server network when serverId is used

- 256,000 ID's per second is theoretical, really it's a max of 256 per millisecond, when exceeded will sleep for 1ms
  - during multi-threaded testing on quad-core machine, we rarely exceeded 256 per millisecond

- Multi-server / Multi-datacenter may result in SQL inserts not exactly at the bottom of the table, depending on delay of insertion and quantity of servers.  But it will be in the bottom pages, and almost guaranteed insert into pages that are in memory.

- If you need more than 4096 servers, you will need to adjust the code and lose either max_servers or timeEpoch

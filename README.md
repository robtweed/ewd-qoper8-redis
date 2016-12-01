# ewd-qoper8-redis: Integrates ewd-qoper8 worker modules with the Redis-based Global Storage database emulation
 
Rob Tweed <rtweed@mgateway.com>  
24 February 2016, M/Gateway Developments Ltd [http://www.mgateway.com](http://www.mgateway.com)  

Twitter: @rtweed

Google Group for discussions, support, advice etc: [http://groups.google.co.uk/group/enterprise-web-developer-community](http://groups.google.co.uk/group/enterprise-web-developer-community)


## ewd-qoper8-redis

This module may be used to simplifiy the integration of the ewd-redis-globals module with ewd-qoper8 worker process modules. 
It additionally loads the ewd-document-store module to provide a very powerful and natural JavaScript interface to the underlying
Global Storage database provided by ewd-redis-globals.

## Installing

       npm install ewd-qoper8-redis
	   
## Using ewd-qoper8-redis

This module should be used with the start event handler of your ewd-qoper8 worker module, eg:

    this.on('start', function(isFirst) {
      var connectRedisTo = require('ewd-qoper8-redis');
      connectRedisTo(this);
    });

This will open a connection to a local Redis database listening on the default port 6379

ewd-qoper8-redis will load and initialise the ewd-document-store module, creating a DocumentStore object within your worker.

ewd-qoper8-redis takes responsibility for handling the 'stop' event, but provides you with 3 new events that you may handle:

- dbOpened: fires after the connection to Redis is opened within a worker process
- dbClosed: fires after the connection to Redis is closed within a worker process.  The worker exits immediately after this event
- DocumentStoreStarted: fires after the DocumentStore object has been instantiated.  This is a good place to handle DocumentStore events, 
 for example to maintain Document indices

The dbOpened event provides you with a single status object argument, allowing you to determine the success (or not) of
opening the connection to Redis, so you could add the following handler in your worker module, for example:

    this.on('dbOpened', function(status) {
      console.log('Redis was opened by worker ' + process.pid + ': status = ' + JSON.stringify(status));
    });


The dbClosed and DocumentStoreStarted events provide no arguments.

Note: if you use ewd-xpress, it looks after loading and configuring ewd-qoper8-redis within your worker processes.

## License

 Copyright (c) 2016 M/Gateway Developments Ltd,                           
 Reigate, Surrey UK.                                                      
 All rights reserved.                                                     
                                                                           
  http://www.mgateway.com                                                  
  Email: rtweed@mgateway.com                                               
                                                                           
                                                                           
  Licensed under the Apache License, Version 2.0 (the "License");          
  you may not use this file except in compliance with the License.         
  You may obtain a copy of the License at                                  
                                                                           
      http://www.apache.org/licenses/LICENSE-2.0                           
                                                                           
  Unless required by applicable law or agreed to in writing, software      
  distributed under the License is distributed on an "AS IS" BASIS,        
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. 
  See the License for the specific language governing permissions and      
   limitations under the License.      

# 0x03. Queuing System in JS

## About
This project focuses on building a queuing system using JavaScript and Redis. It covers various aspects of interacting with Redis, handling asynchronous operations, and building an Express app that integrates with Redis and Kue as a queue system.

- Running Redis server locally
- Caching data using `redis` and `node-redis`
- Handling async operations with Redis
- Using `Kue` as a queue system
- Building an Express app that interacts with a Redis server
- Building an Express app that interacts with Redis and queue

## Tasks :page_with_curl:

* **0. Redis installation and setting the value `School` for the key `Holberton`**
  * **File:** [dump.rdb](dump.rdb)
  * **Sample Command:**
    ```bash
    redis-cli set Holberton School
    ```
  * **Sample Output:**
    ```plaintext
    OK
    ```

* **1. Script that connects to Redis servers running locally**
  * **File:** [0-redis_client.js](0-redis_client.js)
  * **Usage:**
    ```javascript
    const redis = require('redis');
    const client = redis.createClient();
    client.on('connect', () => {
      console.log('Connected to Redis server');
    });
    ```
  * **Sample Output:**
    ```plaintext
    Connected to Redis server
    ```

* **2. Functions to interact with Redis:**
  * `SetNewSchool` - sets the value for a given key.
  * `displaySchoolValues` - retrieves the value of a given key.
  * **File:** [1-redis_op.js](1-redis_op.js)
  * **Sample Usage:**
    ```javascript
    SetNewSchool('Holberton', 'School');
    displaySchoolValues('Holberton', (value) => {
      console.log(value);
    });
    ```
  * **Sample Output:**
    ```plaintext
    School
    ```

* **3. Promisifying displaySchoolValues**
  * Extension of [1-redis_op.js](1-redis_op.js) that modifies `displaySchoolValues` to work using promises.
  * **File:** [2-redis_op_async.js](2-redis_op_async.js)
  * **Usage:**
    ```javascript
    displaySchoolValuesAsync('Holberton').then(value => {
      console.log(value);
    });
    ```
  * **Sample Output:**
    ```plaintext
    School
    ```

* **4. Setting Redis hashes and printing the response**
  * Script that sets Redis hashes using `node-redis` and prints the response using `redis.print`.
  * **File:** [4-redis_advanced_op.js](4-redis_advanced_op.js)
  * **Sample Usage:**
    ```javascript
    const client = redis.createClient();
    client.hmset('school:1', 'name', 'Holberton', redis.print);
    ```
  * **Sample Output:**
    ```plaintext
    OK
    ```

* **5. Implementing the Pub-Sub model in Redis:**
  * Script that creates a Redis client and subscribes to `holberton school channel`.
  * Script that creates a Redis client that publishes to `holberton school channel`.
  * **Files:**
    - [5-subscriber.js](5-subscriber.js)
    - [5-publisher.js](5-publisher.js)
  * **Sample Usage:**
    ```javascript
    // In subscriber.js
    client.subscribe('holberton school channel');

    // In publisher.js
    client.publish('holberton school channel', 'Hello, World!');
    ```
  * **Sample Output from Subscriber:**
    ```plaintext
    Message received: Hello, World!
    ```

* **6. Job creation using Kue**
  * **File:** [6-job_creator.js](6-job_creator.js)
  * **Sample Usage:**
    ```javascript
    const job = queue.create('email', { title: 'Welcome' }).save(err => {
      if (!err) console.log(`Job created: ${job.id}`);
    });
    ```
  * **Sample Output:**
    ```plaintext
    Job created: 1
    ```

* **7. Job processing using Kue**
  * **File:** [7-job_processor.js](7-job_processor.js)
  * **Sample Usage:**
    ```javascript
    queue.process('email', (job, done) => {
      console.log(`Processing job ${job.id}`);
      done();
    });
    ```
  * **Sample Output:**
    ```plaintext
    Processing job 1
    ```

* **8. Tracking job progress and errors with Kue - Job Creator**
  * **File:** [8-job_creator.js](8-job_creator.js)
  * **Sample Usage:**
    ```javascript
    job.on('complete', () => {
      console.log(`Job ${job.id} completed!`);
    });
    ```
  * **Sample Output:**
    ```plaintext
    Job 1 completed!
    ```

* **9. Tracking job progress and errors with Kue - Job Processor**
  * **File:** [9-job_processor.js](9-job_processor.js)
  * **Sample Usage:**
    ```javascript
    job.on('failed', (error) => {
      console.log(`Job ${job.id} failed: ${error.message}`);
    });
    ```
  * **Sample Output:**
    ```plaintext
    Job 1 failed: Connection error
    ```

* **10. Job creation function `createPushNotificationsJobs`**
  * **File:** [8-job.js](8-job.js)
  * **Sample Usage:**
    ```javascript
    const jobs = createPushNotificationsJobs(data);
    ```
  * **Sample Output:**
    ```plaintext
    Job created: 1
    ```

* **11. Unit tests for `createPushNotificationsJobs`**
  * **File:** [8-job.test.js](8-job.test.js)
  * **Sample Test:**
    ```javascript
    it('should create jobs', () => {
      expect(jobs).to.be.an('array').that.is.not.empty;
    });
    ```

* **12. Express orders API server that uses Redis for caching:**
  * **Routes:**
    - `GET /list_products/`: Returns a list of all products.
    - `GET /list_products/:itemId`: Returns information about products with the specified ID.
    - `GET /list/reserve_product/:itemId`: Reserves one unit of product with the given item ID if present and in stock.
  * **File:** [9-stock.js](9-stock.js)
  * **Sample Output for `GET /list_products/`:**
    ```json
    [
      { "id": 1, "name": "Product A" },
      { "id": 2, "name": "Product B" }
    ]
    ```

* **13. Express seat reservation API server that uses Redis for job queues:**
  * **Routes:**
    - `GET /available_seats`: Returns the number of available seats.
    - `GET /reserve_seat`: Seat reservation endpoint.
    - `GET /process`: Reservation jobs processing endpoint.
  * **File:** [100-seat.js](100-seat.js)
  * **Sample Output for `GET /available_seats`:**
    ```json
    { "available_seats": 20 }
    ```

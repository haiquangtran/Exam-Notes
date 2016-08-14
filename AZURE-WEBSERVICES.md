# Developing Windows Azure and Web Services

# Accessing Data
- **Entity Framework**
- **ADO.NET**
  - Designed to support large loads and to excel at security, scalability, flexibility, and dependability.
  - Has a bias toward a disconnect model. For example, when using individual commands such as INSERT, UPDATE, or DELETE Statements, you simply open a connection to the database, execute the command, then close the connection as quickly as possible etc. Use local version then push changes back to DB.  
  - **Connected vs Disconnected Model**
    - Connections are expensive for a RDBMS to maintain. (Consumes processing and networking resources, and DB's can only maintain finite number of connections at once). Keeping connections closed and opening them only for short perods will help mitigate many of database-focused performance problems. 
    - To improve efficiency, ADO.NET uses connection pooling. Since ADO.NET opens and closes connections at a high rate, the minor overheads in establishing connection and cleaning up a connection begin to affect performance. Connection pooling helps combat this problem.
    - Connection Pooling
      - Creates a few connections (let's say 50). 
      - Opens them up, negotiates with the RDBMS about how it will communicate with it, then enables requests to share these active connections, 50 at a time. 
      - So instead of taking up valuable resources performing the same non-trivial task 10,000 times, it does it only 50 times and then efficiently funnels all 10,000 requests through these 50 channels.
      - This means each of these 50 connections would have to handle 200 requests in order to process all 10,000 requests within that minute.
      - Manages the number of active connections for you. You can specify the max number of connections in a connection string. 
      	- With ADO.NET 4.5 accessing SQL server 2012, this limit defaults to 100 simultaneous connections and can scale anywhere between that and 0 without you as a developer having to think about it.
- **WCF Data Services**
  
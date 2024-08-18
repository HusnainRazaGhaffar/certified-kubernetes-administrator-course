# ETCD for Beginners
  - Take me to the [Video Tutorial](https://kodekloud.com/topic/etcd-for-beginners/)

  In this section, we will take a quick look at introduction to ETCD for beginners.
  - What is ETCD?
  - What is a Key-Value Store?
  - How to get started quickly with ETCD?
  - How to operate ETCD?

 ## What is a ETCD?
     - ETCD is a distributed reliable key-value store that is simple, secure & Fast.

## What is a Key-Value Store
   - Traditionally, databases have been in tabular format, you must have heard about SQL or Relational databases. They store data in rows and columns

     ![relational-dbs](../../images/relational-dbs.PNG)

   - A Key-Value Store stores information in a Key and Value format.

     ![key-value](../../images/key-value.PNG)

## Install ETCD
   - It's easy to install and get started with **`ETCD`**.
     - Download the relevant binary for your operating system from github releases page (https://github.com/etcd-io/etcd/releases)

       For Example: To download ETCD v3.5.6, run the below curl command

       ```
       $ curl -LO https://github.com/etcd-io/etcd/releases/download/v3.5.6/etcd-v3.5.6-linux-amd64.tar.gz
       ```
     - Extract it.
       ```
       $ tar xvzf etcd-v3.5.6-linux-amd64.tar.gz
       ```
     - Run the ETCD Service
       ```
       $ ./etcd
       ```
     - When you start **`ETCD`** it will by default listen on port **`2379`**
      - The default client that comes with **`ETCD`** is the [**`etcdctl`**](https://github.com/etcd-io/etcd/tree/main/etcdctl) client. You can use it to store and retrieve key-value pairs.
        ```
        Syntax: To Store a Key-Value pair
        $ ./etcdctl put key1 value1
        ```
        ```
        Syntax: To retrieve the stored data
        $ ./etcdctl get key1
        ```
        ```
        Syntax: To view more commands. Run etcdctl without any arguments
        $ ./etcdctl
        ```
        ![etcdctl](../../images/etcdctl.PNG)

## ETCD Versions
- `etcd` version 2 and version 3 is quite different, there APIs are quite different
- To check the version of v2 of `etcd` use following command. In the output we can see API version is 2 thus `etcd` is of version 2
    
    ```yaml
    ./etcdctl --version
    etcdctl version: 3.3.11
    API version: 2
    ```
    
- To check the version of v3 of `etcd` use following command. In the output we can see API version is 3 thus `etcd` is of version 3
    
    ```bash
    ./etcdctl version
    etcdctl version: 3.3.11
    API version: 3.3
    ```
    
- To use `etcdctl` from v2 to v3
    
    ```bash
    ETCDCTL_API=3 ./etcdctl version
    etcdctl version: 3.3.11
    API version: 3.3
    
    # OR
    
    export ETCDCTL_API=3
    ./etcdctl version
    etcdctl version: 3.3.11
    API version: 3.3
    ```
    
- `etcdctl` client comes with `etcd` that can be used to store and retrieve data from `etcd` datastore
- To store key1 having value1 in `etcd` v2
    
    ```bash
    ./etcdctl set key1 value1
    ```
    
- To store key1 having value1 in `etcd` v3
    
    ```bash
    ./etcdctl put key1 value1
    ```
    
- To get the value of key1 from `etcd` v2 or v3
    
    ```bash
    ./etcdctl get key1
    ```
    
- `etcd` in Kubernetes stores the information about
    - Nodes
    - PODs
    - Configs
    - Secrets
    - Accounts
    - Roles
    - Bindings
    - Others


K8s Reference Docs:
       - https://kubernetes.io/docs/concepts/overview/components/
       - https://etcd.io/docs/
       - https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/


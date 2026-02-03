Axelor Open Suite
================================

Axelor Open Suite reduces the complexity and improve responsiveness of business processes. Thanks to its modularity, you can start with few features and  then activate other modules when needed.

Axelor Open Suite includes the following modules :

* Customer Relationship Management
* Sales management
* Financial and cost management
* Human Resource Management
* Project Management
* Inventory and Supply Chain Management
* Production Management
* Multi-company, multi-currency and multi-lingual

Axelor Open Suite is built on top of [Axelor Open Platform](https://github.com/axelor/axelor-open-platform)


Installation
================================

To compile and run from source, you will need to clone Axelor Open Suite modules, which is a
[git submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules) repository, using following commands:

```bash
$ git clone git@github.com:axelor/open-suite-webapp.git
$ cd open-suite-webapp
$ git checkout master
$ git submodule init
$ git submodule update
$ git submodule foreach git checkout master
$ git submodule foreach git pull origin master
```

You can find more detailed [installation instructions](https://docs.axelor.com/abs/5.0/install/index.html) on our documentation.

Krabu repositories
================================
Main app: https://github.com/krabugrupp
Sub-modules: https://github.com/krabugrupp/axelor-modules


Deploy
================================

1) Build docker image `docker build <image_name>[:TAG] .`
2) Push image into DockerHub `docker push <image_name>[:TAG]`
3) On the server create docker-compose.yml file
    example:

    ```dockerfile
    
    volumes:
    axelor-storage:
    name: axelor-storage
    axelor-database:
    name: axelor-database
    
    services:
    krabutech-axelor:
    container_name: axelor
    image: <image_name>[:TAG]
    restart: always
    ports:
    - 8080:8080
      volumes:
      - axelor-storage:/usr/local/tomcat/temp/axelor
        depends_on:
      - postgres
        environment:
        CATALINA_OPTS: -DDB_URL=jdbc:postgresql://localhost:5432/axelor -DDB_USER=axelor -DDB_PASSWORD=axelor -Deinvoice.authkey=262518:labpgukqgbnwizbiokyvrvlugqjsupgrfrmyiwgqecqelkcvkv
    
    postgres:
    container_name: postgres
    image: postgres:12
    restart: always
    ports:
    - 5432:5432
      volumes:
      - axelor-database:/var/lib/postgresql/data
        environment:
        POSTGRES_USER: axelor
        POSTGRES_PASSWORD: axelor
     ```
4) Run container with `docker-compose using docker-compose up -d`
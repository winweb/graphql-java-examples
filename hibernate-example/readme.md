# graphql-java-hibernate-example

An example of a simple graphql-java implementation backed by a Hibernate repository (via Spring Data JPA) and
integrated with Spring Boot.

It shows how data fetchers can use Hibernate repositories to query the database.

In memory H2 is being used as the database, SQL scripts are execute during startup to add some sample data to
the database. You can find those scripts in [data.sql](src/resources/data.sql).

# Running the example
To build the code type

    ./gradlew build
    
To run the code type    
    
    ./gradlew bootRun
    
Point your browser at 

    http://localhost:8080/    


Some example graphql queries might be

     {
       human(id: "1000") {
         name
         friends {
           name
           friends {
             id
             name
           }
         }
       }
     }


or maybe

    {
      luke: human(id: "1000") {
        ...HumanFragment
      }
      leia: human(id: "1003") {
        ...HumanFragment
      }
    }
    
    fragment HumanFragment on Human {
      name
      homePlanet
      friends {
        name
        __typename
      }
    }

#Add peoples data (test many nest data).

inquiry data by

     {
       human(id: "1005") {
         id
         name
         friends {
           id
           name
           friends {
             id
             name
             friends {
               id
               name
               friends {
                 id
                 name
                 friends {
                   id
                   name
                 }
               }
             }
           }
         }
       }
     }

result

    {
      "data": {
        "human": {
          "id": "1005",
          "name": "People 1",
          "friends": [
            {
              "id": "1006",
              "name": "People 2",
              "friends": [
                {
                  "id": "1007",
                  "name": "People 3",
                  "friends": [
                    {
                      "id": "1008",
                      "name": "People 4",
                      "friends": [
                        {
                          "id": "1009",
                          "name": "People 5",
                          "friends": [
                            {
                              "id": "2001",
                              "name": "R2-D2"
                            }
                          ]
                        }
                      ]
                    }
                  ]
                }
              ]
            }
          ]
        }
      }
    }


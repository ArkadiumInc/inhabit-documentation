Structure
=========

Example of package manifest. Use [ihpm](https://github.com/ArkadiumInc/node-ihpm) to package factive.

````json
{
   "main":"build/modules/myPackage.js",
   "resources":"build/resources",
   "preview":"myPromoImage.png",
   "settings":{  
      "title":"My Contextual factive",
      "description":"This factive provides information about weather at your location",
      "badges":["Tag1","Tag2","Tag3"],
      "categories":[  
         "Entertainment"
      ],
      "jsonSchema":{  
         "schema":{  
            "type":"object",
            "properties":{  
               "dataSourceUrl":{  
                  "type":"string",
                  "title":"Data Source",
                  "default":"my.big.data.source"
               },
               "openInNewWindow":{  
                  "type":"boolean",
                  "title":"Open in new Window",
                  "default":true
               },
               "maxItems":{  
                  "type":"number",
                  "title":"Max amount of items",
                  "default":3,
                  "enum":[1, 2, 3, 4, 5, 6, 7, 8]
               },
               "direction":{  
                  "type":"string",
                  "title":"Direction",
                  "default":"horizontal",
                  "enum":[  
                     "vertical",
                     "horizontal"
                  ]
               }
            }
         },
         "form":[  
            {  
               "key":"dataSourceUrl",
               "type":"string"               
            },
            {  
               "key":"openInNewWindow",
               "type":"checkbox"
            },
            {  
               "key":"maxItems",
               "type":"number"
            },
            {  
               "key":"direction",
               "type":"select"
            }
         ],
         "options":{  

         }
      },
      "defaultConfiguration":{  
         "dataSourceUrl":"//games.sfgate.com/arenaapi/promogames",
         "openInNewWindow":true,
         "maxItems":3,
         "direction":"horizontal"
      },
      "exampleContextualUrl":""
   }
}
````

Manifest contains two main parts, first describes resources necessary for you application to run, second describes how factive will be presented inside the factive store and what setting will be available for it setup.

#### Package description

-   "main":"build/modules/myPackage.js" - location of your factive JavaScript bundle. This bundle should contain class that extends [inhabit-module-base](https://github.com/ArkadiumInc/node-inhabit-module-base)

-   "resources":"build/resources" - location of your resource folder. This folder should have resources like images/jsons etc that you application required.

#### Factives store metadata

-   "preview":"myPromoImage.png" - location of preview image that will be used as thumbnail.

-   "settings"."title":"My Contextual factive" - Title of application that will be used in factives store.

-   "settings"."description":"This factive provides information about weather at your location" - description of factive functionality for factives store

-   "settings"."badges":["Tag1","Tag2","Tag3"] - Factive tags

-   "settings"."categories":["Entertainment"] - Category of factive

-   "settings"."jsonSchema" - json schema that describes form for factive settings http://schemaform.io/examples/bootstrap-example.html

-   "settings"."defaultConfiguration" - default factive configuration

-   "settings"."exampleContextualUrl" - url that can be used to generate contextual factive

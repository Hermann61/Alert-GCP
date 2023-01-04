# Alert-GCP 

{
  "name": "Application Alert",
  "nodes": [
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "datamart",
        "tableId": "WhatsappAlert_temp",
        "returnAll": true,
        "options": {}
      },
      "id": "20e7181d-41e3-4114-9b57-408a45a42724",
      "name": "Google BigQuery",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        -200,
        -20
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "method": "POST",
        "url": "https://graph.facebook.com/v15.0/113214538300708",
        "sendQuery": true,
        "queryParameters": {
          "parameters": [
            {
              "name": "messaging_product",
              "value": "={{ $json[\"body\"][\"messaging_product\"] }}"
            },
            {
              "name": "to",
              "value": "={{ $json[\"body\"][\"to_Patrick\"] }}"
            },
            {
              "name": "type",
              "value": "={{ $json[\"body\"][\"type\"] }}"
            },
            {
              "name": "template",
              "value": "={ \"name\": \"{{ $json[\"body\"][\"template_name\"] }}\", \"language\": { \"code\": \"{{ $json[\"body\"][\"template_language_code\"] }}\" } }"
            },
            {
              "name": "access_token",
              "value": "={{ $json[\"headers_Authorization\"] }}"
            }
          ]
        },
        "options": {
          "response": {
            "response": {
              "fullResponse": true
            }
          }
        }
      },
      "id": "ae2dac19-e6b4-415c-8a4c-aa0f5797e993",
      "name": "HTTP Request",
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 3,
      "position": [
        200,
        -240
      ],
      "alwaysOutputData": true
    },
    {
      "parameters": {
        "keepOnlySet": true,
        "values": {
          "string": [
            {
              "name": "body.messaging_product",
              "value": "whatsapp"
            },
            {
              "name": "body.to_Patrick",
              "value": "5511937536593"
            },
            {
              "name": "body.type",
              "value": "template"
            },
            {
              "name": "body.template_name",
              "value": "teste_video"
            },
            {
              "name": "body.template_language_code",
              "value": "pt_BR"
            },
            {
              "name": "headers_Authorization",
              "value": "EAAgjzpidknEBAORVuIBS5nIeoY5jZCMbbvy5nXSeT2jrKOjmHfH1jyubqAZAxVq7Rx2rG99W0I05Vt42L0oxxdO3afyoZAZAkxphGZBItwdDKg4cTD0VVP2MYrOgJ3ikEXZBZAkdeH8ZAZAQSqKtNz1LoY3gZAPUHA7rigrYU1MPWMYZBnyqs2Iw7fK"
            },
            {
              "name": "body.to_Thiago",
              "value": "5511985696086"
            },
            {
              "name": "body.to_Ramiro",
              "value": "5511997344567"
            },
            {
              "name": "body.to_Jonathas",
              "value": "5511996562550"
            }
          ]
        },
        "options": {
          "dotNotation": true
        }
      },
      "id": "4c46e95c-e753-4a55-b76d-62ea743ffabb",
      "name": "Set",
      "type": "n8n-nodes-base.set",
      "typeVersion": 1,
      "position": [
        40,
        -240
      ]
    },
    {
      "parameters": {
        "value1": "={{ $json[\"statusCode\"] }}",
        "rules": {
          "rules": [
            {
              "operation": "equal",
              "value2": 200
            },
            {
              "operation": "notEqual",
              "value2": 200,
              "output": 1
            }
          ]
        }
      },
      "id": "1ef9986a-f860-42b4-9bc2-0b9b3e566f72",
      "name": "Switch",
      "type": "n8n-nodes-base.switch",
      "typeVersion": 1,
      "position": [
        -660,
        -180
      ]
    },
    {
      "parameters": {
        "operation": "limit"
      },
      "id": "fd968faf-d5b0-4496-a041-07ef70ba0fd4",
      "name": "Item Lists - ok",
      "type": "n8n-nodes-base.itemLists",
      "typeVersion": 1,
      "position": [
        620,
        -340
      ]
    },
    {
      "parameters": {
        "operation": "limit"
      },
      "id": "0261446b-9d12-42be-a524-98eb6c879d74",
      "name": "Item Lists - nok",
      "type": "n8n-nodes-base.itemLists",
      "typeVersion": 1,
      "position": [
        620,
        -140
      ]
    },
    {
      "parameters": {
        "rule": {
          "interval": [
            {
              "field": "cronExpression",
              "expression": "5 * * *"
            }
          ]
        }
      },
      "id": "74204c93-d049-438a-a271-bec3eeef732f",
      "name": "Schedule Trigger",
      "type": "n8n-nodes-base.scheduleTrigger",
      "typeVersion": 1,
      "position": [
        -1360,
        200
      ]
    },
    {
      "parameters": {
        "phoneNumberId": "113214538300708",
        "recipientPhoneNumber": "={{ $node[\"Set\"].json[\"body\"][\"to_Thiago\"] }}",
        "template": "bases|pt_BR"
      },
      "id": "47418224-bb69-400f-b754-cb2d62a0ea48",
      "name": "WhatsApp - ok - Thiago",
      "type": "n8n-nodes-base.whatsApp",
      "typeVersion": 1,
      "position": [
        1220,
        -340
      ],
      "credentials": {
        "whatsAppApi": {
          "id": "7",
          "name": "WhatsApp account"
        }
      }
    },
    {
      "parameters": {
        "phoneNumberId": "113214538300708",
        "recipientPhoneNumber": "={{ $node[\"Set\"].json[\"body\"][\"to_Patrick\"] }}",
        "template": "bases|pt_BR"
      },
      "id": "be8ad6cc-f23e-4768-adea-d5d8e3e02f0c",
      "name": "WhatsApp - ok - Patrick",
      "type": "n8n-nodes-base.whatsApp",
      "typeVersion": 1,
      "position": [
        780,
        -340
      ],
      "credentials": {
        "whatsAppApi": {
          "id": "7",
          "name": "WhatsApp account"
        }
      },
      "continueOnFail": true
    },
    {
      "parameters": {
        "phoneNumberId": "113214538300708",
        "recipientPhoneNumber": "={{ $node[\"Set\"].json[\"body\"][\"to_Ramiro\"] }}",
        "template": "bases|pt_BR"
      },
      "id": "ea555660-70cd-4a3f-8d74-2abc7c1a1575",
      "name": "WhatsApp - ok - Ramiro",
      "type": "n8n-nodes-base.whatsApp",
      "typeVersion": 1,
      "position": [
        940,
        -340
      ],
      "credentials": {
        "whatsAppApi": {
          "id": "7",
          "name": "WhatsApp account"
        }
      },
      "continueOnFail": true
    },
    {
      "parameters": {
        "phoneNumberId": "113214538300708",
        "recipientPhoneNumber": "={{ $node[\"Set\"].json[\"body\"][\"to_Jonathas\"] }}",
        "template": "bases|pt_BR"
      },
      "id": "36687023-cee3-4e27-8b55-b1b03fc0b1eb",
      "name": "WhatsApp - ok - Jonathas",
      "type": "n8n-nodes-base.whatsApp",
      "typeVersion": 1,
      "position": [
        1080,
        -340
      ],
      "credentials": {
        "whatsAppApi": {
          "id": "7",
          "name": "WhatsApp account"
        }
      },
      "continueOnFail": true
    },
    {
      "parameters": {
        "phoneNumberId": "113214538300708",
        "recipientPhoneNumber": "={{ $node[\"Set\"].json[\"body\"][\"to_Patrick\"] }}",
        "template": "bases_|pt_BR"
      },
      "id": "3a19af78-76fa-487f-98d8-7c98484fd055",
      "name": "WhatsApp - nok - Patrick",
      "type": "n8n-nodes-base.whatsApp",
      "typeVersion": 1,
      "position": [
        780,
        -140
      ],
      "credentials": {
        "whatsAppApi": {
          "id": "7",
          "name": "WhatsApp account"
        }
      },
      "continueOnFail": true
    },
    {
      "parameters": {
        "phoneNumberId": "113214538300708",
        "recipientPhoneNumber": "={{ $node[\"Set\"].json[\"body\"][\"to_Ramiro\"] }}",
        "template": "bases_|pt_BR"
      },
      "id": "b14c3a01-5183-4263-86d4-e51a92e994de",
      "name": "WhatsApp - nok - Ramiro",
      "type": "n8n-nodes-base.whatsApp",
      "typeVersion": 1,
      "position": [
        940,
        -140
      ],
      "credentials": {
        "whatsAppApi": {
          "id": "7",
          "name": "WhatsApp account"
        }
      },
      "continueOnFail": true
    },
    {
      "parameters": {
        "phoneNumberId": "113214538300708",
        "recipientPhoneNumber": "={{ $node[\"Set\"].json[\"body\"][\"to_Jonathas\"] }}",
        "template": "bases_|pt_BR"
      },
      "id": "e75098aa-d745-4c51-9837-eb89b43d1ad1",
      "name": "WhatsApp - nok - Jonathas",
      "type": "n8n-nodes-base.whatsApp",
      "typeVersion": 1,
      "position": [
        1080,
        -140
      ],
      "credentials": {
        "whatsAppApi": {
          "id": "7",
          "name": "WhatsApp account"
        }
      },
      "continueOnFail": true
    },
    {
      "parameters": {
        "phoneNumberId": "113214538300708",
        "recipientPhoneNumber": "={{ $node[\"Set\"].json[\"body\"][\"to_Thiago\"] }}",
        "template": "bases_|pt_BR"
      },
      "id": "a82b99d7-32f5-4ebf-bf42-2cf4f0279732",
      "name": "WhatsApp - nok - Thiago",
      "type": "n8n-nodes-base.whatsApp",
      "typeVersion": 1,
      "position": [
        1220,
        -140
      ],
      "credentials": {
        "whatsAppApi": {
          "id": "7",
          "name": "WhatsApp account"
        }
      }
    },
    {
      "parameters": {
        "conditions": {
          "boolean": [
            {
              "value1": "={{ $node[\"Verifica data de inclusão\"].json[\"isAllUpdated\"] }}",
              "value2": true
            }
          ]
        }
      },
      "id": "33058c78-f65c-4e05-b072-4a792603d41e",
      "name": "IF",
      "type": "n8n-nodes-base.if",
      "typeVersion": 1,
      "position": [
        380,
        -240
      ]
    },
    {
      "parameters": {},
      "id": "98d3d4af-850b-4f4e-af91-6cd8815c0b0c",
      "name": "NoOp - OK",
      "type": "n8n-nodes-base.noOp",
      "typeVersion": 1,
      "position": [
        -420,
        -340
      ]
    },
    {
      "parameters": {},
      "id": "79618620-2d29-4da7-94d8-83aa2e640b37",
      "name": "NoOp - NOK",
      "type": "n8n-nodes-base.noOp",
      "typeVersion": 1,
      "position": [
        -420,
        -180
      ]
    },
    {
      "parameters": {},
      "id": "de77e41a-667f-4aa1-a393-8b5cff6606cd",
      "name": "NoOp - ELSE",
      "type": "n8n-nodes-base.noOp",
      "typeVersion": 1,
      "position": [
        -420,
        -20
      ]
    },
    {
      "parameters": {
        "method": "POST",
        "url": "=https://bigquery.googleapis.com/bigquery/v2/projects/cdp-jhsf-prod/queries/select * from datamart.WhatsappAlert",
        "authentication": "genericCredentialType",
        "genericAuthType": "oAuth2Api",
        "options": {}
      },
      "id": "1f4b7b84-85ce-403e-8088-9e6c8e0941d6",
      "name": "HTTP Request1",
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 3,
      "position": [
        -200,
        -340
      ],
      "credentials": {
        "googleBigQueryOAuth2Api": {
          "id": "13",
          "name": "Google BigQuery account 2"
        },
        "httpBasicAuth": {
          "id": "8",
          "name": "Unnamed credential"
        },
        "oAuth2Api": {
          "id": "14",
          "name": "BigQuery other way"
        }
      }
    },
    {
      "parameters": {
        "method": "POST",
        "url": "https://bigquery.googleapis.com/bigquery/v2/projects/cdp-jhsf-prod/queries",
        "authentication": "genericCredentialType",
        "genericAuthType": "oAuth2Api",
        "sendBody": true,
        "bodyParameters": {
          "parameters": [
            {
              "name": "query",
              "value": "#standardSQL \\n select * from datamart.WhatsappAlert"
            }
          ]
        },
        "options": {}
      },
      "id": "2ccfaec9-b32a-4cbb-aad8-979c203b7e88",
      "name": "HTTP Request2",
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 3,
      "position": [
        -200,
        -180
      ],
      "credentials": {
        "googleBigQueryOAuth2Api": {
          "id": "13",
          "name": "Google BigQuery account 2"
        },
        "oAuth2Api": {
          "id": "14",
          "name": "BigQuery other way"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=transactions_membershipid",
        "limit": 1,
        "options": {
          "selectedFields": "data_insercao"
        }
      },
      "id": "8f99b9c7-aa93-4b5e-bb70-57c7d5186b8d",
      "name": "silver.transactions_membershipid",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        400,
        200
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=customer_wallets_membershipid",
        "limit": 1,
        "options": {
          "selectedFields": "data_insercao"
        }
      },
      "id": "3d0c5d01-dc74-4686-9b5b-bd604d7a7c3e",
      "name": "silver.customer_wallets_membershipid",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        200,
        200
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=wallet_executions_membershipid",
        "limit": 1,
        "options": {
          "selectedFields": "data_insercao"
        }
      },
      "id": "cf153f22-e7d6-44b9-8be0-5bb77e477b1a",
      "name": "silver.wallet_executions_membershipid",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        0,
        200
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=customer_addresses_membershipid",
        "limit": 1,
        "options": {
          "selectedFields": "data_insercao"
        }
      },
      "id": "1b6378f3-7eb0-4092-8e4c-465e7a6bdc09",
      "name": "silver.customer_addresses_membershipid",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        -200,
        200
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=customers_membershipid",
        "limit": 1,
        "options": {
          "selectedFields": "data_insercao"
        }
      },
      "id": "ab845cd7-1556-4843-a61a-9eb17a160ee4",
      "name": "silver.customers_membershipid",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        -400,
        200
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=customers_processos_manuais_membershipid",
        "limit": 1,
        "options": {
          "selectedFields": "data_insercao"
        }
      },
      "id": "a47cf08d-befc-4669-bb20-fd07e7d8a9a2",
      "name": "silver.customers_processos_manuais_membershipid",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        -980,
        -300
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=macro_groups_membershipid",
        "limit": 1,
        "options": {
          "selectedFields": "data_insercao"
        }
      },
      "id": "abc3a21e-6421-4c3a-91ba-89209211fbc2",
      "name": "silver.macro_groups_membershipid",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        -600,
        200
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=stores_sub_store_macro_group_membershipid",
        "limit": 1,
        "options": {
          "selectedFields": "data_insercao"
        }
      },
      "id": "5e8e2e7e-353a-4370-84fb-641de4e356a4",
      "name": "silver.stores_sub_store_macro_group_membershipid",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        -800,
        200
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=stores_sub_store_membershipid",
        "limit": 1,
        "options": {
          "selectedFields": "data_insercao"
        }
      },
      "id": "a09f22ca-e8c5-4edd-a89e-e2ad847512ae",
      "name": "silver.stores_sub_store_membershipid",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        -1000,
        200
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "value": "={{const today = new Date();today.setDate(today.getDate()-1);today.toISOString();}}\n\n\n\n",
        "dataPropertyName": "today",
        "toFormat": "YYYY-MM-DD",
        "options": {}
      },
      "id": "4f7d4e0b-107f-4ae5-bc23-63137a3abbd7",
      "name": "Date & Time",
      "type": "n8n-nodes-base.dateTime",
      "typeVersion": 1,
      "position": [
        1020,
        540
      ]
    },
    {
      "parameters": {
        "jsCode": "// console.log('datamart.nps_food', Number($node['datamart.nps_food'].json.date_created), new Date(Number($node['datamart.nps_food'].json.date_created)))\n\nconst today1 = new Date()\ntoday1.setDate(today1.getDate()-1)\nconst today= today1.toISOString().split('T')[0] \n\nreturn [\n  {\n    bu: 'JHSFID',\n    tabela: 'transactions_membership',\n//    data_insercao: $input.all()[0].json.data_insercao.split(' ')[0],\n    data_insercao: $node['silver.transactions_membershipid'].json.data_insercao.split(' ')[0],\n//    total_lines: $input.all()[0].json.length\n    today\n  },\n    {\n    bu: 'JHSFID',\n    tabela: 'customer_wallets',\n    data_insercao: $node['silver.customer_wallets_membershipid'].json.data_insercao.split(' ')[0],\n//    total_lines: $node['silver.customer_wallets_membershipid'].json.length\ntoday\n    },\n  {\n    bu: 'JHSFID',\n    tabela: 'wallet_execution',\n    data_insercao: $node['silver.wallet_executions_membershipid'].json.data_insercao.split(' ')[0],\n//    total_lines: $node['silver.wallet_executions_membershipid'].json.length\ntoday    \n  },\n  {\n    bu: 'JHSFID',\n    tabela: 'customer_address',\n    data_insercao: $node['silver.customer_addresses_membershipid'].json.data_insercao.split(' ')[0],\n//    total_lines: $node['silver.customer_addresses_membershipid'].json.length\n    today\n  },\n  {\n    bu: 'JHSFID',\n    tabela: 'customer_membershipid',\n    data_insercao: $node['silver.customers_membershipid'].json.data_insercao.split(' ')[0],\n//    total_lines: $node['silver.customers_membershipid'].json.length\n    today\n  },\n//  {\n//    bu: 'JHSFID',\n//    tabela: 'customer_processos_manuais_membershipid',\n//    data_insercao: //$node['silver.customers_processos_manuais_membershipid'].json.data_insercao.split(' ')[0],\n//    total_lines: $node['silver.customers_processos_manuais_membershipid'].json.length\n//  },\n  {\n    bu: 'JHSFID',\n    tabela: 'macro_groups_membershipid',\n    data_insercao: $node['silver.macro_groups_membershipid'].json.data_insercao.split(' ')[0],\n//    total_lines: $node['silver.macro_groups_membershipid'].json.length\n    today\n  },\n  {\n    bu: 'JHSFID',\n    tabela: 'store_stores_sub_store-macro_group_membershipid',\n    data_insercao: $node['silver.stores_sub_store_macro_group_membershipid'].json.data_insercao.split(' ')[0],\n//    total_lines: $node['silver.stores_sub_store_macro_group_membershipid'].json.length\n    today\n  },\n  {\n    bu: 'JHSFID',\n    tabela: 'store_sub_store_membershipid',\n    data_insercao: $node['silver.stores_sub_store_membershipid'].json.data_insercao.split(' ')[0],\n//    total_lines: $node['silver.stores_sub_store_membershipid'].json.length\n    today\n  },\n  {\n    bu: 'CJFOOD',\n    tabela: 'categories_per_products_cjfood',\n    data_insercao: $node['silver.categories_per_products_cjfood'].json.lastUpdatedAt.split('T')[0],\n//    total_lines: $node['silver.categories_per_products_cjfood'].json.length\n    today\n  },\n  {\n    bu: 'CJFOOD',\n    tabela: 'customer_addresses_cjfood',\n    data_insercao: $node['silver.customer_addresses_cjfood'].json.lastUpdatedAt.split('T')[0].split(' ')[0],\n//    total_lines: $node['silver.customer_addresses_cjfood'].json.length\n    today\n  },\n  {\n    bu: 'CJFOOD',\n    tabela: 'customer_documents_cjfood',\n    data_insercao: $node['silver.customer_documents_cjfood'].json.lastUpdatedAt.split('T')[0].split(' ')[0],\n//    total_lines: $node['silver.customer_documents_cjfood'].json.length\n    today\n  },\n  {\n    bu: 'CJFOOD',\n    tabela: 'customer_payments_cjfood',\n    data_insercao: $node['silver.customer_payments_cjfood'].json.lastUpdatedAt.split('T')[0].split(' ')[0],\n//    total_lines: $node['silver.customer_payments_cjfood'].json.length\n    today\n  },\n  {\n    bu: 'CJFOOD',\n    tabela: 'customer_phones_cjfood',\n    data_insercao: $node['silver.customer_phones_cjfood'].json.lastUpdatedAt.split('T')[0].split(' ')[0],\n//    total_lines: $node['silver.customer_phones_cjfood'].json.length\n    today\n  },\n  {\n    bu: 'CJFOOD',\n    tabela: 'customers_cjfood',\n    data_insercao: $node['silver.customers_cjfood'].json.lastUpdatedAt.split('T')[0].split(' ')[0],\n//    total_lines: $node['silver.customers_cjfood'].json.length\n    today\n  },\n  {\n    bu: 'CJFOOD',\n    tabela: 'one_signal_data_cjfood',\n    data_insercao: $node['silver.one_signal_data_cjfood'].json.lastUpdatedAt.split('T')[0].split(' ')[0],\n//    total_lines: $node['silver.one_signal_data_cjfood'].json.length\n    today\n  },\n  {\n    bu: 'CJFOOD',\n    tabela: 'order_products_cjfood',\n    data_insercao: $node['silver.order_products_cjfood'].json.lastUpdatedAt.split('T')[0].split(' ')[0],\n//    total_lines: $node['silver.order_products_cjfood'].json.length\n    today\n  },\n  {\n    bu: 'CJFOOD',\n    tabela: 'orders_cjfood',\n    data_insercao: $node['silver.orders_cjfood'].json.lastUpdatedAt.split('T')[0].split(' ')[0],\n//    total_lines: $node['silver.orders_cjfood'].json.length\n    today\n  },\n//   {\n//     bu: 'CJFOOD',\n//     tabela: 'product_categories_cjfood',\n//     data_insercao: $node['silver.product_categories_cjfood'].json.lastUpdatedAt.split('T')[0].split(' ')[0],\n// //    total_lines: $node['silver.product_categories_cjfood'].json.length\n//     today\n//   },\n  {\n    bu: 'CJFOOD',\n    tabela: 'product_prices_cjfood',\n    data_insercao: $node['silver.product_prices_cjfood'].json.lastUpdatedAt.split('T')[0].split(' ')[0],\n//    total_lines: $node['silver.product_prices_cjfood'].json.length\n    today\n  },\n  {\n    bu: 'CJFOOD',\n    tabela: 'products_cjfood',\n    data_insercao: $node['silver.products_cjfood'].json.lastUpdatedAt.split('T')[0].split(' ')[0],\n//    total_lines: $node['silver.products_cjfood'].json.length\n    today\n  },\n  {\n    bu: 'CJFOOD',\n    tabela: 'sellers_cjfood',\n    data_insercao: $node['silver.sellers_cjfood'].json.lastUpdatedAt.split('T')[0].split(' ')[0],\n//    total_lines: $node['silver.sellers_cjfood'].json.length\n    today\n  },\n]"
      },
      "id": "80e71649-f476-40ed-92e6-e2e662d8ce4c",
      "name": "Gera JSON das tabelas",
      "type": "n8n-nodes-base.code",
      "typeVersion": 1,
      "position": [
        600,
        200
      ]
    },
    {
      "parameters": {
        "jsCode": "let isAllUpdated = true;\nlet message = '';\n\n$input.all().forEach(items => {\n  if (String(items.json.data_insercao) < String(items.json.today) && isAllUpdated === true) {\n    isAllUpdated = false\n  }\n\n  if (String(items.json.data_insercao) < String(items.json.today)) {\n    message += `${items.json.bu} - ${items.json.tabela} - Última atualização: ${items.json.data_insercao} - Status: NOK\\n\\n`\n  }\n})\n\nreturn [{ isAllUpdated, message }];"
      },
      "id": "57afa15f-7ff2-4e83-b712-eb1bd06a99d8",
      "name": "Verifica data de inclusão",
      "type": "n8n-nodes-base.code",
      "typeVersion": 1,
      "position": [
        800,
        200
      ]
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=product_categories_cjfood",
        "limit": 1,
        "options": {
          "selectedFields": "data_insercao"
        }
      },
      "id": "bded1e3a-5f3a-4401-99c6-b2f6a40ddf00",
      "name": "silver.stores_sub_store_macro_group_membershipid1",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        -980,
        -60
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "conditions": {
          "boolean": [
            {
              "value1": "={{ $node[\"HTTP Request\"].json[\"statusCode\"] === 200}}",
              "value2": true
            },
            {
              "value1": "={{ $node[\"Verifica data de inclusão\"].json[\"isAllUpdated\"] }}",
              "value2": true
            }
          ]
        }
      },
      "id": "64ecfab5-10e4-418a-9a43-73aa1ff520a7",
      "name": "IF_old",
      "type": "n8n-nodes-base.if",
      "typeVersion": 1,
      "position": [
        380,
        -100
      ]
    },
    {
      "parameters": {
        "fromEmail": "ti@jhsf.io",
        "toEmail": "patricksilva@jhsf.com.br",
        "subject": "teste",
        "text": "teste",
        "options": {}
      },
      "id": "a4192665-7f13-4c4f-9362-23f7f77dd7f8",
      "name": "Send Email",
      "type": "n8n-nodes-base.emailSend",
      "typeVersion": 1,
      "position": [
        820,
        520
      ],
      "credentials": {
        "smtp": {
          "id": "19",
          "name": "SMTP account"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=customer_addresses_cjfood",
        "limit": 1,
        "options": {
          "selectedFields": "lastUpdatedAt"
        }
      },
      "id": "c9c8f9a6-1ea3-4131-9396-1a61670b075b",
      "name": "silver.customer_addresses_cjfood",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        -600,
        400
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=customer_documents_cjfood",
        "limit": 1,
        "options": {
          "selectedFields": "lastUpdatedAt"
        }
      },
      "id": "b72dc97b-3f91-4b55-9f66-63ea356802b6",
      "name": "silver.customer_documents_cjfood",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        -400,
        400
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=categories_per_products_cjfood",
        "limit": 1,
        "options": {
          "selectedFields": "lastUpdatedAt"
        }
      },
      "id": "b95abe6a-e449-482c-b4e2-bcdbf359eabc",
      "name": "silver.categories_per_products_cjfood",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        -800,
        400
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=customer_payments_cjfood",
        "limit": 1,
        "options": {
          "selectedFields": "lastUpdatedAt"
        }
      },
      "id": "b2aad280-8c5f-434e-a314-5cde3152ecb4",
      "name": "silver.customer_payments_cjfood",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        -200,
        400
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=customer_phones_cjfood",
        "limit": 1,
        "options": {
          "selectedFields": "lastUpdatedAt"
        }
      },
      "id": "bdeda3a4-4880-48fc-98c3-f7efefbfbafe",
      "name": "silver.customer_phones_cjfood",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        0,
        400
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=customers_cjfood",
        "limit": 1,
        "options": {
          "selectedFields": "lastUpdatedAt"
        }
      },
      "id": "7035bae8-cae4-4e42-b473-8f37598f4d36",
      "name": "silver.customers_cjfood",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        200,
        400
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=one_signal_data_cjfood",
        "limit": 1,
        "options": {
          "selectedFields": "lastUpdatedAt"
        }
      },
      "id": "f15b2091-3782-4772-b2aa-88921ecccf05",
      "name": "silver.one_signal_data_cjfood",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        400,
        400
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=order_products_cjfood",
        "limit": 1,
        "options": {
          "selectedFields": "lastUpdatedAt"
        }
      },
      "id": "5b187147-9fb0-4773-8754-4868850ad9e4",
      "name": "silver.order_products_cjfood",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        -600,
        600
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=orders_cjfood",
        "limit": 1,
        "options": {
          "selectedFields": "lastUpdatedAt"
        }
      },
      "id": "ebf0c654-531a-4d71-85ab-3bf0908cff9d",
      "name": "silver.orders_cjfood",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        -400,
        600
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=product_categories_cjfood",
        "limit": 1,
        "options": {
          "selectedFields": "lastUpdatedAt"
        }
      },
      "id": "f7d8e0fb-83e5-4d38-9c6e-9c50427b67ce",
      "name": "silver.product_categories_cjfood",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        -200,
        600
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=product_prices_cjfood",
        "limit": 1,
        "options": {
          "selectedFields": "lastUpdatedAt"
        }
      },
      "id": "5b4f132d-629c-4cc6-96ba-7fb50db08fb2",
      "name": "silver.product_prices_cjfood",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        0,
        600
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=products_cjfood",
        "limit": 1,
        "options": {
          "selectedFields": "lastUpdatedAt"
        }
      },
      "id": "2b26040e-fd43-4fd7-985b-ca23c7a106ce",
      "name": "silver.products_cjfood",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        200,
        600
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "silver",
        "tableId": "=sellers_cjfood",
        "limit": 1,
        "options": {
          "selectedFields": "lastUpdatedAt"
        }
      },
      "id": "e2d61164-7716-45e9-ad7d-866a7a4ad6b4",
      "name": "silver.sellers_cjfood",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        400,
        600
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {
        "chatId": "-1001859609993",
        "text": "={{ $node[\"Verifica data de inclusão\"].json[\"message\"] }}",
        "additionalFields": {}
      },
      "id": "64eec6d2-48a2-430d-addf-502c0758ad67",
      "name": "Telegram",
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1,
      "position": [
        1220,
        100
      ],
      "credentials": {
        "telegramApi": {
          "id": "18",
          "name": "Telegram account 3"
        }
      }
    },
    {
      "parameters": {
        "authentication": "serviceAccount",
        "operation": "getAll",
        "projectId": "cdp-jhsf-prod",
        "datasetId": "datamart",
        "tableId": "=nps_food",
        "limit": 1,
        "options": {
          "selectedFields": "date_created"
        }
      },
      "id": "1b9443f6-586f-4eeb-8a2b-7e9042372a6c",
      "name": "datamart.nps_food",
      "type": "n8n-nodes-base.googleBigQuery",
      "typeVersion": 1,
      "position": [
        -1180,
        200
      ],
      "credentials": {
        "googleApi": {
          "id": "4",
          "name": "Account_DataLake"
        }
      }
    },
    {
      "parameters": {},
      "id": "9f081d40-a996-4f68-beda-323bb1f3306e",
      "name": "NoOp - ELSE1",
      "type": "n8n-nodes-base.noOp",
      "typeVersion": 1,
      "position": [
        1220,
        280
      ]
    },
    {
      "parameters": {
        "conditions": {
          "boolean": [
            {
              "value1": "={{ $json[\"isAllUpdated\"] }}"
            }
          ]
        }
      },
      "id": "07b176ce-461d-4ba6-8071-c607f552d07e",
      "name": "Verifica se todas as tabelas estão atualizadas",
      "type": "n8n-nodes-base.if",
      "typeVersion": 1,
      "position": [
        1000,
        200
      ]
    }
  ],
  "pinData": {},
  "connections": {
    "HTTP Request": {
      "main": [
        [
          {
            "node": "IF",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Set": {
      "main": [
        [
          {
            "node": "HTTP Request",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Switch": {
      "main": [
        [
          {
            "node": "NoOp - OK",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "NoOp - NOK",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "NoOp - ELSE",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Item Lists - ok": {
      "main": [
        [
          {
            "node": "WhatsApp - ok - Patrick",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Item Lists - nok": {
      "main": [
        [
          {
            "node": "WhatsApp - nok - Patrick",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "WhatsApp - ok - Patrick": {
      "main": [
        [
          {
            "node": "WhatsApp - ok - Ramiro",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "WhatsApp - ok - Ramiro": {
      "main": [
        [
          {
            "node": "WhatsApp - ok - Jonathas",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "WhatsApp - ok - Jonathas": {
      "main": [
        [
          {
            "node": "WhatsApp - ok - Thiago",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "WhatsApp - nok - Patrick": {
      "main": [
        [
          {
            "node": "WhatsApp - nok - Ramiro",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "WhatsApp - nok - Ramiro": {
      "main": [
        [
          {
            "node": "WhatsApp - nok - Jonathas",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "WhatsApp - nok - Jonathas": {
      "main": [
        [
          {
            "node": "WhatsApp - nok - Thiago",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "IF": {
      "main": [
        [
          {
            "node": "Item Lists - ok",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Item Lists - nok",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "silver.transactions_membershipid": {
      "main": [
        [
          {
            "node": "silver.categories_per_products_cjfood",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "silver.customer_wallets_membershipid": {
      "main": [
        [
          {
            "node": "silver.transactions_membershipid",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "silver.wallet_executions_membershipid": {
      "main": [
        [
          {
            "node": "silver.customer_wallets_membershipid",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "silver.customer_addresses_membershipid": {
      "main": [
        [
          {
            "node": "silver.wallet_executions_membershipid",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "silver.customers_membershipid": {
      "main": [
        [
          {
            "node": "silver.customer_addresses_membershipid",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "silver.macro_groups_membershipid": {
      "main": [
        [
          {
            "node": "silver.customers_membershipid",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "silver.stores_sub_store_macro_group_membershipid": {
      "main": [
        [
          {
            "node": "silver.macro_groups_membershipid",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "silver.stores_sub_store_membershipid": {
      "main": [
        [
          {
            "node": "silver.stores_sub_store_macro_group_membershipid",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Gera JSON das tabelas": {
      "main": [
        [
          {
            "node": "Verifica data de inclusão",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Schedule Trigger": {
      "main": [
        [
          {
            "node": "datamart.nps_food",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Verifica data de inclusão": {
      "main": [
        [
          {
            "node": "Verifica se todas as tabelas estão atualizadas",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "silver.customer_addresses_cjfood": {
      "main": [
        [
          {
            "node": "silver.customer_documents_cjfood",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "silver.categories_per_products_cjfood": {
      "main": [
        [
          {
            "node": "silver.customer_addresses_cjfood",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "silver.customer_documents_cjfood": {
      "main": [
        [
          {
            "node": "silver.customer_payments_cjfood",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "silver.customer_payments_cjfood": {
      "main": [
        [
          {
            "node": "silver.customer_phones_cjfood",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "silver.customer_phones_cjfood": {
      "main": [
        [
          {
            "node": "silver.customers_cjfood",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "silver.customers_cjfood": {
      "main": [
        [
          {
            "node": "silver.one_signal_data_cjfood",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "silver.one_signal_data_cjfood": {
      "main": [
        [
          {
            "node": "silver.order_products_cjfood",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "silver.order_products_cjfood": {
      "main": [
        [
          {
            "node": "silver.orders_cjfood",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "silver.orders_cjfood": {
      "main": [
        [
          {
            "node": "silver.product_categories_cjfood",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "silver.product_categories_cjfood": {
      "main": [
        [
          {
            "node": "silver.product_prices_cjfood",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "silver.product_prices_cjfood": {
      "main": [
        [
          {
            "node": "silver.products_cjfood",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "silver.products_cjfood": {
      "main": [
        [
          {
            "node": "silver.sellers_cjfood",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "silver.sellers_cjfood": {
      "main": [
        [
          {
            "node": "Gera JSON das tabelas",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "datamart.nps_food": {
      "main": [
        [
          {
            "node": "silver.stores_sub_store_membershipid",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Verifica se todas as tabelas estão atualizadas": {
      "main": [
        [
          {
            "node": "Telegram",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "NoOp - ELSE1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": true,
  "settings": {
    "saveManualExecutions": false,
    "callerPolicy": "any",
    "errorWorkflow": "8"
  },
  "versionId": "65488c57-2f3a-4a19-9f33-bd5cdf8f49ae",
  "id": 6,
  "meta": {
    "instanceId": "c0fc8467d37687c098ab341dd984426ddca5393c27e31d91e1e2e4dd9349b133"
  },
  "tags": []
}

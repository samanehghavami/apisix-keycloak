# apisix-keycloak
if you are using AdminApi you don't need apisix.yml and you can route and create cunsomer with curl or postman




curl -i "http://172.27.220.43:9180/apisix/admin/routes" -X PUT -H 'X-API-KEY: edd1c9f034335f136f87ad84b625c8f1' -d '
{
    "id": "endpoint",
    "uri": "/endpoint",
    "plugins":{
        "openid-connect":{
            "client_id":"client in keycloak",
            "client_secret":"client secret in keycloak",
            "discovery":"http://172.27.220.43:8080/auth/realms/apisix_realm/.well-known/openid-configuration",
            "token_endpoint": "http://172.27.220.43:8080/auth/realms/apisix_realm/protocol/openid-connect/token",
            "scope":"openid profile",
            "bearer_only":true,
            "realm":"apisix_realm",
            "introspection_endpoint_auth_method":"client_secret_post",
            "redirect_uri":"http://172.27.220.43:9080/"
        }
    },
    "upstream":{
    
        "type":"roundrobin",
        
        "nodes": [ 
        
          {"host": "flask ip master", "port": 5000, "weight": 1, "priority": 1},
          {"host": "flask ip additional", "port": 5000, "weight": 1, "priority": 0}
	  ]
	  
            }
}'

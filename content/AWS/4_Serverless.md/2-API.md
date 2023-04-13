+++
title = "API Gateway"
weight = 2
pre = "- "
+++

### API Gateway Setup

- AWS > API Gateway > HTTP API > Build (or Create API) > API name > Review and Create
- API Gateway > Develop > Routes : Create endpoints (get, post, etc)
- Dynamic ID ex) get, /book/{id}
- Click Route > Route details > Attach integration
- Integration target > Lambda function > Select lambda function you created it for
- URL : API > invoke URL

### CORS

- API Gateway > Develop > CORS
- access-control-allow-origin : \* (only for the get request)
- access-control-allow-headers : \* => not safe way but just use it for now
- credentials :no

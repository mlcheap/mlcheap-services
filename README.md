# ML cheap services
## Spinning up services
this repository uses apache to give public access to all `ML Cheap` services. After cloning this repository, please follow the following steps
- `.env` contains `SRVDIR` for server component-related & `LOGDIR` for log related data 
- Put sql proxy credentials (service account) in `${SRVDIR}/sql_proxy/credentials.json`
- Put ssl credentials in `${SRVDIR}/ssl/` and private key in `${SRVDIR}/ssl/private`
- Make empty log files `labeler.log`, `sdk.log`, `mongodb.log`, all under `${LOGDIR}`
- run `docker-compose up -d`

### Persisted volumes 
- ML models will be saved in `${SRVDIR}/ml-backend/mdels`
- MongoDB data is bound to `${SRVDIR}/mongodb/`
- Web build files are in the named volume `builddata`, the location is managed by docker

## Overall structure 

- `ml_deep_learning`: the deep learning service
- `ml-backend`: backend for creating and accessing ML models
- `dashboard`: the service for dashboard service
- `admin`: the service for admin console
- `flask_labeler`: the backend for web app
- `flask_sdk`: the backend for public API
- `mongodb`: the mongo DB service
- `web_builder`: the npm module for building web services
- `skillmap`: the sql proxy service for SkillLab's ESCO tags database
- `vacancies`: the sql proxy service for SkillLab's Azuna jobs database
- `httpd`: the Apache web service gives public `https` access to web and api services (`http` port `:80` redirects to the `https`):
  - `https://skillLab.mlcheap.com -> /srv/www/webapp`
  - `https://skillLab.mlcheap.com/api -> http://flask_labeler:5100/api`
  - `https://skillapi.mlcheap.com -> http://flask_sdk:6221`
  - `https://skillai.mlcheap.com -> http://ml-backend:5000`
  - `https://skilldeep.mlcheap.com -> http://ml_deep_learning:80`


### Summary of ports
| Service | Ports |  
|---|---|
ml_deep_learning    |  443/tcp, 0:6000->80/tcp    |             
mlcheap-admin       |  |                                          
mlcheap-dashboard   | 0:4000->4000/tcp, 0:8889->8888/tcp |
mlcheap-httpd       | 0:443->443/tcp, 0:80->80/tcp     | 
mlcheap-labeler     | 0:5100->5100/tcp                     |   
mlcheap-ml-backend  | 0:5000->5000/tcp, 0:8890->8888/tcp |
mlcheap-mongodb     |  27017/tcp                 |                    
mlcheap-sdk         | 0:6221->6221/tcp      |                  
mlcheap-webbuilder  | 0              |                                   
proxy-skillmap      | 0:8765->5432/tcp                        
proxy-vacancies     | 0:5678->5432/tcp |

## Apache configuration  
The configuration file `http/my-httpd.conf `overrides the default configuration by loading the following modules for proxy and reverse proxy 
```
LoadModule proxy_html_module modules/mod_proxy_html.so
LoadModule proxy_module modules/mod_proxy.so
LoadModule proxy_http_module modules/mod_proxy_http.so
```
And the following module for rewriting the rule 
```
LoadModule rewrite_module modules/mod_rewrite.so
```
And the following for ssl certification 
```
LoadModule ssl_module modules/mod_ssl.so
```

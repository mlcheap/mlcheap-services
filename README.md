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

- `ml_deep_learning`: the deep learning service, published `localhost:6000`
- `flask_labeler`: the backend for web app, published `localhost:5100/api`
- `flask_sdk`: the backend for public API, published `localhost:6221`
- `mongodb`: the mongo DB service
- `web_builder`: the npm module for building web services
- `ml-backend`: backend for creating and accessing ML models, published `localhost:5000`
- `skillmap`: the sql proxy service for SkillLab's ESCO tags database, published `localhost:8765`
- `vacancies`: the sql proxy service for SkillLab's Azuna jobs database, published `localhost:5678`
- `httpd`: the Apache web service gives public `https` access to web and api services (`http` port `:80` redirects to the `https`):
  - `https://skillLab.mlcheap.com -> /srv/www/webapp`
  - `https://skillLab.mlcheap.com/api -> http://flask_labeler:5100/api`
  - `https://skillapi.mlcheap.com -> http://flask_sdk:6221`
  - `https://skillai.mlcheap.com -> http://ml-backend:5000`

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

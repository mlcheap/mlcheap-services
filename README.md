# ML cheap services
- `.env` contains `SRVDIR` for server component-related & `LOGDIR` for log related data 
- Put sql proxy credentials (service account) in `${SRVDIR}/sql_proxy/credentials.json`
- Put ssl credentials in `${SRVDIR}/ssl/` and private key in `${SRVDIR}/ssl/private`
- Make empty log files `labeler.log`, `sdk.log`, `mongodb.lo`, all under `${LOGDIR}`
Output notes
- ML models will be saved in `${SRVDIR}/ml-backend/models`
- MongoDB data is bound to `${SRVDIR}/mongodb/`
- Web build files are `${SRVDIR}/www/webapp`

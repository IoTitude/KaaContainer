import requests
import json
import shutil

#ADMIN CREDENTIALS
adminUser = 'admin'
adminPass = 'admin123'

#PARAMETERS
tenantName = "Tenant"
tenantUser = "tenant"
tenantPass = "tenant123"
appName = "KaMU"
ecfName = "KaMU Event Class Family"
ecfClassName = "KaMUEventClassFamily"
ecfNamespace = "org.kaaproject.kaa.event.kamu"
devFirstName = "Deve"
devLastName = "Loper"
devUser = "developer"
devPass = "developer123"
logName = "KaMUlog"

tenantTempPass = ""
tenantID = ""
appToken = ""
appID = ""
ecfID = ""
devTempPass = ""
devID = ""
ecfMapID = "1"
verifierToken = ""
sdkID = ""

def createAdminUser(username, password):
    url = "http://172.17.0.4:8080/kaaAdmin/rest/api/auth/createKaaAdmin"
    params = {'username':username,'password':password}
    r = requests.post(url, params=params)
    print r.text
    print "Admin user created"

def createTenantUser(username, password, tenantName, tenantUser):
    global tenantID
    global tenantTempPass
    url = "http://172.17.0.4:8080/kaaAdmin/rest/api/tenant"
    headers = {'Content-Type': 'application/json'}
    data = '{"tenantName":"'+ tenantName +'","username":"'+ tenantUser +'","authority":"TENANT_ADMIN","mail":"asd@asd.fi"}'
    r = requests.post(url, auth=(username, password), headers=headers, data=data)
    print r.text
    jsonData = json.loads(r.text)
    tenantID = jsonData['tenantId']
    tenantTempPass = jsonData['tempPassword']
    print "Tenant user created"

def checkAuth(username, password):
    r = requests.get("http://172.17.0.4:8080/kaaAdmin/rest/api/auth/checkAuth", auth=(username, password))
    print r.text

def changePassword(username, password, paramUser, paramOldPass, paramNewPass):
    url = "http://172.17.0.4:8080/kaaAdmin/rest/api/auth/changePassword"
    params = {'username':paramUser,'oldPassword':paramOldPass, 'newPassword':paramNewPass}
    r = requests.post(url, auth=(username, password), params=params)
    print r.text

def createApplication(username, password, appName, tenantID):
    global appID
    global appToken
    url = "http://172.17.0.4:8080/kaaAdmin/rest/api/application"
    headers = {'Content-Type': 'application/json'}
    data = '{"name":"'+ appName +'", "tenantId":"'+ tenantID +'"}'
    r = requests.post(url, auth=(username, password), headers=headers, data=data)
    print r.text
    jsonData = json.loads(r.text)
    appID = jsonData['id']
    appToken = jsonData['applicationToken']
    print "Application created"

def createEventClassFamily(username, password, ecfName, ecfClassName, ecfNamespace):
    global ecfID
    url = "http://172.17.0.4:8080/kaaAdmin/rest/api/eventClassFamily"
    headers = {'Content-Type': 'application/json'}
    data = '{"name":"'+ ecfName +'","className":"'+ ecfClassName +'","description":"Event Class Family for KaMU","namespace":"'+ ecfNamespace +'"}'
    r = requests.post(url, auth=(username, password), headers=headers, data=data)
    print r.text
    jsonData = json.loads(r.text)
    ecfID = jsonData['id']
    print "Event class family created"

def addEventClassFamilySchema(username, password, ecfID):
    url = "http://172.17.0.4:8080/kaaAdmin/rest/api/addEventClassFamilySchema"
    files = {'eventClassFamilyId': ('', ecfID, 'text/plain'), 'file': ('event_class_schema.json', open('event_class_schema.json', 'rb'), 'application/json'),}
    r = requests.post(url, auth=(username, password), files=files)
    print r.text
    print "Event class family schema added"

def createDeveloperUser(username, password, devUser, devFirstName, devLastName):
    global devID
    global devTempPass
    url = "http://172.17.0.4:8080/kaaAdmin/rest/api/user"
    url2 = "http://httpbin.org/post"
    headers = {'Content-Type': 'application/json'}
    data = '{"username":"'+ devUser +'","firstName":"'+ devFirstName +'","lastName":"'+ devLastName +'","mail":"asd@asd.com","authority":"TENANT_DEVELOPER"}'
    r = requests.post(url, auth=(username, password), headers=headers, data=data)
    print r.text
    jsonData = json.loads(r.text)
    devID = jsonData['id']
    devTempPass = jsonData['tempPassword']
    print "Developer user created"

def createLogSchema(username, password, appID):
    url = "http://172.17.0.4:8080/kaaAdmin/rest/api/createLogSchema"
    metadata = json.dumps({"applicationId":appID,"name":logName,"description":"Log schema for KaMU"})
    files = {'logSchema': ('', metadata, 'application/json'), 'file': ('log_schema.json', open('log_schema.json', 'rb'), 'application/json'),}
    r = requests.post(url, auth=(username, password), files=files)
    print r.text
    print "Log schema created"

def createLogAppender(username, password, appID, appToken, tenantID):
    url = "http://172.17.0.4:8080/kaaAdmin/rest/api/logAppender"
    headers = {'Content-Type': 'application/json'}
    data = {"pluginClassName": "org.kaaproject.kaa.server.appenders.cassandra.appender.CassandraLogAppender", "applicationId": appID, "applicationToken": appToken, "jsonConfiguration": "{\"cassandraServers\": [{\"host\":\"192.168.142.46\",\"port\":9042}],\"cassandraCredential\": {\"org.kaaproject.kaa.server.appenders.cassandra.config.gen.CassandraCredential\": {\"user\":\"cassandra\",\"password\":\"cassandra\"}},\"keySpace\":\"kaa\",\"tableNamePattern\":\"data\",\"columnMapping\": [{\"type\":\"HEADER_FIELD\",\"value\":{\"string\":\"timestamp\"},\"columnName\":\"timestamp\",\"columnType\":\"BIGINT\",\"partitionKey\":false,\"clusteringKey\":true},{\"type\":\"EVENT_FIELD\",\"value\":{\"string\":\"id\"},\"columnName\":\"id\",\"columnType\":\"TEXT\",\"partitionKey\":true,\"clusteringKey\":false},{\"type\":\"EVENT_FIELD\",\"value\":{\"string\":\"temperature\"},\"columnName\":\"temperature\",\"columnType\":\"DOUBLE\",\"partitionKey\":false,\"clusteringKey\":false},{\"type\":\"EVENT_FIELD\",\"value\":{\"string\":\"conductivity\"},\"columnName\":\"conductivity\",\"columnType\":\"DOUBLE\",\"partitionKey\":false,\"clusteringKey\":false},{\"type\":\"EVENT_FIELD\",\"value\":{\"string\":\"pressure\"},\"columnName\":\"pressure\",\"columnType\":\"DOUBLE\",\"partitionKey\":false,\"clusteringKey\":false},{\"type\":\"EVENT_FIELD\",\"value\":{\"string\":\"level\"},\"columnName\":\"level\",\"columnType\":\"DOUBLE\",\"partitionKey\":false,\"clusteringKey\":false},{\"type\":\"EVENT_FIELD\",\"value\":{\"string\":\"flow\"},\"columnName\":\"flow\",\"columnType\":\"DOUBLE\",\"partitionKey\":false,\"clusteringKey\":false}],\"clusteringMapping\":[{\"columnName\":\"timestamp\",\"order\":\"DESC\"}],\"cassandraBatchType\":{\"org.kaaproject.kaa.server.appenders.cassandra.config.gen.CassandraBatchType\":\"UNLOGGED\"},\"cassandraSocketOption\":null,\"executorThreadPoolSize\":1,\"callbackThreadPoolSize\":2,\"dataTTL\":0,\"cassandraWriteConsistencyLevel\":{\"org.kaaproject.kaa.server.appenders.cassandra.config.gen.CassandraWriteConsistencyLevel\":\"ONE\"},\"cassandraCompression\":{\"org.kaaproject.kaa.server.appenders.cassandra.config.gen.CassandraCompression\":\"NONE\"},\"cassandraExecuteRequestType\":{\"org.kaaproject.kaa.server.appenders.cassandra.config.gen.CassandraExecuteRequestType\":\"SYNC\"}}", "description": "Log appender for KaMU", "headerStructure": ["TIMESTAMP"], "name": "KaMU Log Appender", "maxLogSchemaVersion": 2, "minLogSchemaVersion": 2, "tenantId": tenantID, "pluginTypeName": "Cassandra"}
    n = json.dumps(data)
    o = json.loads(n)
    r = requests.post(url, auth=(username, password), headers=headers, json=o)
    print r.text
    print "Log appender created"

def getEventClasses(username, password, ecfID):
    url = "http://172.17.0.4:8080/kaaAdmin/rest/api/eventClasses"
    params = {'eventClassFamilyId':ecfID,'version':1, 'type':"EVENT"}
    r = requests.get(url, auth=(username, password), params=params)
    print r.text
    return r.text

def parseEventClasses(username, password, ecfID):
    textToParse = getEventClasses(username, password, ecfID)
    jsonData = json.loads(textToParse)
    dictionary = {}
    for x in range(0, len(jsonData)):
        dictionary[jsonData[x]["fqn"]] = jsonData[x]["id"]
    return dictionary

def createEventFamilyMap(username, password, appID, ecfID, ecfName):
    global ecfMapID
    dictionary = parseEventClasses(username, password, ecfID)
    url = "http://172.17.0.4:8080/kaaAdmin/rest/api/applicationEventMap"
    url2 = "http://httpbin.org/post"
    headers = {'Content-Type': 'application/json'}
    dataArray = []
    for key in dictionary:
        dataArray.append({"action":"BOTH","eventClassId":dictionary[key],"fqn":key})
    data = {"applicationId": appID,"ecfId": ecfID,"ecfName": ecfName,"eventMaps": dataArray, "version":1 }
    jsonDump = json.dumps(data)
    jsonData = json.loads(jsonDump)
    r = requests.post(url, auth=(username, password), headers=headers, json=jsonData)
    print r.text
    jsonData = json.loads(r.text)
    ecfMapID = jsonData['id']
    print "Event family map created"

def createUserVerifier(username, password, appID):
    global verifierToken
    url = "http://172.17.0.4:8080/kaaAdmin/rest/api/userVerifier"
    headers = {'Content-Type': 'application/json'}
    data = '{"pluginClassName": "org.kaaproject.kaa.server.verifiers.trustful.verifier.TrustfulUserVerifier", "pluginTypeName":"Trustful verifier", "applicationId":' + appID + ', "name":"User Verifier","description": "Default user verifier", "jsonConfiguration": ""}'
    r = requests.post(url, auth=(username, password), headers=headers, data=data)
    print r.text
    jsonData = json.loads(r.text)
    verifierToken = jsonData['verifierToken']
    print "User verifier created"

def createSDKProfile(username, password, appID, verifierToken, ecfMapID):
    global sdkID
    url = "http://172.17.0.4:8080/kaaAdmin/rest/api/createSdkProfile"
    headers = {'Content-Type': 'application/json'}
    data = '{"applicationId":' + appID + ', "configurationSchemaVersion": 1, "logSchemaVersion": 2, "name": "KaMU SDK", "notificationSchemaVersion": 1, "profileSchemaVersion": 0, "defaultVerifierToken":"' + verifierToken + '", "aefMapIds": [' + ecfMapID + ']}'
    print data
    r = requests.post(url, auth=(username, password), headers=headers, data=data)
    print r.text
    jsonData = json.loads(r.text)
    sdkID = jsonData['id']
    print "SDK profile created"

def generateSDKProfile(username, password, sdkID):
    url = "http://172.17.0.4:8080/kaaAdmin/rest/api/sdk"
    params = {'sdkProfileId':sdkID,'targetPlatform':'JAVA'}
    r = requests.post(url, auth=(username, password), params=params, stream=True)
    with open('kamu_sdk.jar', 'wb') as fd:
        for chunk in r.iter_content(1024):
            fd.write(chunk)
    print "SDK profile generated"

createAdminUser(adminUser, adminPass)
createTenantUser(adminUser, adminPass, tenantName, tenantUser)
changePassword(adminUser, adminPass, tenantUser, tenantTempPass, tenantPass)
createApplication(tenantUser, tenantPass, appName, tenantID)
createEventClassFamily(tenantUser, tenantPass, ecfName, ecfClassName, ecfNamespace)
addEventClassFamilySchema(tenantUser, tenantPass, ecfID)
createDeveloperUser(tenantUser, tenantPass, devUser, devFirstName, devLastName)
changePassword(adminUser, adminPass, devUser, devTempPass, devPass)
createLogSchema(devUser, devPass, appID)
createLogAppender(devUser, devPass, appID, appToken, tenantID)
createEventFamilyMap(devUser, devPass, appID, ecfID, ecfName)
createUserVerifier(devUser, devPass, appID)
createSDKProfile(devUser, devPass, appID, verifierToken, ecfMapID)
generateSDKProfile(devUser, devPass, sdkID)

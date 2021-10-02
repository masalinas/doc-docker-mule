# Description
Mule CE on docker

Note: This example uses MuleSoft CE. To use MuleSoft EE please change the line # 31 to download the desired standalone software and also add below line to copy the valid license file.

COPY <Local-Dir>/muleLicenseKey.lic /opt/mule/conf/muleLicenseKey.lic

# Build the docker image
docker build -t mule-ce:4.3.0 .

# Start mule container
- Create this folders 
/opt/mule/apps
/opt/mule/logs

- start docker
docker run -d --name mule-ce.4.3.0 -p "80:8081" -v /opt/mule/apps:/opt/mule/apps -v /opt/mule/logs:/opt/mule/logs mule-ce:4.3.0

# Run a mule application to test
Generate the jar file previously for this mule application

``` xml
<?xml version="1.0" encoding="UTF-8"?>

<mule xmlns:ee="http://www.mulesoft.org/schema/mule/ee/core" xmlns:http="http://www.mulesoft.org/schema/mule/http"
	xmlns="http://www.mulesoft.org/schema/mule/core"
	xmlns:doc="http://www.mulesoft.org/schema/mule/documentation" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.mulesoft.org/schema/mule/core http://www.mulesoft.org/schema/mule/core/current/mule.xsd
http://www.mulesoft.org/schema/mule/http http://www.mulesoft.org/schema/mule/http/current/mule-http.xsd
http://www.mulesoft.org/schema/mule/ee/core http://www.mulesoft.org/schema/mule/ee/core/current/mule-ee.xsd">
	<http:listener-config name="HTTP_Listener_config" doc:name="HTTP Listener config" doc:id="b8a39b7b-d853-403a-b8dd-c83f47c84bc9" >
		<http:listener-connection host="0.0.0.0" port="8081" />
	</http:listener-config>
	<flow name="mule-dockerFlow" doc:id="9bcaaf34-dfb3-4f01-8992-45c36ac80f6b" >
		<http:listener doc:name="Listener" doc:id="36635102-6321-4129-893b-952db7bd2882" config-ref="HTTP_Listener_config" path="/test"/>
		<ee:transform doc:name="Transform Message" doc:id="c51eb0e8-80de-4c54-8ae8-de8d6f0bce27" >
			<ee:message >
				<ee:set-payload ><![CDATA[%dw 2.0
output text/plain
---
"Mule Application Running in Docker"]]></ee:set-payload>
			</ee:message>
		</ee:transform>
	</flow>
</mule>
```

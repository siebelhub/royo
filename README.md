# royo
Roll Your Own Outbound REST Service

#Instructions

1. Import SIFs (See eScript business service for comments)
2. Deploy ProxyBS (chatCmdOut) as an inbound WebService (just to create web service/web service ports records)
3. Run database updates (see below)
4. Invoke the Outbounder BS method to test

This example calls a service from a chatbot platform (Blip/Take) and uses a Lime protocol (different from OpenAPI swagger format).

You'll need import the server certificates in your SES tomcat (internal) truststore to a correct SSL negotiation.

Note: For successful communication, a valid certificate is required on the client (SES tomcat internal), self-signed certificates
are not accepted by the server.

The environment setup should be done following this Oracle DOC:

How To Set Up And Test Outbound RESTFul API Services From Versions 17.0 - 20.7 (Doc ID 2409710.2)

or, for 20.8 or later:

How To Set Up And Test Outbound RESTFul API Services From 20.8 Onwards (Doc ID 2739175.2)

To enable restoutbound.log trace file in internal siebel server Tomcat log dir, use:

How To Generate Detailed Log Files siebeljbs_*.log And restoutbound.log For Outbound REST API Calls (Doc ID 2797583.1)

SQL for database updates (this is NOT supported by Oracle)

/*******Begin Database Updates ************/

UPDATE SIEBEL.S_WS_WEBSERVICE
SET TYPE_CD = 'REST',
INBOUND_FLG = 'N'
WHERE NAME = 'chatCmdOut';

UPDATE SIEBEL.S_WS_PORT
SET PORT_ADDRESS = 'https://http.msging.net/commands', 
PROTOCOL_CD = 'SOAP_RPC_LITERAL'
WHERE NAME = 'chatCmdOut';

commit;

/*******End Database Updates ************/

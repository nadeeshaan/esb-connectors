Product: Integration tests for WSO2 ESB ExactTarget connector

Pre-requisites:

 - Maven 3.x
 - Java 1.6 or above
 - The org.wso2.esb.integration.integration-base project is required. The test suite has been configured to download this project automatically. If the automatic download fails, download the following project and compile it using the mvn clean install command to update your local repository:
   https://github.com/wso2-dev/esb-connectors/tree/master/integration-base

Tested Platform:

 - Microsoft WINDOWS V-7
 - UBUNTU 13.04, Mac OSx 10.9
 - WSO2 ESB 4.9.0-ALPHA
 - Java 1.7

Note:
	Set up a new ExactTarget account and follow all the instructions given below in step 4 to generate an access token.

Steps to follow in setting integration test.

 1. Download ESB 4.9.0-ALPHA by navigating the following the URL: https://svn.wso2.org/repos/wso2/scratch/ESB/.

 2. Create an ExactTarget account using URL https://code.exacttarget.com/developer-edition/

		Note: Marketing Cloud Developer Edition is currently not available for registration.

 3. Follow the steps in https://code.exacttarget.com/apis-sdks/soap-api/using-app-center-to-get-an-api-key.html to generate the access token.

 4. Follow the below mentioned steps for adding valid certificate to access ExactTarget API over https.

	i) 	 Extract the certificate from browser(Mozilla Firefox) by navigating to https://webservice.s7.exacttarget.com/Service.asmx
	ii)  Go to new ESB folder and place the downloaded certificate in both "<ESB_HOME>/repository/resources/security/" and "{EXACTTARGET_CONNECTOR_HOME}/exacttarget-connector/exacttarget-connector-1.0.0/org.wso2.carbon.connector/src/test/resources/keystores/products/" folders.
	iii) Navigate to "<ESB_HOME>/repository/resources/security/" using command prompt and execute the following command.

				keytool -importcert -file CERT_FILE_NAME -keystore client-truststore.jks -alias "CERT_NAME"

		 This command will import ExactTarget certificate into keystore.
		 To import the certificate give "wso2carbon" as password. Press "Y" to complete certificate import process.

		 NOTE : CERT_FILE_NAME - Replace CERT_FILE_NAME with the file name that was extracted from ExactTarget with the extension. (e.g. exacttarget.crt)
			    CERT_NAME - Replace CERT_NAME with an arbitrary name for the certificate. (e.g. ExactTarget)

	iv)  Navigate to "{EXACTTARGET_CONNECTOR_HOME}/exacttarget-connector/exacttarget-connector-1.0.0/org.wso2.carbon.connector/src/test/resources/keystores/products/" using command prompt and execute the following command.

				keytool -importcert -file CERT_FILE_NAME -keystore wso2carbon.jks -alias "CERT_NAME"

		 This command will import ExactTarget certificate into keystore.
		 To import the certificate give "wso2carbon" as password. Press "Y" to complete certificate import process.

		 NOTE : CERT_FILE_NAME - Replace CERT_FILE_NAME with the file name that was extracted from ExactTarget with the extension. (e.g. exacttarget.crt)
			    CERT_NAME - Replace CERT_NAME with an arbitrary name for the certificate. (e.g. ExactTarget)

 5. Compress modified ESB as wso2esb-4.9.0-ALPHA.zip and copy that zip file in to location "{ESB_Connector_Home}/repository/".

 6. Make sure that exacttarget is specified as a module in ESB_Connector_Parent pom.
        <module>exacttarget/exacttarget-connector/exacttarget-connector-1.0.0/org.wso2.carbon.connector</module>

 7. Update the properties in 'exacttarget.properties' file at location "{EXACTTARGET_CONNECTOR_HOME}/exacttarget-connector/exacttarget-connector-1.0.0/org.wso2.carbon.connector/src/test/resources/artifacts/ESB/connector/config" as below.

	i) 		apiUrl					           - Use the API URL as "https://webservice.s7.exacttarget.com/Service.asmx".
	ii)		accessToken				           - Access Token obtained by following the steps in 3.
	iii) 	email		                       - Use a valid email address.
	iv)		openEventId	 		               - Use an ID of a created openEvent.
	v)		subscriberAtr1					   - Attribute name of the subscribers profile.
	vi)		subscriberAtr2					   - Attribute name of the subscribers profile.
	vii) 	dataExtCustomerKey	 	           - The unique identifier used to create the data extension.
	viii)	dataExtField1			           - The property name of created DataExtension.
	ix)		dataExtField2					   - The property name of created DataExtension.
	x)		sendClassificationCustomerKey	   - External Key name for SendClassification.

	Note:-  As a Prerequisite 'openEvents', 'subscribers', 'sendClassifications', 'Emails', 'DataExtension', should have been created manually using the ExactTarget trial account.

			i)    Consider the following steps to create subscriber's profile attributes.
				  - Navigate to Subscribers -> All Subscribers -> Profile Management in ExactTarget trial account.
				  - Create two attributes. Use the given values for the 'subscriberAtr1' and 'subscriberAtr2' properties as names for the attributes.

	        ii)   Consider the following steps when creating a Data Extension and its Data Extension Fields.
				  - Navigate to Subscribers -> Data Extensions -> Create -> Standard Data Extension -> 'Create New Data Extension' Wizard in ExactTarget trial account.
				  - External Key value given in step 1 of the wizard should be same as the property value of 'dataExtCustomerKey'.
				  - Should create two fields in step 3 of the wizard with the values given for 'dataExtField1' and 'dataExtField2' properties.

			iii)  Consider the following steps when creating a SendClassification.
				  - Navigate to Admin -> Send Classifications in ExactTarget trial account.
				  - Create a SendClassification by giving values for Name and External Key fields. Provide the External Key field value for 'sendClassificationCustomerKey' property.

			iv)   Consider the following steps to properly validate the email content.
				  - Navigate to Content -> Emails -> Select required email content to validate -> Click Validate button and proceed with the validation.
				  - If Problem(s) found, Fix the validation errors following their instructions. Make sure all the listed emails are valid.


8. Navigate to "{ESB_Connector_Home}/" and run the following command.
      $ mvn clean install
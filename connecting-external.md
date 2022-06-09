---
copyright:
  years: 2021, 2022
lastupdated: "2022-06-09"

keywords: mysql drivers, python, java, javascript, certificate

subcollection: databases-for-mysql

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:pre: .pre}
{:note: .note}

# Connecting an external application
{: #external-app}

Your applications and drivers use connection strings to make a connection to {{site.data.keyword.databases-for-mysql_full}}. The service provides connection strings specifically for drivers and applications. Connection strings are displayed in the *Endpoints* panel of your deployment's *Overview*, and can also be retrieved from the [cloud databases CLI plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-connections) and the [API](https://{DomainName}/apidocs/cloud-databases-api#discover-connection-information-for-a-deployment-f-e81026).

The connection strings can be used by any of the credentials you created on your deployment. While you can use the admin user for all your connections and applications, it might be better to create users specifically for your applications to connect with. Documentation on generating credentials is on the [Creating Users and Getting Connection Strings](/docs/databases-for-mysql?topic=databases-for-mysql-connection-strings) page.

## Connecting with a language's driver
{: #connecting-lang-driver}

All the information a driver needs to make a connection to your deployment is in the "mysql" section of your connection strings. The table contains a breakdown for reference.

Field Name|Index|Description
----------|-----|-----------
`Type`||Type of connection - for MySQL, it is "URI"
`Scheme`||Scheme for a URI - for MySQL, it is "mysql"
`Path`||Path for a URI - for MySQL, it is the database name. The default is `ibmclouddb`.
`Authentication`|`Username`|The username that you use to connect.
`Authentication`|`Password`|A password for the user - might be shown as `$PASSWORD`
`Authentication`|`Method`|How authentication takes place; "direct" authentication is handled by the driver.
`Hosts`|`0...`|A hostname and port to connect to
`Composed`|`0...`|A URI combining Scheme, Authentication, Host, and Path
`Certificate`|`Name`|The allocated name for the self-signed certificate for database deployment
`Certificate`|Base64|A base64 encoded version of the certificate.
{: caption="Table 1. mysql/URI connection information" caption-side="top"}

* `0...` indicates one or more of these entries in an array.

Many MySQL drivers are able to make a connection to your deployment when given the URI-formatted connection string found in the "composed" field of the connection information. For example

```sh
mysql://ibm_cloud_30399dec_4835_4967_a23d_30587a08d9a8:$PASSWORD@981ac415-5a35-4ac7-b6bb-fb609326dc42.8f7bfd8f3faa4218aec56e069eb46187.databases.appdomain.cloud:32704/ibmclouddb?ssl-mode=verify-full
```
{: pre}

For more information on `ssl-mode` states, see [Additional Connection parameters](https://dev.mysql.com/doc/refman/8.0/en/connecting-using-uri-or-key-value-pairs.html){: .external}.

The following example uses the information from your connection string and the Java driver [jdbc](https://dev.mysql.com/doc/connector-j/8.0/en/){: .external} to connect to your database.

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

public class App {
 private final String STATUS_COMMAND = "SHOW VARIABLES LIKE '%version%';";
 
 private Connection connect = null;
 private Statement stmt = null;
 private ResultSet rs = null;

 private final String url = "mysql://127.0.0.1:30799";
 private final String username = "";
 private final String password = "";
 private final Boolean useSSL = true;

 public static void main(String args[]) throws Exception {
   App app = new App();
   final byte maxConnectionAttempt = 5;
   byte currentConnectionAttempt = 0;

   while (!app.connectDatabase() && currentConnectionAttempt < maxConnectionAttempt)
      ++currentConnectionAttempt;

   if (currentConnectionAttempt >= maxConnectionAttempt) {
      System.out.println(currentConnectionAttempt + " weren't successfull!");
         } else {
            app.printStatus();
            app.closeConnection();
         }
   }

   public void printStatus() {
         try {
            stmt = connect.createStatement();
            rs = stmt.executeQuery(STATUS_COMMAND);
            while (rs.next())
            System.out.println(rs.getString(1) + ": " + rs.getString(2));
         } catch (SQLException ex) {
            System.out.println("SQLException: " + ex.getMessage());
            System.out.println("SQLState: " + ex.getSQLState());
            System.out.println("VendorError: " + ex.getErrorCode());
            closeConnection();
         }
   }

   private void closeConnection() {
      try {
         if (rs != null) {
            rs.close();
         }

         if (stmt != null) {
            stmt.close();
         }

         if (connect != null) {
            connect.close();
         }
      } catch (Exception e) {
         System.out.println(e.getMessage());
      }
      System.out.println("Connection closed.");
   }

   public boolean connectDatabase() {
      // https://dev.mysql.com/doc/connector-j/8.0/en/connector-j-usagenotes-connect-drivermanager.html
      try {
            Properties props = new Properties();
            props.setProperty("user", username);
            props.setProperty("password", password);
            props.setProperty("useSSL", Boolean.toString(useSSL));
            props.setProperty("sslMode", (useSSL ? "REQUIRED" : "DISABLED"));
            props.setProperty("requireSSL", Boolean.toString(useSSL));
            props.setProperty("verifyServerCertificate", Boolean.toString(useSSL));

            Class.forName("com.mysql.cj.jdbc.Driver");
            connect = DriverManager.getConnection("jdbc:" + url, props);
            return true;
      } catch (SQLException ex) {
            System.out.println("SQLException: " + ex.getMessage());
            System.out.println("SQLState: " + ex.getSQLState());
            System.out.println("VendorError: " + ex.getErrorCode());
      } catch (ClassNotFoundException ex) {
            System.out.println("Connector class can not be found: " + ex.getMessage());
      }
      return false;
   }
}

```
{: .codeblock}

The following example uses the information from your connection string and the Python driver [pymysql](https://pypi.org/project/PyMySQL/#documentation){: .external} to connect to your database. This is just a simple connection example, without error handling or retry logic and might not be suitable for production.

```python
import pymysql
connection = pymysql.connect(
  host="hostname",
  port=32089,
  user="username",
  passwd="password",
  ssl_ca="/home/user/mysql_ca.crt",
  ssl_verify_cert=True,
  ssl_verify_identity=True)
  
cursor = connection.cursor()
cursor.execute("SHOW STATUS;")

for row in cursor:
    print(row[0] + "\t" + row[1])
    
cursor.close()
connection.close()
```
{: .codeblock}

## Driver TLS and self-signed certificate support
{: #connecting-cert-support}

All connections to {{site.data.keyword.databases-for-mysql}} are TLS 1.2 enabled, so the driver you use to connect needs to be able to support encryption. Your deployment also comes with a self-signed certificate so the driver can verify the server upon connection. 

### Using the self-signed certificate
{: #connecting-using-cert}

1. Copy the certificate information from the *Endpoints* panel or the Base64 field of the connection information. 
2. If needed, decode the Base64 string into text. 
3. Save the certificate to a file. (You can use the name that is provided or your own file name).
4. Provide the path to the certificate to the driver or client.

![CLI Endpoints panel](images/cli-endpoints-pane.png){: caption="Figure 1. CLI Endpoints panel" caption-side="bottom"}

### CLI plug-in support for the self-signed certificate
{: #connecting-cli-cert}

You can display the decoded certificate for your deployment with the CLI plug-in with the command `ibmcloud cdb deployment-cacert "your-service-name"`. It decodes the base64 into text. Copy and save the command's output to a file and provide the file's path to the driver.

## Other Drivers
{: #connecting-drivers}

MySQL has an array of language drivers. The table below covers a few of the most common. Consult MySQL's [Connectors and APIs](https://dev.mysql.com/doc/refman/5.7/en/connectors-apis.html){: .external} for more information.

Language|Driver|Examples
-------|-------|-------
PHP|`mysql`|[Link](https://www.php.net/manual/en/mysqli.quickstart.transactions.php){: .external}
Ruby|`ruby-mysql`|[Link](https://dev.mysql.com/doc/refman/5.7/en/apis-ruby-rubymysql.html){: .external}
C#|`ODBC`|[Link](https://dev.mysql.com/doc/connector-net/en/){: .external}
Go|`mysql`|[Link](https://pkg.go.dev/github.com/go-sql-driver/mysql){: .external}
{: caption="Table 2. MySQL drivers" caption-side="top"}

When connecting to MySQL using PHP, it is necessary to change the auth plug-in from `sha256_password` to `mysql_native_password`.
{: .note}

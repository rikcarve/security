# Java Security 1x1
## Keytool
```shell
keytool -importpass -storetype pkcs12 -alias myEntry -keystore password-store.p12
keytool -list -keystore password-store.p12 -storetype pkcs12
```

## Elytron

```shell
/subsystem=elytron/credential-store=mycs:add(relative-to=jboss.server.data.dir, location=example.jceks,create=true,credential-reference={clear-text=mypwd})
/subsystem=elytron/credential-store=mycs:add-alias(alias=myEntry,secret-value=mysecretpwd)
/subsystem=elytron/credential-store=mycs:read-aliases()
```

```xml
                <credential-store name="cs12" relative-to="jboss.server.data.dir" location="password-store.p12" create="true">
                    <implementation-properties>
                        <property name="keyStoreType" value="PKCS12"/>
                    </implementation-properties>
                    <credential-reference clear-text="MASK-Bmfq4mvDV7F;A1B2C3D4;351"/>
                </credential-store>
```

```shell
elytron-tool.bat mask -i 351 -s A1B2C3D4 -x mypwd
```

### Use elytron in datasource
```xml
                <datasource jndi-name="java:/jdbc/myDS" pool-name="MySqlDS">
                    <connection-url>jdbc:mysql://localhost:3306/test</connection-url>
                    <driver>mysql</driver>
                    <security>
                        <user-name>rik</user-name>
                        <credential-reference store="cs12" alias="dbpwd"/>
                    </security>
```

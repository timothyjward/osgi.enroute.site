---
title: Microservice 
layout: tutorial
lprev: 020-tutorial_qs.html 
lnext: 010-tutorial.html
summary: An OSGi™ based microservice tutorial  (< 10 minutes). 
---

## Introduction

This Tutorial walks through the creation of REST microservice which is comprised of the following four structural components:
* The API module
* The DAO implemetation module
* The Rest Service module 
* The Application module

As with the Quick Start Tutorial, the following is Maven and command-line based; the reader may follow this verbatim or use their favorite Java/IDE. 

We start by creating the required project skeleton. 

if you change the example `groupId`, `artifactId` values to alternates, you'll also need to update the `packageinfo` and `import` statements in the files used in this tutorial.
{: .note }

From the same root directory as [Quick Start](tutorial_qs) create the main project:

{% highlight shell-session %}
mvn -s settings.xml archetype:generate -DarchetypeGroupId=org.osgi.enroute.archetype -DarchetypeArtifactId=project-bare -DarchetypeVersion=7.0.0-SNAPSHOT
{% endhighlight %}

input the following values:

{% highlight shell-session %}
Define value for property 'groupId': org.osgi.enroute.examples
Define value for property 'artifactId': microservice
Define value for property 'version' 1.0-SNAPSHOT: : 0.0.1-SNAPSHOT
Define value for property 'package' org.osgi.enroute.examples: :
Confirm properties configuration:
groupId: org.osgi.enroute.examples
artifactId: microservice
version: 0.0.1-SNAPSHOT
package: org.osgi.enroute.examples
Y: :
{% endhighlight %}

Having created the root project we'll now created the required application modules. 

## The microservice DAO API 

Now change directory into the newly created `microservice` project directory

**To create the `api` module**

{% highlight shell-session %}
mvn -s ../settings.xml archetype:generate -DarchetypeGroupId=org.osgi.enroute.archetype -DarchetypeArtifactId=api -DarchetypeVersion=7.0.0-SNAPSHOT
{% endhighlight %}

input the following values:

{% highlight shell-session %}
Define value for property 'groupId': org.osgi.enroute.examples.microservice
Define value for property 'artifactId': dao-api
Define value for property 'version' 1.0-SNAPSHOT: : 0.0.1-SNAPSHOT
Define value for property 'package' org.osgi.enroute.examples.microservice: :
Confirm properties configuration:
groupId: org.osgi.enroute.examples.microservice
artifactId: dao-api
version: 0.0.1-SNAPSHOT
package: org.osgi.enroute.examples.microservice
Y: :
{% endhighlight %}

* Delete the template files created in `dao-api/src/main/java/org/osgi/enroute/examples/microservice` as these are not required.
* Create the directory `dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao` into which we create the following three files:

**dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao/package-info.java**
{% highlight java %}
@org.osgi.annotation.bundle.Export
@org.osgi.annotation.versioning.Version("1.0.0")
package org.osgi.enroute.examples.microservice.dao;
{% endhighlight %}

**dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao/PersonDao.java**
{% highlight java %}
package org.osgi.enroute.examples.microservice.dao;
 
import java.util.List;
 
import org.osgi.annotation.versioning.ProviderType;
import org.osgi.enroute.examples.microservice.dao.dto.PersonDTO;
 
@ProviderType
public interface PersonDao {
     
    public List<PersonDTO> select();
 
    public PersonDTO findByPK(Long pk) ;
 
    public Long save(PersonDTO data);
 
    public void update(PersonDTO data);
 
    public void delete(Long pk) ;
}
{% endhighlight %}

**dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao/AddressDao.java**
{% highlight java %}
package org.osgi.enroute.examples.microservice.dao;
 
import java.util.List;
 
import org.osgi.annotation.versioning.ProviderType;
import org.osgi.enroute.examples.microservice.dao.dto.AddressDTO;
 
@ProviderType
public interface AddressDao {
     
    public List<AddressDTO> select(Long personId);
 
    public AddressDTO findByPK(String emailAddress);
 
    public void save(Long personId,AddressDTO data);
 
    public void update(Long personId,AddressDTO data);
 
    public void delete(Long personId) ;
 
} 
{% endhighlight %}

Now create a `dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao/dto` directory to which we add the following three files

**dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao/dto/package-info.java**
{% highlight java %}
@org.osgi.annotation.bundle.Export
@org.osgi.annotation.versioning.Version("1.0.0")
package org.osgi.enroute.examples.microservice.dao.dto;
{% endhighlight %}

**dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao/dto/PersonDTO.java**
{% highlight java %}
package org.osgi.enroute.examples.microservice.dao.dto;
 
import java.util.ArrayList;
import java.util.List;
 
public class PersonDTO {
 
    public long personId;
    public String firstName;
    public String lastName;
 
    public List<AddressDTO> addresses = new ArrayList<>();
}
{% endhighlight %}

**dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao/dto/AddressDTO.java**
{% highlight java %}
package org.osgi.enroute.examples.microservice.dao.dto;
 
public class AddressDTO {
 
    public long personId;
    public String emailAddress;
    public String city;
    public String country;
} 
{% endhighlight %} 

## The microservice DAO Impl

Check that you are in the `microservice` project directory, then ...

**To create the `impl` module**

{% highlight shell-session %}
mvn -s ../settings.xml archetype:generate -DarchetypeGroupId=org.osgi.enroute.archetype -DarchetypeArtifactId=ds-component -DarchetypeVersion=7.0.0-SNAPSHOT
{% endhighlight %}

input the following values:

{% highlight shell-session %}
Define value for property 'groupId': org.osgi.enroute.examples.microservice
Define value for property 'artifactId': dao-impl
Define value for property 'version' 1.0-SNAPSHOT: : 0.0.1-SNAPSHOT
Define value for property 'package' org.osgi.enroute.examples.microservice: :
Confirm properties configuration:
groupId: org.osgi.enroute.examples.microservice
artifactId: dao-impl
version: 0.0.1-SNAPSHOT
package: org.osgi.enroute.examples.microservice
Y: :
{% endhighlight %}

Now add the following module dependencies to the `<dependencies>` section in `dao-impl/pom.xml`

{% highlight xml %}
<dependency>
    <groupId>org.osgi.enroute.examples.microservice</groupId>
    <artifactId>dao-api</artifactId>
    <version>0.0.1-SNAPSHOT</version>
</dependency>
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.7.25</version>
</dependency>
{% endhighlight %}

* Delete the template files in `dao-impl/src/main/java/org/osgi/enroute/examples/microservice` as these are not required. 
* Now create the directory `dao-impl/src/main/java/org/osgi/enroute/examples/microservice/dao/impl` into which we create the following four files:

**dao-impl/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/PersonTable.java**
{% highlight java %}
package org.osgi.enroute.examples.microservice.dao.impl;
public interface PersonTable {
 
    String TABLE_NAME = "PERSONS";
 
    String SQL_SELECT_ALL_PERSONS = "SELECT * FROM " + TABLE_NAME;
 
    String SQL_DELETE_PERSON_BY_PK = "DELETE FROM " + TABLE_NAME + " where PERSON_ID=?";
 
    String SQL_SELECT_PERSON_BY_PK = "SELECT * FROM " + TABLE_NAME + " where PERSON_ID=?";
 
    String SQL_INSERT_PERSON = "INSERT INTO " + TABLE_NAME + "(FIRST_NAME,LAST_NAME) VALUES(?,?)";
 
    String SQL_UPDATE_PERSON_BY_PK = "UPDATE " + TABLE_NAME + " SET FIRST_NAME=?, LAST_NAME=? WHERE PERSON_ID=?";
 
    String PERSON_ID = "person_id";
 
    String FIRST_NAME = "first_name";
 
    String LAST_NAME = "last_name";
 
    String INIT = "CREATE TABLE IF NOT EXISTS persons (" //
            + "person_id  mediumint(9) NOT NULL AUTO_INCREMENT," //
            + "first_name varchar(255) NOT NULL," //
            + "last_name varchar(255) NOT NULL," //
            + "PRIMARY KEY (person_id)" //
            + ") ;";
}
{% endhighlight %}

**dao-impl/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/PersonDaoImpl.java**
{% highlight java %}
package org.osgi.enroute.examples.microservice.dao.impl;
 
import static java.sql.Statement.RETURN_GENERATED_KEYS;
import static org.osgi.enroute.examples.microservice.dao.impl.PersonTable.FIRST_NAME;
import static org.osgi.enroute.examples.microservice.dao.impl.PersonTable.INIT;
import static org.osgi.enroute.examples.microservice.dao.impl.PersonTable.LAST_NAME;
import static org.osgi.enroute.examples.microservice.dao.impl.PersonTable.PERSON_ID;
import static org.osgi.enroute.examples.microservice.dao.impl.PersonTable.SQL_DELETE_PERSON_BY_PK;
import static org.osgi.enroute.examples.microservice.dao.impl.PersonTable.SQL_INSERT_PERSON;
import static org.osgi.enroute.examples.microservice.dao.impl.PersonTable.SQL_SELECT_ALL_PERSONS;
import static org.osgi.enroute.examples.microservice.dao.impl.PersonTable.SQL_SELECT_PERSON_BY_PK;
import static org.osgi.enroute.examples.microservice.dao.impl.PersonTable.SQL_UPDATE_PERSON_BY_PK;
 
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.concurrent.atomic.AtomicLong;
 
import org.osgi.enroute.examples.microservice.dao.AddressDao;
import org.osgi.enroute.examples.microservice.dao.PersonDao;
import org.osgi.enroute.examples.microservice.dao.dto.PersonDTO;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.transaction.control.TransactionControl;
import org.osgi.service.transaction.control.jdbc.JDBCConnectionProvider;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
 
@Component
public class PersonDaoImpl implements PersonDao {
     
    private static final Logger logger = LoggerFactory.getLogger(PersonDaoImpl.class);
 
    @Reference
    TransactionControl transactionControl;
 
    @Reference(name="provider")
    JDBCConnectionProvider jdbcConnectionProvider;
 
    @Reference
    AddressDao addressDao;
 
    Connection connection;
 
    @Activate
    void start(Map<String, Object> props) throws SQLException {
        connection = jdbcConnectionProvider.getResource(transactionControl);
        transactionControl.supports(() -> connection.prepareStatement(INIT).execute());
    }
 
    @Override
    public List<PersonDTO> select() {
 
        return transactionControl.notSupported(() -> {
 
            List<PersonDTO> dbResults = new ArrayList<>();
 
            ResultSet rs = connection.createStatement().executeQuery(SQL_SELECT_ALL_PERSONS);
 
            while (rs.next()) {
                PersonDTO personDTO = mapRecordToPerson(rs);
                personDTO.addresses = addressDao.select(personDTO.personId);
                dbResults.add(personDTO);
            }
 
            return dbResults;
        });
    }
 
    @Override
    public void delete(Long primaryKey) {
 
        transactionControl.required(() -> {
            PreparedStatement pst = connection.prepareStatement(SQL_DELETE_PERSON_BY_PK);
            pst.setLong(1, primaryKey);
            pst.executeUpdate();
            addressDao.delete(primaryKey);
            logger.info("Deleted Person with ID : {}", primaryKey);
            return null;
        });
    }
 
    @Override
    public PersonDTO findByPK(Long pk) {
 
       return transactionControl.supports(() -> {
 
            PersonDTO personDTO = null;
 
            PreparedStatement pst = connection.prepareStatement(SQL_SELECT_PERSON_BY_PK);
            pst.setLong(1, pk);
 
            ResultSet rs = pst.executeQuery();
 
            if (rs.next()) {
                personDTO = mapRecordToPerson(rs);
                personDTO.addresses = addressDao.select(pk);
            }
 
            return personDTO;
        });
    }
 
    @Override
    public Long save(PersonDTO data) {
 
        return transactionControl.required(() -> {
 
            PreparedStatement pst = connection.prepareStatement(SQL_INSERT_PERSON, RETURN_GENERATED_KEYS);
 
            pst.setString(1, data.firstName);
            pst.setString(2, data.lastName);
 
            pst.executeUpdate();
 
            AtomicLong genPersonId = new AtomicLong(data.personId);
 
            if (genPersonId.get() <= 0) {
                ResultSet genKeys = pst.getGeneratedKeys();
 
                if (genKeys.next()) {
                    genPersonId.set(genKeys.getLong(1));
                }
            }
 
            logger.info("Saved Person with ID : {}", genPersonId.get());
 
            if (genPersonId.get() > 0) {
                data.addresses.stream().forEach(address -> {
                    address.personId = genPersonId.get();
                    addressDao.save(genPersonId.get(), address);
                });
            }
 
            return genPersonId.get();
        });
    }
 
    @Override
    public void update(PersonDTO data) {
 
        transactionControl.required(() -> {
 
            PreparedStatement pst = connection.prepareStatement(SQL_UPDATE_PERSON_BY_PK);
            pst.setString(1, data.firstName);
            pst.setString(2, data.lastName);
            pst.setLong(3, data.personId);
            pst.executeUpdate();
 
            logger.info("Updated person : {}", data);
 
            data.addresses.stream().forEach(address -> addressDao.update(data.personId, address));
             
            return null;
        });
    }
 
    protected PersonDTO mapRecordToPerson(ResultSet rs) throws SQLException {
        PersonDTO personDTO = new PersonDTO();
        personDTO.personId = rs.getLong(PERSON_ID);
        personDTO.firstName = rs.getString(FIRST_NAME);
        personDTO.lastName = rs.getString(LAST_NAME);
        return personDTO;
    }
}
{% endhighlight %}

**dao-impl/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/AddressTable.java**
{% highlight java %}
package org.osgi.enroute.examples.microservice.dao.impl;
public interface AddressTable {
 
    String TABLE_NAME = "ADDRESSES";
 
    String SQL_SELECT_ADDRESS_BY_PERSON = "SELECT * FROM " + TABLE_NAME + " WHERE PERSON_ID = ? ";
 
    String SQL_DELETE_ADDRESS = "DELETE FROM " + TABLE_NAME + " WHERE EMAIL_ADDRESS = ? AND  PERSON_ID=?";
 
    String SQL_DELETE_ALL_ADDRESS_BY_PERSON_ID = "DELETE FROM " + TABLE_NAME + " WHERE PERSON_ID=?";
 
    String SQL_SELECT_ADDRESS_BY_PK = "SELECT * FROM " + TABLE_NAME + " where EMAIL_ADDRESS=?";
 
    String SQL_ADD_ADDRESS = "INSERT INTO " + TABLE_NAME + "(EMAIL_ADDRESS,PERSON_ID,CITY,COUNTRY) VALUES(?,?,?,?)";
 
    String SQL_UPDATE_ADDRESS_BY_PK_AND_PERSON_ID = "UPDATE " + TABLE_NAME + " SET CITY=?, COUNTRY=? "
            + "WHERE EMAIL_ADDRESS = ? AND  PERSON_ID=?";
 
    String PERSON_ID = "person_id";
 
    String EMAIL_ADDRESS = "email_address";
 
    String CITY = "city";
 
    String COUNTRY = "country";
 
    String INIT = "CREATE TABLE IF NOT EXISTS " + TABLE_NAME +" (" //
            + "email_address varchar(255) NOT NULL," //
            + "person_id  mediumint(9) NOT NULL," //
            + "city varchar(100) NOT NULL," //
            + "country varchar(2) NOT NULL," //
            + "PRIMARY KEY (email_address)" + ") ;";
}
{% endhighlight %}

**dao-impl/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/AddressDaoImpl.java**
{% highlight java %}
package org.osgi.enroute.examples.microservice.dao.impl;
 
import static org.osgi.enroute.examples.microservice.dao.impl.AddressTable.SQL_ADD_ADDRESS;
import static org.osgi.enroute.examples.microservice.dao.impl.AddressTable.SQL_DELETE_ALL_ADDRESS_BY_PERSON_ID;
import static org.osgi.enroute.examples.microservice.dao.impl.AddressTable.SQL_SELECT_ADDRESS_BY_PERSON;
import static org.osgi.enroute.examples.microservice.dao.impl.AddressTable.SQL_SELECT_ADDRESS_BY_PK;
import static org.osgi.enroute.examples.microservice.dao.impl.AddressTable.SQL_UPDATE_ADDRESS_BY_PK_AND_PERSON_ID;
 
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;
 
import org.osgi.enroute.examples.microservice.dao.AddressDao;
import org.osgi.enroute.examples.microservice.dao.dto.AddressDTO;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.transaction.control.TransactionControl;
import org.osgi.service.transaction.control.jdbc.JDBCConnectionProvider;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
 
@Component
public class AddressDaoImpl implements AddressDao {
 
    private static final Logger logger = LoggerFactory.getLogger(AddressDaoImpl.class);
 
    @Reference
    TransactionControl transactionControl;
 
    @Reference(name="provider")
    JDBCConnectionProvider jdbcConnectionProvider;
 
    Connection connection;
 
    @Activate
    void activate(Map<String, Object> props) throws SQLException {
        connection = jdbcConnectionProvider.getResource(transactionControl);
        transactionControl.supports( () -> connection.prepareStatement(AddressTable.INIT).execute());
    }
 
    @Override
    public List<AddressDTO> select(Long personId) {
 
        return transactionControl.notSupported(() -> {
 
            List<AddressDTO> dbResults = new ArrayList<>();
 
            PreparedStatement pst = connection.prepareStatement(SQL_SELECT_ADDRESS_BY_PERSON);
            pst.setLong(1, personId);
 
            ResultSet rs = pst.executeQuery();
 
            while (rs.next()) {
                AddressDTO addressDTO = mapRecordToAddress(rs);
                dbResults.add(addressDTO);
            }
 
            return dbResults;
        });
    }
 
    @Override
    public AddressDTO findByPK(String pk) {
 
        return transactionControl.supports(() -> {
 
            AddressDTO addressDTO = null;
 
            PreparedStatement pst = connection.prepareStatement(SQL_SELECT_ADDRESS_BY_PK);
            pst.setString(1, pk);
 
            ResultSet rs = pst.executeQuery();
 
            if (rs.next()) {
                addressDTO = mapRecordToAddress(rs);
            }
 
            return addressDTO;
        });
    }
 
    @Override
    public void save(Long personId, AddressDTO data) {
         
        transactionControl.required(() -> {
            PreparedStatement pst = connection.prepareStatement(SQL_ADD_ADDRESS);
            pst.setString(1, data.emailAddress);
            pst.setLong(2, data.personId);
            pst.setString(3, data.city);
            pst.setString(4, data.country);
            logger.info("Saved Person with id {}  and Address : {}", personId, data);
            pst.executeUpdate();
             
            return null;
        });
    }
 
    @Override
    public void update(Long personId, AddressDTO data) {
         
        transactionControl.required(() -> {
            PreparedStatement pst = connection.prepareStatement(SQL_UPDATE_ADDRESS_BY_PK_AND_PERSON_ID);
            pst.setString(1, data.city);
            pst.setString(2, data.country);
            pst.setString(3, data.emailAddress);
            pst.setLong(4, data.personId);
            logger.info("Updated Person Address : {}", data);
            pst.executeUpdate();
             
            return null;
        });
    }
 
    @Override
    public void delete(Long personId) {
         
        transactionControl.required(() -> {
            PreparedStatement pst = connection.prepareStatement(SQL_DELETE_ALL_ADDRESS_BY_PERSON_ID);
            pst.setLong(1, personId);
            logger.info("Deleted Person {} Addresses", personId);
            pst.executeUpdate();
 
            return null;
        });
    }
 
    protected AddressDTO mapRecordToAddress(ResultSet rs) throws SQLException {
        AddressDTO addressDTO = new AddressDTO();
        addressDTO.personId = rs.getLong(AddressTable.PERSON_ID);
        addressDTO.emailAddress = rs.getString(AddressTable.EMAIL_ADDRESS);
        addressDTO.city = rs.getString(AddressTable.CITY);
        addressDTO.country = rs.getString(AddressTable.COUNTRY);
        return addressDTO;
    }
}
{% endhighlight %}


## The REST Service

Check that you are in the `microservice` project directory, then ...

**To create the `rest-component` module**

{% highlight shell-session %}
mvn -s ../settings.xml archetype:generate -DarchetypeGroupId=org.osgi.enroute.archetype -DarchetypeArtifactId=rest-component -DarchetypeVersion=7.0.0-SNAPSHOT
{% endhighlight %}

input the following values:

{% highlight shell-session %}
Define value for property 'groupId': org.osgi.enroute.examples.microservice
Define value for property 'artifactId': rest-service
Define value for property 'version' 1.0-SNAPSHOT: : 0.0.1-SNAPSHOT
Define value for property 'package' org.osgi.enroute.examples.microservice: :
Confirm properties configuration:
groupId: org.osgi.enroute.examples.microservice
artifactId: rest-service
version: 0.0.1-SNAPSHOT
package: org.osgi.enroute.examples.microservice
Y: :
{% endhighlight %}

Now add the following module dependencies to the `<dependencies>` section in `rest-service/pom.xml`

{% highlight xml %}
<dependency>
    <groupId>org.apache.servicemix.specs</groupId>
    <artifactId>org.apache.servicemix.specs.json-api-1.1</artifactId>
    <version>2.9.0</version>
</dependency>
<dependency>
    <groupId>org.osgi.enroute.examples.microservice</groupId>
    <artifactId>dao-api</artifactId>
    <version>0.0.1-SNAPSHOT</version>
</dependency>
<dependency>
    <groupId>org.apache.johnzon</groupId>
    <artifactId>johnzon-core</artifactId>
    <version>1.1.0</version>
</dependency>
{% endhighlight %}

* Delete the itemplate files in `rest-service/src/main/java/org/osgi/enroute/examples/microservice` as these are not required. 
* Now create the directory `rest-service/src/main/java/org/osgi/enroute/examples/microservice/rest` into which we create the following two files: 

**rest-service/src/main/java/org/osgi/enroute/examples/microservice/rest/RestComponentImpl.java**
{% highlight java %}
package org.osgi.enroute.examples.microservice.rest;
 
import java.util.List;
 
import javax.ws.rs.DELETE;
import javax.ws.rs.GET;
import javax.ws.rs.POST;
import javax.ws.rs.Path;
import javax.ws.rs.PathParam;
import javax.ws.rs.Produces;
import javax.ws.rs.core.MediaType;
 
import org.osgi.enroute.examples.microservice.dao.PersonDao;
import org.osgi.enroute.examples.microservice.dao.dto.PersonDTO;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.http.whiteboard.propertytypes.HttpWhiteboardResource;
import org.osgi.service.jaxrs.whiteboard.propertytypes.JaxrsResource;
import org.osgi.service.jaxrs.whiteboard.propertytypes.RequireJSON;
 
@Component(service=RestComponentImpl.class)
@JaxrsResource
@Path("person")
@Produces(MediaType.APPLICATION_JSON)
@RequireJSON
@HttpWhiteboardResource(pattern="/microservice/*", prefix="static")
public class RestComponentImpl {
     
    @Reference
    private PersonDao personDao;
 
    @GET
    @Path("{person}")
    public PersonDTO getPerson(@PathParam("person") Long personId) {
        return personDao.findByPK(personId);
    }
 
    @GET
    public List<PersonDTO> getPerson() {
        return personDao.select();
    }
 
    @DELETE
    @Path("{person}")
    public boolean deletePerson(@PathParam("person") long personId) {
        personDao.delete(personId);
        return true;
    }
 
    @POST
    public PersonDTO postPerson(PersonDTO person) {
        if (person.personId > 0) {
            personDao.update(person);
            return person;
        }
        else {
            long id = personDao.save(person);
            person.personId = id;
            return person;
        }
    }
}
{% endhighlight %}

**rest-service/src/main/java/org/osgi/enroute/examples/microservice/rest/JsonpConvertingPlugin.java**
{% highlight java %}
package org.osgi.enroute.examples.microservice.rest;
 
import static javax.ws.rs.core.MediaType.APPLICATION_JSON;
import static javax.ws.rs.core.MediaType.APPLICATION_JSON_TYPE;
import static org.osgi.service.component.annotations.ServiceScope.PROTOTYPE;
import static org.osgi.util.converter.ConverterFunction.CANNOT_HANDLE;
 
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.lang.annotation.Annotation;
import java.lang.reflect.Type;
import java.math.BigDecimal;
import java.math.BigInteger;
import java.util.Collection;
import java.util.List;
import java.util.Map;
 
import javax.json.Json;
import javax.json.JsonArray;
import javax.json.JsonArrayBuilder;
import javax.json.JsonNumber;
import javax.json.JsonObject;
import javax.json.JsonObjectBuilder;
import javax.json.JsonReader;
import javax.json.JsonString;
import javax.json.JsonStructure;
import javax.json.JsonValue;
import javax.json.JsonValue.ValueType;
import javax.json.JsonWriter;
import javax.ws.rs.WebApplicationException;
import javax.ws.rs.core.MediaType;
import javax.ws.rs.core.MultivaluedMap;
import javax.ws.rs.ext.MessageBodyReader;
import javax.ws.rs.ext.MessageBodyWriter;
 
import org.osgi.service.component.annotations.Component;
import org.osgi.service.jaxrs.whiteboard.propertytypes.JaxrsExtension;
import org.osgi.service.jaxrs.whiteboard.propertytypes.JaxrsMediaType;
import org.osgi.util.converter.Converter;
import org.osgi.util.converter.Converters;
import org.osgi.util.converter.TypeReference;
 
@Component(scope = PROTOTYPE)
@JaxrsExtension
@JaxrsMediaType(APPLICATION_JSON)
public class JsonpConvertingPlugin<T> implements MessageBodyReader<T>, MessageBodyWriter<T> {
 
    private final Converter converter = Converters.newConverterBuilder()
            .rule(JsonValue.class, this::toJsonValue)
            .rule(this::toScalar)
            .build();
 
    private JsonValue toJsonValue(Object value, Type targetType) {
        if (value == null) {
           return JsonValue.NULL;
        } else if (value instanceof String) {
            return Json.createValue(value.toString());
        } else if (value instanceof Boolean) {
            return ((Boolean) value) ? JsonValue.TRUE : JsonValue.FALSE;
        } else if (value instanceof Number) {
            Number n = (Number) value;
            if (value instanceof Float || value instanceof Double) {
                return Json.createValue(n.doubleValue());
            } else if (value instanceof BigDecimal) {
                return Json.createValue((BigDecimal) value);
            } else if (value instanceof BigInteger) {
                return Json.createValue((BigInteger) value);
            } else {
                return Json.createValue(n.longValue());
            }
        } else if (value instanceof Collection || value.getClass().isArray()) {
            return toJsonArray(value);
        } else {
            return toJsonObject(value);
        }
    }
 
    private JsonArray toJsonArray(Object o) {
        List<?> l = converter.convert(o).to(List.class);
     
        JsonArrayBuilder builder = Json.createArrayBuilder();
        l.forEach(v -> builder.add(toJsonValue(v, JsonValue.class)));
        return builder.build();
    }
 
    private JsonObject toJsonObject(Object o) {
 
        Map<String, Object> m = converter.convert(o).to(new TypeReference<Map<String, Object>>(){});
 
        JsonObjectBuilder jsonBuilder = Json.createObjectBuilder();
        m.entrySet().stream().forEach(e -> jsonBuilder.add(e.getKey(), toJsonValue(e.getValue(), JsonValue.class)));
        return jsonBuilder.build();
    }
 
    private Object toScalar(Object o, Type t) {
 
        if (o instanceof JsonNumber) {
            JsonNumber jn = (JsonNumber) o;
            return converter.convert(jn.bigDecimalValue()).to(t);
        } else if (o instanceof JsonString) {
            JsonString js = (JsonString) o;
            return converter.convert(js.getString()).to(t);
        } else if (o instanceof JsonValue) {
            JsonValue jv = (JsonValue) o;
            if (jv.getValueType() == ValueType.NULL) {
                return null;
            } else if (jv.getValueType() == ValueType.TRUE) {
                return converter.convert(Boolean.TRUE).to(t);
            } else if (jv.getValueType() == ValueType.FALSE) {
                return converter.convert(Boolean.FALSE).to(t);
            }
        }
        return CANNOT_HANDLE;
    }
 
    @Override
    public boolean isWriteable(Class<?> c, Type t, Annotation[] a, MediaType mediaType) {
        return APPLICATION_JSON_TYPE.isCompatible(mediaType) || mediaType.getSubtype().endsWith("+json");
    }
 
    @Override
    public boolean isReadable(Class<?> c, Type t, Annotation[] a, MediaType mediaType) {
        return APPLICATION_JSON_TYPE.isCompatible(mediaType) || mediaType.getSubtype().endsWith("+json");
    }
 
    @Override
    public void writeTo(T o, Class<?> arg1, Type arg2, Annotation[] arg3, MediaType arg4,
            MultivaluedMap<String, java.lang.Object> arg5, OutputStream out)
            throws IOException, WebApplicationException {
 
        JsonValue jv = converter.convert(o).to(JsonValue.class);
 
        try (JsonWriter jw = Json.createWriter(out)) {
            jw.write(jv);
        }
    }
 
    @SuppressWarnings("unchecked")
    @Override
    public T readFrom(Class<T> arg0, Type arg1, Annotation[] arg2, MediaType arg3, MultivaluedMap<String, String> arg4,
            InputStream in) throws IOException, WebApplicationException {
 
        try (JsonReader jr = Json.createReader(in)) {
            JsonStructure read = jr.read();
            return (T) converter.convert(read).to(arg1);
        }
    }
}
{% endhighlight %}

We now create the directory `rest-service/src/main/resources/static/main/html` in which we create the following file:

**rest-service/src/main/resources/static/main/html/person.html**
{% highlight html %}
<link rel="import" href="https://polygit.org/polymer+v2.2.0/components/iron-ajax/iron-ajax.html">
<link rel="import" href="https://polygit.org/polymer+v2.2.0/components/iron-input/iron-input.html">
<link rel="import" href="https://polygit.org/polymer+v2.2.0/components/polymer/polymer-element.html">
 
<dom-module id="person-app">
  <template>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
    <iron-ajax id="xhr" handle-as="json" content-type="application/json"></iron-ajax>
 
    <div>
      <div class="list-group">
        <template is="dom-repeat" items="[[data]]">
          <div class="list-group-item">
            <a href="#" on-click="selectPerson" class="[[item == selected ? 'active' : '']]">
              <div>First name: <span>[[item.firstName]]</span></div>
              <div>Last name: <span>[[item.lastName]]</span></div>
            </a>
            <button type="button" class="btn btn-danger" on-click="deletePerson">Delete</button>
          </div>
        </template>
      </div>
      <div>
        <template is="dom-if" if="[[selected]]">
          <h4>Addresses for [[selected.firstName]] [[selected.lastName]]:</h4>
          <ul class="list-group">
            <template is="dom-repeat" items="[[selected.addresses]]" as="address">
              <li class="list-group-item">
                <ul>
                  <li>Email: <span>[[address.emailAddress]]</span></li>
                  <li>City: <span>[[address.city]]</span></li>
                  <li>Country: <span>[[address.country]]</span></li>
                </ul>
              </li>
            </template>
          </ul>
        </template>
      </div>
    </div>
 
    <div>
      <h4>Add a Person</h4>
      <input type="text" class="form-control" id="fName" placeholder="First Name">
      <input type="text" class="form-control" id="lName" placeholder="Last Name">
      <h5>Addresses</h5>
      <button class="btn btn-default" type="button" on-click="addAddress">+</button>
      <template is="dom-repeat" items="[[addresses]]" as="address">
        <div class="input-group">
          <input type="email" class="form-control" id="[[address]]-email" placeholder="Email">
          <input type="text" class="form-control" id="[[address]]-city" placeholder="City">
          <input type="text" class="form-control" id="[[address]]-country" placeholder="Country">
        </div>
      </template>
      <button type="submit" class="btn btn-default" on-click="addPerson">Add</button>
      </form>
    </div>
 
  </template>
  <script>
    class Person extends Polymer.Element {
      static get is() {
        return 'person-app';
      }
       
      static get properties() {
        return {
          selected: {
            type: Object
          },
          addresses: {
            type: Array,
            value: []
          },
          data: {
            type: Array,
            value: []
          }
        }
      }
       
      ready() {
        super.ready();
        this.getData(this);
      }
       
      getData(self) {
        this.$.xhr.url = this.ownerDocument.location.origin + '/person';
        this.$.xhr.method = 'GET';
        this.$.xhr.body = null;
        this.$.xhr.generateRequest().completes.then(function(request) { self.data = request.response;});
      }
 
      selectPerson(event) {
        this.selected = event.model.item;
      }
 
      deletePerson(event) {
       
        if(this.selected && this.selected.personId == event.model.item.personId) {
          selected = null;
        }
         
        var self = this;
 
        this.$.xhr.url = this.ownerDocument.location.origin + '/person/' + event.model.item.personId;
        this.$.xhr.method = 'DELETE';
        this.$.xhr.body = null;
        this.$.xhr.generateRequest().completes.then(function() { self.getData(self); });
      }
       
      addAddress() {
        this.push('addresses', 'address' + this.addresses.length);
      }
       
      addPerson() {
        var person = { firstName: this.$.fName.value, lastName: this.$.lName.value, addresses: []};
         
        var shadowRoot = this.shadowRoot;
         
        this.addresses.forEach(function(id) {
            person.addresses.push(
              {
                emailAddress: shadowRoot.getElementById(id + '-email').value,
                city: shadowRoot.getElementById(id + '-city').value,
                country: shadowRoot.getElementById(id + '-country').value
              });
          });
           
        this.addresses = [];
        this.$.fName.value = null;
        this.$.lName.value = null;
       
        var self = this;
       
        this.$.xhr.url = this.ownerDocument.location.origin + '/person/';
        this.$.xhr.method = 'POST';
        this.$.xhr.body = person;
        this.$.xhr.generateRequest().completes.then(function() { self.getData(self); });
      }
    }
    customElements.define(Person.is, Person);
  </script>
</dom-module>
{% endhighlight %}  

And we create the `rest-service/src/main/resources/static/css` directory for the `style.css` file

**rest-service/src/main/resources/static/css/style.css**
{% highlight css %}
/*
    osgi.enroute.examples.component Style Sheet
*/
 
/* Space out content a bit */
body {
  padding-top: 20px;
  padding-bottom: 20px;
}
 
/* Everything but the jumbotron gets side spacing for mobile first views */
.header,
.marketing,
.footer {
  padding-right: 15px;
  padding-left: 15px;
}
 
/* Custom page header */
.header {
  border-bottom: 1px solid #e5e5e5;
}
/* Make the masthead heading the same height as the navigation */
.header h3 {
  padding-bottom: 19px;
  margin-top: 0;
  margin-bottom: 0;
  line-height: 40px;
}
 
/* Custom page footer */
.footer {
  padding-top: 19px;
  color: #777;
  border-top: 1px solid #e5e5e5;
}
 
/* Customize container */
@media (min-width: 768px) {
  .container {
    max-width: 730px;
  }
}
.container-narrow > hr {
  margin: 30px 0;
}
 
/* Main marketing message and sign up button */
.jumbotron {
  text-align: center;
  border-bottom: 1px solid #e5e5e5;
}
.jumbotron .btn {
  padding: 14px 24px;
  font-size: 21px;
}
 
/* Supporting marketing content */
.marketing {
  margin: 40px 0;
}
.marketing p + h4 {
  margin-top: 28px;
}
 
/* Responsive: Portrait tablets and up */
@media screen and (min-width: 768px) {
  /* Remove the padding we set earlier */
  .header,
  .marketing,
  .footer {
    padding-right: 0;
    padding-left: 0;
  }
  /* Space out the masthead */
  .header {
    margin-bottom: 30px;
  }
  /* Remove the bottom border on the jumbotron for visual effect */
  .jumbotron {
    border-bottom: 0;
  }
}
{% endhighlight %}

Also in directory `rest-service/src/main/resources/static` add the following `index.html`

**rest-service/src/main/resources/static/index.html**
{% highlight html %}
<!DOCTYPE html>
<html lang="en">
 
<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>OSGI enRoute quickstart example</title>
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
  <link rel="import" href="main/html/person.html">
</head>
 
<body>
  <div class="container">
 
    <div class="row">
      <div class="col-md-1">
        <img src="main/img/enroute-logo-64.png" class="img-responsive"/>
      </div>
      <div class="col-md-3">
        <h2> OSGi enRoute</h2>
      </div>
    </div>
    <div class="row center-block">
      <h3 class="text-muted">The Microservice Example</h3>
    </div>
     
    <div class="row center-block">
      <person-app></person-app>
    </div>
 
    <div class="row">
      <p>&copy; OSGi Alliance 2017</p>
    </div>
  </div>
 
  <script src="https://polygit.org/components/webcomponentsjs/webcomponents-loader.js"></script>
</body>
 
</html>
{% endhighlight %}

## The Composite Application 

Check that you are in the `microservice` project directory, then ...

**To create the `application` module**

{% highlight shell-session %}
mvn -s ../settings.xml archetype:generate -DarchetypeGroupId=org.osgi.enroute.archetype -DarchetypeArtifactId=application -DarchetypeVersion=7.0.0-SNAPSHOT
{% endhighlight %}

input the following values:

{% highlight shell-session %}
Define value for property 'groupId': org.osgi.enroute.examples.microservice
Define value for property 'artifactId': rest-app
Define value for property 'version' 1.0-SNAPSHOT: : 0.0.1-SNAPSHOT
Define value for property 'package' org.osgi.enroute.examples.microservice: :
Define value for property 'impl-artifactId': dao-impl
Define value for property 'impl-groupId' org.osgi.enroute.examples.microservice: :
Define value for property 'impl-version' 0.0.1-SNAPSHOT: :
Confirm properties configuration:
groupId: org.osgi.enroute.examples.microservice
artifactId: rest-app
version: 0.0.1-SNAPSHOT
package: org.osgi.enroute.examples.microservice
impl-artifactId: dao-impl
impl-groupId: org.osgi.enroute.examples.microservice
impl-version: 0.0.1-SNAPSHOT
Y: :
{% endhighlight %}

Add the following dependencies inside `<dependencies>` section of the file `rest-app/pom.xml`

{% highlight xml %}
<dependency>
    <groupId>org.osgi.enroute</groupId>
    <artifactId>osgi-api</artifactId>
    <type>pom</type>
</dependency>
<dependency>
    <groupId>org.osgi.enroute.examples.microservice</groupId>
    <artifactId>rest-service</artifactId>
    <version>0.0.1-SNAPSHOT</version>
</dependency>
<dependency>
    <groupId>org.apache.johnzon</groupId>
    <artifactId>johnzon-core</artifactId>
    <version>1.1.0</version>
</dependency>
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <version>1.4.196</version>
    <scope>runtime</scope>
</dependency>
{% endhighlight %}

Also add the following plugin inside `<plugins>` section in the file `rest-app/pom.xml`

{% highlight xml %}
<plugin>
    <groupId>biz.aQute.bnd</groupId>
    <artifactId>bnd-maven-plugin</artifactId>
</plugin>
{% endhighlight %}

Create the directory `rest-app/src/main/java/config` in which 

**rest-app/src/main/java/config/package-info.java**
{% highlight java %}
@org.osgi.annotation.bundle.Requirement(namespace="osgi.extender", name="osgi.configurator", version="1")
package config;
{% endhighlight %}

Create the directory `rest-app/src/main/resources/OSGI-INF/configurator` and then the following file 

**rest-app/src/main/resources/OSGI-INF/configurator/configuration.json**
{% highlight java %}
{
    // Global Settings
    ":configurator:resource-version" : 1,
    ":configurator:symbolicname" : "org.osgi.enroute.examples.microservice.config",
    ":configurator:version" : "0.0.1.SNAPSHOT",
     
     
    // Configure a JDBC resource provider
    "org.apache.aries.tx.control.jdbc.xa~microservice": {
           "name": "microservice.database",
           "osgi.jdbc.driver.class": "org.h2.Driver",
           "url": "jdbc:h2:./data/database" },
     
    // Target the Dao impls at the provider we configured
    "org.osgi.enroute.examples.microservice.dao.impl.PersonDaoImpl": {
           "provider.target": "(name=microservice.database)" },
    "org.osgi.enroute.examples.microservice.dao.impl.AddressDaoImpl": {
           "provider.target": "(name=microservice.database)" }
}
{% endhighlight %}

Copy the contents below over `rest-app/rest-app.bndrun`

**rest-app/rest-app.bndrun**
{% highlight shell-session %}
index: target/index.xml
 
-standalone: ${index}
 
-resolve.effective: active
 
-runrequires: \
    osgi.identity;filter:='(osgi.identity=org.osgi.enroute.examples.microservice.rest-service)',\
    osgi.identity;filter:='(osgi.identity=org.apache.johnzon.core)',\
    osgi.identity;filter:='(osgi.identity=org.h2)',\
    bnd.identity;version='0.0.1.201801031655';id='org.osgi.enroute.examples.microservice.rest-app'
-runfw: org.apache.felix.framework
-runee: JavaSE-1.8
{% endhighlight %}

## Build & Run

Build the modules and install in local maven repository from the top level project directory

    mvn install

We now Generate OSGi™ index with the project dependencies from the top level project directory

    mvn bnd-resolver:resolve

And generate the runnable jar from the top level project directory

    mvn package

To run the resultant OSGi based REST Microservice change back to the top level project directory and run

    java -jar rest-app/target/rest-app.jar

The REST service can be seen by pointing a browser to [http://localhost:8080/microservice/index.html](http://localhost:8080/microservice/index.html)

Stop the application using Ctrl+C in the console.




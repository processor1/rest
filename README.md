# rest

Restful application development with jax-rs ,json(jsonp-jsonb) ,writing restul api application in java ee or jakarta ee is very easy ,developing complex application is easy ,you can develop a whole application just by clicking (using netbeans ide).
Java EE 8 add jsonp and jsonb (manipuating json data) you can convert java object to or from json object .

When it comes to restful api application development or implementation in java ee the implementation can be done in many ways: you can use Spring, JAX-RS, or you might just write your own bare servlets if you're good and brave enough. All you need is the ability to expose HTTP methods – the rest is all about how you organize them and how you guide the client when making calls to your API.

JAX-RS is nothing more than a specification, a set of interfaces and annotations offered by Java EE. And then, of course, we have the implementations; some of the more well known are #RESTEasy and #Jersey.

Also, if you ever decide to build a JEE-compliant application server, the guys from Oracle will tell you that, among many other things, your server should provide a JAX-RS implementation for the deployed apps to use. That's why it's called Java Enterprise Edition Platform.

Before deciding on a server to use, we should see what JAX-RS implementation (name, vendor, version and known bugs) it provides, at least for Production environments. For instance, #Glassfish comes with #Jersey, while #Wildfly or Jboss come with #RESTEasy.


If you want to start playing with JAX-RS, the shortest path is: have a Maven webapp project with the following dependency in the pom.xml:

<dependency>
    <groupId>javax</groupId>
    <artifactId>javaee-api</artifactId>
    <version>7.0</version>
    <scope>provided</scope>
</dependency>


After the dependency is added we first have to write the entry class: an empty class which extends javax.ws.rs.core.Application and is annotated with javax.ws.rs.ApplicationPath:

package org.config;

import javax.ws.rs.ApplicationPath;
import java.util.Set;
import java.util.HashSet;

@ApplicationPath("/api")
public class RestApplication extends Application {

@Override public Set<Class<?>> getClasses(){
Set<Class<?>> resourceClasses=new HashSet<Class<?>>();
  resourceClasses.add(BookRest.class);
  resourceClasses.add(AudioRest.class);
  resouceClasses.add(PersonRest.class);
  resourceClasses.add(Notification.class);
  return resouceClasses;
}


@Path("/notifications")
public class NotificationsResource {
    @GET
    @Path("/ping")
    public Response ping() {
        return Response.ok().entity("Service online").build();
    }
 
    @GET
    @Path("/get/{id}")
    @Produces(MediaType.APPLICATION_JSON)
    public Response getNotification(@PathParam("id") int id) {
        return Response.ok()
          .entity(new Notification(id, "john", "test notification"))
          .build();
    }
 
    @POST
    @Path("/post/")
    @Consumes(MediaType.APPLICATION_JSON)
    @Produces(MediaType.APPLICATION_JSON)
    public Response postNotification(Notification notification) {
        return Response.status(201).entity(notification).build();
    }
}



When it comes to testing the  rest endpoints you can use postman  tool or curl .
curl http://localhost:8080/simple-jaxrs-ex/api/notifications/ping/
 
curl http://localhost:8080/simple-jaxrs-ex/api/notifications/get/1
 
curl -X POST -d '{"id":23,"text":"lorem ipsum","username":"johana"}'
http://localhost:8080/simple-jaxrs-ex/api/notifications/post/ --header "Content-Type:application/json"


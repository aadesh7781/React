3
HelloWorld.java
package helloWorld;

import javax.ws.rs.GET;
import javax.ws.rs.PUT;
import javax.ws.rs.Path;
import javax.ws.rs.Produces;
import javax.ws.rs.Consumes;
import javax.ws.rs.QueryParam;
import javax.ws.rs.PathParam;
import javax.ws.rs.core.MediaType;
import javax.ws.rs.core.Response;

@Path("helloworld")

public class HelloWorld {

    @GET
    @Produces("text/html")
    public String getHtml() {

        return "<html><body><h1>Hello World!</h1></body></html>";

    }

    @PUT
    @Path("{id}")
    @Consumes(MediaType.APPLICATION_JSON)
    @Produces(MediaType.TEXT_PLAIN)

    public Response updateMessage(@PathParam("id") String id, String message) {

        String result = "Updated message ID: " + id + " with " + message;

        return Response.ok(result).build();
    }

    @GET
    @Path("Upper")
    @Produces(MediaType.TEXT_PLAIN)

    public String toUpperCase(@QueryParam("text") String Text){

        return Text.toUpperCase();

    }

    @GET
    @Path("Lower")
    @Produces(MediaType.TEXT_PLAIN)

    public String toLowerCase(@QueryParam("text") String Text){

        return Text.toLowerCase();

    }

}

prac4
restclient.java
import javax.ws.rs.client.Client;
import javax.ws.rs.client.ClientBuilder;
import javax.ws.rs.client.WebTarget;
import javax.ws.rs.core.MediaType;

public class restclient {

    public static void main(String args[])
    {

        Client c = ClientBuilder.newClient();

        WebTarget t =
        c.target("http://localhost:8080/RestDemo/webresources/helloworld/");

        String res = t.request(MediaType.TEXT_HTML).get(String.class);

        System.out.println("Response " + res);

        c.close();
    }
}

NewClass.java
import javax.ws.rs.client.Client;
import javax.ws.rs.client.ClientBuilder;
import javax.ws.rs.client.WebTarget;
import javax.ws.rs.core.MediaType;

public class NewClass{

    public static void main(String args[])
    {

        Client c = ClientBuilder.newClient();

        WebTarget t =
        c.target("http://localhost:8080/RestDemo/webresources/helloworld/Lower")
        .queryParam("text", "HELLO STUDENTS");

        WebTarget m =
        c.target("http://localhost:8080/RestDemo/webresources/helloworld/Upper")
        .queryParam("text", "hello students");

        String res = t.request(MediaType.TEXT_PLAIN).get(String.class);
        String result = m.request(MediaType.TEXT_PLAIN).get(String.class);

        System.out.println("Response " + res);
        System.out.println("Response " + result);

        c.close();
    }
}
6 
Practical 6 — JAX-RS Microservice using Docker
Step 1 — Create Project

Open NetBeans

File → New Project
Java Web → Web Application
Project Name → microservice-jaxrs
Finish
Step 2 — Create REST Resource

Right click project

New → Restful Web Services from Patterns

Select

Simple Root Resource

Fill:

Resource Package : com.example
Path : microservice
Class Name : MicroserviceResource
MIME type : text/plain

Click Finish. 

prac6_016

Step 3 — Write Code

Open

MicroserviceResource.java

Paste full code:

package com.example;

import javax.ws.rs.core.Context;
import javax.ws.rs.core.UriInfo;
import javax.ws.rs.PathParam;
import javax.ws.rs.Consumes;
import javax.ws.rs.PUT;
import javax.ws.rs.Path;
import javax.ws.rs.GET;
import javax.ws.rs.Produces;
import javax.ws.rs.core.MediaType;

@Path("microservice")

public class MicroserviceResource {

    @Context
    private UriInfo context;

    public MicroserviceResource() {
    }

    @GET
    @Produces("text/plain")
    public String hello() {
        return "Jax-Rs Microservice -DockerReady";
    }

    @GET
    @Path("status")
    @Produces(MediaType.APPLICATION_JSON)
    public String status() {
        return "{\"status\":\"healthy\",\"service\":\"microservice\"}";
    }

    @PUT
    @Consumes("text/plain")
    public void putText(String content) {
    }
}
Step 4 — Build Project

Right click project:

Clean and Build

Ye create karega:

dist/microservice-jaxrs.war

WAR file ek web archive package hota hai jisme dependencies hoti hain. 

prac6_016

Step 5 — Create Dockerfile

Project folder me create karo:

Dockerfile

Paste:

FROM tomcat:8.5-jdk8

COPY dist/microservice-jaxrs.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080

CMD ["catalina.sh","run"]

Ye Tomcat container me WAR deploy karega. 

prac6_016

Step 6 — Build Docker Image

Open CMD in project folder.

Run:

docker build -t jaxrs-microservice .

Image create ho jayega.

Step 7 — Check Image
docker images

You should see:

jaxrs-microservice
Step 8 — Run Container

Run:

docker run -p 8080:8080 jaxrs-microservice

Container start ho jayega.

Step 9 — Test Microservice

Open browser:

http://localhost:8080/microservice

Output:

Jax-Rs Microservice -DockerReady
Status API
http://localhost:8080/microservice/status

Output:

{"status":"healthy","service":"microservice"}
Quick Commands (Exam Fast Version)
docker build -t jaxrs-microservice .
docker run -p 8080:8080 jaxrs-microservice

7
Practical 7 — Library REST API
Step 1 — Create Project

Open NetBeans

File → New Project
Java Web → Web Application
Project Name → LibraryREST
Finish
Step 2 — Create REST Resource

Right click project

New → RESTful Web Services from Patterns

Select

Simple Root Resource

Fill:

Resource Package : com.library.rest
Path : books
Class Name : LibraryResource
MIME Type : application/json

Click Finish. 

prac7_016

Step 3 — Create Book Model Class

Right click Source Packages

New → Java Class

Name:

Book

Package:

com.library.model
Book.java Code
package com.library.model;

public class Book {

    private long id;
    private String title;
    private String isbn;

    public Book(){
    }

    public Book(long id, String title, String isbn){
        this.id = id;
        this.title = title;
        this.isbn = isbn;
    }

    public long getId(){
        return id;
    }

    public void setId(long id){
        this.id = id;
    }

    public String getTitle(){
        return title;
    }

    public void setTitle(String title){
        this.title = title;
    }

    public String getIsbn(){
        return isbn;
    }

    public void setIsbn(String isbn){
        this.isbn = isbn;
    }
}
Step 4 — Write REST Service

Open

LibraryResource.java

Paste this code.

LibraryResource.java
package com.library.rest;

import javax.ws.rs.PathParam;
import javax.ws.rs.Consumes;
import javax.ws.rs.PUT;
import javax.ws.rs.Path;
import javax.ws.rs.GET;
import javax.ws.rs.Produces;
import java.util.List;
import java.util.ArrayList;
import com.library.model.Book;
import javax.ws.rs.core.MediaType;
import javax.ws.rs.POST;
import javax.ws.rs.DELETE;

@Path("books")

public class LibraryResource {

    private static List<Book> bookList = new ArrayList<>();

    static{
        bookList.add(new Book(1,"Java Programming","ISBN101"));
        bookList.add(new Book(2,"Web Services","ISBN102"));
        bookList.add(new Book(3,"REST API","ISBN103"));
    }

    @GET
    @Produces(MediaType.APPLICATION_JSON)
    public List<Book> getAllBooks(){
        return bookList;
    }

    @GET
    @Path("{id}")
    @Produces(MediaType.APPLICATION_JSON)
    public Book getBookById(@PathParam("id") long id){
        for (Book b : bookList){
            if (b.getId() == id){
                return b;
            }
        }
        return null;
    }

    @POST
    @Consumes(MediaType.APPLICATION_JSON)
    public String addBook(Book book){
        bookList.add(book);
        return "Book added successfully";
    }

    @PUT
    @Path("{id}")
    @Consumes(MediaType.APPLICATION_JSON)
    public String updateBook(@PathParam("id") long id, Book updatedBook){
        for (Book b : bookList){
            if (b.getId() == id){
                b.setTitle(updatedBook.getTitle());
                b.setIsbn(updatedBook.getIsbn());
                return "Book updated successfully";
            }
        }
        return "Book not found";
    }

    @DELETE
    @Path("{id}")
    public String deleteBook(@PathParam("id") long id){
        for (Book b : bookList){
            if (b.getId() == id){
                bookList.remove(b);
                return "Book deleted successfully";
            }
        }
        return "Book not found";
    }
}
Step 5 — ApplicationConfig

Create class

ApplicationConfig.java

Code:

package com.library.rest;

import javax.ws.rs.core.Application;
import javax.ws.rs.ApplicationPath;

@ApplicationPath("/api")

public class ApplicationConfig extends Application{
}

This defines REST base URL. 

prac7_016

Step 6 — Run Project
Right click Project → Clean and Build
Right click Project → Deploy
Right click Project → Test RESTful Web Services
Example URLs
Get all books
http://localhost:8080/LibraryREST/api/books

Output:

[
 {"id":1,"title":"Java Programming","isbn":"ISBN101"},
 {"id":2,"title":"Web Services","isbn":"ISBN102"},
 {"id":3,"title":"REST API","isbn":"ISBN103"}
]
Get book by id
http://localhost:8080/LibraryREST/api/books/3

Output:

{"id":3,"title":"REST API","isbn":"ISBN103"}

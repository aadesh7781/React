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

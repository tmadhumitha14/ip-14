# ip-14
client side:
package javaapplication27;
import d1.NewWebService_Service;
import d1.NewWebService;
public class JavaApplication27 {
public static void main(String[] args) {
        String id = "1"; // Example ID value
                System.out.println(pro(id));
    }
    private static String pro(String id) {
        // Instantiate the web service client
        d1.NewWebService_Service service = new d1.NewWebService_Service();
        d1.NewWebService port = service.getNewWebServicePort();
        return port.pro(id);
    }
}

Serverside:
package demo;
import java.sql.*;
import javax.jws.WebService;
import javax.jws.WebMethod;
import javax.jws.WebParam;
@WebService(serviceName = "NewWebService")
public class NewWebService {
    @WebMethod(operationName = "hello")
    public String hello(@WebParam(name = "name") String txt) {
        return "Hello " + txt + " !";
    }
    @WebMethod(operationName = "pro")
    public String pro(@WebParam(name = "id") String id) {
        Connection connection = null;
        try {
            connection = DriverManager.getConnection("jdbc:derby://localhost:1527/products");
            int productId = Integer.parseInt(id);
            PreparedStatement stmt = connection.prepareStatement("SELECT discription FROM product WHERE id = ?");
            stmt.setInt(1, productId);
            ResultSet rs = stmt.executeQuery();
            if (rs.next()) {
                String productDescription = rs.getString("discription");
                rs.close();
                stmt.close();
                return productDescription;
            } else {
                return "no data found";
            }
        } catch (SQLException | NumberFormatException e) {
            e.printStackTrace();
            return "An error occurred while processing the request";
        } finally {
            if (connection != null) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }}}}}

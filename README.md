index 

<!DOCTYPE html>
<html>
<head>
<title>Registration Event</title>
</head>
<body bgcolor="wheat">
<h2>REGISTRATION FOR AN EVENT</h2>
<form action="./register" method="get">
<table>
<tr><td>NAME</td><td><input type="text" name="name" id="name"></td></tr>
<tr><td>MOBILE</td><td><input type="text" name="phone" id="phone"></td></tr>
<tr><td>EMAIL</td><td><input type="email" name="email" id="email"></td></tr>
<tr>
<td>ATTENDEES</td>
<td>
<select name="attendees no" id="attendees no">
<option value="1">ONE PERSON</option>
<option value="2">TWO PERSON</option>
<option value="3">THREE PERSON</option>
<option value="4">FOUR PERSON</option>
</select>
</td>
</tr>
<tr><td><input type="submit" value="Register"></td></tr>
</table>
</form>
</body>
</html>

register.java 

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.Statement;

public class Registration extends HttpServlet {

    private static final long serialVersionUID = 1L;

    public Registration() {
        super();
    }

    protected void doGet(HttpServletRequest request,
                         HttpServletResponse response)
            throws ServletException, IOException {

        PrintWriter out = response.getWriter();

        String name = request.getParameter("name");
        String Phone = request.getParameter("phone");
        String email = request.getParameter("email");
        String attendeesno = request.getParameter("attendees no");

        Connection con = null;
        Statement s = null;

        try {
            Class.forName("com.mysql.cj.jdbc.Driver");

            con = DriverManager.getConnection(
                "jdbc:mysql://localhost:3306/realdb",
                "root", "pnxg"
            );

            s = con.createStatement();

            int result = s.executeUpdate(
                "insert into event values('" + name + "','" + Phone +
                "','" + email + "','" + attendeesno + "')"
            );

            out.println("<html><body bgcolor='wheat'><center><h1>");

            if (result != 0)
                out.println(name + "You are successfully registered");
            else
                out.println(name + "You are unable to register, please fill form correctly");

            out.println("</h1></center></body></html>");

            s.close();
            con.close();

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    protected void doPost(HttpServletRequest request,
                          HttpServletResponse response)
            throws ServletException, IOException {
        doGet(request, response);
    }
}

Sure, I can provide you with an example of a Java web application using the Model-View-Controller (MVC) pattern for adding two numbers. Here is a basic example:

Model:

The model represents the data and business logic of the application. In this case, the model will contain a method for adding two numbers.
```
public class CalculatorModel {
    public int addNumbers(int num1, int num2) {
        return num1 + num2;
    }
}

```
Controller:

The controller is responsible for processing user input, interacting with the model to update data, and returning the appropriate view. In this case, the controller will receive the user input (the two numbers to be added) from the view, pass them to the model to perform the addition, and then return the result to the view.
```
public class CalculatorController {
    private CalculatorModel model;
    public CalculatorController(CalculatorModel model) {
        this.model = model;
    }
    public int addNumbers(int num1, int num2) {
        return model.addNumbers(num1, num2);
    }
}

```
View:

The view is responsible for displaying the user interface and taking input from the user. In this case, the view will have two input fields for the user to enter the two numbers to be added and a button to submit the form. When the user submits the form, the controller will be called to perform the addition and return the result, which will then be displayed on the page.
```
<html>
    <head>
        <title>Calculator</title>
    </head>
    <body>
        <h1>Addition</h1>
        <form method="post">
            <input type="text" name="num1" placeholder="Enter first number" />
            <input type="text" name="num2" placeholder="Enter second number" />
            <input type="submit" value="Add" />
        </form>
        <%
            if (request.getMethod().equals("POST")) {
                int num1 = Integer.parseInt(request.getParameter("num1"));
                int num2 = Integer.parseInt(request.getParameter("num2"));
                CalculatorModel model = new CalculatorModel();
                CalculatorController controller = new CalculatorController(model);
                int result = controller.addNumbers(num1, num2);
                out.println("<h2>Result: " + result + "</h2>");
            }
        %>
    </body>
</html>

```
In this example, the view is implemented using JSP (JavaServer Pages), but it could also be implemented using other frameworks such as Thymeleaf, JSF, or Spring MVC.

When the user submits the form, the controller is instantiated and passed the model. The controller then calls the `addNumbers` method on the model with the two numbers entered by the user, and returns the result. The result is then displayed on the page using the `out.println` method.

Note that this is a very basic example and does not include error handling or input validation, which would be necessary in a real-world application.

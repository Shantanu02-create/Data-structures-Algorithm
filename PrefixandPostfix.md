import java.util.Stack;

public class InfixConversion {

    //Function to define precedence
    static int precedence(char ch) {
        switch (ch) {
            case '^': return 3;
            case '*':
            case '/': return 2;
            case '+':
            case '-': return 1;
            default: return -1;
        }
    }

    // Infix -> Postfix
    static String infixToPostfix(String expr) {
        StringBuilder result = new StringBuilder();
        Stack<Character> stack = new Stack<>();

        for (int i = 0; i < expr.length(); i++) {
            char c = expr.charAt(i);

            // Operand
            if (Character.isLetterOrDigit(c)) {
                result.append(c);
            }
            // Opening bracket
            else if (c == '(') {
                stack.push(c);
            }
            // Closing bracket
            else if (c == ')') {
                while (!stack.isEmpty() && stack.peek() != '(') {
                    result.append(stack.pop());
                }
                if (!stack.isEmpty() && stack.peek() == '(')
                    stack.pop();
            }
            // Operator
            else {
                while (!stack.isEmpty() && precedence(stack.peek()) >= precedence(c)) {
                    // Handle right-associativity of ^
                    if (c == '^' && stack.peek() == '^')
                        break;
                    result.append(stack.pop());
                }
                stack.push(c);
            }
        }
        // Pop remaining operators
        while (!stack.isEmpty()) {
            result.append(stack.pop());
        }

        return result.toString();
    }

    // Infix -> Prefix
    static String infixToPrefix(String expr) {
        // 1. Reverse expression with swapped brackets
        StringBuilder rev = new StringBuilder();
        for (int i = expr.length() - 1; i >= 0; i--) {
            char c = expr.charAt(i);
            if (c == '(') rev.append(')');
            else if (c == ')') rev.append('(');
            else rev.append(c);
        }

        // 2. Convert reversed expression to postfix
        String revPostfix = infixToPostfix(rev.toString());

        // 3. Reverse postfix -> Prefix
        return new StringBuilder(revPostfix).reverse().toString();
    }

    // Main
    public static void main(String[] args) {
        String expr1 = "A+B*(C^D-E)";
        String expr2 = "(a+(b*c)/(d-e))";
        String expr3 = "A^B*C/(D*E-F)";

        // Prefix results
        System.out.println("Infix: " + expr1 + " -> Prefix: " + infixToPrefix(expr1));
        System.out.println("Infix: " + expr2 + " -> Prefix: " + infixToPrefix(expr2));

        // Postfix result
        System.out.println("Infix: " + expr3 + " -> Postfix: " + infixToPostfix(expr3));
    }
}

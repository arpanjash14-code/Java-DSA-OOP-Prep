# Week 10 — Java AWT, Swing & Event Handling Practice

## Part A: Predict the Output

### Q1. Button click event

```java
import javax.swing.*;
import java.awt.event.*;

public class Main {
    public static void main(String[] args) {
        JButton button = new JButton("Click");

        button.addActionListener(e ->
            System.out.println("Clicked")
        );

        button.doClick();
    }
}
```

**Answer:**

```text
Clicked
```

**Explanation:** `doClick()` programmatically activates the button, triggering its registered action listeners.

---

### Q2. Lambda expression and `ActionListener`

Which method is implemented by this lambda?

```java
button.addActionListener(e ->
    System.out.println("Hello")
);
```

**Answer:** `actionPerformed(ActionEvent e)`.

**Explanation:** `ActionListener` is a functional interface with one abstract method.

---

### Q3. FlowLayout

```java
import java.awt.*;

public class Main {
    public static void main(String[] args) {
        Frame f = new Frame();

        f.setLayout(new FlowLayout());

        f.add(new Button("A"));
        f.add(new Button("B"));
        f.add(new Button("C"));

        System.out.println(f.getComponentCount());
    }
}
```

**Answer:**

```text
3
```

**Explanation:** Three components were added to the frame. `FlowLayout` controls their placement; it does not change the component count.

---

### Q4. BorderLayout

Which region is used when adding a component without specifying a region to a container using `BorderLayout`?

**Answer:** `BorderLayout.CENTER`.

Example:

```java
frame.setLayout(new BorderLayout());
frame.add(new Button("Hello"));
```

The button is added to the center region.

**Exam trap:** A `BorderLayout` has five regions: `NORTH`, `SOUTH`, `EAST`, `WEST`, and `CENTER`.

---

### Q5. Swing class identification

Which class represents a clickable Swing button?

A. `Button`

B. `JButton`

C. `JLabel`

D. `JFrame`

**Answer:** B. `JButton`.

- `Button` is an AWT component.
- `JButton` is a Swing component.
- `JLabel` displays text or icons.
- `JFrame` represents a top-level Swing window.

---

### Q6. Event source and listener

```java
JButton button = new JButton("Submit");

button.addActionListener(e ->
    System.out.println("Submitted")
);
```

Identify the event source and listener.

**Answer:**
- Event source: `button`
- Listener: the object created by the lambda expression
- Event type: `ActionEvent`
- Listener interface: `ActionListener`

---

## Part B: AWT vs Swing

### Q7. Fill in the blanks

| AWT | Swing |
|---|---|
| `Button` | `JButton` |
| `Label` | `JLabel` |
| `Frame` | `JFrame` |
| `Panel` | `JPanel` |
| `TextField` | `JTextField` |
| `Checkbox` | `JCheckBox` |
| `TextArea` | `JTextArea` |

**Remember:** AWT classes are primarily in `java.awt`; Swing components are primarily in `javax.swing`.

### Q8. True or False

1. Swing is built on top of AWT.
2. All Swing components are heavyweight.
3. AWT stands for Abstract Window Toolkit.
4. `JFrame` is an AWT class.
5. Swing supports pluggable look and feel.
6. `JPanel` is commonly used as a container.

**Answers:**

1. True
2. False
3. True
4. False
5. True
6. True

---

## Part C: Layout Managers

### Q9. Match the layout manager

| Layout manager | Behavior |
|---|---|
| `FlowLayout` | Places components sequentially and wraps when needed |
| `BorderLayout` | Organizes components into five regions |
| `GridLayout` | Arranges components in equal-sized grid cells |
| `GridBagLayout` | Allows flexible grid-based arrangements |

### Q10. GridLayout

```java
setLayout(new GridLayout(2, 3));
```

**Question:** How many rows and columns are specified?

**Answer:** 2 rows and 3 columns, giving 6 cells.

### Q11. Replacing the layout

```java
Panel p = new Panel();
p.setLayout(new FlowLayout());
p.setLayout(new GridLayout(2, 2));
```

**Question:** Which layout manager is active at the end?

**Answer:** `GridLayout`.

**Explanation:** The second call replaces the panel's previous layout manager.

---

## Part D: Event Handling

### Q12. The `ActionListener` interface

Which method must be implemented when a class directly implements `ActionListener`?

**Answer:**

```java
public void actionPerformed(ActionEvent e)
```

Example:

```java
import java.awt.event.*;

class MyListener implements ActionListener {
    public void actionPerformed(ActionEvent e) {
        System.out.println("Event received");
    }
}
```

### Q13. Event delegation model

Name its three main parts.

**Answer:**

1. Event source
2. Event object
3. Event listener

### Q14. MouseAdapter

Why is `MouseAdapter` useful?

**Answer:** `MouseListener` contains multiple abstract methods. `MouseAdapter` provides empty implementations, so a subclass can override only the methods it needs.

Example:

```java
import java.awt.event.*;

class MyMouseListener extends MouseAdapter {
    @Override
    public void mouseClicked(MouseEvent e) {
        System.out.println("Mouse clicked");
    }
}
```

**Exam trap:** Java does not support extending two classes. A class that already extends another class can implement `MouseListener` instead of extending `MouseAdapter`.

### Q15. Functional interfaces

Which of the following can be implemented using a lambda expression?

A. `ActionListener`

B. `Runnable`

C. `MouseListener`

D. Both A and B

**Answer:** D. Both A and B.

`ActionListener` and `Runnable` are functional interfaces. `MouseListener` has multiple abstract methods and cannot be implemented by a single lambda alone.

---

## Part E: Radio Buttons and Checkboxes

### Q16. Multiple selection

Which component is generally appropriate when users can select several independent options?

**Answer:** `JCheckBox`.

### Q17. Mutually exclusive options

Which combination is typically used to let users choose exactly one option among several radio buttons?

**Answer:** Multiple `JRadioButton` components inside a `ButtonGroup`.

Example:

```java
JRadioButton a = new JRadioButton("Option A");
JRadioButton b = new JRadioButton("Option B");

ButtonGroup group = new ButtonGroup();
group.add(a);
group.add(b);
```

**Exam trap:** `ButtonGroup` is not a visible GUI component. It controls selection behavior.

---

## Part F: Write the Programs

### Q18. Create a Swing window

Write a program that creates a window with a label and a button.

**Solution:**

```java
import javax.swing.*;

public class Main {
    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            JFrame frame = new JFrame("My Application");

            JLabel label = new JLabel("Welcome to Java");
            JButton button = new JButton("Click Me");

            JPanel panel = new JPanel();
            panel.add(label);
            panel.add(button);

            frame.add(panel);
            frame.setSize(350, 200);
            frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            frame.setLocationRelativeTo(null);
            frame.setVisible(true);
        });
    }
}
```

### Q19. Handle a button click

Write a program that prints `Button clicked` whenever a Swing button is activated.

**Solution:**

```java
import javax.swing.*;

public class Main {
    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            JFrame frame = new JFrame("Event Handling");
            JButton button = new JButton("Click Me");

            button.addActionListener(e ->
                System.out.println("Button clicked")
            );

            frame.add(button);
            frame.setSize(300, 200);
            frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            frame.setLocationRelativeTo(null);
            frame.setVisible(true);
        });
    }
}
```

### Q20. Use an adapter class

Write a program that prints a message when the user closes an AWT window.

**Solution:**

```java
import java.awt.*;
import java.awt.event.*;

public class Main {
    public static void main(String[] args) {
        Frame frame = new Frame("Window");

        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.out.println("Closing window");
                frame.dispose();
            }
        });

        frame.setSize(300, 200);
        frame.setVisible(true);
    }
}
```

**Why use `WindowAdapter`?** It avoids implementing every method in `WindowListener`.

---

## Part G: NPTEL Quick Test

Try answering these before checking the key.

1. What is the full form of AWT?
2. Which package contains `JButton`?
3. Which package contains `ActionListener`?
4. What is the default layout of an AWT `Panel`?
5. What is the default layout of an AWT `Frame`?
6. Which method handles an action event?
7. Which layout has five regions?
8. Which layout places components in equal-sized cells?
9. Can a lambda implement `ActionListener`?
10. What is the purpose of an adapter class?
11. What does `ButtonGroup` do?
12. What is the Swing Event Dispatch Thread?
13. Why should long-running work not execute on the EDT?
14. Does a button need a `MouseListener` to handle normal button activation?
15. What is the difference between `JFrame` and `JPanel`?

### Answer Key

1. Abstract Window Toolkit.
2. `javax.swing`.
3. `java.awt.event`.
4. `FlowLayout`.
5. `BorderLayout`.
6. `actionPerformed(ActionEvent e)`.
7. `BorderLayout`.
8. `GridLayout`.
9. Yes.
10. It provides empty implementations of listener methods.
11. It groups radio buttons for mutually exclusive selection.
12. The thread that processes Swing events and GUI updates.
13. It can block event processing and make the interface unresponsive.
14. No. `ActionListener` is normally used.
15. `JFrame` is a top-level window; `JPanel` is a container placed inside other containers.

---

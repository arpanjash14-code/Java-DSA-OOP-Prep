# Week 10 — Java AWT, Swing & Event Handling

## 1. What Is GUI Programming?

GUI stands for **Graphical User Interface**. It allows users to interact with a program through windows, buttons, text fields, menus, and other visual components.

Java's traditional GUI libraries include:

- **AWT:** Abstract Window Toolkit
- **Swing:** A GUI toolkit built on top of AWT
- **JavaFX:** A more modern GUI toolkit, separate from Swing and AWT

For traditional Java syllabus questions, focus on AWT, Swing, and the event delegation model.

## 2. AWT (Abstract Window Toolkit)

AWT is Java's original GUI toolkit. Many AWT components use native operating-system peers, so their appearance and behavior can vary by platform.

Common AWT components:

| Component | Purpose |
|---|---|
| `Frame` | Top-level window |
| `Panel` | Container for organizing components |
| `Label` | Displays text |
| `Button` | Clickable button |
| `TextField` | Single-line text input |
| `TextArea` | Multiline text input |
| `Checkbox` | On/off selection |
| `Choice` | Drop-down selection |
| `List` | List of selectable items |
| `Canvas` | Area for custom drawing |

Example:

```java
import java.awt.*;

public class AwtExample {
    public static void main(String[] args) {
        Frame frame = new Frame("My AWT Window");

        Label label = new Label("Hello Java");
        Button button = new Button("Click Me");

        frame.setLayout(new FlowLayout());
        frame.add(label);
        frame.add(button);

        frame.setSize(300, 200);
        frame.setVisible(true);
    }
}
```

This creates a window containing a label and a button. It does not yet define what happens when the button is clicked.

## 3. Swing

Swing provides a richer set of GUI components. Most Swing components are lightweight and implemented primarily in Java, although top-level containers still rely on native windowing infrastructure.

Common Swing components:

| Component | Purpose |
|---|---|
| `JFrame` | Main application window |
| `JPanel` | Container for components |
| `JLabel` | Displays text or an icon |
| `JButton` | Clickable button |
| `JTextField` | Single-line input |
| `JTextArea` | Multiline input |
| `JCheckBox` | Independent selection |
| `JRadioButton` | Usually one choice in a group |
| `JComboBox` | Drop-down list |
| `JList` | List of selectable values |
| `JTable` | Tabular data |
| `JScrollPane` | Adds scrolling to a component |

Example:

```java
import javax.swing.*;

public class SwingExample {
    public static void main(String[] args) {
        JFrame frame = new JFrame("My Swing Window");

        JLabel label = new JLabel("Hello Java");
        JButton button = new JButton("Click Me");

        JPanel panel = new JPanel();
        panel.add(label);
        panel.add(button);

        frame.add(panel);
        frame.setSize(300, 200);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLocationRelativeTo(null);
        frame.setVisible(true);
    }
}
```

**Exam note:** In real Swing applications, create and update the GUI on the Event Dispatch Thread (EDT), commonly using `SwingUtilities.invokeLater()`.

## 4. AWT vs Swing

| AWT | Swing |
|---|---|
| Package: `java.awt` | Package: `javax.swing` |
| Traditional AWT controls often use native peers | Most Swing controls are lightweight |
| Smaller component set | Richer component set |
| Appearance can vary with the platform | Supports pluggable look and feel |
| Example: `Button` | Example: `JButton` |
| Example: `Frame` | Example: `JFrame` |

**Common trap:** Swing is built on top of AWT; it does not replace every AWT class. Swing still uses AWT classes for events, layouts, graphics, and top-level window infrastructure.

## 5. Containers and Layout Managers

Containers hold and organize GUI components.

Examples:
- AWT: `Frame`, `Panel`
- Swing: `JFrame`, `JPanel`

A **layout manager** controls how components are arranged.

### FlowLayout

Places components in a row and wraps them when necessary.

```java
setLayout(new FlowLayout());
```

### BorderLayout

Organizes components into five regions:

- `BorderLayout.NORTH`
- `BorderLayout.SOUTH`
- `BorderLayout.EAST`
- `BorderLayout.WEST`
- `BorderLayout.CENTER`

```java
setLayout(new BorderLayout());
add(new JButton("Top"), BorderLayout.NORTH);
add(new JButton("Center"), BorderLayout.CENTER);
```

The center region receives the remaining available space.

### GridLayout

Arranges components in a grid of rows and columns.

```java
setLayout(new GridLayout(2, 3));
```

This creates a grid with two rows and three columns.

### GridBagLayout

A flexible grid-based layout manager that supports different component sizes and positioning constraints.

**Exam trap:** A layout manager controls component placement; it is not itself a GUI component.

## 6. Event Handling

An **event** is an occurrence such as a button click, key press, or mouse movement.

Java's traditional GUI event handling uses the **event delegation model**.

Its three main parts are:

1. **Event source:** The component that generates an event.
2. **Event object:** Contains information about the event.
3. **Event listener:** An object registered to handle the event.

For example, a `JButton` can generate an `ActionEvent`, which is handled by an `ActionListener`.

## 7. The `ActionListener` Interface

`ActionListener` belongs to `java.awt.event`.

It defines one required method:

```java
void actionPerformed(ActionEvent e);
```

Example:

```java
import javax.swing.*;
import java.awt.event.*;

public class EventExample {
    public static void main(String[] args) {
        JFrame frame = new JFrame("Event Example");
        JButton button = new JButton("Click Me");

        button.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                System.out.println("Button clicked");
            }
        });

        frame.add(button);
        frame.setSize(300, 200);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLocationRelativeTo(null);
        frame.setVisible(true);
    }
}
```

Each click normally generates an action event and prints `Button clicked`.

### Using a Lambda Expression

Because `ActionListener` is a functional interface, a lambda can implement it:

```java
button.addActionListener(e ->
    System.out.println("Button clicked")
);
```

This is shorter and common in modern Java.

## 8. Common Event Classes and Listeners

| Event class | Listener interface | Typical event |
|---|---|---|
| `ActionEvent` | `ActionListener` | Button activation |
| `ItemEvent` | `ItemListener` | Selection change |
| `MouseEvent` | `MouseListener` / `MouseMotionListener` | Mouse activity |
| `KeyEvent` | `KeyListener` | Keyboard activity |
| `WindowEvent` | `WindowListener` | Window opening or closing |
| `FocusEvent` | `FocusListener` | Focus gained or lost |
| `AdjustmentEvent` | `AdjustmentListener` | Scrollbar adjustment |
| `TextEvent` | `TextListener` | AWT text value change |

The exact event depends on the component and the action performed.

**Exam trap:** A `JButton` click is commonly handled with `ActionListener`, not `MouseListener`. Action events represent the button's activation and can occur through keyboard interaction as well.

## 9. Adapter Classes

Some listener interfaces have several methods. Implementing the entire interface can be inconvenient when only one method is needed.

Adapter classes provide empty implementations of listener methods so a subclass can override only the methods it needs.

Examples include:
- `MouseAdapter`
- `KeyAdapter`
- `WindowAdapter`
- `FocusAdapter`

Example:

```java
import java.awt.event.*;
import javax.swing.*;

public class WindowExample {
    public static void main(String[] args) {
        JFrame frame = new JFrame("Window");

        frame.addWindowListener(new WindowAdapter() {
            public void windowClosing(WindowEvent e) {
                System.out.println("Window closing");
            }
        });

        frame.setSize(300, 200);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
    }
}
```

**Important:** You cannot extend two classes at once. If your class already extends another class, you can implement a listener interface instead of extending an adapter class.

## 10. Anonymous Inner Classes

An anonymous inner class is a class without a declared name, created and instantiated in one expression.

Example:

```java
button.addActionListener(new ActionListener() {
    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println("Clicked");
    }
});
```

It is useful for small event handlers.

If a functional interface is used, a lambda is often a simpler alternative.

## 11. `JRadioButton` and `ButtonGroup`

A `JRadioButton` represents an option. A `ButtonGroup` makes radio buttons mutually exclusive.

```java
import javax.swing.*;

public class RadioExample {
    public static void main(String[] args) {
        JRadioButton java = new JRadioButton("Java");
        JRadioButton python = new JRadioButton("Python");

        ButtonGroup group = new ButtonGroup();
        group.add(java);
        group.add(python);
    }
}
```

Adding radio buttons to the same group prevents more than one of those buttons from being selected at once.

**Exam trap:** `ButtonGroup` manages selection; it is not a visible GUI component and does not itself lay out the buttons.

## 12. `JCheckBox` vs `JRadioButton`

- A `JCheckBox` allows independent on/off choices.
- A `JRadioButton` is often used for one selection among mutually exclusive choices.
- Radio buttons become mutually exclusive when placed in the same `ButtonGroup`.

Example: selecting several programming languages is a checkbox use case; selecting exactly one payment method is a radio-button use case.

## 13. Swing's Event Dispatch Thread

Swing uses an Event Dispatch Thread (EDT) to process GUI events and most GUI updates.

Example:

```java
import javax.swing.*;

public class Main {
    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            JFrame frame = new JFrame("Safe GUI Startup");
            frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            frame.setSize(300, 200);
            frame.setVisible(true);
        });
    }
}
```

Avoid long-running operations on the EDT because they can make the interface unresponsive. Background work can be handled using appropriate concurrency tools such as `SwingWorker`.

## 14. Rapid Revision Table

| Topic | Key fact |
|---|---|
| AWT package | `java.awt` |
| Swing package | `javax.swing` |
| AWT button | `Button` |
| Swing button | `JButton` |
| AWT window | `Frame` |
| Swing window | `JFrame` |
| Button action listener | `ActionListener` |
| Button click event | Usually `ActionEvent` |
| Listener method | `actionPerformed(ActionEvent e)` |
| Event model | Event delegation model |
| Adapter class | Provides empty listener-method implementations |
| FlowLayout | Places components sequentially and wraps |
| BorderLayout | Five regions |
| GridLayout | Rows

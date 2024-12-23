import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.security.SecureRandom;

public class PasswordGenerator {

    public static void main(String[] args) {
        // Create the main window (JFrame)
        JFrame window = new JFrame("Password Generator");
        window.setSize(400, 300);
        window.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        window.setLayout(new FlowLayout());
        window.getContentPane().setBackground(new Color(240, 240, 240)); // Set background color

        // Create and add components
        JLabel lengthLabel = new JLabel("Password Length:");
        lengthLabel.setForeground(Color.BLACK);
        window.add(lengthLabel);

        JTextField lengthField = new JTextField(10);
        window.add(lengthField);

        JButton generateButton = new JButton("Generate Password");
        window.add(generateButton);

        JLabel passwordLabel = new JLabel("Generated Password:");
        passwordLabel.setForeground(Color.BLACK);
        window.add(passwordLabel);

        JTextArea passwordText = new JTextArea(4, 30);
        passwordText.setEditable(false);
        passwordText.setForeground(Color.BLACK);
        passwordText.setLineWrap(true);
        passwordText.setWrapStyleWord(true);
        window.add(new JScrollPane(passwordText));

        JButton copyButton = new JButton("Copy to Clipboard");
        window.add(copyButton);

        // Action to generate password
        generateButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                try {
                    int length = Integer.parseInt(lengthField.getText());

                    if (length <= 0) {
                        JOptionPane.showMessageDialog(window, "Password length should be greater than zero.", "Error", JOptionPane.ERROR_MESSAGE);
                        return;
                    }

                    // Define character set
                    String characters = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*()_+[]{}|;:,.<>?/`~";
                    StringBuilder password = new StringBuilder();
                    SecureRandom random = new SecureRandom();

                    // Generate random password
                    for (int i = 0; i < length; i++) {
                        int index = random.nextInt(characters.length());
                        password.append(characters.charAt(index));
                    }

                    // Update the text area with the generated password
                    passwordText.setText(password.toString());

                } catch (NumberFormatException ex) {
                    JOptionPane.showMessageDialog(window, "Please enter a valid number for password length.", "Error", JOptionPane.ERROR_MESSAGE);
                }
            }
        });

        // Action to copy password to clipboard
        copyButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                String password = passwordText.getText();
                if (!password.isEmpty()) {
                    Toolkit.getDefaultToolkit().getSystemClipboard().setContents(new StringSelection(password), null);
                    JOptionPane.showMessageDialog(window, "Password copied to clipboard!", "Success", JOptionPane.INFORMATION_MESSAGE);
                } else {
                    JOptionPane.showMessageDialog(window, "No password to copy!", "Warning", JOptionPane.WARNING_MESSAGE);
                }
            }
        });

        // Set the window to be visible
        window.setVisible(true);
    }
}

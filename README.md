import java.awt.*;
import java.awt.event.*;
import javax.swing.*;
import java.io.*;

public class TypingSpeedTesterMain implements ActionListener {
    
    private TypingSpeedTesterGUI gui;
    private Timer timer;
    private int elapsedTime = 0;
    private boolean running = false;
    
    public TypingSpeedTesterMain() {
        gui = new TypingSpeedTesterGUI();
        gui.getStartButton().addActionListener(this);
        gui.getChooseFileButton().addActionListener(this);
        
        timer = new Timer(1000, e -> {
            elapsedTime++;
            gui.getTimerLabel().setText("Time: " + elapsedTime);
            if (elapsedTime >= 60) {
                running = false;
                timer.stop();
                int wordCount = gui.getInputArea().getText().split("\\s+").length;
                int wpm = (int) Math.round((double) wordCount / elapsedTime * 60);
                gui.getResultLabel().setText("Your typing speed is " + wpm + " WPM");
                gui.getStartButton().setEnabled(true);
            }
        });
    }
    
    public void actionPerformed(ActionEvent e) {
        if (e.getSource() == gui.getChooseFileButton()) {
            JFileChooser chooser = new JFileChooser();
            int result = chooser.showOpenDialog(gui);
            if (result == JFileChooser.APPROVE_OPTION) {
                try {
                    File file = chooser.getSelectedFile();
                    FileReader reader = new FileReader(file);
                    BufferedReader br = new BufferedReader(reader);
                    StringBuilder sb = new

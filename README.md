# Projects-

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class QuizApplication {

   
    private JFrame frame;
    private JLabel questionLabel, timerLabel;
    private JRadioButton option1, option2, option3, option4;
    private ButtonGroup optionsGroup;
    private JButton nextButton;
    private int questionIndex = 0;
    private int timeLeft = 10; // Time in seconds for each question
    private Timer timer;

    private int score = 0; // Track correct answers

    private String[] questions = {
            "What is the capital of France?",
            "Which planet is known as the Red Planet?",
            "Who wrote 'Hamlet'?"
    };

    private String[][] options = {
            {"Berlin", "Madrid", "Paris", "Rome"},
            {"Earth", "Mars", "Jupiter", "Saturn"},
            {"Shakespeare", "Dickens", "Hemingway", "Tolstoy"}
    };

    private int[] correctAnswers = {2, 1, 0}; // Index of correct answers

    public QuizApplication() {
        // Initialize the GUI
        frame = new JFrame("Quiz Application");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(450, 400);
        frame.setLayout(new BorderLayout());

        questionLabel = new JLabel();
        questionLabel.setHorizontalAlignment(SwingConstants.CENTER);
        questionLabel.setFont(new Font("Arial", Font.BOLD, 16));
        frame.add(questionLabel, BorderLayout.NORTH);

        JPanel optionsPanel = new JPanel();
        optionsPanel.setLayout(new GridLayout(4, 1));
        option1 = new JRadioButton();
        option2 = new JRadioButton();
        option3 = new JRadioButton();
        option4 = new JRadioButton();
        optionsGroup = new ButtonGroup();
        optionsGroup.add(option1);
        optionsGroup.add(option2);
        optionsGroup.add(option3);
        optionsGroup.add(option4);
        optionsPanel.add(option1);
        optionsPanel.add(option2);
        optionsPanel.add(option3);
        optionsPanel.add(option4);
        frame.add(optionsPanel, BorderLayout.CENTER);

        JPanel bottomPanel = new JPanel();
        bottomPanel.setLayout(new BorderLayout());

        timerLabel = new JLabel("Time left: " + timeLeft + "s");
        timerLabel.setHorizontalAlignment(SwingConstants.CENTER);
        bottomPanel.add(timerLabel, BorderLayout.WEST);

        nextButton = new JButton("Next");
        bottomPanel.add(nextButton, BorderLayout.EAST);
        frame.add(bottomPanel, BorderLayout.SOUTH);

        nextButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                checkAnswer();
            }
        });

        loadQuestion();

        frame.setVisible(true);
    }

    private void loadQuestion() {
        if (questionIndex < questions.length) {
            questionLabel.setText(questions[questionIndex]);
            option1.setText(options[questionIndex][0]);
            option2.setText(options[questionIndex][1]);
            option3.setText(options[questionIndex][2]);
            option4.setText(options[questionIndex][3]);
            optionsGroup.clearSelection();
            timeLeft = 10;
            timerLabel.setText("Time left: " + timeLeft + "s");
            startTimer();
        } else {
            endQuiz();
        }
    }

    private void startTimer() {
        if (timer != null) {
            timer.stop();
        }
        timer = new Timer(1000, new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                timeLeft--;
                timerLabel.setText("Time left: " + timeLeft + "s");
                if (timeLeft <= 0) {
                    timer.stop();
                    JOptionPane.showMessageDialog(frame, "Time's up! The correct answer was: " +
                            options[questionIndex][correctAnswers[questionIndex]]);
                    questionIndex++;
                    loadQuestion();
                }
            }
        });
        timer.start();
    }

    private void checkAnswer() {
        if (timer != null) {
            timer.stop();
        }
        int selectedOption = -1;
        if (option1.isSelected()) {
            selectedOption = 0;
        } else if (option2.isSelected()) {
            selectedOption = 1;
        } else if (option3.isSelected()) {
            selectedOption = 2;
        } else if (option4.isSelected()) {
            selectedOption = 3;
        }

        if (selectedOption == correctAnswers[questionIndex]) {
            JOptionPane.showMessageDialog(frame, "Correct!");
            score++;
        } else if (selectedOption != -1) { // Avoid message if no selection
            JOptionPane.showMessageDialog(frame, "Wrong! The correct answer was: " +
                    options[questionIndex][correctAnswers[questionIndex]]);
        }

        questionIndex++;
        loadQuestion();
    }

    private void endQuiz() {
        JOptionPane.showMessageDialog(frame, "Quiz Over! Your score: " + score + "/" + questions.length);
        frame.dispose();
    }

    public static void main(String[] args) {
        new QuizApplication();
    }
}

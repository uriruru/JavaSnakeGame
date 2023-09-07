import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.ComponentAdapter;
import java.awt.event.ComponentEvent;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;
import java.util.ArrayList;
import java.util.Random;

public class SnakeGame extends JPanel implements ActionListener {
    private static final int UNIT_SIZE = 20;
    private static final int INITIAL_DELAY = 200; // Initial delay for snake speed (adjust as needed)
    private int WIDTH;
    private int HEIGHT;
    private int GAME_UNITS;
    private final ArrayList<Integer> snakeX = new ArrayList<>();
    private final ArrayList<Integer> snakeY = new ArrayList<>();
    private int appleX;
    private int appleY;
    private int direction = KeyEvent.VK_RIGHT;
    private boolean running = false;
    private boolean appleEaten = false;
    private int score = 0;
    private boolean isPaused = false;
    private Timer timer;

    public SnakeGame(int width, int height) {
        WIDTH = width;
        HEIGHT = height;
        GAME_UNITS = (WIDTH * HEIGHT) / (UNIT_SIZE * UNIT_SIZE);

        setPreferredSize(new Dimension(WIDTH, HEIGHT));
        setBackground(Color.BLACK);
        setFocusable(true);
        addKeyListener(new MyKeyAdapter());
        addComponentListener(new MyComponentAdapter());
        startGame();
    }

    public void startGame() {
        if (timer != null) {
            timer.stop();
        }

        snakeX.clear();
        snakeY.clear();
        snakeX.add(WIDTH / 2);
        snakeY.add(HEIGHT / 2);
        spawnApple();
        running = true;
        appleEaten = false;
        score = 0;
        direction = KeyEvent.VK_RIGHT;
        timer = new Timer(INITIAL_DELAY, this);
        timer.start();
    }

    public void spawnApple() {
        Random random = new Random();
        appleX = random.nextInt((WIDTH / UNIT_SIZE)) * UNIT_SIZE;
        appleY = random.nextInt((HEIGHT / UNIT_SIZE)) * UNIT_SIZE;
    }

    public void move() {
        int newX = snakeX.get(0);
        int newY = snakeY.get(0);

        switch (direction) {
            case KeyEvent.VK_UP:
                newY -= UNIT_SIZE;
                break;
            case KeyEvent.VK_DOWN:
                newY += UNIT_SIZE;
                break;
            case KeyEvent.VK_LEFT:
                newX -= UNIT_SIZE;
                break;
            case KeyEvent.VK_RIGHT:
                newX += UNIT_SIZE;
                break;
        }

        if (newX == appleX && newY == appleY) {
            appleEaten = true;
            score++;
            spawnApple();
        }

        if (!appleEaten) {
            snakeX.remove(snakeX.size() - 1);
            snakeY.remove(snakeY.size() - 1);
        } else {
            appleEaten = false;
        }

        snakeX.add(0, newX);
        snakeY.add(0, newY);

        checkCollision();
    }

    public void checkCollision() {
        // Check wall collision
        if (snakeX.get(0) < 0 || snakeX.get(0) >= WIDTH || snakeY.get(0) < 0 || snakeY.get(0) >= HEIGHT) {
            running = false;
        }

        // Check self collision
        for (int i = 1; i < snakeX.size(); i++) {
            if (snakeX.get(i) == snakeX.get(0) && snakeY.get(i) == snakeY.get(0)) {
                running = false;
                break;
            }
        }

        if (!running) {
            // Game over
            // Optionally, you can display a game over message here
        }
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        if (running && !isPaused) {
            move();
        }
        repaint();
    }

    @Override
    protected void paintComponent(Graphics g) {
        super.paintComponent(g);
        draw(g);
    }

    public void draw(Graphics g) {
        if (running) {
            // Draw the apple
            g.setColor(Color.RED);
            g.fillOval(appleX, appleY, UNIT_SIZE, UNIT_SIZE);

            // Draw the snake
            for (int i = 0; i < snakeX.size(); i++) {
                g.setColor(Color.GREEN);
                g.fillRect(snakeX.get(i), snakeY.get(i), UNIT_SIZE, UNIT_SIZE);
            }

            // Draw the score
            g.setColor(Color.WHITE);
            g.setFont(new Font("Arial", Font.BOLD, 20));
            g.drawString("Score: " + score, 10, 20);
        } else {
            // Display game over message
            g.setColor(Color.RED);
            g.setFont(new Font("Arial", Font.BOLD, 40));
            g.drawString("Game Over", WIDTH / 2 - 80, HEIGHT / 2 - 20);
            g.setColor(Color.WHITE);
            g.setFont(new Font("Arial", Font.PLAIN, 20));
            g.drawString("Press 'R' to Restart", WIDTH / 2 - 100, HEIGHT / 2 + 20);
        }

        if (isPaused) {
            g.setColor(Color.WHITE);
            g.setFont(new Font("Arial", Font.BOLD, 30));
            g.drawString("Paused", WIDTH / 2 - 40, HEIGHT / 2 - 20);
        }
    }

    private class MyKeyAdapter extends KeyAdapter {
        @Override
        public void keyPressed(KeyEvent e) {
            int key = e.getKeyCode();
            if (running) {
                if (key == KeyEvent.VK_P) {
                    isPaused = !isPaused;
                }
                if (!isPaused) {
                    if ((key == KeyEvent.VK_LEFT) && (direction != KeyEvent.VK_RIGHT)) {
                        direction = KeyEvent.VK_LEFT;
                    }
                    if ((key == KeyEvent.VK_RIGHT) && (direction != KeyEvent.VK_LEFT)) {
                        direction = KeyEvent.VK_RIGHT;
                    }
                    if ((key == KeyEvent.VK_UP) && (direction != KeyEvent.VK_DOWN)) {
                        direction = KeyEvent.VK_UP;
                    }
                    if ((key == KeyEvent.VK_DOWN) && (direction != KeyEvent.VK_UP)) {
                        direction = KeyEvent.VK_DOWN;
                    }
                    if (key == KeyEvent.VK_R) {
                        startGame();
                    }
                }
            } else {
                if (key == KeyEvent.VK_R) {
                    startGame();
                }
            }
        }
    }

    private class MyComponentAdapter extends ComponentAdapter {
        @Override
        public void componentResized(ComponentEvent e) {
            int newWidth = getWidth();
            int newHeight = getHeight();
            int newGameUnits = (newWidth * newHeight) / (UNIT_SIZE * UNIT_SIZE);

            if (newGameUnits != GAME_UNITS) {
                WIDTH = newWidth;
                HEIGHT = newHeight;
                GAME_UNITS = newGameUnits;

                // Restart the game with the new dimensions
                startGame();
            }
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            JFrame frame = new JFrame("Snake Game");
            int width = 400; // Set your desired initial width
            int height = 400; // Set your desired initial height
            SnakeGame game = new SnakeGame(width, height);
            frame.add(game);
            frame.pack();
            frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            frame.setLocationRelativeTo(null);
            frame.setVisible(true);
        });
    }
}

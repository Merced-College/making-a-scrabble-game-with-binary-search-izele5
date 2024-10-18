// Fernando Gonzalez, Brian Carreno, Izel Escoto, Divaey Kumar
// 10/17/2024
import java.util.*;
import java.io.*;

public class ScrabbleGame {
    private ArrayList<Word> wordList = new ArrayList<>(); // List to store words from the dictionary
    private Random random = new Random(); // Random number generator for selecting letters
    private char[] letters = new char[4]; // Array to hold the 4 random letters given to the user

    // Constructor to initialize the game and load the dictionary file
    public ScrabbleGame() throws IOException {
        loadDictionary("CollinsScrabbleWords_2019.txt"); // Load words from the specified dictionary file
    }

    // Method to load the dictionary from a file and store the words in the wordList
    private void loadDictionary(String dictionaryFile) throws IOException {
        BufferedReader reader = new BufferedReader(new FileReader(dictionaryFile)); // Open file for reading
        String line;
        while ((line = reader.readLine()) != null) { 
            wordList.add(new Word(line.trim().toUpperCase())); // Add each word to wordList, trimming whitespace and converting to uppercase
        }
        Collections.sort(wordList); // Sort the wordList to enable binary search
        reader.close(); // Close the file reader
    }

    // Method to generate 4 random letters for the player
    private void generateRandomLetters() {
        for (int i = 0; i < 4; i++) {
            letters[i] = (char) ('A' + random.nextInt(26)); // Generate a random letter (A-Z) for each index
        }
    }

    // Method to perform a binary search and check if the player's word is valid (exists in the dictionary)
    private boolean isValidWord(String word) {
        Word searchWord = new Word(word); // Create a Word object with the player's input
        int index = Collections.binarySearch(wordList, searchWord); // Perform binary search in wordList
        return index >= 0; // Return true if the word is found, false otherwise
    }

    // Method to allow the user to exchange one of their letters
    private void exchangeLetter() {
        Scanner scanner = new Scanner(System.in); // Scanner to read user input
        System.out.println("Would you like to exchange one of your letters? (yes/no)");
        String response = scanner.nextLine().toLowerCase(); // Get the user's response and convert it to lowercase

        if (response.equals("yes")) { // If the user wants to exchange a letter
            System.out.println("Enter the index (1-4) of the letter you want to exchange:");
            int index = scanner.nextInt() - 1; // Get the index of the letter to exchange (1-4), and adjust for 0-based index
            if (index >= 0 && index < 4) { // Ensure the index is valid
                letters[index] = (char) ('A' + random.nextInt(26)); // Generate a new random letter and replace the chosen one
                System.out.println("Letter exchanged! Your new letters are: " + Arrays.toString(letters)); // Show the new set of letters
            } else {
                System.out.println("Invalid index."); // Handle invalid input
            }
        }
    }

    // Main game loop
    public void play() {
        generateRandomLetters(); // Generate 4 random letters for the user
        System.out.println("Your letters are: " + Arrays.toString(letters)); // Display the letters

        exchangeLetter(); // Give the user the option to exchange a letter

        Scanner scanner = new Scanner(System.in); // Scanner to read user input for the word
        System.out.println("Enter a word made from these letters:");
        String userWord = scanner.nextLine().toUpperCase(); // Get the user's word and convert it to uppercase

        if (isValidWord(userWord)) { // Check if the word is valid using binary search
            System.out.println("Valid word: " + userWord); // If valid, print success message
        } else {
            System.out.println("Invalid word. Try again."); // If invalid, print failure message
        }
    }

    // Main method to start the game
    public static void main(String[] args) throws IOException {
        ScrabbleGame game = new ScrabbleGame(); // Create a new ScrabbleGame instance
        game.play(); // Start the game
    }
}
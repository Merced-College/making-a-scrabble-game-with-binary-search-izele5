// Fernando Gonzalez, Brian Carreno, Izel Escoto, Divaey Kumar
// 10/17/2024

import java.util.*;

public class Word implements Comparable<Word> {
    private String text;

    public Word(String text) {
        this.text = text;
    }

    public String getText() {
        return text;
    }

    @Override
    public int compareTo(Word other) {
        return this.text.compareTo(other.text);
    }
}